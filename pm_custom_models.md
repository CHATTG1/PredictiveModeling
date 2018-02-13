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

# Development and persistence of the custom model

Using the {{site.data.keyword.pm_full}} service, you can deploy a model and
generate predictive analytics by making score requests against
the deployed model.
{: shortdesc}

## Working with custom models

### Scenario name: Product line prediction.

### Scenario description:

Our client runs one of the most
famous chain stores in Europe. They would like us to figure out
their clients' interests in terms of their product line such as
personal accessories, camping equipment, and outdoor protection.
A data scientist develops a predictive model and shares it with
you (the developer). Your task is to deploy the model and
generate predictive analytics by making score requests against
the deployed model.

### Development and persistence of the custom model

#### Using Data Science Experience

Use [Data Science Experience](https://console.bluemix.net/catalog/services/data-science-experience) to create custom models. After you sign up, you must sign in to complete the following steps.

1. Create an organization and a space. The first time you sign in, you must provide this information. Click **Continue** to accept the default values.
2. After the organization is created, go to **Projects** and click
   **New project**.
3. Specify a name and description for your project and click
   **Create**. The project name that you specified is used as
   the Target Container name.
4. After the project is created, you can perform one of the following tasks:
   
   *  **add notebooks** and start developing your own models or upload one of the following sample notebooks:
        *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **add models** and start developing your own models by using the model wizard.


#### Using local environment

You can also use environment of your choice to develop model and later publish, deploy and score using {{site.data.keyword.pm_full}} [common API client library]() available on PyPI.
For more information about the client library, see the sample [notebook](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125) and [documentation](pm_service_client_library.html).

**Note:** The common API client library is in **beta**.

### Deployment and scoring of the custom model

You can use the API to deploy and score online, batch, and streaming models.

*  [Deploying online models](pm_service_api_spark_online.html)
*  [Deploying batch models](pm_service_api_spark_batch.html)
*  [Deploying streaming models](pm_service_api_spark_streaming.html)

## Learn more

Ready to get started? To create an instance of a service or bind
an application, see [Using the service with Spark and Python models](using_pm_service_dsx.html) or
[Using the service with IBM® SPSS® models](using_pm_service.html).

For more information about the API, see [Service API for Spark and Python models](pm_service_api_spark.html) or [Service
API for IBM® SPSS® models](pm_service_api_spss.html).

For more information about IBM® SPSS® Modeler and the modeling algorithms it
provides, see [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

For more information about IBM® Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).
