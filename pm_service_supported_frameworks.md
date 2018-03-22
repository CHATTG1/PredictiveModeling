---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Supported frameworks

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_supported_frameworks.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


The {{site.data.keyword.pm_full}} service supports the following frameworks for model development and validation as a part of model lifecycle management.
{: shortdesc}

## Spark MLlib

The {{site.data.keyword.pm_full}} service supports Spark MLlib for the Online, Batch (beta), Stream (beta) Deployment, and Scoring service. Only specific versions and runtime environments are supported.

### Capabilities

* Deployment types:
  * Online
  * Batch with Object Storage, DB2 Warehouse on Cloud, and DB2 on Cloud
  * Stream with Message Hub
*  Continuous Learning System

### Supported Versions

*  Spark 2.1

### Restrictions

  *  Only classification and regression models are supported.
  *  Models that contain references to custom transformers, user-defined functions, and classes are not supported.
  * Batch and Stream deployments use IBM Cloud Apache Spark service. Depending on the subscribed service plan, note the following limitations in number of parallel spark-submit jobs ([Using spark-submit documentation](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite: only one spark-submit job at a time
    * Reserved Enterprise: 150 spark-submit jobs at a time

## Scikit-learn

There is support for scikit-learn for the {{site.data.keyword.pm_full}} Online Deployment and Scoring service. Only specific versions and runtime environments are supported.

### Capabilities

* Deployment types:
  * Online

### Supported Versions

- Scikit-Learn 0.17 on Anaconda 4.2.x for Python 3.5 Runtime
- Scikit-Learn 0.19 on Anaconda 5.0.0 fot Python 3.5 Runtime

### Python Runtime

The Python runtime for models that needs to be deployed by using WML Online Deployment and Scoring service is Python 3.5.

In some cases, where models were created by using the Python 2.7 runtime, deployment might fail because of the incompatibilities between Python2.7 and Python3.5.

### Supported Python Packages

References for the list of Scikit-Learn model's dependent Python packages and its versions that can be used in WML Online Deployment and Scoring service
- For Scikit-Learn 0.17 on Anaconda 4.2.x for Python 3.5 Runtime: [Packages included in Anaconda 4.2.0 for Python version 3.5](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).
- For Scikit-Learn 0.19 on Anaconda 5.0.0 for Python 3.5 Runtime: [Packages included in Anaconda 5.0.0 for 64-bit Linux with Python 3.5](https://docs.anaconda.com/anaconda/packages/old-pkg-lists/5.0.0/py3.5_linux-64).

### Restrictions

There are some restrictions, which may include some or all of the following limitations:

* Classification and regression models are only supported.
* Models can contain references only to those packages (and package version) supported by Anaconda 4.2.x for Python 3.5 runtime.
* Models that require Pandas Dataframe as input type for "predict()" API are not supported.
* Models that contain references to custom transformers, user-defined functions and classes are not supported.

## XGBoost

There is support for XGBoost for the {{site.data.keyword.pm_full}} Online Deployment and Scoring service. Only specific versions and runtime environments are supported.

### Capabilities

* Deployment types:
  * Online

### Supported Versions

- XGBoost 0.6a2
- Anaconda 4.2.x for Python 3.5 Runtime
- Scikit-Learn 0.17

### Python Runtime

The Python runtime for models that needs to be deployed using WML Online Deployment and Scoring service is Python 3.5.

In some cases, where models were created by using the Python 2.7 runtime, deployment might fail because of the incompatibilities between Python2.7 and Python3.5.

### Supported Python Packages

The list of XGBoost model's dependent Python packages and its versions that can be used in WML Online Deployment and Scoring service can be found [here](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restrictions

There are some restrictions, which include some or all of the following limitations:

* Models can contain references only to those packages (and package version) supported by Anaconda 4.2.x for Python 3.5 runtime.
* The "schema" endpoint returned during deployment request is not implemented.
* Probability of prediction is not returned as a response of scoring request at this stage.
* Models containing references to custom transformers, user-defined functions and classes are not supported.

## SPSS Modeler

There is support for IBM® SPSS® Modeler streams for the {{site.data.keyword.pm_full}} Online, Batch Deployment and Scoring service. Only specific versions and runtime environments are supported.

### Capabilities

* Deployment types:
  * Online
  * Batch
* Retraining

### Supported Versions

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Restrictions

The following restrictions exist for IBM® SPSS® Modeler streams:

*  Streams that are created by using the IBM® Data Science Experience Flow Editor are not supported.
*  When a scoring branch is prepared for use in real-time scoring, input data coming in on the score request must replace the source node designed into the scoring branch and the resulting predictive analytic output must flow back into the response flow (effectively replacing the terminal node in the scoring branch design).
*  Because the scoring branch is prepared for real-time execution in {{site.data.keyword.Bluemix_short}}, it cannot require a connection to an external service. For example, an IBM Analytical Decision Management scoring branch design cannot contain references to rules or models stored in an IBM® SPSS® Collaboration and Deployment Services repository.
*  The execution of a scoring branch for real-time scoring in {{site.data.keyword.Bluemix_short}} cannot require an external service. For example, you cannot deploy and score against model algorithms that require an IBM® SPSS® Analytic Server and Apache Hadoop data store in real time.
*  {{site.data.keyword.pm_short}} supports Modeler embedded Python scripting. There are a few restrictions due to the method used for processing streams before they run in {{site.data.keyword.pm_short}}. Typically, if a user chooses to control the execution of the stream, they will reference the terminal node of the branch. For {{site.data.keyword.pm_short}}, when we process the stream, we identify the nodes from JSON that will be overridden, and then do the replacement before the stream runs. This causes the stream to fail in the script because the referenced input and export nodes no longer exist. The solution is use the ID of another node to uniquely identify the branch during execution. This ensures that the stream executes as defined in the embedded Python script.

The following additional exclusion apply:

- DB data/function
- Hadoop
- Analytic Server
- R/Python extensions
- Social Network Analysis
- Entity Analytics
- Text Analytics Japanese edition

## Predictive Markup Model Language (PMML)

There is support for PMML for the {{site.data.keyword.pm_full}} Online Deployment and Scoring services. Only specific versions and runtime environments are supported.

### Capabilities

* Supported models in PMML format:
  * R
  * SAS
  * SPSS
* Deployment types:
  * Online

### Supported Versions

* PMML version from 2.0 to 4.2

## Deep Learning Frameworks: TensorFlow, Keras and Caffe

There is support for TensorFlow, Keras and Caffe for the {{site.data.keyword.pm_full}} Training and Online Deployment and Scoring services. Only specific versions and runtime environments are supported.

### Capabilities

* Training Service
* Deployment types:
  * Online
  * Batch

### Supported Versions

* TensorFlow version 1.5
* Keras version 2.1.3 (with Tensorflow v1.5 as backend)
* Caffe version 1.0

### Python Runtime

The Python runtime for models that needs to be deployed by using WML Online Deployment and Scoring service is Python 3.6.

In some cases, where models were created by using the Python 2.7 runtime, deployment might fail because of the incompatibilities between Python2.7 and Python3.6.

### Prerequisites for deploying and serving
A trained deep learning model targeted for deployment and serving must satisfy certain prerequisites. Refer the links below for specific details.

- TensorFlow: [Deploying and scoring a TensorFlow model](ml_dlaas_model_tensorflow_deploy_score.html).
- Keras: [Deploying and scoring a Keras model](ml_dlaas_model_keras_deploy_score.html).
- Caffe: [Deploying and scoring a Caffe model](ml_dlaas_model_caffe_deploy_score.html).

### Restrictions

There are some restrictions, which may include some or all of the following limitations:

* Online Deployment and scoring service supports only JSON content as payload for online scoring functionality.
* TensorFlow: 
    * The models trained using `tf.estimator` framework is not supported.
    * User defined Tensor operations are not supported.
* Caffe:
    * User defined layers and blobs are not supported.


## Learn more

For more information about current support for IBM® SPSS® Analytic Server-trained predictive models, see the [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) section of [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
