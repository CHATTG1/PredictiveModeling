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

# Deploying and scoring a Tensorflow model

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_model_tensorflow_deploy_score.html).  Check there for the most up-to-date information.**

Update any bookmarks you might have to the old location.


_____________


After a trained model has been identified for serving, you can deploy it by using the deployment and scoring system of {{site.data.keyword.pm_full}}. The deployed model can then be used to perform online scoring.
{: shortdesc}

The [Command Line Interface (CLI)](ml_dlaas_environment.html) client or the [Python client library - `watson-machine-learning-client`](ml_dlaas_environment_pyclient.html) can be used for deploying and scoring the models.

## Prerequisites for deploying and serving a TensorFlow model

Before you deploy a TensorFlow model and score it, the following prerequisites must be implemented at the time you save the trained model:
   
* You must use the `tf.saved_model.builder.SavedModelBuilder` API to save the TensorFlow model.
* The model must be tagged with the `tf.saved_model.tag_constants.SERVING` tag at the time of you save the model. If multiple signatures are tagged with `tf.saved_model.tag_constants.SERVING`, the candidate signature for online scoring will be randomly picked up.  
* The signature definition that is used for serving the model must be saved along with the model. You must use the `tf.core.protobuf.meta_graph_pb2.SignatureDef` API to define the signature definition.

## Deploy a TensorFlow model

To deploy a TensorFlow model you must retrieve the model ID, which you then use to issue the command to deploy the model. 

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
   1       a8379aaa-ea31-4c22-824d-89a01315dd6d   tensorflow-1.5   training-HrlzIHskg model saved from CLI   
   
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
   ------------------------------------  -------------------  --------------
   GUID                                  NAME                 FRAMEWORK
   87b0cd9a-5816-4180-b7ae-02392b6f6931  mnist example        tensorflow-1.5
   a0c0eba9-73a3-4763-b578-bf4010651735  My cool mnist model  tensorflow-1.5
   ------------------------------------  -------------------  --------------
   ```
   
2. Deploy the model using the `Model Id` value that you identified in the preceding step.
   
   **Using CLI client:**
   From the command prompt, run the following command:
   
   ```
   bx ml deploy a8379aaa-ea31-4c22-824d-89a01315dd6d "my_deployment"
   ```
   {: codeblock}
   
   Sample Output:
   
   ```
    Deploying the model with MODEL-ID 'a8379aaa-ea31-4c22-824d-89a01315dd6d'...
    DeploymentId       9d6a656c-e9d4-4d89-b335-f9da40e52179   
    Scoring endpoint   https://2000ab8b-7e81-41b3-ad07-b70f849594f5.wml-fvt.stage1.ng.bluemix.net/v3/published_models/a8379aaa-ea31-4c22-824d-89a01315dd6d/deployments/9d6a656c-e9d4-4d89-b335-f9da40e52179/online   
    Name               test34   
    Type               tensorflow-1.5   
    Runtime            python-3.5
    Status             DEPLOY_SUCCESS   
    Created at         2017-11-28T12:46:19.770Z   
    OK
    Deploy model successful
   ```
    
   **Using Python client - watson-machine-learning-client:** Run the following command:
   
   ```
   deployment_details = client.deployments.create(model_id, name="Mnist model deployment")
   ```
   {: codeblock}

## Online Scoring on a TensorFlow model

To perform online scoring on a TensorFlow model, you must create a scoring payload in a JSON document format. Then you must specify input data for the payload.

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
|`values`| Input data for the model if it contains only one input Tensor|
|`keyed_values`| A list of JSON objects defined in "Table 3". This property must be used when the model contains more than 1 input Tensor|

{: caption="Table 2. Key and value entries required to specify input data for scoring" caption-side="top"}


| Key | Values |
| --- | --- |
|`key`| Name of the input Tensor used for scoring|
|`values`| Scoring input record corresponding to the input Tensor specified in `key` parameter|

{: caption="Table 3. Key and value entries to be specified for `keyed_values` in Table 2" caption-side="top"}
*Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments_deployment_id_online)
for details about scoring input and output format specifications.*

1. Create the scoring payload. 

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
   
   Refer to the following two examples for defining the `payload` key for a model with the `SignatureDef` defined in the code sample above:
   
   Method 1:
   ```
   {
       "values": [
           ["this is a simple text input data"],
           ["this is another simple text input data"]
       ] 
   }
   ```
   
   Method 2:
   ```
   {
       "keyed_values": [
           {
               "key": "x_input_data",
               "values": [
                   ["this is a simple text input data"],
                   ["this is another simple text input data"]
               ]
           }
       ]
   }
   ```
   
   The complete scoring payload looks as follows:
   
   Method 1:
   ```
   {   
       "modelId": "a8379aaa-ea31-4c22-824d-89a01315dd6d",
       "deploymentId": "9d6a656c-e9d4-4d89-b335-f9da40e52179"
       "payload": {
           "values": [
               ["this is a simple text input data"],
               ["this is another simple text input data"]
           ]
       }
   }
   ```
   {: codeblock}
   
   Method 2:
   ```
   {   
       "modelId": "a8379aaa-ea31-4c22-824d-89a01315dd6d",
       "deploymentId": "9d6a656c-e9d4-4d89-b335-f9da40e52179"
       "payload": {
           "keyed_values": [
               {
                   "key": "x_input_data",
                   "values": [
                       ["this is a simple text input data"],
                       ["this is another simple text input data"]
                   ]
               }
           ]
       }
   }
   ```
   {: codeblock}
      
   Where the `x_input_data` value is the key for the input tensor as per the `SignatureDef` of the model and the values for the key `values` pertain to two records for which prediction has to be performed.
   
   *Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments_deployment_id_online)
   for complete details about scoring input and output format specifications.*
   
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
    Fetching scoring results for the deployment 'e27c1fb7-0560-43df-bc9f-4c64580d67cd' ...
    {"values":[5,4]}
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
   {"values": [5, 4]}
   ```
   
### Interpreting the scoring output
   
The scoring output returns a JSON object where key-value pairs refers to the following.
   
| Key | Values |
| --- | --- |
|`values`| Output data for the model that contains only one output Tensor|
|`keyed_values`| A list of JSON objects defined in "Table 5". This property will be returned only when the model contains multiple output Tensors|
{: caption="Table 4. Key and value entries of the scoring output JSON" caption-side="top"}

| Key | Values |
| --- | --- |
|`key`| Name of the output Tensor used for scoring|
|`values`| Scoring output record corresponding to the output Tensor specified in `key` parameter|
{: caption="Table 5. Key and value entries of `keyed_values` in Table 4" caption-side="top"}

*Refer [Watson Machine Learning API documentation](http://watson-ml-v3-api.mybluemix.net/#!/Deployments/post_v3_wml_instances_instance_id_published_models_published_model_id_deployments_deployment_id_online)
for complete details about scoring input and output format specifications.*
   
Refer to the example of the `SignatureDef` key specified in the preceding step. The scoring output for that `SignatureDef` will be formatted as in the following example:
   
```
{"values": ["category1", "category3"]}
```
   
Where the values specified for the `values` key are the outputs obtained from the single output Tensor of the Tensorflow model.
   
## Learn more

Get started using these [sample training runs](ml_dlaas_working_with_sample_models.html) or create your own [new training runs](ml_dlaas_working_with_new_models.html).

    
    

