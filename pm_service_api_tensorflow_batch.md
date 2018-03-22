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
{:table: .aria-labeledby="caption"}

# Deploying and scoring a Tensorflow model in Batch

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_tensorflow_batch.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


After a trained model has been identified for serving, you can deploy it by using the batch deployment. The batch deployment will read the input data file from input connection and save the prediction result to files in a output connection.
{: shortdesc}

## Prerequisites for deploying and serving a TensorFlow model in Batch

Before you deploy a TensorFlow model and score it, the following prerequisites must be implemented at the time you save the trained model:
   
* You need to train the model using the training service. Details on how to train a model can be found [here](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_working_with_new_models.html)
* You must use the `tf.saved_model.builder.SavedModelBuilder` API to save the TensorFlow model. You can use Experiments to train and save the model.
* The model must be tagged with the `tf.saved_model.tag_constants.SERVING` tag at the time of you save the model. If multiple signatures are tagged with `tf.saved_model.tag_constants.SERVING`, the candidate signature for online scoring will be randomly picked up.  
* The signature definition that is used for serving the model must be saved along with the model. You must use the `tf.core.protobuf.meta_graph_pb2.SignatureDef` API to define the signature definition.
* The model input tensor's shape must be defined to accept batch of input records.
* [Cloud Object Storage](https://console.bluemix.net/catalog/services/cloud-object-storage) instance details has to be provided during the batch deployment. This is used for storing the input data files for the prediction and storing the prediction result. 
* Model should be persisted in WML Repository.

## Generating the access token

Generate an access token by using the user and password available
from the Service Credentials tab of the {{site.data.keyword.pm_full}} service instance.

Request example:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Output example:

```
{"token":"**********"}
```
{: codeblock}

Use the following terminal command to assign your token value to
the environment variable token:

```
token="<token_value>"
```
{: codeblock}

## Working with published models

Use the following API call to get your instance details, which include the following items:

* published models `url` value
* deployments `url` value
* usage information

Request example:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Output example:

```
{
	"metadata": {
		"guid": "3ba4bc3e-b47f-4288-a5c4-48de2ec32852",
		"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
		"created_at": "2018-03-14T06:32:10.104Z",
		"modified_at": "2018-03-14T06:40:28.341Z"
	},
	"entity": {
		"source": "Bluemix",
		"published_models": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
		},
		"usage": {
			"expiration_date": "2018-04-01T00:00:00.000Z",
			"computation_time": {
				"current": 0
			},
			"model_count": {
				"limit": 1000,
				"current": 1
			},
			"prediction_count": {
				"current": 0
			},
			"gpu_count": {
				"limit": 8,
				"current": 0
			},
			"capacity_units": {
				"current": 0
			},
			"deployment_count": {
				"limit": 1000,
				"current": 0
			}
		},
		"plan_id": "0f2a3c2c-456b-40f3-9b19-726d2740b11c",
		"status": "Active",
		"organization_guid": "96370d2b-69f6-4919-8a4f-ed5e826f8d5f",
		"region": "us-south",
		"account": {
			"id": "b56398ea52f470c3173f4cf3bef5cc7e",
			"name": "IBM",
			"type": "TRIAL"
		},
		"owner": {
			"ibm_id": "**************",
			"email": "*********************",
			"user_id": "842c9fdb-189f-47e4-95e5-51ee9d1a",
			"country_code": "POL",
			"beta_user": true
		},
		"deployments": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/deployments"
		},
		"space_guid": "aa3b3ebd-4b5d-4f52-b8a0-308d090b41d6",
		"plan": "standard"
	}
}
```
{: codeblock}

By supplying the **published_models** `url` value, you can use the following API call to get the model details:

Request example:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

Output example:

```
{
	"limit": 1000,
	"resources": [{
		"metadata": {
			"guid": "935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"created_at": "2018-03-14T06:40:28.235Z",
			"modified_at": "2018-03-14T06:40:28.291Z"
		},
		"entity": {
			"runtime_environment": "python-3.5",
			"learning_configuration_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/learning_configuration",
			"author": {
				"name": "********",
				"email": "**************"
			},
			"name": "k_tf16_mnist_1128",
			"description": "Tensorflow model for predecting Hand-written digits",
			"learning_iterations_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/learning_iterations",
			"feedback_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/feedback",
			"latest_version": {
				"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/935836d4-286b-42cf-b08e-c3d0a5c5d671/versions/aa7b501b-f709-4afe-afa9-2bbf3c9faf1b",
				"guid": "aa7b501b-f709-4afe-afa9-2bbf3c9faf1b",
				"created_at": "2018-03-14T06:40:28.291Z"
			},
			"model_type": "tensorflow-1.5",
			"deployments": {
				"count": 0,
				"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/deployments"
			},
			"evaluation_metrics_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/evaluation_metrics"
		}
	}],
	"first": {
		"url": "https://deployment/v3/wml_instances/{instance_id}/published_models/?limit=1000"
	}
}
```
{: codeblock}

Note the **deployments** `url` value that you need to create the following batch deployment.

## Creating a batch deployment with Cloud Object Storage

To use a REST API call to create a batch deployment of your tensorflow model, provide the following details:

*  The access token created in the previous step
*  Cloud Object Storage details, which will be used for storing the input data files for the prediction and for storing the prediction result.
*  To create a deployment, use the **deployments** `url` value from previous section.
*  Before performing batch deployment and scoring on a tensorflow model, you must upload the input data files in Input Connection(Cloud Object Storage). 
   
### Add the input data files in the Input Connection for Tensorflow model
   
The input records for batch scoring must be specified as a JSON object in separate files. The JSON object in the file must contain the key-value pairs in the following table.

| Key | Values |
| --- | --- |
|Input tensor name| Scoring input record corresponding to the input Tensor specified by key of this JSON object|


{: caption="Table 2. Key and value entries required to specify input data for batch scoring" caption-side="top"}

* These input data files needs to be placed in the root of bucket in Input Connection(COS) and each input data file must have a single scoring record. 
* Multiple inputs records can provided by placing in separate input data files. 
* You must make sure only input data files are present in the input bucket of COS.
* Dimensions of input data specified in the file must match the dimension of the corresponding input Tensor.

1. Creating of input data file

Let us say you have a model defined with the `SignatureDef` as below:
   
   
   ```
   tf.saved_model.signature_def_utils.build_signature_def(
           inputs={
               "x_input_data":
                   classification_inputs
           },
           outputs={
               "y_predicton":
                   classification_outputs_classes
           },
           method_name=tf.saved_model.signature_constants.CLASSIFY_METHOD_NAME)
   ```
   {: codeblock}
   
   Refer to the following example for defining the `input_data_file` key for a model with the `SignatureDef` defined in the code sample above:
   
   Method 1:
   ```
   {
   "inputs": [[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 12.0, 99.0, 91.0, 142.0, 155.0, 246.0, 182.0, 155.0, 155.0, 155.0, 155.0, 131.0, 52.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 138.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 252.0, 210.0, 122.0, 33.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 220.0, 254.0, 254.0, 254.0, 235.0, 189.0, 189.0, 189.0, 189.0, 150.0, 189.0, 205.0, 254.0, 254.0, 254.0, 75.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 35.0, 74.0, 35.0, 35.0, 25.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 13.0, 224.0, 254.0, 254.0, 153.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 90.0, 254.0, 254.0, 247.0, 53.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 6.0, 152.0, 246.0, 254.0, 254.0, 49.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 66.0, 158.0, 254.0, 254.0, 249.0, 103.0, 8.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 54.0, 251.0, 254.0, 254.0, 254.0, 248.0, 74.0, 5.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 140.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 202.0, 125.0, 45.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 58.0, 181.0, 234.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 252.0, 140.0, 22.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 30.0, 50.0, 73.0, 155.0, 253.0, 254.0, 254.0, 254.0, 254.0, 191.0, 2.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 91.0, 200.0, 254.0, 254.0, 254.0, 254.0, 118.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4.0, 192.0, 254.0, 254.0, 254.0, 154.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 141.0, 254.0, 254.0, 254.0, 116.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 25.0, 126.0, 86.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 3.0, 188.0, 254.0, 254.0, 250.0, 61.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 24.0, 209.0, 254.0, 15.0, 0.0, 0.0, 0.0, 0.0, 0.0, 23.0, 137.0, 254.0, 254.0, 254.0, 209.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 168.0, 254.0, 254.0, 48.0, 9.0, 0.0, 0.0, 9.0, 127.0, 241.0, 254.0, 254.0, 255.0, 242.0, 63.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 101.0, 254.0, 254.0, 254.0, 205.0, 190.0, 190.0, 205.0, 254.0, 254.0, 254.0, 254.0, 242.0, 67.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 33.0, 166.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 250.0, 138.0, 55.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7.0, 88.0, 154.0, 116.0, 194.0, 194.0, 154.0, 154.0, 88.0, 49.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]]
   }
   ```
      
   Where the `x_input_data` value is the key for the input tensor as per the `SignatureDef` of the model i.e `inputs` and the values for the key pertain to the record for which prediction has to be performed.
   
   *Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments)
   for complete details about scoring input format specification.*

Request example:

```
curl -v -X POST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"MNIST Prediction",
      "type": "batch",
      "description": "Batch Deployment",
       "input":{
          "type": "cloudobjectstorage",  
          "source": {
          	 "bucket": "batchtensorflow-in"
          },
          "connection": {
            "access_key": "********",
            "secret_key": "********",
            "url": "**********************"
          }
       },
       "output":{
          "type": "cloudobjectstorage",  
          "target": {
          	 "bucket": "batch"
          },
       "connection": {
          "access_key": "***************",
          "secret_key": "***************",
          	 "url": "*****************************"
          }
       }
    }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

Output example:

```
{
	"metadata": {
		"guid": "1d72a336-7611-4e58-b81e-e36607e6346f",
		"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/deployments/1d72a336-7611-4e58-b81e-e36607e6346f",
		"created_at": "2018-03-14T06:53:31.622Z",
		"modified_at": "2018-03-14T06:53:31.868Z"
	},
	"entity": {
		"runtime_environment": "python-3.5",
		"name": "batchDeploymentTest",
		"description": "Testdescription",
		"published_model": {
			"author": {
				"name": "*********",
				"email": "*************"
			},
			"name": "k_tf16_mnist_1128",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"guid": "935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"description": "Tensorflow model for predecting Hand-written digits",
			"created_at": "2018-03-14T06:53:31.599Z"
		},
		"model_type": "tensorflow-1.5",
		"status": "INITIALIZING",
		"output": {
			"type": "cloudobjectstorage",
			"target": {
				"bucket": "batch"
			},
			"connection": {
				"access_key": "*****************",
				"secret_key": "***********************",
				"url": "https://s3-api.dal-us-geo.objectstorage.softlayer.net"
			}
		},
		"type": "batch",
		"deployed_version": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/935836d4-286b-42cf-b08e-c3d0a5c5d671/versions/aa7b501b-f709-4afe-afa9-2bbf3c9faf1b",
			"guid": "aa7b501b-f709-4afe-afa9-2bbf3c9faf1b"
		},
		"input": {
			"type": "cloudobjectstorage",
			"source": {
				"bucket": "batchtensorflow-in"
			},
			"connection": {
				"access_key": "*****************",
				"secret_key": "*********************",
				"url": "https://s3-api.dal-us-geo.objectstorage.softlayer.net"
			}
		}
	}
}
```
{: codeblock}

**Note**: You can also use the Dashboard to create a batch
deployment.

## Obtaining deployment details

You can check the status and parameters related to the deployment model by using the **metadata** `url` value. 
Request example:

```
curl -v -X GET -H "Content-Type:application/json" -H "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

```
{
	"metadata": {
		"guid": "1d72a336-7611-4e58-b81e-e36607e6346f",
		"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671/deployments/1d72a336-7611-4e58-b81e-e36607e6346f",
		"created_at": "2018-03-14T06:53:31.622Z",
		"modified_at": "2018-03-14T06:53:31.868Z"
	},
	"entity": {
		"runtime_environment": "python-3.5",
		"name": "batchDeploymentTest",
		"description": "Testdescription",
		"published_model": {
			"author": {
				"name": "********",
				"email": "****************"
			},
			"name": "k_tf16_mnist_1128",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"guid": "935836d4-286b-42cf-b08e-c3d0a5c5d671",
			"description": "Tensorflow model for predecting Hand-written digits",
			"created_at": "2018-03-14T06:53:31.599Z"
		},
		"status_details": {
			"status": "SUCCESS"
		},
		"model_type": "tensorflow-1.5",
		"status": "SUCCESS",
		"output": {
			"type": "cloudobjectstorage",
			"target": {
				"bucket": "batch"
			},
			"connection": {
				"access_key": "***********",
				"secret_key": "************************",
				"url": "************************************"
			}
		},
		"type": "batch",
		"deployed_version": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/935836d4-286b-42cf-b08e-c3d0a5c5d671/versions/aa7b501b-f709-4afe-afa9-2bbf3c9faf1b",
			"guid": "aa7b501b-f709-4afe-afa9-2bbf3c9faf1b"
		},
		"input": {
			"type": "cloudobjectstorage",
			"source": {
				"bucket": "batchtensorflow-in"
			},
			"connection": {
				"access_key": "***********",
				"secret_key": "*******************",
				"url": "******************************"
			}
		}
	}
}
```
{: codeblock}

The prediction result is saved to a json file in the IBM Cloud Object
Storage. Refer to the following sample row for an example of the output.

Input data file preview: (filename: `num_3.json`)
   
```
{
"inputs": [[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 12.0, 99.0, 91.0, 142.0, 155.0, 246.0, 182.0, 155.0, 155.0, 155.0, 155.0, 131.0, 52.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 138.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 252.0, 210.0, 122.0, 33.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 220.0, 254.0, 254.0, 254.0, 235.0, 189.0, 189.0, 189.0, 189.0, 150.0, 189.0, 205.0, 254.0, 254.0, 254.0, 75.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 35.0, 74.0, 35.0, 35.0, 25.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 13.0, 224.0, 254.0, 254.0, 153.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 90.0, 254.0, 254.0, 247.0, 53.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 6.0, 152.0, 246.0, 254.0, 254.0, 49.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 66.0, 158.0, 254.0, 254.0, 249.0, 103.0, 8.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 54.0, 251.0, 254.0, 254.0, 254.0, 248.0, 74.0, 5.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 140.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 202.0, 125.0, 45.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 58.0, 181.0, 234.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 252.0, 140.0, 22.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 30.0, 50.0, 73.0, 155.0, 253.0, 254.0, 254.0, 254.0, 254.0, 191.0, 2.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 91.0, 200.0, 254.0, 254.0, 254.0, 254.0, 118.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4.0, 192.0, 254.0, 254.0, 254.0, 154.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 141.0, 254.0, 254.0, 254.0, 116.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 25.0, 126.0, 86.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 3.0, 188.0, 254.0, 254.0, 250.0, 61.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 24.0, 209.0, 254.0, 15.0, 0.0, 0.0, 0.0, 0.0, 0.0, 23.0, 137.0, 254.0, 254.0, 254.0, 209.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 168.0, 254.0, 254.0, 48.0, 9.0, 0.0, 0.0, 9.0, 127.0, 241.0, 254.0, 254.0, 255.0, 242.0, 63.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 101.0, 254.0, 254.0, 254.0, 205.0, 190.0, 190.0, 205.0, 254.0, 254.0, 254.0, 254.0, 242.0, 67.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 33.0, 166.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 254.0, 250.0, 138.0, 55.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7.0, 88.0, 154.0, 116.0, 194.0, 194.0, 154.0, 154.0, 88.0, 49.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]]
}
```
{: codeblock}

Output file preview:

```
[{
	"filename": "num_3.json",
	"proba": [-2345609.0, -16723717.0, 12337402.0, 23062920.0, -10168320.0, 3675351.25, -9855991.0, -4942459.0, 605047.6875, -16657809.0],
	"classes": 3
}]
```
{: codeblock}

## Deleting a batch deployment

To delete the deployment use the following query:

Request example:

```
curl -v -X DELETE -H "Content-Type:application/json" -H
"Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}

### Interpreting the scoring output
   
The scoring output returns a JSON object where key-value pairs refers to the following.
   
| Key | Values |
| --- | --- |
|Name of the output tensor used for scoring| Prediction result obtained from the corresponding output tensor|
{: caption="Table 3. Key and value entries of the scoring output JSON" caption-side="top"}
   
**Note:** First key in object array of each prediction result is filename of input data file. This is required to map input data file with its corresponding prediction result. 
      
Refer to the example of the `SignatureDef` key specified in the preceding step. The scoring output for that `SignatureDef` will be formatted as in the following example:
   
```
[{
	"filename": "num_3.json",
	"proba": [-2345609.0, -16723717.0, 12337402.0, 23062920.0, -10168320.0, 3675351.25, -9855991.0, -4942459.0, 605047.6875, -16657809.0],
	"classes": 3
}]
```
   
   
## Learn more

Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).

For more information about IBM Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).