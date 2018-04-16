---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# End to end example for running a DDL based DL training

This document assumes that you have created an account and have uploaded your training data to IBM COS. Also you have some familiarity with the basic concepts and know how to execute a training. The example below walks through some of the commonly used concepts in training your model and saving your model for scoring via IBM WML

## Concepts
The goal of IBM WML is to basically allow you to run a training that you were able to run on your machine and be able to run the exact same training on IBM WML with little to no changes. The intent is to allow you to use more powerful hardware without the need to make changes in your code. 

Every training via IBM WML basically takes in a definition file that describes what framework/version/compute configuration needs to be used, where the data/results are supposed to be located and what command needs to be executed to process this data and get the results. A example of a basic definition is:

``` yml
model_definition:
  name: tf-mnist
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  framework:
    name: tensorflow-ddl
    version: "1.5"
    runtimes:
      name: python
      version: "3.5"
  execution:
    command: python mnist-train-ddl.py ; python transform.py
    compute_configuration:
      name: p100
      nodes: 2
training_data_reference:
  name: MNIST image data files
  connection:
    endpoint_url: <auth-url>
    access_key_id: <username>
    secret_access_key: <password>
  source:
    bucket: mnist-training-data
  type: s3
training_results_reference:
  name: DL Model Storage
  connection:
    endpoint_url: <auth-url>
    access_key_id: <username>
    secret_access_key: <password>
  target:
    bucket: mnist-training-models
  type: s3
```

Save this as manifest.yml. This definition is responsible for carrying out a simple mnist training using tensorflow 1.5 that is enhanced with IBM DL as mentioned in the framework name (`tensorflow`) and version (`1.5-py3-ddl`) of the framework section above. Notice that the command executed by this training run two python jobs one after another.
The first job executes the command `mpirun <number of parameters> python mnist-train-ddl.py`. After the training is completed, the second job executes
the command `python transform.py`. This jobs takes the meta model and the checkpoints and generate a model that can be used for serving in WML.
We will be running this training on two `P100` GPUs nodes as mentioned in the compute configuration section. 
To train on more or fewer nodes, modify the compute configuration.

The `training_data_reference` section provides information about what bucket in IBM COS has the mnist training data, and the `training_results_reference` provides information on how to connect to result bucket where the results of this training will be stored.

Here's a sample `mnist-train-ddl.py` that can be used for training:

```
'''
Based on https://github.com/aymericdamien/TensorFlow-Examples/blob/master/examples/3_NeuralNetworks/convolutional_network.py:
A Convolutional Network implementation example using TensorFlow library.
This example is using the MNIST database of handwritten digits
(http://yann.lecun.com/exdb/mnist/)
Author: Aymeric Damien
Project: https://github.com/aymericdamien/TensorFlow-Examples/
Modifications:
*****************************************************************
Licensed Materials - Property of IBM
(C) Copyright IBM Corp. 2017. All Rights Reserved.
US Government Users Restricted Rights - Use, duplication or
disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
*****************************************************************
'''
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import argparse
import sys
import tensorflow as tf
import numpy as np
import tempfile
import os
from tensorflow.examples.tutorials.mnist import input_data

chkptpath = os.getenv("RESULT_DIR")+"/mnist"
training_data_dir = os.getenv("DATA_DIR")
learner_id = os.getenv("LEARNER_ID")


if training_data_dir == None:
    exit(1)


FLAGS = None

def deepnn(x,keep_prob):
    """deepnn builds the graph for a deep net for classifying digits.
    Args:
      x: an input tensor with the dimensions (N_examples, 784), where 784 is the
      number of pixels in a standard MNIST image.
    Returns:
      A tuple (y, keep_prob). y is a tensor of shape (N_examples, 10), with values
      equal to the logits of classifying the digit into one of 10 classes (the
      digits 0-9). keep_prob is a scalar placeholder for the probability of
      dropout.
    """
    # Reshape to use within a convolutional neural net.
    # Last dimension is for "features" - there is only one here, since images are
    # grayscale -- it would be 3 for an RGB image, 4 for RGBA, etc.
    with tf.name_scope('reshape'):
        x_image = tf.reshape(x, [-1, 28, 28, 1])

    # First convolutional layer - maps one grayscale image to 32 feature maps.
    with tf.name_scope('conv1'):
        W_conv1 = weight_variable([5, 5, 1, 32])
        b_conv1 = bias_variable([32])
        h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1) + b_conv1)

    # Pooling layer - downsamples by 2X.
    with tf.name_scope('pool1'):
        h_pool1 = max_pool_2x2(h_conv1)

    # Second convolutional layer -- maps 32 feature maps to 64.
    with tf.name_scope('conv2'):
        W_conv2 = weight_variable([5, 5, 32, 64])
        b_conv2 = bias_variable([64])
        h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2)

    # Second pooling layer.
    with tf.name_scope('pool2'):
        h_pool2 = max_pool_2x2(h_conv2)

    # Fully connected layer 1 -- after 2 round of downsampling, our 28x28 image
    # is down to 7x7x64 feature maps -- maps this to 1024 features.
    with tf.name_scope('fc1'):
        W_fc1 = weight_variable([7 * 7 * 64, 1024])
        b_fc1 = bias_variable([1024])
        h_pool2_flat = tf.reshape(h_pool2, [-1, 7 * 7 * 64])
        h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)

    # Dropout - controls the complexity of the model, prevents co-adaptation of
    # features.
    with tf.name_scope('dropout'):
        h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)

    # Map the 1024 features to 10 classes, one for each digit
    with tf.name_scope('fc2'):
        W_fc2 = weight_variable([1024, 10])
        b_fc2 = bias_variable([10])
        y_conv = tf.matmul(h_fc1_drop, W_fc2) + b_fc2
    return y_conv


def conv2d(x, W):
    """conv2d returns a 2d convolution layer with full stride."""
    return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding='SAME')


def max_pool_2x2(x):
    """max_pool_2x2 downsamples a feature map by 2X."""
    return tf.nn.max_pool(x, ksize=[1, 2, 2, 1],
                          strides=[1, 2, 2, 1], padding='SAME')


def weight_variable(shape):
    """weight_variable generates a weight variable of a given shape."""
    initial = tf.truncated_normal(shape, stddev=0.1)
    return tf.Variable(initial)


def bias_variable(shape):
    """bias_variable generates a bias variable of a given shape."""
    initial = tf.constant(0.1, shape=shape)
    return tf.Variable(initial)


def main():
    ############################################################################
    # Import MNIST data
    ############################################################################
    mnist = input_data.read_data_sets(training_data_dir)

    # Parameters
    learning_rate = 0.001
    training_iters = 2500
    batch_size = 100
    display_step = 1

    # Network Parameters
    n_input = 784 # MNIST data input (img shape: 28*28)
    n_classes = 10 # MNIST total classes (0-9 digits)
    dropout = 0.75 # Dropout, probability to keep units

    # tf Graph input
    x = tf.placeholder(tf.float32, [None, n_input], name="x")
    # Construct model
    keep_prob = tf.placeholder_with_default(1.0,shape=(), name="keepprob")
    pred = deepnn(x,1.0)
    pRes = tf.identity(pred,name="pRes")

    if os.getenv("OMPI_COMM_WORLD_RANK") == "0":
         print("writing checkpoint file", chkptpath+"_basegraph.meta")
         tf.train.export_meta_graph(chkptpath+"_basegraph.meta", as_text=True)

    #import the ddl library; this creates objects for distribution so 
    #it must be done after exporting meta graph
    import ddl
    y = tf.placeholder(tf.int64, [None], name="y")
    # Define loss and optimizer
    with tf.name_scope('loss'):
        cost = tf.reduce_mean(
            tf.losses.sparse_softmax_cross_entropy(labels=y, logits=pred))

    with tf.name_scope('adam_optimizer'):
        optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate)
        objective = optimizer.minimize(cost)

    predictor = tf.argmax(pred, 1, name="predictor") 
    # Evaluate model
    with tf.name_scope('accuracy'):
        correct_prediction = tf.equal(predictor, y)
        correct_prediction = tf.cast(correct_prediction, tf.float32)
        accuracy = tf.reduce_mean(correct_prediction)


    saver = tf.train.Saver() 

    # Launch the graph
    with tf.Session(config=tf.ConfigProto()) as sess:
        sess.run(tf.global_variables_initializer())
        step = 1
        # Keep training until reach max iterations
        while step * batch_size < training_iters:

            ###################################################
            ### USE ddl.rank() and ddl.size() to load data  ###
            ###################################################
            batch_x, batch_y = mnist.train.next_batch(batch_size*ddl.size())

            #select one of partitions
            batch_x = np.split(batch_x,ddl.size())[ddl.rank()]
            batch_y = np.split(batch_y,ddl.size())[ddl.rank()]

            # Run optimization op (backprop)
            sess.run(objective, feed_dict={x: batch_x, y: batch_y})
            if step % display_step == 0:
                # Calculate batch loss and accuracy
                loss, acc = sess.run([cost, accuracy], feed_dict={x: batch_x,
                                                                  y: batch_y})
                print("DDL " + str(ddl.rank()) + "] Iter " + str(step * batch_size) +
                  ", Minibatch Loss= " + "{:.6f}".format(loss) +
                  ", Training Accuracy= " + "{:.5f}".format(acc))
            step += 1
            if os.getenv("OMPI_COMM_WORLD_RANK") == "0" and step%10==0 and step!=0:
                saver.save(sess, chkptpath,global_step=step)
                print('[%d] save checkpoint' % step+" path: "+chkptpath)


        print("DDL "+str(ddl.rank())+"] Optimization Finished!")



        # Calculate accuracy for 256 mnist test images
        print("DDL "+str(ddl.rank())+"] Testing Accuracy:", \
            sess.run(accuracy, feed_dict={x: mnist.test.images[:256],
                                          y: mnist.test.labels[:256]}))



main()

```

Here is the transformation file used in the second job (save it as `transform.py`) necessary for transforming the meta graph and the checkpoint to generate a model that can be used with WML serve. 

```

import tensorflow as tf
import os

modelpath = os.getenv("RESULT_DIR")+"/model"
chkptpath = os.getenv("RESULT_DIR")+"/mnist"
chkptnumber = "-20"

################################################################################
# Loads the base graph images->logits with weights from a checkpoint
################################################################################
def load_basegraph(restorepath):
    rname = restorepath+"_basegraph.meta"
    print(" Loading graph from "+rname)
    restorer = tf.train.import_meta_graph(rname)
    graph = tf.get_default_graph()
    # get access to the placeholder in graph
    pImgs = graph.get_tensor_by_name("x:0")
    pRes  = graph.get_tensor_by_name("pRes:0")                              
    kp = graph.get_tensor_by_name("keepprob:0");
    return restorer,pImgs,pRes,kp


################################################################################
def chkpt2model(graphloadpath,weightloadpath,modelwritepath):
    
    print("Running the program to transform meta graph and checkpoint in servable model")
    restorer,pImgs,pRes,kp = load_basegraph(graphloadpath)
    values, indices = tf.nn.top_k(pRes, 10)                                     
    # for modelbuilder 
    prd = tf.argmax(pRes, 1, name="predictor") 

    config = tf.ConfigProto()
    config.gpu_options.allow_growth=True
    with tf.Session(config=config) as sess:
        restorer.restore(sess, weightloadpath)

        sess.graph._unsafe_unfinalize()
        export_path = modelwritepath
        builder = tf.saved_model.builder.SavedModelBuilder(export_path)
        classification_inputs = tf.saved_model.utils.build_tensor_info(pImgs)
        classification_outputs_classes = tf.saved_model.utils.build_tensor_info(prd)

        classification_signature = tf.saved_model.signature_def_utils.build_signature_def(
            inputs={tf.saved_model.signature_constants.CLASSIFY_INPUTS: classification_inputs},
            outputs={
                tf.saved_model.signature_constants.CLASSIFY_OUTPUT_CLASSES: 
                                               classification_outputs_classes
            },
            method_name=tf.saved_model.signature_constants.CLASSIFY_METHOD_NAME)

        tensor_info_x = tf.saved_model.utils.build_tensor_info(pImgs)
        tensor_info_y = tf.saved_model.utils.build_tensor_info(pRes)
        tensor_info_c = tf.saved_model.utils.build_tensor_info(prd)
        prediction_signature = tf.saved_model.signature_def_utils.build_signature_def(
            inputs={'images': tensor_info_x},
            outputs={'class_index': tensor_info_c},
            method_name=tf.saved_model.signature_constants.PREDICT_METHOD_NAME)

        legacy_init_op = tf.group(tf.tables_initializer(), name='legacy_init_op')

        builder.add_meta_graph_and_variables(
            sess, [tf.saved_model.tag_constants.SERVING],
            signature_def_map={
                'predict_images': prediction_signature,
                tf.saved_model.signature_constants.DEFAULT_SERVING_SIGNATURE_DEF_KEY: classification_signature,
            },
            legacy_init_op=legacy_init_op,
            clear_devices=True)
        
        save_path = str(builder.save())
        print("Model for serving is saved here: %s" % save_path)


chkpt2model(chkptpath,chkptpath+chkptnumber,modelpath)

```


There are a few concepts worth noting here. There are a handful of environment variables available in the environment in which the training is executed:

- **DATA_DIR**: this is a reference to your training data bucket mounted as a folder. In the above example we use the `$DATA_DIR/mnist` which translates to `training data bucket/mnist` where we have our mnist data set.
- **RESULT_DIR**: this is a reference to your current training_id/ folder under result bucket. This is unique per training. Use this to save your meta model and checkpoints. In the above example we write the meta model to `$RESULT_DIR/mnist` which translated to `result bucket/<training id>/mnist`. Also the checkpoints are written to `$RESULT_DIR/mnist-<iteration-id>`.


To extract a model that can be used in WML serve capability, additional changes are made in the above code. First, before DDL operators are loaded,
we generate a meta graph using the `tf.train.export_meta_graph()` as shown above. Second, we save the weights of the model using
`tf_saver.Saver().save()` method. These two provides the necessary bits to generate a model protobuf and variables for inference. An additional
`ddl.py` file is necessary, see below to get the code. For additional 
information on how to generate a model that can used in serving with Tensorflow, please refer to the [tensorflow documentation](https://www.tensorflow.org/serving/serving_basic).

Since the above TF session consists of DDL operators, we need to create a new python process with the contents of the `transform.py` file above to 
convert the meta graph and the weights of the model into model protobuf and variables folder for inferencing. 
Notice that the first job, when run in DLaaS environment saves the model in the `RESULT_DIR/mnist` directory, which is a 
directory suffixed by the training id of that job. The second job grabs that directory name, loads the meta-graph and the checkpoint data.
The checkpoint to load can be controlled by this parameter: `chkptnumber`. This enables users to test the accuracy of the models
at various points in the training phase.  This job will run and
produce outputs in `RESULT_DIR/model` folder. There you should find a `saved_model.pb` and a `variables` folder. 

Now the model is ready for scoring in the WML service following the steps described [here](https://apsx.stage1.ng.bluemix.net/docs/content/analyze-data/ml_dlaas_model_deploy_score.html?context=analytics)

As mentioned above, notice that the code imports `ddl.py` file. 
This files enables IBM DL based distribution of the training. In a distributed 
scheme, multiple learners start on multiple nodes so the user needs to enhance the code to ensure certain operations like generating 
checkpoints are done by one learner instead of all learners. For this, notice that ddl provides a function ( `ddl.rank()` ) to the get the rank of the 
tasks. This can be used to get the rank and take appropriate steps like splitting the input data and printing the progress. Please [see](ml_dlaas_ibm_ddl.md) 
here for more details about DDL APIs.

Place the content of this snippet in `ddl.py` and put it in the same folder as the other files.

```

# *****************************************************************
#
# Licensed Materials - Property of IBM
#
# (C) Copyright IBM Corp. 2018. All Rights Reserved.
#
# US Government Users Restricted Rights - Use, duplication or
# disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
#
# *****************************************************************

# Integration of DDL in Tensorflow scripts

# This Python module exports the following functions:
# - num_hosts(): number of hosts deployed
# - rank(): MPI rank number assigned to this process
# - size(): total number of MPI ranks (= number of GPUs)
# - local_rank(): id of GPU used by this process

# - init(options): initialize this module
# - bcast(init_val): broadcast initial value
# - allreduce_n(grads): DDL-reduces all gradient tensors in one go
# - grads_reduce(grads_and_vars):

import os
import tensorflow as tf

# Design decision: get all MPI data via env NOT from ddl_MDR.init()!

mpi_init_status = True

# Get some MPI environment variables values:
mpi_vars = { 'OMPI_COMM_WORLD_SIZE':0,
             'OMPI_COMM_WORLD_LOCAL_SIZE':0,
             'OMPI_COMM_WORLD_RANK':0,
             'OMPI_COMM_WORLD_LOCAL_RANK':0 }

# Ensure all the necessary ENV variables are available;
# else don't initialize DDL & restrict DDL API invocations.
for var in mpi_vars:
    val = os.getenv(var)
    if val is None:
        mpi_init_status = False
        break;
    mpi_vars[var] = val

if mpi_init_status:
    #Get as number:
    world_size = int(mpi_vars['OMPI_COMM_WORLD_SIZE'])
    local_size = int(mpi_vars['OMPI_COMM_WORLD_LOCAL_SIZE'])
    world_rank = int(mpi_vars['OMPI_COMM_WORLD_RANK'])
    rank_local = int(mpi_vars['OMPI_COMM_WORLD_LOCAL_RANK'])

    # Get any options to configure DDL:
    DDL_OPTIONS = os.getenv('DDL_OPTIONS')
    if DDL_OPTIONS is None:
        print('DDL: DDL_OPTIONS is not set; need explicit init(options).')
else:
    world_size = 1
    local_size = 1
    world_rank = 0
    rank_local = 0

# The number of nodes (or hosts)
def num_hosts():
    return world_size // local_size

# rank runs from 0 to size (excl)
# uniquely identifies each parallel process
def rank():
    return world_rank

# size is the total number of GPUs (MPI processes)
# typically product of number of nodes and GPUs per node
def size():
    return world_size

# local_rank runs from 0 to the number of GPUs (excl) per node
# number of GPUs typically the same for each node, say 4
def local_rank():
    return rank_local

_tf_Session = tf.Session.__init__

def new_init(session_object, target='', graph=None, config=None):
    # Echo all info so far using our own API:
    # mpi info and assigned GPU:
    print('DDL: rank: {}, size: {}, gpuid: {}, hosts: {}'.format(
        rank(), size(), local_rank(), num_hosts()))

    if config is None:
        config = tf.ConfigProto()

    # Weird TF behavior: without allow_growth set here, TF will already
    # here allocate all memory on the GPU; even if later sessions use
    # a config that also sets allow_growth.
    config.gpu_options.allow_growth = True
    config.gpu_options.visible_device_list = str(local_rank())

    _tf_Session(session_object, target, graph, config)

# Session redefined, if required
if mpi_init_status:
    tf.Session.__init__ = new_init

ddl_MDR=None
def _init(options):
    """A function which initializes DDL.
    """
    # Dynamically load DDL TF library:
    global ddl_MDR
    if ddl_MDR is not None:
        raise RuntimeError('DDL has already been initialized.')
    ddl_MDR = tf.load_op_library('ddl_MDR.so')

    # Initialize DDL:
    with tf.Session() as sess:
        sess.run(ddl_MDR.init(local_rank(), mode = options))
    print('DDL: module {} initialized.'.format(rank()))

_prev_bcast_op = None
# Creates broadcast node to share MPI root initial value among workers.
# Inserts control dependency on global prev_bcast_op under the covers.
# Returns initial value (Tensor) of root.
def _bcast(init_val):
    global _prev_bcast_op
    if _prev_bcast_op is None: # first time
        init_val = ddl_MDR.bcast(init_val)
    else:
        with tf.control_dependencies([_prev_bcast_op]):
            init_val = ddl_MDR.bcast(init_val)
    _prev_bcast_op = init_val
    return init_val

# Intercept tf.Variable constructor:
_tf_Var = tf.Variable.__init__

# Our replacement:
# Insert a Bcast operator that will provide the initial value.
# Mind to enforce a certain consistent execution order across multiple instances.
def newVar(self, *args, **kwargs):
  #print("rank: {}, kwargs: {}".format(rank, kwargs))
  if 'trainable' in kwargs and kwargs['trainable'] and 'initial_value' in kwargs:
    name = kwargs['name'] if 'name' in kwargs else 'Anon'
    #print("* Initialized TF variable {}".format(name))
    init_val = kwargs['initial_value']
    del kwargs['initial_value']
    # Mind: init_val could be a callable.
    #print(init_val)
    with tf.device('/gpu:0'):
      if callable(init_val):
        init_val = init_val()
        #print(init_val)
      init_val = _bcast(init_val)
      #print(init_val)
      _tf_Var(self, initial_value=init_val, *args, **kwargs)
  else:
    _tf_Var(self, *args, **kwargs)

# Maybe have function that broadcasts init value for all variables?

# One big AllReduce over all grads.
def _allreduce_n(grads, average=True, device_dense='', device_sparse=''):
    #with tf.device('/gpu:0'):
    return ddl_MDR.all_reduce_n(grads, op='avg' if average else 'sum')

# Reduces gradients in grads_and_vars list.

def _grads_reduce_one(grads_and_vars, average=True):
    # Unzip the list of tuples:
    grads, vars = zip(*grads_and_vars)
    # One big AllReduce over all grads; zip again with the weights:
    return zip(_allreduce_n(grads, average), vars)
    
def _grads_reduce_two(grads_and_vars, average=True):

    divider0 = len(grads_and_vars)/2;
    grads_and_vars0 = grads_and_vars[:divider0]
    grads_and_vars1 = grads_and_vars[divider0:]

    return _grads_reduce_one(grads_and_vars0,average) + _grads_reduce_one(grads_and_vars1,average)
 
dll_group_size = 10000000000
if os.getenv('DDL_GROUP_SIZE') is not None:
    dll_group_size = int(os.getenv('DDL_GROUP_SIZE'))

#These two functions are two different styles to group gradients
#both are experimental now, but we use the first one with a huge dll_group_size
#to fallback onto the single allreduce behavior.
#1. group until the collected size is larger than dll_group_size
def _grads_reduce_by_group(grads_and_vars, average=True):
 
    cur_grads_size = 0
    cur_grads_vars = []
    ret_grads_vars = []
    
    for (g, v) in grads_and_vars:
        tensor_size = g.get_shape().num_elements()
        
        #if the current size is too small or adding a new is OK
        if(cur_grads_size+tensor_size < dll_group_size or cur_grads_size < 200000):
            cur_grads_size  +=  tensor_size
            cur_grads_vars.append([g, v])
        #else allreduce
        else:
            if not ret_grads_vars:
                ret_grads_vars += _grads_reduce_one(cur_grads_vars, average)
            else:
                with tf.control_dependencies([ret_grads_vars[-1][0]]):        
                    ret_grads_vars += _grads_reduce_one(cur_grads_vars, average)
                
            #the current one needs to be remembered    
            cur_grads_size = tensor_size
            cur_grads_vars = [(g, v)]
            
    #for any residue        
    if cur_grads_size >0:
        if not ret_grads_vars:
            ret_grads_vars += _grads_reduce_one(cur_grads_vars, average)
        else:
            with tf.control_dependencies([ret_grads_vars[-1][0]]):        
                ret_grads_vars += _grads_reduce_one(cur_grads_vars, average)

    return ret_grads_vars
#2. process any tensor larger than dll_group_size individually 
def _grads_reduce_by_size(grads_and_vars, average=True):
 
    small_grads_size = 0
    small_grads_vars = []
    ret_grads_vars = []
    
    for (g, v) in grads_and_vars:
        tensor_size = g.get_shape().num_elements()
        
        #if the current one is too small, keep adding
        if(tensor_size < dll_group_size) :
            small_grads_vars.append([g, v])
            small_grads_size += tensor_size
        #else do allreduce
        else :
            if not ret_grads_vars:
                ret_grads_vars += _grads_reduce_one([(g,v)], average)
            else:
                with tf.control_dependencies([ret_grads_vars[-1][0]]):        
                    ret_grads_vars += _grads_reduce_one([(g,v)], average)
       
        if small_grads_size > dll_group_size:  
            if not ret_grads_vars:
                ret_grads_vars += _grads_reduce_one(small_grads_vars, average)
            else:
                with tf.control_dependencies([ret_grads_vars[-1][0]]):        
                    ret_grads_vars += _grads_reduce_one(small_grads_vars, average)
                    
            small_grads_size = 0
            small_grads_vars = []
                    
    #for all small ones        
    if small_grads_vars:
        with tf.control_dependencies([ret_grads_vars[-1][0]]):        
            ret_grads_vars += _grads_reduce_one(small_grads_vars, average)

    return ret_grads_vars
   
_grads_reduce = _grads_reduce_by_group
    
# Intercept apply_gradients function:
from tensorflow.python.training import optimizer

_tf_Opt_apply = optimizer.Optimizer.apply_gradients

# Note: cannot create control dependencies on existing nodes!
# Nodes once created are immutable.

# Our replacement:
def new_apply_gradients(self, grads_and_vars, global_step=None, name=None):
    # Sort grads_and_vars on var name:
    grads_and_vars.sort(key=lambda e: e[1].name)
    # Collect all gradients:
    grads = [gv[0] for gv in grads_and_vars]
    # Assume list is non-empty!
    (grad0,var0) = grads_and_vars.pop(0)
    # Add control deps from all gradients to this first AllReduce node:
    new_grads_and_vars = []
    with tf.control_dependencies(grads):
        grad = blc_MDR.all_reduce(grad0, op='avg')
        new_grads_and_vars.append((grad, var0))
    # Insert rest of all_reduce operators:
    for (gradi, vari) in grads_and_vars:
        # Add control dep from previous node to this one:
        with tf.control_dependencies([grad]):
            grad = blc_MDR.all_reduce(gradi, op='avg')
            new_grads_and_vars.append((grad, vari))
    return _tf_Opt_apply(self, new_grads_and_vars, global_step, name)

# Alternative that uses the new all_reduce_n node:
def new_apply_gradients_flat(self, grads_and_vars, global_step=None, name=None):
    grads_and_vars = _grads_reduce(grads_and_vars, average=True)
    return _tf_Opt_apply(self, grads_and_vars, global_step, name)

if mpi_init_status:
    if DDL_OPTIONS is None:
        # User must explicitly call API functions; no override.
        print('DDL: Expect explicit API calls.')
        init = _init
        bcast = _bcast
        allreduce_n = _allreduce_n
        grads_reduce = _grads_reduce
    else:
        # User indicates init should happen at import-time, i.e., now.
        _init(DDL_OPTIONS)

        # Redefine:
        tf.Variable.__init__ = newVar

        # Redefine:
        optimizer.Optimizer.apply_gradients = new_apply_gradients_flat
else:
    def raiseErr(*args):
        raise RuntimeError('Could not initialize DDL. Run this program using mpirun.')

    init = raiseErr
    bcast = raiseErr
    allreduce_n = raiseErr
    grads_reduce = raiseErr

```

## Conclusion

The above example demonstrates how you can basically take an example of from Tensorflow tutorial, add a few lines of code to activate DDL and easily train on multiple GPUs IBM WML, obtains a model that can be used for deployment and scoring.

## Related

- Try IBM WML [scoring](https://apsx.stage1.ng.bluemix.net/docs/content/analyze-data/ml_dlaas_model_deploy_score.html?context=analytics) capabilities
- Tensorflow [serving](https://www.tensorflow.org/serving/serving_basic)

