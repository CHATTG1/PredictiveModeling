---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# Create a Training Run

Training runs are the organizing principle for using deep learning functions in {{site.data.keyword.pm_full}}. A typical scenario might consist of dozens to hundreds of training runs. Each run is defined individually and consists of the following parts: the neural network defined by using one of the [supported deep learning frameworks](pm_service_supported_frameworks.html) and the configuration for how to run your training including the number of GPUs and location of the object storage that contains your data set.
{: shortdesc}

<!--<p align="center"><img src="images/experiment_to_training_runs_text.svg?lang=en" alt="relation of experiments to training runs"></p>-->

This document will explain how a training run can be set up and executed.  You may also want to refer to a practical example described in [Tutorial - Build a TensorFlow Model to recognize Handwritten Digits](ml_dlaas_working_with_sample_models.html)

## Creating a model definition .zip file

You will define your neural network and associated data handling using one of the [supported deep learning frameworks](ml_dlaas_supported_framework.html).

See also [Coding Guidelines for Deep Learning Programs](ml_dlaas_code_guidelines.html) for structuring your code to integrate well with the service. 

You will tthen package these files together by using the .zip format. For example, if the model was written in Torch then package your .lua files; if in Caffe then compress the .prototxt file; or if in Tensorflow/Keras/MXNet then compress your .py files.  Other compression formats, such as gzip or tar are not supported. Consult the documentation for the Deep Learning framework you want to use in order to prepare the model definition files.  

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

## Creating a training manifest file

The manifest is a YAML formatted file which contains different fields describing the model to be trained, including the deep learning framework to use, the cloud object storage configuration, the resource requirements, and several arguments (including hyperparameters) required for model execution during training and testing. Below we describe the different fields of the model training file for deep learning, continuing our tensorflow handwriting recognition example.

* `model_definition.name`: You can provide any value to name to help identify your training run after it is launched.  However, this does not have to be unique - the service will assign a unique model-id for each launched training run.
* `model_definition.description`: This is another field that you can use to describe the training run.
* `model_definition.author`: Provide the author name and email address under the keys *name* and *email*.
* `model_definition.framework`: This field provides framework specific information the name and version must match one of the [Supported deep learning frameworks](ml_dlaas_supported_framework.html).
    - `model_definition.framework.name`: Name of framework
    - `model_definition.framework.version`: Version of framework.  This should be a string, please enclose the value in double quotes, for example `version: "0.8"`
    - `model_definition.framework.runtimes`:  Optional.  Provide a list of names and versions of relevant libraries or tools.  For Deep Learning, currently the only relevant information is the version of python.  The version should be provided as a string.  If this information is not provided, `python3` is assumed. 
* `model_definition.execution`: This field provides information concerning the command to launch the training.
    - `model_definition.execution.command`: This field identifies the main program file along with any arguments that deep learning needs to execute.
    - `model_definition.compute_configuration.name`: This field specifies the resources that will be allocated for training and should be one of the following values - `small` (1 GPU, 6 CPU, 12Gb memory), `medium` (2 GPUs, 12 CPUs, 24Gb memory), `large` (4 GPUs, 24 CPUs, 48Gb memory)
* `training_data_reference`: This section specifies the object store and bucket where the data files used to train the model is loaded from. 
    - `name`: A descriptive name for this objectstore and bucket 
    - `connection`: The connection variables for the data store.
    - `type`: Type of data store, currently this can only be set to `s3`.  
    - `source.bucket`: The bucket where the training data resides.
* `training_results_reference`: This section specifies the object store where the resulting model files and logs will be stored after training completes.
    - `name`: A descriptive name for this objectstore and bucket 
    - `connection`: The connection variables for the data store. The list of connection variables supported is data store type dependent.
    - `type`: Type of data store, currently this can only be set to `s3`.
    - `target.bucket`: The bucket where the training results will be written.

For example, the following training manifest can be used to define a training run to train a tensorflow model:

```
model_definition:
  framework:
    name: tensorflow
    version: "1.2"
    runtimes:
    - name: python
      version: "3.5"
  name: tf-mnist
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 2000000
    compute_configuration:
      name: small
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
{: codeblock}

where `convolutional_network.py` is the tensorflow program (that is part of the model definition zip) to execute while the remainder are arguments to the program.  Program arguments `--trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz`, `--trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz`, `--testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz`, `--testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz` values refer to the data file paths in the object store container `mnist-training-data`. Program arguments `--trainingIters 20000` and `--learningRate 0.001` pass the values of hyperparameters.

Make note of the following requirements:

* When training manifest or model definition files refers to files uploaded to the Object Storage instance, the references should use relative paths of the form `${DATA_DIR}/path` as shown in the preceding example.
* Before training begins, all files within the training data bucket are downloaded to the training environment filesystem operated by the service and the filesystem folder is available to the training program in the environment variable `DATA_DIR`.  To avoid the overhead/delay of transferring unnecessary data files, use separate buckets to store data files used in different training objectives.


## Submit a training run

After you prepare the model definition .zip and training manifest file, submit the run by using the `bx ml train` command: `bx ml train <path-to-model-definition-zip> <path-to-training-manifest-yaml>`

```
bx ml train tf-model.zip tf-train.yaml
```
{: codeblock}

Sample Output:

When the command is submitted successfully, a unique model ID is returned. For example, the following output shows a `Model-ID` value of `training-DOl4q2LkR`:

```
Starting to train ...
OK
Model-ID is 'training-DOl4q2LkR'
```

## Monitor a training run

To list all training runs, whether completed or not, run the following command:

```
bx ml list training-runs
```
{: codeblock}

Sample Output:

```
Fetching the list of training runs ...
SI No   Name                       guid                 status    submitted-at
1       tf-mnist                   training-DOl4q2LkR   pending   2017-10-26T11:16:51Z

1 records found.
OK
List all training-runs successful
```
{: codeblock}

**Note**: The service only retains details of training runs for 7 days. After 7 days, the details are removed and do not appear in this list.

To check on the status of a particular training run use the cli command `bx ml show training-runs <model-id>`:

```
bx ml show training-runs training-DOl4q2LkR
```
{: codeblock}

Sample Output:

```
Fetching the training runs details with MODEL-ID 'training-DOl4q2LkR' ...
ModelId        training-DOl4q2LkR
url            /v3/models/training-DOl4q2LkR
Name           tf-mnist
State          running
Submitted_at   2017-10-26T11:10:37Z
OK
Show training-runs details successful
```
{: codeblock}

To continously monitor the logs from a training run use the cli command `bx ml monitor training-runs <model-id>`:

```
bx ml monitor training-runs training-DOl4q2LkR
```
{: codeblock}

Sample Output:

```
Starting to fetch status messages for model-id 'training-DOl4q2LkR'
...
Status: STORING

Training with training/test data at:
  DATA_DIR: /job/dl-data
  MODEL_DIR: /job/model-code
  TRAINING_JOB:

  TRAINING_COMMAND: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001 --trainingIters 200000

Storing trained model at:

  RESULT_DIR: /job/dl-models
...
```
{: codeblock}

You can add append `logs` or `metrics` to see only the log lines or only the metrics, foe example `bx ml monitor training-runs <model-id> logs` or `bx ml monitor training-runs <model-id> metrics` .

During a training run the training program should write the model files to a folder `$RESULT_DIR/model` - the `RESULT_DIR` environment variable will point to a filesystem folder where a model sub-folder will be created.

When a training run has completed successfully (or failed) all files written to `$RESULT_DIR` and the logs from the run should be written to the Cloud Object Storage bucket specified in the setting `training_results_reference` within the training manifest file, under a folder with the same name as the model id.

## Cancelling a training run

To cancel a training run that is in progress, use the command `bx ml cancel training-runs <model-id>`:

```
bx ml cancel training-runs training-aQk7F5Ukg
```
{: codeblock}

Sample Output:

```
Canceling training for MODEL-ID 'training-aQk7F5Ukg'...
OK
Cancel training successful
```
{: codeblock}

## Storing a trained model to the repository

Once a training run has completed successfully, the trained model can be permanently stored into the repository from from where it can be later deployed for scoring.  To do this use the command `bx ml store training-runs <model-id>`:

```
bx ml store training-runs training-DOl4q2LkR
```
{: codeblock}

Sample Output:

```
OK
Model store successful. Model-ID is '19db0ae7-3a9d-44e7-8e9d-fce3f4f8e0eb'.
```
{: codeblock}

## Delete a training run

To delete a training run (this will not remove the model and logs that were written to your Object Storage instance OR remove the model from the repository if stored there but rather remove all history of the training run from the service).

```
bx ml delete training-runs training-DOl4q2LkR
```
{: codeblock}

Sample Output:

```
Deleting the training run 'training-DOl4q2LkR' ...
OK
Delete training-runs successful
```
{: codeblock}



