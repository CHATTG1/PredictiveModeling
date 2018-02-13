---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Tutorial

The following tutorial guides you through the steps to train a TensorFlow model in the cloud by using the {{site.data.keyword.pm_full}} and Object Storage Open Stack for {{site.data.keyword.Bluemix}} services.
{: shortdesc}

## Tutorial - Build a TensorFlow model to recognize handwritten digits

Use the MNIST computer vision dataset to train your model and recognize hand-written digits. 

To go through this tutorial by using the Python client library, refer to the [documentation](https://wml-api-pyclient-dev.mybluemix.net/) and [accompanying sample notebook](https://dataplatform.ibm.com/analytics/notebooks/bf0150bf-a239-4de6-809c-2368c62b4494/view?access_token=4cf4726e821fa2806a39faacd693c050b70142488f1d83f6ef5c6f08b8d5685b).

### Step 1: Set up your environment.

Before you can create your model, you must create the services; retrieve your credentials and use them to authenticate; and then, define environment variables. 

#### Step 1.1 Create services, retrieve credentials, and install the AWS CLI.

1. Create your {{site.data.keyword.pm_full}} service and generate credentials by following [these steps](https://console.bluemix.net/docs/services/PredictiveModeling/ml_getting_access.html#setting-up-your-environment) and then, note your {{site.data.keyword.pm_short}} credentials

2. Create an [IBM Cloud Object Storage service instance](ml_dlaas_object_store.html) and note the credentials and endpoint URL required for access.

3. Install the AWS CLI and configure it [as described here](ml_dlaas_object_store.html)

#### Step 1.2 Export authentication information and log in by using the CLI

4. You must install the [IBM Cloud CLI and Machine Learning CLI Plug-in](ml_dlaas_environment.html). For more information, see [Extending IBM Cloud CLI with plug-ins](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins).

5. Log in to {{site.data.keyword.Bluemix}} through the command line interface by running the following command:

   ```
   bx api https://api.ng.bluemix.net
   bx login
   ```
   {: codeblock}
   
6. When you are prompted, type your IBMid and password.

#### Step 1.3 Define environment variables and test the connection.

You must define several environment variables. This is also a good time to test your ability to connect to the service.

7. Define environment variables for {{site.data.keyword.pm_full}} based on your specific WML service credentials that you exported in the preceding steps.

   ```
   export ML_USERNAME=<username>
   export ML_PASSWORD=<password>
   export ML_ENV=<url>
   export ML_INSTANCE=<instance-id>
   ```
   {: codeblock}

8. To test the WML CLI, list training jobs by running the following command:

   ```
   bx ml list training-runs
   ```
   {: codeblock}

   Sample Output: 

   ```
   Fetching the list of training runs ...
   SI No   Name                 guid                 status      submitted-at
   
   0 records found.
   OK
   List all trained-runs successful
   ```
   {: codeblock}

### Step 2: Upload data files.

You must download data files; create a directory and a bucket for the data; and then, upload the data files.

9. Upload data files for handwriting recognition to a new object storage bucket. The 4 data files are located [here](http://yann.lecun.com/exdb/mnist/).
10. Download the data files to your local machine. 
11. Use the AWS CLI to create the `test_data` bucket by running the following command:

   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 mb s3://test_data
   ```
   {: codeblock}

9. Upload the files to the `test_data` bucket by running the following command from the directory where you uploaded the data files:

   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 cp t10k-labels-idx1-ubyte.gz s3://test_data/
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 cp train-labels-idx1-ubyte.gz s3://test_data/
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 cp t10k-images-idx3-ubyte.gz s3://test_data/
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 cp train-images-idx3-ubyte.gz s3://test_data/
   ```
   {: codeblock}
   
   Now check the files have been uploaded:
   
   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 ls s3://test_data/
   ```
   
   Sample Output:
   
   ```
2017-10-02 10:07:55    1648877 t10k-images-idx3-ubyte.gz
2017-10-02 10:10:32       4542 t10k-labels-idx1-ubyte.gz
2017-10-02 10:10:04    9912422 train-images-idx3-ubyte.gz
2017-10-02 10:10:53      28881 train-labels-idx1-ubyte.gz
   ```
   {: codeblock}

8. Use the AWS CLI to create an object store bucket `test_results` to hold the resulting model by running the following command:  

   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 mb s3://test_results
   ```
   {: codeblock}

### Step 3: Create your deep learning program.

You must create a deep learning tensorflow program to train a model. For this, you must use the `input_data.py` and `convolutional_network.py` files, which you can find in the [tf-model.zip](https://github.com/pmservice/wml-sample-models/tree/master/tensorflow/hand-written-digit-recognition/definition) file.

In the `convolutional_network.py ` file, there is one part which is important for {{site.data.keyword.pm_full}} service to score the model properly. 

1. Save the model to the `RESULT_DIR/model` directory after training is complete by using the following format:

   ```
   model_path = os.environ["RESULT_DIR"]+"/model"
   
   classification_inputs = tf.saved_model.utils.build_tensor_info(x)
   classification_outputs_classes = tf.saved_model.utils.build_tensor_info(predictor)
   
   classification_signature = (
       tf.saved_model.signature_def_utils.build_signature_def(
           inputs={
               tf.saved_model.signature_constants.CLASSIFY_INPUTS:
                   classification_inputs
           },
           outputs={
               tf.saved_model.signature_constants.CLASSIFY_OUTPUT_CLASSES:
                   classification_outputs_classes
           },
              method_name=tf.saved_model.signature_constants.CLASSIFY_METHOD_NAME))
   
   builder = tf.saved_model.builder.SavedModelBuilder(model_path)
   legacy_init_op = tf.group(tf.tables_initializer(), name='legacy_init_op')
   builder.add_meta_graph_and_variables(
       sess, [tf.saved_model.tag_constants.SERVING],
       signature_def_map={
           'predict_images': classification_signature,
       },
       legacy_init_op=legacy_init_op)
   
   save_path = str(builder.save())
   ```
   {: codeblock}

2. Compress them into a .zip file called `tf-train.zip` by running the following command:

   ```
   zip tf-train.zip convolutional_network.py input_data.py
   ```
   {: codeblock}
   
   Sample Output:
   
   ```
  adding: convolutional_network.py (deflated 65%)
  adding: input_data.py (deflated 72%)
   ```
   {: codeblock}
   
1. Create the following manifest file [`tf-train.yaml`](https://github.com/pmservice/wml-sample-models/tree/master/tensorflow/hand-written-digit-recognition/meta) that describes the object store connections and command for the job.  For the connections, use the values from the object store credentials noted in the preceding steps. 

```
model_definition:
  name: tf-mnist-showtest1
  author:
    name: DL Developer
    email: dl@example.com
  description: Simple MNIST model implemented in TF
  framework:
    name: tensorflow
    version: "1.2"
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 20000
    compute_configuration: 
      name: small
training_data_reference:
  name: training_data_reference_name
  connection:
    endpoint_url: "https://s3-api.us-geo.objectstorage.service.networklayer.com"
    access_key_id: "722432c254bc4eaa96e05897bf2779e2"
    secret_access_key: "286965ac10ecd4de8b44306288c7f5a3e3cf81976a03075c"
  source:
    bucket: training-data
  type: s3
training_results_reference:
  name: training_results_reference_name
  connection:
    endpoint_url: "https://s3-api.us-geo.objectstorage.service.networklayer.com"
    access_key_id: "722432c254bc4eaa96e05897bf2779e2"
    secret_access_key: "286965ac10ecd4de8b44306288c7f5a3e3cf81976a03075c"
  target:
    bucket: training-results
  type: s3
```
{: codeblock}

### Step 4: Submit the training job.

1. Submit the training job
   
   ```
   bx ml train tf-train.zip tf-train.yaml
   ```
   {: codeblock}
   
   Sample Output: 
   
   ```
   Starting to train ...
   OK
   Model-ID is 'training-HrlzIHskg'
   ```
   
1. Monitor the training run
   
   ```
   bx ml show training-runs training-HrlzIHskg
   ```
   {: codeblock}
   
   Sample Output: 
   
   ```
   Fetching the training runs details with MODEL-ID 'training-HrlzIHskg' ...
   ModelId        training-HrlzIHskg
   url            /v3/models/training-HrlzIHskg
   Name           tf-mnist-showtest1
   State          running
   Submitted_at   2017-11-17T17:01:39Z
   OK
   Show trained-runs details successful
   ```
   
   1. Monitor the logs of training job
   
   ```
   bx ml monitor training-runs training-HrlzIHskg
   ```
   {: codeblock}
   
   Sample Output: 
   
   ```
   Starting to fetch status messages for model-id 'training-HrlzIHskg'
   Status: COMPLETED
   ```
   
1. After the status changes to `COMPLETED`, you can inspect the trained model and logs in the object store.  These appear in the `training-HrlzIHskg` folder in the `test_results` bucket.
1. You can list the files you have in "test_results" 
   
   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 ls s3://test_data/
   ```
   {: codeblock}
   
   ```
   training-HrlzIHskg/learner-1/load-data.log
   training-HrlzIHskg/learner-1/load-model.log
   training-HrlzIHskg/learner-1/training-log.txt
   training-HrlzIHskg/model/saved_model.pb
   training-HrlzIHskg/model/variables/variables.data-00000-of-00001
   training-HrlzIHskg/model/variables/variables.index
   training-HrlzIHskg/saved_model.tar.gz
   ```
   
1. You can download the saved model by running the command below
   
   ```
   aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 cp s3://test_data/saved_model.tar.gz saved_model.tar.gz
   ```
   {: codeblock}
   

### Step 5: Deploy and score the model 

1. Store trained model to WML repository
   
   ```
   bx ml store training-runs training-HrlzIHskg
   ```
   {: codeblock}
   
   ```
   OK
   Model store successful. 
   Model-ID is 'a8379aaa-ea31-4c22-824d-89a01315dd6d'
   ```
   
   Due to asynchronous persistence operation, you need to wait a little while for model to become available for deployment although it will be listed when listing models stored in WML Repository in the next step. 
   
1. List the models stored in WML Repository
   
   ```
   bx ml list models
   ```
   {: codeblock}
   
   ```
   Fetching the list of models ...
   SI No   Model Id                               Model Type       Model Name   
   1       a8379aaa-ea31-4c22-824d-89a01315dd6d   tensorflow-1.2   training-HrlzIHskg model saved from CLI   
   
   1 records found.
   OK
   List models successful
   ```
   
1. Deploy stored model to WML
   
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
   Type               tensorflow-1.2   
   Runtime            python-3.5   
   Created at         2017-11-28T12:46:19.770Z   
   OK
   Deploy model successful
   ```
   
1. Score the deployed model. To score the model, the `scoring_payload.json ` file must use the following format: 
   
   ```
   {"modelId": "a8379aaa-ea31-4c22-824d-89a01315dd6d","deploymentId": "9d6a656c-e9d4-4d89-b335-f9da40e52179","payload":{"inputs":[[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0313725508749485,0.48235297203063965,0.6352941393852234,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.3294117748737335,0.40392160415649414,0.7921569347381592,0.8823530077934265,0.3333333432674408,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.07058823853731155,0.1568627506494522,0.6705882549285889,0.874509871006012,0.9411765336990356,0.9254902601242065,0.29019609093666077,0.22745099663734436,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.19215688109397888,0.250980406999588,0.5254902243614197,0.9411765336990356,1.0,0.9921569228172302,0.6313725709915161,0.22352942824363708,0.05490196496248245,0.1411764770746231,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.38431376218795776,0.8313726186752319,0.8352941870689392,0.9529412388801575,0.9921569228172302,0.9921569228172302,0.9921569228172302,0.7529412508010864,0.2666666805744171,0.08235294371843338,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.6274510025978088,0.9019608497619629,0.9058824181556702,0.9019608497619629,0.6235294342041016,0.3137255012989044,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.1411764770746231,0.3294117748737335,0.4745098352432251,0.10588236153125763,0.10588236153125763,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.16862745583057404,0.4941176772117615,0.3686274588108063,0.22352942824363708,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.3333333432674408,0.7921569347381592,0.1882353127002716,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.05490196496248245,0.9294118285179138,0.9529412388801575,0.13725490868091583,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.1411764770746231,0.7058823704719543,1.0,0.8235294818878174,0.7568628191947937,0.14509804546833038,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.125490203499794,0.24705883860588074,0.24705883860588074,0.7176470756530762,0.5568627715110779,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.5843137502670288,0.8000000715255737,0.03529411926865578,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.9960784912109375,0.8823530077934265,0.05490196496248245,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.9960784912109375,0.3686274588108063,0.01568627543747425,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.8627451658248901,0.04313725605607033,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.08627451211214066,0.6784313917160034,0.9960784912109375,0.24705883860588074,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.007843137718737125,0.4901961088180542,0.9921569228172302,0.8784314393997192,0.125490203499794,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.08627451211214066,0.9921569228172302,0.7843137979507446,0.1411764770746231,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.01568627543747425,0.5098039507865906,0.027450982481241226,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0],[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.3607843220233917,0.9921569228172302,0.5529412031173706,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.07450980693101883,0.7686275243759155,0.988235354423523,0.9450981020927429,0.0941176563501358,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.5764706134796143,0.8823530077934265,0.3803921937942505,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.988235354423523,0.988235354423523,0.9921569228172302,0.10588236153125763,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.125490203499794,0.9921569228172302,0.988235354423523,0.7647059559822083,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.988235354423523,0.988235354423523,0.6980392336845398,0.03529411926865578,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.5490196347236633,0.9921569228172302,0.988235354423523,0.40000003576278687,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.988235354423523,0.988235354423523,0.5490196347236633,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.062745101749897,0.7960785031318665,0.9921569228172302,0.988235354423523,0.21568629145622253,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.4705882668495178,0.9921569228172302,0.9921569228172302,0.5529412031173706,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.11372549831867218,0.9921569228172302,1.0,0.8431373238563538,0.12156863510608673,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.7725490927696228,0.988235354423523,0.988235354423523,0.5490196347236633,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.11372549831867218,0.988235354423523,0.9921569228172302,0.16470588743686676,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.7725490927696228,0.988235354423523,0.988235354423523,0.12156863510608673,0.0,0.0,0.0,0.0,0.0,0.0,0.05098039656877518,0.30980393290519714,0.988235354423523,0.9921569228172302,0.10588236153125763,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.917647123336792,0.988235354423523,0.9254902601242065,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.988235354423523,0.988235354423523,0.6980392336845398,0.03529411926865578,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.3333333432674408,0.988235354423523,0.988235354423523,0.7411764860153198,0.5529412031173706,0.5490196347236633,0.5490196347236633,0.5490196347236633,0.5490196347236633,0.30980393290519714,0.1882353127002716,0.6470588445663452,0.988235354423523,0.988235354423523,0.5490196347236633,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.33725491166114807,0.9921569228172302,0.9921569228172302,0.9921569228172302,1.0,0.9921569228172302,0.9921569228172302,0.9921569228172302,0.9921569228172302,1.0,0.9921569228172302,0.9921569228172302,0.9921569228172302,0.9921569228172302,0.5529412031173706,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.22352942824363708,0.9137255549430847,0.988235354423523,0.988235354423523,0.9921569228172302,0.988235354423523,0.988235354423523,0.9490196704864502,0.8392157554626465,0.8431373238563538,0.9529412388801575,0.988235354423523,0.988235354423523,0.988235354423523,0.5490196347236633,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.29411765933036804,0.7647059559822083,0.7647059559822083,0.2196078598499298,0.21568629145622253,0.21568629145622253,0.19215688109397888,0.12156863510608673,0.12156863510608673,0.19607844948768616,0.8196079134941101,0.988235354423523,0.8627451658248901,0.12156863510608673,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.2862745225429535,0.917647123336792,0.988235354423523,0.4392157196998596,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.8823530077934265,0.988235354423523,0.988235354423523,0.4392157196998596,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.8862745761871338,0.9921569228172302,0.9921569228172302,0.4392157196998596,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.3960784673690796,0.9764706492424011,0.988235354423523,0.9490196704864502,0.29019609093666077,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.4431372880935669,0.988235354423523,0.988235354423523,0.9647059440612793,0.3450980484485626,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.14901961386203766,0.917647123336792,0.988235354423523,0.6039215922355652,0.38823533058166504,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.572549045085907,0.988235354423523,0.3294117748737335,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0]],"keep_prob":1.0}}
   ```

1. To score, run the following command, which passes the `scoring_payload.json` file to the scoring processor:

   ```
   bx ml score scoring_payload.json
   ```
   {: codeblock}
   
   Sample Output: 
   
   ```
Fetching scoring results for the deployment 'e27c1fb7-0560-43df-bc9f-4c64580d67cd' ...
{"classes":[5,4]}
OK
Score request successful
   ```

## Learn more

Congratulations! You completed this tutorial.
