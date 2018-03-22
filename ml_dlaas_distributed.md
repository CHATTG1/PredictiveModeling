---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Distributed Deep Learning <span class='tag--beta'>Beta</span>

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_distributed.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________



Deep learning models training can be significantly accelerated with distributed computing on GPUs. {{site.data.keyword.pm_full}} provides support for speeding up training using data parallelism three types of distributed learning:
* [TensorFlow with parameter server and workers](https://www.tensorflow.org/deploy/distributed)
* [TensorFlow with IBM DDL](https://github.ibm.com/Bluemix-Docs/PredictiveModeling/blob/staging/ml_dlaas_ibm_ddl.md)
* [TensorFlow with Horovod](https://github.com/uber/horovod)

You can use any of supported mechanisms to accelerate training cycle by distributing computation on an elastic GPU cluster.
To learn how to use distributed deep learning with python client, [follow examples in this notebook](https://dataplatform.ibm.com/analytics/notebooks/05fed4ec-c1c2-48e7-b8af-9831d2497421/view?access_token=86aee64efbffaa43cae59b6274dcbe35716a2d4ab9fee46b6b8fac3080fb5e36).

## Restrictions:

* Distributed deep learning is in **beta** phase now.
* **Lite plan** of {{site.data.keyword.pm_full}} does not support distributed deep learning.
* TensorFlow is only supported at this stage.
* Online deployment (scoring) is only supported for native TensorFlow.

## TensorFlow

Refer to the documentation [here](https://www.tensorflow.org/deploy/distributed) to learn more about distributed tensorflow. Out of box, tensorflow supports distributed using a notion of parameter server and worker approach, think of this as more of a master worker approach where workers are responsible for carrying out the work and master is responsible for sharing the learnings(weights calculated) amongst the workers.

Our current approach is that all the nodes start as equal and are  provided with an id, that you can refer using environment variable `$LEARNER_ID` and have a host name that has a prefix `$LEARNER_NAME_PREFIX` and full host name as `$LEARNER_NAME_PREFIX-$LEARNER_ID`. Similarly you can find the total number of nodes using `$NUM_LEARNERS`. It is users responsibility to write code which can then designate some of these nodes as parameter servers (at least 1 needed) and workers (at least 1 needed). We are providing a sample launcher script that shows one approach of extract this infomation out.

When a distributed learning is started these nodes come up per distributed tensorflow setup grpc server is started on these nodes on port `2222`  the command that is provided by the user as the part of the manifest is execute. Refer to the example to see how the launcher script can be used to control how to provide appropriate task id and job name to these nodes and different node can act as a worker or parameter server depending on its learner id.

### Requirements
#### API

To run TensorFlow distributed deep learning:
* `FRAMEWORK_NAME` should be set to `tensorflow`
* `FRAMEWORK_VERSION` should be `1.5`
* `COMPUTE_CONFIGURATION` name should be any of `P100` and number of `nodes` higher than one.

#### User code
TODO - is there anything user needs to add to his code to run on DLaaS ?

##### Launcher script
TODO

## IBM Distributed Deep Learning Library for TensorFlow
The IBM Distributed Deep Learning (DDL) library for TensorFlow automatically distributes the computatin across multiple nodes and multiple GPUs.
Users are required to start with a single node GPU training code. Users modify their code with a few statement to active DDL based distribution of their code
to leverage the multi-GPU training. 

### Requirements
 User program must be written for single GPU training.

#### API

To run ddl distributed deep learning:
* `FRAMEWORK_NAME` should be set to `tensorflow-ddl`
* `FRAMEWORK_VERSION` should be `1.5`
* `COMPUTE_CONFIGURATION` name should be any of `P100` and number of `nodes` higher than one.

#### User code

See [here](ml_dlaas_ibm_ddl.md) for more details on DDL and  step by step process to modify users code to enable DDL based training and scoring in WML.

## Horovod
Similar to IBM DDL, [horovod](https://github.com/uber/horovod) is also based on a similar approach of no parameter server and workers talking amongst each other and learning from each other. Horovod is installed and configured for use if you decide to use horovod. Using the documentation mentioned [here](https://github.com/uber/horovod#usage) you can run an existing horovod example. As a user there is no need for you to do any installation or the need to run the underlying mpi commands to orchestrate the process. You can simply run your command and we take care of setting up the underlying infrastructure and orchestration. Refer to a sample example [here] on how you can run the examples mentioned [here] (https://github.com/uber/horovod/tree/master/examples)

### Introduction
TODO
#### What is DDL
TODO
#### How to use DDL
TODO

### Requirements
#### API

To run horovod distributed deep learning:
* `FRAMEWORK_NAME` should be set to `tensorflow-horovod`
* `FRAMEWORK_VERSION` should be `1.5`
* `COMPUTE_CONFIGURATION` name should be any of `P100` and number of `nodes` higher than one.

#### User code
TODO - is there anything user needs to add to his code to run on DLaaS ?


## Next Steps

* Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).
* Go in depth with the following Developer Works article: [Introducing deep learning and long-short term memory networks](https://www.ibm.com/developerworks/analytics/library/iot-deep-learning-anomaly-detection-1/index.html).


## References
* [Horovod: fast and easy distributed deep learning in {TensorFlow}](https://github.com/uber/horovod)
* [Bringing HPC Techniques to Deep Learning](http://research.baidu.com/bringing-hpc-techniques-deep-learning/)
* [Accurate, Large Minibatch SGD:Training ImageNet in 1 Hour](https://research.fb.com/wp-content/uploads/2017/06/imagenet1kin1h5.pdf)
* [IBM Research achieves record deep learning performance with new software technology](https://www.ibm.com/blogs/research/2017/08/distributed-deep-learning/)
