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

# Continuous learning system

The {{site.data.keyword.pm_full}} continuous learning system provides automated monitoring of model performance, retraining, and redeployment to ensure prediction quality.
{: shortdesc}

**Scenario name**: Best drug for heart treatment selection.

**Scenario description**: A biomedical company that produces heart drugs wants to deploy a model that selects the best drug for heart treatment. When new evidence emerges on drug effectiveness, a model update should be considered. A data scientist prepares a predictive model and shares it with you (the developer). Your task is to deploy the model, set the learning configuration, and run the learning iteration to evaluate, retrain, and redeploy the model.

## Prerequisites

To work with this example, you need the following services:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) to store the feedback data
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) service

   Use [this link](https://console.bluemix.net/catalog/services/apache-spark) to create an Apache Spark instance if you don't already have one.

## Using the sample model

1. Go to the **Samples** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. In the **Sample Models** section, find the **Heart Drug Selection**
   tile and click the **Add model** icon (+).

The sample Heart Drug Selection model appears in the list of available models on the **Models** tab.


## Set continuous learning system for published model

1.  Go to the **Models** tab of the {{site.data.keyword.pm_full}} Dashboard.
2.  From the **Actions** menu, click **View Details**.
3.  Scroll down to **Retraining Details** and press **Edit**.
4.  You must provide the following inputs:

    **Feedback Connection**: {{site.data.keyword.dashdbshort}} details, which are used as input (feedback data) for the model evaluation and retraining.

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
        "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Use the following instructions to prepare the  `DRUG_FEEDBACK_DATA` table in {{site.data.keyword.dashdbshort}}.
    
    - Create a [{{site.data.keyword.dashdbshort}} Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) instance (an entry plan is offered).
    - Create the **DRUG_FEEDBACK_DATA** table in **{{site.data.keyword.dashdbshort_notm}}**.
      + Download the [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) file from the GitHub repository.
      + Click **Open the console** to get started with **{{site.data.keyword.dashdbshort_notm}}**.
      + Click **Load Data** and select the **Desktop** load type.
      + **Drag** the previously downloaded file to the load area and click **Next**.
      + Select **Schema** to import data and click **New Table**.
      + Type a name for the new table and click **Next**.
      + Use a semicolon (;) as the **field separator**.
      + Click **Next** to create a table with the uploaded data.

     **Note**: You can add new feedback records to the feedback database by using the [REST API endpoint](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback).

     **Minimal Data Size**: The minimal number of records in feedback data set to start evaluation of the model (part of learning system iteration).

     ```
     20
     ```
     {: codeblock}

     **Spark Connection**: Spark service credentials can be found on the **Service Credentials** tab of the {{site.data.keyword.Bluemix_notm}} Spark service dashboard.

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

     **Thresholds**: The threshold value that triggers the retraining process. If the metric value, such as the accuracy value that is calculated during model evaluation is less than the threshold value, it triggers retraining.

     ```
     0.8
     ```

     **Automatically deploy the retrained model**: Deploy the retrained model if it has a better metric value than the deployed one.

5.  Click **Set up**.

## Run continuous learning system iteration

A published model is evaluated iteratively. If the metric value, such as the accuracy value that is calculated during model evaluation is less than the threshold value, it triggers retraining. Both data sets, training and feedback, are used for this iterative process of retraining and evaluation.

1. To start a new iteration, from the model **Details** form, on the **Monitor** tab, click **Evaluate**.
3. To check the iteration result go either to the Evaluation Events table or Chart view. 

   You can view information about model quality that is calculated based on the feedback data (monitoring). You can also view information about the retrained model quality, which is the new model version that was created and evaluated.

4. To see the list of automatically trained models and their quality, click **View Details** > **Overview** and go to the **Version History** section.

## Feedback data store

You can use a [feedback endpoint](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) to send new records to the feedback store that you defined in the machine learning configuration. {{site.data.keyword.dashdbshort}} is currently supported as a feedback data store. If the feedback table does not exist, the service creates it. If the table exists, the schema is verified to match that of the training table and an extra column named `_training` is appended to the table. The `_training` column is used to mark records that are consumed during the retraining process.

For more information and examples of a feedback data store, see the [API documentation](pm_service_api_spark_learning_system.html).

**Note:** The feedback table is managed, modified, and used by the {{site.data.keyword.pm_full}} service.

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
