# IBM Distributed library for TensorFlow

## Introduction
IBM Deep learning Distributed library (DDL) hooks into popular open source machine learning frameworks like TensorFlow, Caffe, Torch and Chainer and enables these frameworks to scale to multiple GPUs.  In this cloud service, we support IBM DDL with TensorFlow. 

## Integration with TensorFlow
DDL is indirectly integrated into TensorFlow in the form of custom operators (see below for more details on the integration). 
The custom operators are provided in a shared library, which is loaded and invoked in the Python training script for the end user when the job requests
ddl enabled framework.


## Usage

We expect the user to have a single GPU training code prior to integrating DDL code. Once DDL is integrated that same script will now train on multiple
GPUs. 

To use DDL enabled Tensorflow, make the following changes to the single GPU program:

1. Add `import ddl` in user python script. This should be imported anywhere before creating a optimizer instance (like SGD).

   This import does the following:

   - it will override tf.Session.init to ensure only one GPU will be used by each learner 
   - it will initialize MPI environment and DDL internally 
   - it will override the optimizer to call AllReduceN functions for gradient reduction across all learners

2. Because DDL trains the job on multiple GPUs, few other additional changes are required to the user script to obtain proper convergence.
   
   - change the learning rate scheduling (ie. multiplying the existing rate with ddl.size()). If per-GPU batch size is kept the same, global batch size will increase with additional GPUs so corresponding increase in learning rate is required to obtain proper convergence.
 distributed training can increase the global/effective batch size
   - generate statistically orthogonal partitions in order to feed each learner with different samples by using ddl.rank()

3. make sure that the ddl.py is present in the PYTHON_PATH.

4. See the example [here](ml_dlaas_e2e_ddl_example.md) to obtain ddl.py file, train a model using DDL and score it in WML. 
    

## DDL functions and semantics
For advanced users of DDL,  understanding DDL approach and the helper functions and operators will enable them to take 
advantage of the full capabilities of DDL.

### DDL Approach

TensorFlow lets a user define a computation (data-flow) graph which subsequently is executed for purposes of for instance training a neural network. The graph consists of operator nodes that perform a certain operation on incoming tensors to produce output result tensors. There are many built-in operator nodes both for mathematical operations but also for logical operations and control-flow. A user can also define her own operators either at the Python level or directly in C++. TensorFlow takes the use of the computation graph to the extreme: everything needed for a particular application will be part of the graph.

Upfront it should be clearly stated that we adopt a strategy where we choose to run a single TensorFlow process per compute device, typically a GPU. The orchestration of having multiple processes run across several hosts each having a number of GPUs is accomplished by MPI. From the point of view of a single worker process, we assume that it runs autonomously without knowledge of its sister processes that together form a single training session. A worker will be supplied with its own batch of data to process and holds its own copy of the set of weights and biases that form the model parameters. To achieve a synchronous training session 3 conditions must be met:

1. Every worker must start in the same state, i.e., initialization of all weights and biases must be guaranteed to be identical. This is easily achieved by announcing 1 worker to be the master and have it broadcast its initial values to all other workers. This step could be skipped if it is known that all initial values will indeed be the same; if there are random number distributions involved one has to ensure that the seeds for all workers are the same.
2. Weights and biases must be updated with knowledge of all workers' gradients. This implies that the gradients must be shared, in fact averaged, across all workers.
3. The weight and bias updates must be kept in lock-step across all workers, in other words, each worker must synchronize with all others at some point in each iteration of the training loop.

To make TensorFlow use DDL as a communication library one has to intercept the gradients after they have been computed and before they are used to update the weight and bias parameters. Since the TensorFlow program works by first constructing a computation graph and, once completed, executing that graph, it should be obvious that a possible solution consists of introducing additional special communication nodes into the graph. This is exactly the approach taken here and the extra operator nodes are provided by the TensorFlow DDL library that exposes several operator creation functions that can be called from a Python script. The integration process therefore consists of these steps:

1. Prepare the necessary TensorFlow operators for DDL communication; this is provided by the TensorFlow DDL shared library.
2. Decide in the Python graph construction script where to introduce the communication operators. This is typically the hardest part since one has to understand the inner workings of the particular third-party script code.
3. Make sure that options are in place that allow a user to switch from regular to DDL execution. Also, protect the user from clashing command-line argument combinations.

### DDL TF operator functions/semantics

1. Init function:  This must be called before any real TF operators. Typically, we can execute this op on CPU using an additional session. The input is the DDL configuration. This will inform the targeted network topology and learner mapping to it. The output consists of MPI information (rank,size) and GPU assignment for TF.

                >           .Input("user_gpuid: int32") => preferred gpuid
                >           .Output("rank: int32")   => MPI rank
                >           .Output("size: int32")   => MPI size
                >           .Output("local_rank: int32")   => local MPI rank
                >           .Output("local_size: int32")   => local MPI size
                >           .Output("local_gpuid: int32")  => assigned gpuid
                >           .Attr("mode: string")    => mode
                >           .Attr("pci: bool = true")    => when True, it will use PCI device number to reorder GPUs

2. Bcast function:  Broadcast is to synchronize all the trainable parameters (i.e., weights and biases) before the training. Broadcast can be called once init has been called and completed on the assigned GPU device. Each and every trainable parameter must be broadcasted to ensure good convergence.

                >     .Input("input: T")     => input tensor to broadcast, always rank0 broadcasts to others
                >     .Output("output: T")   => output tensor with the broadcasted result
                >     .Attr("T: {float}")    => supported datatype

3. AllReduceN function : This is an aggregated version of AllReduce. Essentially, this takes an array of N tensors, performs allreduce in a single shot, and return an array of N reduced tensors. The benefits of using AllReduceN are better performance and simpler integration.

                >     .Input("input: N*T")    => input tensor to allreduce
                >     .Output("output: N*T")  => output tensor
                >     .Attr("op: {'sum', 'avg'}") => reduce operation
                >     .Attr("T: {float}")   => supported datatype
                >     .Attr("mpi: bool = false") => use pure MPI_Allreduce to get a baseline
                >     .Attr("check: float = 0.0") => compare result against MPI_Allreduce when check >0 where check is error-tolerance between
                > 0-1.0

## Model serving

In addition to training with DDL, users need to take those trained models and inference with them. For this the following section 
describes the process to produce servable model from training with non-serializable operators, e.g. for distribution
in tensorflow. 

A servable model is the persisted version of a tf-graph that accepts a set of placeholders as input 
and produces a set of outputs. A graph can only be persisted if all operators in the graph are 
`serializable` by tensorflow. The following steps allow to generate serializable models from 
tensorflow graphs for training that contain non-serializable operators. 

1. Execute the description of the base graph, from e.g. from an input placeholder to classification logits. 
    All inputs and outputs require names that can be used in later steps. 
    Before any other tensorflow functions are called or any session is run. 
    Write this `base graph` via `tf.train.export_meta_graph` to a file. 

2. Augment the graph with the additions to facilitate training, e.g. loss function, optimizer, synchronization operators 
    for parallel/distributed learning etc.. Initialize variables and run training steps. 
    Checkpoints can be written via the tf.train.Saver() object. 
    These checkpoints contain graph export files as well, which may fail to load via the `Saver` API, for example 
    due to training synchronization operations. 

3. Translate the combination of the `base graph` from step 1 with a checkpoint with trained weights from 
    step 2 into a servable model using the model builder API. 
    tf.train.import_meta_graph loads the `base graph`, this returns an object `restorer` through which weights can be loaded into that graph.
    `graph.get_tensor_by_name` allows to create python variables that refer to the inputs and outputs. 
    Then restore weights and other parameters via `restorer.restore()` from one of the saved checkpoints. 

    At this point, the session that performed the weight restoration contains a graph usable for inference. 
    This can be exported through `tf.saved_model.builder.SavedModelBuilder()`, as per tensorflow documentation.
    
    
A note on data loading. The servable model is most likely receiving input data for inference through a different path 
(e.g. a network protocol as HTTP) than training (e.g. from a local filesystem). Thus, if the loading for training 
is part of the initial graph, e.g. taking a file name as input and producing logits as output, it can be problematic 
to establish a different input source. 

A reliable way to produce a servable model is to separate the data loading 
from the actual training step. I.e., the training step has an input placeholder for a tensor or encoded tensor that 
is provided via a call to `session.run()` for a training step and a separate execution, e.g. through another `session.run()` 
or via other means, that produces this input from the training data source so that it can be passed to the 
training step via a `feed_dict`.  Data loading can be established AFTER the basegraph is written, and hence is
separated from the content of the basegraph. 

For more details, please see the Tensorflow serving [documentation](https://www.tensorflow.org/serving/).    

