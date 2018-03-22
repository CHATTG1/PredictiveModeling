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

# Deploying and scoring a Keras model

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_model_keras_deploy_score.html).  Check there for the most up-to-date information.**

Update any bookmarks you might have to the old location.


_____________


After a trained Keras model with TensorFlow as backend has been identified for serving, you can deploy it by using the deployment and scoring system of {{site.data.keyword.pm_full}}. The deployed model can then be used to perform online scoring.
{: shortdesc}

The [Command Line Interface (CLI)](ml_dlaas_environment.html) client or the [Python client library - `watson-machine-learning-client`](ml_dlaas_environment_pyclient.html) can be used for deploying and scoring the models.

## Prerequisites for deploying and serving a Keras model

Before you deploy a Keras model and score it, the following prerequisites must be implemented at the time you save the trained model:
   
* Keras's save() API should be used for saving the trained Keras model.
* The extension of the saved model file must be `.h5` or `.hdf5` 
* Model should be persisted in WML Repository.

## Deploy a Keras model

To deploy a Keras model you must retrieve the model ID, which you then use to issue the command to deploy the model. 

1. Identify the model ID of the model that has to be deployed. Choose one of the following techniques:
   
   **Using CLI client:**
   The `Model Id` value can be identified by using the `list models` option. The output displays the `Model Id` for the models that are stored in the {{site.data.keyword.pm_short}} repository.  
   
   From the command prompt, run the following command: 
   
   ```
   bx ml list models
   ```
   {: codeblock}
    
    Sample Output:
    
   ```
   Fetching the list of models ...
   SI No   Model Id                               Model Type       Model Name   
   1       91bdf650-bef5-49ff-95c9-b4dad233fe80   tensorflow-1.5   k_keras_seq_mnist_local_tar1   
   
   1 records found.
   OK
   List models successful
   ```
    
   **Using the Python client - watson-machine-learning-client:**
   
   The `Model Id` value can be identified from the details returned by listing the models stored in the {{site.data.keyword.pm_short}} repository when you run the `client.repository.list()` API command. In the output, the `GUID` column represents the model ID of the corresponding model.
   
   From your Python client, run the following command:
   
   ```
   client.repository.list()
   ```
   {: codeblock}
   
   Sample Output:

   ```
   ------------------------------------  ----------------------------   --------------
   GUID                                  NAME                           FRAMEWORK
   91bdf650-bef5-49ff-95c9-b4dad233fe80  k_keras_seq_mnist_local_tar1   tensorflow-1.5
   bad97714-70b3-44c4-8cca-71fb85c2e74b  k2_keras_seq_mnist_local_tar   tensorflow-1.5
   ------------------------------------  ----------------------------   --------------
   ```
   
2. Deploy the model using the `Model Id` value that you identified in the preceding step.
   
   **Using CLI client:**
   From the command prompt, run the following command:
   
   ```
   bx ml deploy 91bdf650-bef5-49ff-95c9-b4dad233fe80 "my_deployment"
   ```
   {: codeblock}
   
   Sample Output:
   
   ```
    Deploying the model with MODEL-ID '91bdf650-bef5-49ff-95c9-b4dad233fe80'...
    DeploymentId       41c4e3d8-295b-43f4-808a-367b61e15e9e   
    Scoring endpoint   https://ibm-watson-ml-fvt.stage1.mybluemix.net/v3/wml_instances/a91ea548-493f-444b-b4e6-c4e5287de360/published_models/91bdf650-bef5-49ff-95c9-b4dad233fe80/deployments/41c4e3d8-295b-43f4-808a-367b61e15e9e/online   
    Name               keras_test_dep   
    Type               tensorflow-1.5   
    Runtime            None Provided
    Status             DEPLOY_SUCCESS
    Created at         2018-03-13T13:44:48.103Z   
    OK
    Deploy model successful
   ```
    
   **Using Python client - watson-machine-learning-client:** Run the following command:
   
   ```
   deployment_details = client.deployments.create(model_id, name="Mnist model deployment")
   ```
   {: codeblock}

## Online Scoring on a Keras model

To perform online scoring on a Keras model, you must create a scoring payload in a JSON document format. Then you must specify input data for the payload.

### Creating a scoring payload

The scoring payload must contain the following key-value entries.

| Key | Values |
| --- | --- |
| `modelId` | "Model Id" of the model that will be used for the scoring request |
| `deploymentId` | "DeploymentId" of the model that will be used for the scoring request |
| `payload` | Input data for scoring. Refer the following section on how to specify the input data |

{: caption="Table 1. Key and value entries required as scoring payload" caption-side="top"}
   
### Specifying the input data in the scoring payload
   
The input data for scoring is specified as a JSON object for the `payload` key in the scoring payload. The `payload` JSON object contains the key-value pairs in the following table.


| Key | Values |
| --- | --- |
|`values`| Input data as required by the Keras model|


{: caption="Table 2. Key and value entries required to specify input data for scoring" caption-side="top"}


1. Create the scoring payload. 
   
   Refer to the following example for defining the `payload` key for a model:
   
   Method 1:
   ```
   {
       "values": [
           ["this is a simple text input data"],
           ["this is another simple text input data"]
       ] 
   }
   ```
   
   The complete scoring payload looks as follows:
   
   Method 1:
   ```
   {   
       "modelId": "91bdf650-bef5-49ff-95c9-b4dad233fe80",
       "deploymentId": "41c4e3d8-295b-43f4-808a-367b61e15e9e"
       "payload": {
           "values": [
               ["this is a simple text input data"],
           ]
       }
   }
   ```
   {: codeblock}
   
   *Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments_deployment_id_online)
   for complete details about scoring input format specifiaction.*
   
2. Extract URL for scoring. This step is required only if Python client is used for scoring. The URL for scoring should be extracted from the object returned from API used for deploying the model.
   
   **Using Python client - watson-machine-learning-client:**
   
   ```
   deployment_details = client.deployments.create(model_guid, name="Mnist model deployment")
   scoring_url = client.deployments.get_scoring_url(deployment_details))
   ```
   {: codeblock}
   
3. Perform scoring.
   
   **Using CLI client:** From the command prompt, run the following command:
   
   ```
   bx ml score scoring_payload.json
   ```
   {: codeblock}
   
   Sample Output:
   
   ```
    Fetching scoring results for the deployment '41c4e3d8-295b-43f4-808a-367b61e15e9e' ...
    {
        'fields': [
            'prediction', 'prediction_classes', 'probability'
        ],
        'values': [
            [[0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1], 6, [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1]]
        ]
    }
    OK
    Score request successful
   ```
   
   **Using Python client - watson-machine-learning-client**:
   
   ```
   predictions = client.deployments.score(scoring_url, scoring_data)
   print(predictions)
   ```
   {: codeblock}
   
   Where `scoring_url` refers to the URL extracted in step and `scoring_data` refers to the scoring payload.
   
   Sample Output:
   
   ```
   {
           "fields": [
               "prediction", "prediction_classes", "probability"
           ],
           "values": [
               [[0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1], 6, [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1]]
           ]
   }
   ```
   
### Interpreting the scoring output
   
The scoring output returns a JSON object where key-value pairs refers to the following.
   
| Key | Values |
| --- | --- |
|`fields`|Gives the following outputs:</br>1. `prediction`: refers to the output obtained from the `model.predict()` function.</br>2. `prediction_classes`: refers to output from the function `model.predict_classes()` of Sequential model</br>3. `probability`: refers to output from the function `model.predict_proba()` of Sequential model|
|`values`| Prediction output corresponding to the fields mentioned above|
{: caption="Table 3. Key and value entries of the scoring output JSON" caption-side="top"}


*Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments_deployment_id_online)
for complete details about scoring input format specification.*
   
```
{
           "fields": [
               "prediction", "prediction_classes", "probability"
           ],
           "values": [
               [[pred1], pred_class1, [prob1]]
           ]
}
```
   
Where the values specified for the `values` key are the outputs obtained from the single output Tensor of the Keras model.
   
## Learn more

Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).

    
    

