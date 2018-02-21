---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# Create a Training definition

Training definitions are the organizing principle for using deep learning functions in {{site.data.keyword.pm_full}}. A typical scenario might consist of dozens to hundreds of training definitions . Each training definition is defined individually and consists of the following parts: the neural network defined by using one of the [supported deep learning frameworks](pm_service_supported_frameworks.html) and location of the object storage that contains your data set.
{: shortdesc}

<!--<p align="center"><img src="images/experiment_to_training_runs_text.svg?lang=en" alt="relation of experiments to training runs"></p>-->

This document will explain how a training definitions can be set up and stored.  You may also want to refer to a practical example described in [Tutorial - Build a TensorFlow Model to recognize Handwritten Digits](ml_dlaas_working_with_sample_models.html)


Note: If you are interested in python client please refer to this [section]((ml_dlaas_environment_pyclient.html).

## Creating a model definition .zip file

After you define the neural network and associated data handling by using one of the [supported deep learning frameworks](ml_dlaas_supported_framework.html), then package these files together by using the .zip format. For example, if the model was written in Torch then package your .lua files; if in Caffe then compress the .prototxt file; or if in TensorFlow/Keras/MXNet then compress your .py files.  Other compression formats, such as gzip or tar are not supported. Consult the documentation for the Deep Learning framework you want to use in order to prepare the model definition files.  

<!-- Supposedly this isn't true anymore >> NOTE: All model definition files must be in the first level of the zip file so ensure there are no nested directories in the zip file. -->

For example, a zip file `tf-model.zip` that contains the model definition for tensorflow might contain the following output:

```
unzip -l tf-model.zip
```
{: codeblock}

Sample Output:

```
Archive:  tf-model.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     7094  09-21-2017 11:38   convolutional_network.py
     5486  09-19-2017 13:49   input_data.py
---------                     -------
    12580                     2 files
```
{: codeblock}

## Upload training data

Your training data must be [uploaded to a compatible Object Storage service instance](ml_dlaas_object_store.html). The credentials from that Object Storage instance will be used below in your manifest file below. The object store is also used to store the trained model and log files at the end of your training run.

## Creating a training definition manifest file

The manifest is a YAML formatted file which contains different fields describing the model to be trained, including the deep learning framework to use, the cloud object storage configuration and several arguments required for model execution during training and testing. Below we describe the different fields of the training definition file for deep learning, continuing our tensorflow handwriting recognition example.

* `name`: You can provide any value to name to help identify your training run after it is launched.  However, this does not have to be unique - the service will assign a unique model-id for each training definition.
* `description`: This is another field that you can use to describe the training definition.
* `author`: Provide the author name and email address under the keys *name* and *email*.
* `framework`: This field provides framework specific information the name and version must match one of the [Supported deep learning frameworks](ml_dlaas_supported_framework.html).
    - `framework.name`: Name of framework
    - `framework.version`: Version of framework.  This should be a string, please enclose the value in double quotes, for example `version: "0.8"`
    - `framework.runtimes`:  Optional.  Provide a list of names and versions of relevant libraries or tools.  For Deep Learning, currently the only relevant information is the version of python.  The version should be provided as a string.  If this information is not provided, `python3` is assumed.
* `training_data_reference`: This section specifies the object store and bucket where the data files used to train the model is loaded from.
    - `name`: A descriptive name for this objectstore and bucket
    - `connection`: The connection variables for the data store.
    - `type`: Type of data store, currently this can only be set to `s3`.  
    - `source.bucket`: The bucket where the training data resides.

For example, the following training definition manifest can be used to create a training definition:

```
name: training-definition-1
description:  Simple MNIST model implemented in TF
framework:
  name: tensorflow
  version: '1.2'
  runtimes:
  - name: python
    version: '3.5'
training_data_reference:
- name: MNIST image data files
  connection:
    endpoint_url: <auth-url>
    access_key_id: <username>
    secret_access_key: <password>
  source:
    bucket: mnist-training-models
    type: s3
author:
  name: WML User
  email: wmluser@ibm.com
```
{: codeblock}


# Generate a sample training definition manifest file.

Sample manifest file template can be generated by using the `bx ml generate-manifest` command: `bx ml generate-manifest training-definitions`
```
bx ml generate-manifest training-definitions
```
{: codeblock}

Sample Output:

```
OK
A sample manifest file is generated under training-definitions.yml
```

## Store a training definition

After you prepare the model definition .zip and training manifest file, store the training definition by using the `bx ml store training-definitions` command: `bx ml store training-definitions <path-to-model-definition-zip> <path-to-training-definitions-manifest-yaml>`

```
bx ml store training-definitions tf-model.zip tf-train-def.yaml
```
{: codeblock}

Sample Output:

When the command is submitted successfully, a unique training-definition ID is returned. For example, the following output shows a `Training-Definition-ID` value of `e73b7b9d-d44b-481a-80a9-8e8b13eb63a3`:

```
Creating training-definition ...
OK
training-definition ID is 'e73b7b9d-d44b-481a-80a9-8e8b13eb63a3' and version ID is '354950bb-a5e0-4e5b-8c39-f5439078a705'
```


## List a training definition

To list all training definitions,  run the following command:

```
bx ml list training-definitions
```
{: codeblock}

Sample Output:

```
Fetching the list of training-definitions ...
SI No   Name                          guid                                   framework    created-at
1       caffe_training_definitionc1   0787fae0-3221-48c2-9270-db6f40e57d3b   caffe        2018-01-30T16:16:33.295Z
2       tf-training-definition2       512bb4a4-552d-4984-b404-2987b2600e3b   tensorflow   2018-01-30T16:16:54.516Z

2 records found.
OK
List all training-definitions successful
```
{: codeblock}


To check the details of a particular training-definition run use the cli command `bx ml show training-definitions <training-defintions--id>`:

```
bx ml show training-definitions 512bb4a4-552d-4984-b404-2987b2600e3b

```
{: codeblock}

Sample Output:

```
Fetching the training-definition details with ID '512bb4a4-552d-4984-b404-2987b2600e3b' ...
TrainingDefinitiontId   512bb4a4-552d-4984-b404-2987b2600e3b
name                    tf-training-definition2
framework               tensorflow
url                     https://ibm-watson-ml.mybluemix.net/v3/ml_assets/training_definitions/512bb4a4-552d-4984-b404-2987b2600e3b
created_at              2018-01-30T16:16:54.516Z
OK

```
{: codeblock}


## Delete a training definition

To delete a training definition.

```
bx ml delete training-definitions fe119693-b42a-4516-9482-ee74c0c5ad8a
```
{: codeblock}

Sample Output:

```
Deleting the training-definition 'fe119693-b42a-4516-9482-ee74c0c5ad8a' ...
OK
Delete training-definition successful
```
{: codeblock}
