---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Deploying and scoring a Tensorflow model  model in Batch

After a trained model has been identified for serving, you can deploy it by using the batch deployment. The batch deployment will read the input data files from source bucket and save the prediction result to files in a target bucket.
{: shortdesc}

## Prerequisites for deploying and serving a Tensorflow model in Batch

Before you deploy a Tensorflow model and score it, the following prerequisites must be implemented at the time you save the trained model:
   
* You need to train the model using the training service. Details on how to train a model can be found [here](https://dataplatform.ibm.com/docs/content/analyze-data/ml_dlaas_working_with_new_models.html)
* You must use the `tf.saved_model.builder.SavedModelBuilder` API to save the TensorFlow model. You can use Experiments to train and save the model.
* The model must be tagged with the `tf.saved_model.tag_constants.SERVING` tag at the time of you save the model.
* The signature definition that is used for serving the model must be saved along with the model. You must use the `tf.core.protobuf.meta_graph_pb2.SignatureDef` API to define the signature definition.
* [Cloud Object Storage](https://console.bluemix.net/catalog/services/cloud-object-storage) instance details, which are used as construct input for the model and storage for the model output. Prepare the input data .json file as shown below. You should add the prepared input files in source bucket of your Cloud Object Storage instance.

## Batch Deployment of TensorFlow model

To perform batch deployment on a Tensorflow model, you must create the input data in source bucket(Cloud Object Storage) for that particular framework. 
   
### Add the input data files in the Source Bucket for Tensorflow model
   
The input records for batch scoring must be specified as a JSON object in separate files. The JSON object in the file must contain the key-value pairs in the following table.

| Key | Values |
| --- | --- |
|Input tensor name| Scoring input record corresponding to the input Tensor specified by key of this JSON object|


{: caption="Table 2. Key and value entries required to specify input data for batch scoring" caption-side="top"}

* These input data files needs to be placed in the root of bucket in Source bucket(COS) and each input data file must have a single scoring record. 
* Multiple inputs records can provided by placing in separate input data files. 
* You must make sure only input data files are present in the source bucket of COS.
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
   for complete details about scoring input format specifiaction.*

## Create a Batch Deployment 

1.  By assuming the connection is created in COS using the **Experiments** of your **Project** 
2.  By assuming the tensorflow model is saved in repository service, go to the Models section of the your **Project Assets**
3.  Search for the saved tensorflow model name and click on it.
4.  Select the **Deployments** tab, click **Add Deployment**
5.  Select the **Batch Prediction** tab, provide Name, Description
6.  Click on **Select source**, provide the source bucket name where you have saved the input data files.
7.  Click on **Select target**, provide the target bucket name where you need the prediction result to be stored.
8.  Click **Save**.

The prediction result is saved to target bucket in {{site.data.keyword.Bluemix}} Cloud Object Storage specified. 

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

For more information about {{site.data.keyword.DSX_full}} and the modeling
    algorithms it provides, see [https://dataplatform.ibm.com](https://dataplatform.ibm.com).
    

