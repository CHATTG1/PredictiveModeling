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

# Set up an Object Store for Deep Learning

To work with the {{site.data.keyword.pm_full}} service and the deep learning functions that are part of that service, you must have access to a Cloud Object Storage instance.  You must define Cloud Object Storage buckets to provide the training data and to store the trained model and logs, and you can use the same or separate Cloud Object Storage instances for this purpose.
{: shortdesc}

## Creating a Cloud Object Storage instance

1. Go to the [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) page    
2. Configure your Object Storage instance and click **Create**.
3. After the service is created, in the side menu, click **Service Credentials** to obtain the credentials, such as the API URL, user name, and password to access the Object Storage instance.  When generating credentials please add the following in the section **Inline Configuration Parameters (Optional)**

  `{"HMAC":true}`

4. In the generated credentials you should then see a section `cos_hmac_keys` which provides S3 compatible access_key_id and secret_access_key, note these down.

```
{
  ...
  "cos_hmac_keys": {
    "access_key_id": "JHG89798hijygyfysaada34434j",
    "secret_access_key": "hjkJHF6545lkhlkh678"
  },
  ...
}
```

As well as these details you will need a public service endpoint URL which can be obtained from the `endpoint` section on the left hand menu.  A typical public service endpoint URL would be:

`https://s3-api.us-geo.objectstorage.softlayer.net`

## Accessing the Cloud Object Storage (IaaS) instance

The Cloud Object Storage (IaaS) service provides S3 compatible APIs and can be accessed by using any compatible client application.

## Amazon Web Services (AWS) command line interface (CLI)

1. Install [AWS CLI](https://aws.amazon.com/cli/) using `pip install awscli`
2. Configure the credentials for the Cloud Object Storage (noted in step 4 of creating a Cloud Object Storage instance) in your AWS credentials file (for linux, located in ~/.aws/credentials).

```
...
[ibm_cos]
aws_access_key_id=<access_key_id>
aws_secret_access_key=<secret_access_key>
...

```
{: codeblock}

You can use the AWS CLI to list and update buckets and files, specifying the above profile and the Cloud Object Storage endpoint from your credentials.  For example, to list the buckets in the instance, run the following command (using the endpoint URL noted in step 4 of creating a Cloud Object Storage instance):

```
aws --endpoint-url=<ibm-cos-endpoint-url> --profile ibm_cos s3 ls
```
{: codeblock}

## Data formatting

Different frameworks require train and test data sets in different formats. For example, Caffe requires data sets in LevelDB or LMDB format while Torch requires data sets in Torch proprietary format. We assume that data is already in the format needed by the specific framework. Details on how to convert raw data to framework specific format is beyond the scope of this document. Consult the documentation for the deep learning framework you wish to use for more information.

