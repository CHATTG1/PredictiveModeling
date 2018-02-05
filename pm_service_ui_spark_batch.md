---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying batch models

Using the {{site.data.keyword.pm_full}} service, you can deploy a model and
generate predictive analytics by making score requests against
the deployed model.
{: shortdesc}


**Scenario name**: Customer satisfaction prediction.

**Scenario description**: A telecommunications company wants to know
which customers are at risk of leaving. The presented model
predicts customer churn. A data scientist develops a predictive
model and shares it with you (the developer). Your task is to
deploy the model and generate predictive analytics by making
score requests against the deployed model.

## Prerequisites

To work with this example, you need the following services:

* [Object Storage](https://console.bluemix.net/catalog/services/object-storage) instance details, which are used as input (customer data to score) for the model and storage for the model output. Download the sample input data .csv file from [here](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv). You should add the input file to your Object Storage instance.
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) service instance credentials. You can use [this link](https://console.bluemix.net/catalog/services/apache-spark) to create one.


## Using the sample model

1.  Go to the Samples tab of the {{site.data.keyword.pm_full}} Dashboard.
2.  In the Sample Models section, find the Customer Satisfaction Prediction tile and click the Add model icon (+).

The  sample Customer Satisfaction Prediction model appears
in the list of available models on the Models tab.

## Creating a batch deployment with Object Storage

1.  Go to the Models tab of the {{site.data.keyword.pm_full}} Dashboard.
2.  From the **Actions** menu, click **Create Deployment**.
3.  In Create Deployment form provide Name, Description, and Batch Type.
4.  You must provide the following inputs:

    **Input Connection**: Object Storage details, which are used as input (customer data to score) for the model and storage for the model output (results.csv in this case, which is automatically created).

    ```
       {
          "source":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"TelcoCustomerData.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Output Connection**

    ```
       {
          "target":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"result.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Spark Connection**: Spark service credentials can be found on the Service Credentials tab of the {{site.data.keyword.Bluemix_short}} Spark service dashboard.

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

5.  Click **Save**.

The prediction result is saved to a .csv file in {{site.data.keyword.Bluemix}} Object Storage. Following is a sample row.

Input file preview:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Output file preview:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## Obtaining deployment details

You can check the status and parameters related to the deployed model.

1. Go to the Deployments tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. From the **Actions** menu, click **View Details**.

## Deleting a batch deployment

You can delete the deployment if it's no longer needed using a
query such as the following sample.

1. Go to the Deployments tab of the {{site.data.keyword.pm_full}}
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
