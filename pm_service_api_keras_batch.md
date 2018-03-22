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

# Deploying and scoring a Keras model in Batch

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_keras_batch.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


After a trained model has been identified for serving, you can deploy it by using the batch deployment. The batch deployment will read the input data file from input connection and save the prediction result to files in a output connection.
{: shortdesc}

## Prerequisites for deploying and serving a Keras model in Batch

Before you deploy a Keras model and score it, the following prerequisites must be implemented at the time you save the trained model:
   
* You need to train the model using the training service. Details on how to train a model can be found [here](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_working_with_new_models.html)
* Keras's save() API should be used for saving the trained Keras model.  You can use Experiments to train and save the model.
* [Cloud Object Storage](https://console.bluemix.net/catalog/services/cloud-object-storage) instance details has to be provided during the batch deployment. This is used for storing the input data files for the prediction and for storing the prediction result.
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
		"modified_at": "2018-03-14T07:06:06.056Z"
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
				"current": 0
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
			"ibm_id": "*******",
			"email": "*****************",
			"user_id": "842c9fdb-189f-47e4-95e5-51ee9d1a14a1",
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
			"guid": "1853b42c-a209-4861-b72d-d4715a786344",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344",
			"created_at": "2018-03-14T07:10:20.113Z",
			"modified_at": "2018-03-14T07:10:20.160Z"
		},
		"entity": {
			"runtime_environment": "None Provided",
			"learning_configuration_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/learning_configuration",
			"author": {
				"name": "*********",
				"email": "*************"
			},
			"name": "k2_keras_seq_mnist_local_tar",
			"description": "Keras-Tensorflow mnist model",
			"learning_iterations_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/learning_iterations",
			"feedback_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/feedback",
			"latest_version": {
				"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/1853b42c-a209-4861-b72d-d4715a786344/versions/18dd9b56-79be-499f-a6a3-e382f712a9c0",
				"guid": "18dd9b56-79be-499f-a6a3-e382f712a9c0",
				"created_at": "2018-03-14T07:10:20.160Z"
			},
			"model_type": "tensorflow-1.5",
			"deployments": {
				"count": 0,
				"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/deployments"
			},
			"evaluation_metrics_url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/evaluation_metrics"
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

To use a REST API call to create a batch deployment of your keras model, provide the following details:

*  The access token created in the previous step
*  Cloud Object Storage details, which will be used for storing the input data files for the prediction and for storing the prediction result.
*  To create a deployment, use the **deployments** `url` value from previous section.
*  Before performing batch deployment and scoring on a keras model, you must upload the input data files in Input Connection(Cloud Object Storage).  
   
### Add the input data files in the Input Connection for Keras model
   
The input records for batch scoring must be specified as a JSON object in separate files. The JSON object in the file must contain the key-value pairs in the following table.


| Key | Values |
| --- | --- |
|`values`| Input scoring data as required by the Keras model as a JSON object|


{: caption="Table 2. Key and value entries required to specify input data for batch scoring" caption-side="top"}

* These input data files needs to be placed in the root of bucket in Input Connection(COS) and each input data file must have a single scoring record. 
* Multiple inputs records can provided by placing in separate input data files. 
* You must make sure only input data files are present in the input bucket of COS.


1. Creating of input data file
   
   Refer to the following example for defining the `input_data_file` key for a model:
   
   Method 1:
      ```
      {
        "values":[[[[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.2705882489681244], [0.5960784554481506], [0.929411768913269], [0.9960784316062927], [0.9960784316062927], [1.0], [0.9960784316062927], [0.9882352948188782], [0.20392157137393951], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.34117648005485535], [0.6431372761726379], [0.929411768913269], [0.9921568632125854], [0.9960784316062927], [0.8549019694328308], [0.5411764979362488], [0.32549020648002625], [0.15294118225574493], [0.6039215922355652], [0.9960784316062927], [0.529411792755127], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5411764979362488], [0.9647058844566345], [0.9921568632125854], [0.9960784316062927], [0.8470588326454163], [0.6549019813537598], [0.21176470816135406], [0.019607843831181526], [0.0], [0.0], [0.0], [0.3921568691730499], [0.7490196228027344], [0.0117647061124444], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.9137254953384399], [0.9960784316062927], [0.6627451181411743], [0.2078431397676468], [0.0235294122248888], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.13725490868091583], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.6823529601097107], [0.9960784316062927], [0.3686274588108063], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.14901961386203766], [0.9607843160629272], [0.8666666746139526], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5568627715110779], [0.9960784316062927], [0.5843137502670288], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5921568870544434], [0.9960784316062927], [0.43921568989753723], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.886274516582489], [0.9490196108818054], [0.125490203499794], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0470588244497776], [0.9411764740943909], [0.7960784435272217], [0.1725490242242813], [0.1725490242242813], [0.1725490242242813], [0.1725490242242813], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.27450981736183167], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.8039215803146362], [0.3333333432674408], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.14509804546833038], [0.7215686440467834], [0.6627451181411743], [0.5215686559677124], [0.5215686559677124], [0.6352941393852234], [0.8313725590705872], [0.9960784316062927], [0.9960784316062927], [0.6509804129600525], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.03529411926865578], [0.20000000298023224], [0.6941176652908325], [0.9960784316062927], [0.4901960790157318], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.10588235408067703], [0.8196078538894653], [0.9960784316062927], [0.40784314274787903], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.24705882370471954], [0.8196078538894653], [0.9960784316062927], [0.7607843279838562], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0235294122248888], [0.0], [0.03921568766236305], [0.5372549295425415], [0.95686274766922], [0.9960784316062927], [0.7764706015586853], [0.20392157137393951], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.05098039284348488], [0.34117648005485535], [0.47843137383461], [0.5764706134796143], [0.8745098114013672], [0.9960784316062927], [0.9686274528503418], [0.49803921580314636], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0117647061124444], [0.4627451002597809], [0.9803921580314636], [0.8235294222831726], [0.9725490212440491], [0.9960784316062927], [0.9882352948188782], [0.7803921699523926], [0.1921568661928177], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5411764979362488], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9803921580314636], [0.7882353067398071], [0.2823529541492462], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.30980393290519714], [0.6549019813537598], [0.772549033164978], [0.34117648005485535], [0.027450980618596077], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]]]]
      }
      ```
   
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
          	 "bucket": "batchkeras-in"
          },
          "connection": {
            "access_key": "*****",
            "secret_key": "***********",
            "url": "************************"
          }
       },
       "output":{
          "type": "cloudobjectstorage",  
          "target": {
          	 "bucket": "batchkeras-out"
          },
       "connection": {
          "access_key": "************",
          "secret_key": "****************",
          	 "url": "**********************"
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
		"guid": "22c965f3-2474-45da-8726-623bb724ecfd",
		"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/deployments/22c965f3-2474-45da-8726-623bb724ecfd",
		"created_at": "2018-03-14T09:42:16.662Z",
		"modified_at": "2018-03-14T09:42:16.975Z"
	},
	"entity": {
		"runtime_environment": "None Provided",
		"name": "batchDeploymentTest",
		"description": "Testdescription",
		"published_model": {
			"author": {
				"name": "*****",
				"email": "**************"
			},
			"name": "k2_keras_seq_mnist_local_tar",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/3ba4bc3e-b47f-4288-a5c4-48de2ec32852/published_models/1853b42c-a209-4861-b72d-d4715a786344",
			"guid": "1853b42c-a209-4861-b72d-d4715a786344",
			"description": "Keras-Tensorflow mnist model",
			"created_at": "2018-03-14T07:26:53.966Z"
		},
		"model_type": "tensorflow-1.5",
		"status": "INITIALIZING",
		"output": {
			"type": "cloudobjectstorage",
			"target": {
				"bucket": "batchkeras-out"
			},
			"connection": {
				"access_key": "***********",
				"secret_key": "******************",
				"url": "***************************"
			}
		},
		"type": "batch",
		"deployed_version": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/1853b42c-a209-4861-b72d-d4715a786344/versions/18dd9b56-79be-499f-a6a3-e382f712a9c0",
			"guid": "18dd9b56-79be-499f-a6a3-e382f712a9c0"
		},
		"input": {
			"type": "cloudobjectstorage",
			"source": {
				"bucket": "batchkeras-in"
			},
			"connection": {
				"access_key": "************",
				"secret_key": "******************",
				"url": "*******************************"
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
		"guid": "22c965f3-2474-45da-8726-623bb724ecfd",
		"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344/deployments/22c965f3-2474-45da-8726-623bb724ecfd",
		"created_at": "2018-03-14T09:42:16.662Z",
		"modified_at": "2018-03-14T09:42:16.975Z"
	},
	"entity": {
		"runtime_environment": "None Provided",
		"name": "batchDeploymentTest",
		"description": "Testdescription",
		"published_model": {
			"author": {
				"name": "******",
				"email": "*************"
			},
			"name": "k2_keras_seq_mnist_local_tar",
			"url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1853b42c-a209-4861-b72d-d4715a786344",
			"guid": "1853b42c-a209-4861-b72d-d4715a786344",
			"description": "Keras-Tensorflow mnist model",
			"created_at": "2018-03-14T07:26:53.966Z"
		},
		"status_details": {
			"status": "SUCCESS"
		},
		"model_type": "tensorflow-1.5",
		"status": "SUCCESS",
		"output": {
			"type": "cloudobjectstorage",
			"target": {
				"bucket": "batchkeras-out"
			},
			"connection": {
				"access_key": "************",
				"secret_key": "********************",
				"url": "**********************************"
			}
		},
		"type": "batch",
		"deployed_version": {
			"url": "https://ibm-watson-ml.mybluemix.net/v3/ml_assets/models/1853b42c-a209-4861-b72d-d4715a786344/versions/18dd9b56-79be-499f-a6a3-e382f712a9c0",
			"guid": "18dd9b56-79be-499f-a6a3-e382f712a9c0"
		},
		"input": {
			"type": "cloudobjectstorage",
			"source": {
				"bucket": "batchkeras-in"
			},
			"connection": {
				"access_key": "***********",
				"secret_key": "******************",
				"url": "*******************************"
			}
		}
	}
}
```
{: codeblock}

The prediction result is saved to a json file in the IBM Cloud Object
Storage. Refer to the following sample row for an example of the output.

Input data file preview: (filename: `img1.json`)
   
```
{
  "values":[[[[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.2705882489681244], [0.5960784554481506], [0.929411768913269], [0.9960784316062927], [0.9960784316062927], [1.0], [0.9960784316062927], [0.9882352948188782], [0.20392157137393951], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.34117648005485535], [0.6431372761726379], [0.929411768913269], [0.9921568632125854], [0.9960784316062927], [0.8549019694328308], [0.5411764979362488], [0.32549020648002625], [0.15294118225574493], [0.6039215922355652], [0.9960784316062927], [0.529411792755127], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5411764979362488], [0.9647058844566345], [0.9921568632125854], [0.9960784316062927], [0.8470588326454163], [0.6549019813537598], [0.21176470816135406], [0.019607843831181526], [0.0], [0.0], [0.0], [0.3921568691730499], [0.7490196228027344], [0.0117647061124444], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.9137254953384399], [0.9960784316062927], [0.6627451181411743], [0.2078431397676468], [0.0235294122248888], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.13725490868091583], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.6823529601097107], [0.9960784316062927], [0.3686274588108063], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.14901961386203766], [0.9607843160629272], [0.8666666746139526], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5568627715110779], [0.9960784316062927], [0.5843137502670288], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5921568870544434], [0.9960784316062927], [0.43921568989753723], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.886274516582489], [0.9490196108818054], [0.125490203499794], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0470588244497776], [0.9411764740943909], [0.7960784435272217], [0.1725490242242813], [0.1725490242242813], [0.1725490242242813], [0.1725490242242813], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.27450981736183167], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.8039215803146362], [0.3333333432674408], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.14509804546833038], [0.7215686440467834], [0.6627451181411743], [0.5215686559677124], [0.5215686559677124], [0.6352941393852234], [0.8313725590705872], [0.9960784316062927], [0.9960784316062927], [0.6509804129600525], [0.0313725508749485], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.03529411926865578], [0.20000000298023224], [0.6941176652908325], [0.9960784316062927], [0.4901960790157318], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.10588235408067703], [0.8196078538894653], [0.9960784316062927], [0.40784314274787903], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.24705882370471954], [0.8196078538894653], [0.9960784316062927], [0.7607843279838562], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0235294122248888], [0.0], [0.03921568766236305], [0.5372549295425415], [0.95686274766922], [0.9960784316062927], [0.7764706015586853], [0.20392157137393951], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.05098039284348488], [0.34117648005485535], [0.47843137383461], [0.5764706134796143], [0.8745098114013672], [0.9960784316062927], [0.9686274528503418], [0.49803921580314636], [0.05098039284348488], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0117647061124444], [0.4627451002597809], [0.9803921580314636], [0.8235294222831726], [0.9725490212440491], [0.9960784316062927], [0.9882352948188782], [0.7803921699523926], [0.1921568661928177], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.5411764979362488], [0.9960784316062927], [0.9960784316062927], [0.9960784316062927], [0.9803921580314636], [0.7882353067398071], [0.2823529541492462], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.30980393290519714], [0.6549019813537598], [0.772549033164978], [0.34117648005485535], [0.027450980618596077], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]], [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]]]]
}
```
{: codeblock}

Output file preview:

```
{
	"img1.json": {
		"fields": ["prediction", "prediction_classes", "probability"],
		"values": [
			[0.119932159781456, 0.09185332804918289, 0.10031602531671524, 0.11291926354169846, 0.10230886191129684, 0.08999011665582657, 0.11523492634296417, 0.0850936770439148, 0.09283886104822159, 0.08951272070407867], 0, [0.119932159781456, 0.09185332804918289, 0.10031602531671524, 0.11291926354169846, 0.10230886191129684, 0.08999011665582657, 0.11523492634296417, 0.0850936770439148, 0.09283886104822159, 0.08951272070407867]
		]
	}
}
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
|`fields`|Gives the following outputs:</br>1. `prediction`: refers to the output obtained from the `model.predict()` function.</br>2. `prediction_classes`: refers to output from the function `model.predict_classes()` of Sequential model</br>3. `probability`: refers to output from the function `model.predict_proba()` of Sequential model|
|`values`| Prediction output corresponding to the fields mentioned above|
{: caption="Table 3. Key and value entries of the scoring output JSON" caption-side="top"}

**Note:** First key in object array of each prediction result is filename of input data file. This is required to map input data file with its corresponding prediction result. 
      
The scoring output will be formatted as in the following example:
   
```
{
	"img1.json": {
		"fields": ["prediction", "prediction_classes", "probability"],
		"values": [
			[0.119932159781456, 0.09185332804918289, 0.10031602531671524, 0.11291926354169846, 0.10230886191129684, 0.08999011665582657, 0.11523492634296417, 0.0850936770439148, 0.09283886104822159, 0.08951272070407867], 0, [0.119932159781456, 0.09185332804918289, 0.10031602531671524, 0.11291926354169846, 0.10230886191129684, 0.08999011665582657, 0.11523492634296417, 0.0850936770439148, 0.09283886104822159, 0.08951272070407867]
		]
	}
}
```

   
## Learn more

Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).

For more information about IBM Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).