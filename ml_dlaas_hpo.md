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

# Hyper parameters optimization

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_hpo.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


You can run your experiments with HPO to easily find the best quality model.
To learn how to use HPO with python client, [follow examples in this notebook](TBD).


## Introduction to HPO

Hyper Parameters Optimization (HPO) is a mechanism for automatically exploring a search space of potential hyper-parameters, building a series of models and comparing the models using metrics of interest.  To use HPO you must specify ranges of values to explore for each hyper-parameter.

### Optimization algorithms

Currently two HPO methods are supported.

`random` implements a simple algorithm which will randomly assign hyper-parameter values from the ranges specified for an experiment.  

`rbfopt` uses a technique called RBFOpt to explore the search space.  Determining the parameters of a Neural-Networks effectively is a challenging problem due to the extremely large configuration space (for instance: how many nodes per layer, activation functions, learning rates, drop-out rates, filter sizes, etc.) and the computational cost of evaluating a proposed configuration (e.g., evaluating a single configuration can take hours to days).
To address this challenging problem the `rbfopt` algorithm uses a model-based global optimization algorithm that does not require derivatives.
Similarly to Bayesian Optimization which fits a Gaussian model to the unknown objective function our approach fits a radial basis function model.
The underlying optimization software is open source and available here: [RbfOpt: A blackbox optimization library in Python](https://github.com/coin-or/rbfopt) 

An example application of RbfOpt in the context of Neural Networks is available here;
[A. Fokoue, G. Diaz, G. Nannicini, H. Samulowitz. An effective algorithm for hyperparameter optimization of neural networks. IBM Journal of Research and Development, 61(4-5), 2017](http://ieeexplore.ieee.org/document/8030298/)


## Requirements

### How to write the code with HPO

To use HPO you can broadly re-use the same code for your non-HPO experiments and training runs.  However there are two important considerations.  

##### Obtaining hyper-parameter values specified by the HPO algorithm

If your code is running as part of an HPO experiment it will need to obtain values for hyper-parameters assigned by the HPO algorithm.  The hyper-parameters will be supplied in a file called `config.json` as a JSON formatted dictionary, located in the current folder and can be read using the following example snippet (which expects hyper-parameters to be defined for `initial_learning_rate` and `total_iterations`:

```
hyper_params = json.loads(open("config.json").read())
learning_rate = float(hyper_params["initial_learning_rate"])
training_iters = int(hyper_params["total_iterations"])
```
{: codeblock}

##### Logging metrics for collection by the HPO algorithm

For code using `tensorflow`, `keras` and `pytorch` APIs you will need to use the following logging approach based on the python code in [emetrics.py](https://github.com/pmservice/wml-sample-models/raw/master/deep-learning/metrics/emetrics.py).  

You can download this python file and add it to your training definition zip, then import it and use it in your program as shown in the snippet below.

```
from emetrics import EMetrics
import os

with EMetrics.open(os.environ["SUBID"]) as em:
	for epoch in range(0,total_epochs):
    	# perform training for epoch
    	# compute training metrics and assign to values train_accuracy, train_loss etc
    	em.record(EMetrics.TRAIN_GROUP,epoch,{'accuracy': train_accuracy, 'loss': train_loss})
    	
    	# compute test(validation) metrics and assign to values test_accuracy, test_loss etc
    	# NOTE for these use the group EMetrics.TEST_GROUP so that the service will recognize these metrics as computed on test/validation data
    	em.record(EMetrics.TEST_GROUP,epoch,{'accuracy': test_accuracy, 'loss': test_loss})
```
{: codeblock}

Note - the metrics collection object returned from `EMetrics.open(...)` must be closed when training is complete - if necessary call its `close()` method explictly.  When the object is closed, information will be passed back to the HPO algorithm.  In the example code above, the object is closed implicitly when the `with` statement completes.

### Experiments with HPO

Define your experiments as described in [working with experiments](ml_dlaas_working_with_experiments.html)

To use HPO you will need to create an experiment with a `training_reference` which includes an extra `hyper_parameters_optimization` section.  This section provides the configuration for the HPO algorithm.

There are two sub-sections within `hyper_parameters_optimization` to consider:

`hyper_parameters_optimization.method` is the first sub-section which configures the HPO algorithm... 

`hyper_parameters_optimization.method.name` specifies the HPO algorithm to use, one of `random` and `rbfopt`
`hyper_parameters_optimization.method.parameters` provides a list of parameters to the algorithm.  You will need to provide the following parameters:

```    
hyper_parameters_optimization:
    method:
      name: random
      parameters:
      - name: objective
        string_value: accuracy
      - name: maximize_or_minimize
        string_value: maximize
      - name: num_optimizer_steps
        int_value: 4  
```
{: codeblock}

where `objective` specifies a test metric which your program records and which HPO will use to compare models, `maximize_or_minimize` specifies whether HPO should attempt to `minimise` or `maximise` the metric.  `num_optimizer_steps` sets an upper bound on the number of models which HPO will train. 

`hyper_parameters_optimization.hyper_parameters` is the second sub-section which configures a list of named hyper-parameters.  For each hyper-parameter specify the ranges of values which HPO may select for that hyper-parameter when training models.  Your code must be ready to read values for each of these named hyper-parameters from a file `config.json` as discussed above.

### Defining ranges of values for hyper parameters

Use `double_range` or `int_range` to define ranges of values for a hyper-parameter with an optional `step`.  The following entry in the `hyper_parameters` list specifies that the double values 0.005, 0.006, 0.007, 0.008, 0.009 and 0.01 may be used in the hyper-parameter called `learning_rate`.  

```
      - name: learning_rate
        double_range:
          min_value: 0.005
          max_value: 0.01
          step: 0.001
```
{: codeblock}

A power optional attribute is also provided - in the following example the fc hyper-parameter values can vary from 2^5 (32) to 2^10 (1024):

```
    - name: fc
      int_range:
        min_value: 5
        max_value: 10
        power: 2
```
{: codeblock}


Use `double_values`, `int_values` or `string_values` to define a set of values that can be used:

```
      - name: learning_rate
        double_values: [ 0.001, 0.002, 0.005 ]
```
{: codeblock}



The full experiment manifest example is:

```
settings:
  name: Experiment1
  description: This is a sample experiment
  author:
    name: Bob Smith
    email: bobsmith@example.com
training_references:
- name: model1
  training_definition_url: https://ibm-watson-ml.mybluemix.net/v3/ml_assets/training_definitions/5b21ea03-0936-4829-a96c-946638e84d6d
  command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
    --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
    --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001 --trainingIters 4
  compute_configuration:
      name: k80
  hyper_parameters_optimization:
    method:
      name: random
      parameters:
      - name: objective
        string_value: accuracy
      - name: maximize_or_minimize
        string_value: maximize
      - name: num_optimizer_steps
        int_value: 4
    hyper_parameters:
    - name: learning_rate
      double_range:
        min_value: 0.005
        max_value: 0.01
        step": 0.001
    - name: conv_filter_size1
      int_range:
        min_value: 5
        max_value: 6
    - name: conv_filter_size2
      int_range:
        min_value: 5
        max_value: 6
    - name: fc
      int_range:
        min_value: 5
        max_value: 10
        power: 2
training_results_reference:
  name: training-results-reference_name
  connection:
    endpoint_url: <URL>
    access_key_id: <ACCESS KEY>
    secret_access_key: <SECRET ACCESS KEY>
  target:
    bucket: training-data
  type: s3
```
{: codeblock}


## Next Steps

* Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).
* Go in depth with the following Developer Works article: [Introducing deep learning and long-short term memory networks](https://www.ibm.com/developerworks/analytics/library/iot-deep-learning-anomaly-detection-1/index.html).
