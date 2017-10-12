---

copyright:
  years: 2016, 2017
lastupdated: "2017-10-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Working with new deep learning models

{: shortdesc}


The file(s) comprising the model definition should be zipped and the zip file uploaded to a new **experiment**.  At present, deep learning only supports zip format for model files, other compression formats like gzip, bzip, tar etc., are not supported. Note that all model definition files has to be in the first level of the zip file and there are no nested directories in the zip file.You will need to use the URL for the experiment's version in the model training definition file `model_definition.definition_location`.

**TODO reference needed to section on creating repository / experiments or describe that more here **



* `model_definition.definition_location`: This is the URL of the experiment
* `model_definition.framework`: This field provides framework specific information. 
* `model_definition.execution`: This field provides information concerning the command to launch the training.
    - `model_definition.execution.resourceType`: This field specifies the resources that will be allocated for training and should be one of the following values - small (1 GPU), medium (2 GPUs), large (3 GPUs)
* `training_data`: This section specifies the object store where the data files used to train the model are loaded from. 

For example, the following model training definition file can be used to define a job to train a tensorflow model: 

```
{
  "model_definition": {
    "framework": {
      "name": "tensorflow",
      "version": "1.2-py3"
    },
    "name": "tf-mnist",
    "author": "Ann Other",
    "description": "Simple MNIST model implemented in TF",
    "definition_location": "https://ibm-watson-ml.stage1.mybluemix.net/v3/ml_assets/experiments/c7e148f1-a83a-4bf8-a69c-fa815480cd15/versions/8cd6a2fc-f64b-4af5-bad8-6d623001aee6",
    "execution": {
      "command": "python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001 --trainingIters 20000",
      "resourceType": "small"
    }
  },
  "training_data": [
    {
      "connection": {
        "auth_url": "<url>",
        "userId": "<username>",
        "password": "<password>"
      },
      "source": {
        "container": "tf_training_data",
        "type": "softlayer_objectstore"
      }
    }
  ]
}

```
{: codeblock]

<<<<<<< HEAD
where `cnn.lua` is the model code to execute while the remainder are arguments to the model. The `trainFile`, `validFile`, `testFile`, `embeddingFile` values refer to the dataset path in the `learner`, `numLabels`, `epoch`, `batchSize`, `learningRate`, and `saveMode` training parameters and hyperparameters, and `outputprefix` refers to the output directory path in the `learner` data path.



[A quick deep learning tutorial](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)
=======
where `convolutional_network.py` is the tensorflow program to execute while the remainder are arguments to the program.  Program arguments `--trainImagesFile train-images-idx3-ubyte.gz`, `--trainLabelsFile train-labels-idx1-ubyte.gz`, `--testImagesFile t10k-images-idx3-ubyte.gz`, `--testLabelsFile t10k-labels-idx1-ubyte.gz` values refer to the dataset paths in the objectstore container `tf_training_data`. Program arguments `--trainingIters 20000` and `--learningRate 0.001` pass the values of hyperparameters.


Once you have created a training definition file, you can start a training job by POSTing to the WML /v3/models endpoint.  For example, if the training definition is in file training_definition.json and a WML authorisation token is stored in environment variable `$TOKEN`, the following curl command could be used:

```
curl -X POST -sS -H "Content-Type: application/json"  
   -H "Authorization: BEARER ${TOKEN}" 
   -H "X-WML-User-Client: WML-User"  
   --data-binary "@training_definition.json" 
   "https://ibm-watson-ml.stage1.mybluemix.net/v3/models"
```

>>>>>>> origin/staging