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

# REST API

The {{site.data.keyword.pm_short}} service is a set of REST APIs that you can call from
any programming language and that permit the integration of Data Science
Experience analytics in your applications. Bind your
{{site.data.keyword.Bluemix_short}} applications to the {{site.data.keyword.pm_short}} service instance and
generate the predictive analytics that your applications need to
deliver higher value to your users. Manage models in the
[administration dashboard](pm_service_ui_spark.html), and use the dashboard to create online,
batch, or streaming deployments integrated with your
applications.
{: shortdesc}

Develop applications against Spark, Python, and IBM® SPSS® models that are deployed on a service instance
through the powerful [REST APIs](https://watson-ml-api.mybluemix.net/):

*  Retrieve the metadata for a given predictive model
*  Deploy models and manage deployed models
    *  Online deployment (scoring)
    *  Batch deployment (supporting {{site.data.keyword.Bluemix}} Object Storage and {{site.data.keyword.dashdbshort}})
    *  Stream deployment (supporting {{site.data.keyword.Bluemix}} Message Hub)
*  Retrieve the metadata for a given deployment
*  Monitor and retrain deployed models by using the continuous learning system
*  Generate predictive analytics by making score requests against
   deployed models

For more information, see the following examples of REST API use:

*  [Deploying online models](pm_service_api_spark_online.html)
*  [Scoring online models](pm_service_api_develop_score.html)
*  [Deploying batch models](pm_service_api_spark_batch.html)
*  [Deploying stream models](pm_service_api_spark_streaming.html)
*  [Continuous learning system](pm_service_api_spark_learning_system.html)

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
