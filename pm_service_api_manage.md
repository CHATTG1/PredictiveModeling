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

# Managing the deployed SPSS models

You can use the {{site.data.keyword.pm_full}} service API to upload a file that contains the IBM® SPSS® Modeler scoring branch to deploy. After it is uploaded, it is made available for scoring data in your applications. 
{: shortdesc}

Specifically you can perform the following tasks:

*  [Deploying or refreshing a predictive model](#deploying-or-refreshing-a-predictive-model)
*  [Retrieving a list of all currently deployed models](#retrieving-a-list-of-all-currently-deployed-models)
*  [Downloading a copy of a specific deployed model file](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [Deleting a deployed predictive model](#deleting-a-deployed-predictive-model)

## Deploying or refreshing a predictive model

Each
model file is given a context ID as a convenient alias to use for
referencing the deployed model in subsequent service calls. If a
model exists for a context ID, it is replaced by the following `PUT` call as
a means of refreshing the predictive analytics in use by your
applications.

Request example:

```
PUT http://{PA Bluemix load balancer URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound application}
```
{: codeblock}

Request parameters:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the deployment succeeds:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

Response when the deployment fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## Retrieving a list of all currently deployed models

Use the following API call to retrieve a summary of all models that are currently deployed on
this service instance.

Request example:

```
GET http://{PA Bluemix load balancer URL}/pm/v1/model?accesskey={access_key for this bound application}
```
{: codeblock}

Request parameters:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the request for a deployed model summary succeeds:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Response when the request for a deployed model summary fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"  
         }
```
{: codeblock}

## Downloading a copy of a specific deployed model file

Use the following API call to download a copy of a specific deployed model
file.

Request example:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Request parameters:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the download request succeeds:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Response when the downlaod request fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}

## Deleting a deployed predictive model

Use the following API call to delete the predictive model from the Machine
Learning service instance. After you run this call, the predictive model
will no longer be available for download or scoring data in your
applications.

Request example:

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}
```
{: codeblock}

Request parameters:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the undeploy succeeds:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

Response when the undeploy fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## Learn more

Ready to get started? To create an instance of a service or bind
an application, see [Using the service with Spark and Python models](using_pm_service_dsx.html) or
[Using the service with IBM® SPSS® models](using_pm_service.html).

For more information about the API, see [Service API for Spark and Python models](pm_service_api_spark.html) or [Service
API for IBM® SPSS® models](pm_service_api_spss.html).

For more information about IBM® SPSS® Modeler and the modeling algorithms it
provides, see [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

For more information about IBM Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).