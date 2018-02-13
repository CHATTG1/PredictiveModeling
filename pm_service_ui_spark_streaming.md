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

# Deploying streaming models

You can use the {{site.data.keyword.pm_full}} service to deploy the model and
generate predictive analytics by making score requests against the deployed streaming model.
{: shortdesc}

**Scenario name**: Sentiment Analysis.

**Scenario description**: A marketing agency wants to know the
sentiment about a particular topic. The agency would like us to
develop a model that classifies a statement as POSITIVE or
NEGATIVE. A data scientist prepares a predictive model and shares
it with you (the developer). Your task is to deploy the model and
generate predictive analytics by making score requests against
the deployed model.

See this [document](https://github.com/pmservice/tweet-sentiment-prediction) for more information.

## Prerequisites

To work with this example, you need the following resources:

* [Message Hub](https://console.bluemix.net/catalog/services/message-hub) topic details, which are used as input (tweet text) for the model and storage for the model output (prediction results). Make sure that two topics are created: input with tweet text and output topic.
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) service instance credentials. Use [this link](https://console.bluemix.net/catalog/services/apache-spark) to create one.


## Using the sample model

1. Go to the **Samples** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. In the **Sample Models** section, find the **Sentiment Prediction**
   tile and click the Add model icon (+).

The Sentiment Prediction sample model appears in the list
of available models on the Models tab.


## Creating a streaming deployment with IBM Message Hub

1.  Go to the **Models** tab of the {{site.data.keyword.pm_full}} Dashboard.
2.  From the **Actions** menu, click **Create Deployment**.
3.  In the **Create Deployment** form complete the **Name**, **Description**, and **Stream Type** fields.
4.  You must provide the following inputs:

    **Input Connection**: IBM Message Hub topics details, which are used as input (tweets) for the model and storage for the model output  (prediction results).

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
        "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
        "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
        "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
        "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
        "user":"Dv5kEVNNsbuJ9RFE",
        "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
     "source":{
        "topic":"sinput",
        "type":"kafka"
     }
  }
    ```
    {: codeblock}

    **Output Connection**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
       "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
       "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
       "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
       "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
       "user":"Dv5kEVNNsbuJ9RFE",
       "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
    "target":{
       "topic":"soutput",
       "type":"kafka"
    }
 }
    ```
    {: codeblock}

    **Spark Connection**: Spark service Credentials can be found on the Service Credentials tab of the {{site.data.keyword.Bluemix_notm}} Spark service dashboard.

     ```
{
     "credentials":{
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
       "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
       "cluster_master_url": "https://spark.bluemix.net",
       "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
       "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
       "plan": "ibm.SparkService.PayGoPersonal"
     },
     "version":"2.0"
}
     ```
     {: codeblock}

5. Click **Save**.

The prediction result is sent to defined MessageHub topic (Output connection).

## Obtaining deployment details

You can check the status, and parameters related to the deployed model.

1. Go to the **Deployments** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. From the **Actions** menu, click **View Details**.

## Deleting a streaming deployment

You can delete the deployment if it's no longer needed by running a
query such as the following sample.

1. Go to the **Deployments** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. From the **Actions** menu, click **Delete**.

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
