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

# Building predictive analytics applications

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_spark_building.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


You can use the {{site.data.keyword.pm_full}} service to deploy a Node.js application.
{: shortdesc}

**Scenario name**: Product line prediction.

**Scenario description**: Our client is running one of the most
famous chain stores in Europe. They would like us to figure out
their clients' interests in terms of their product line such as
Personal Accessories, Camping Equipment, and Outdoor Protection.
A Data Scientist develops a predictive model and shares it with
you (the developer). Your task is to prepare and deploy the
Node.js application that recommends sport product lines by making
score requests against the deployed model.

1. Log on to {{site.data.keyword.Bluemix_short}} and create an instance of {{site.data.keyword.pm_full}}.
2. Launch the {{site.data.keyword.pm_full}} Dashboard.
3. Go to the **Samples** tab.
4. In the **Sample Models** section, find the **Product Line Prediction**
   tile and click the **Add model** icon (+). The
   sample Product Line Prediction model appears in the list of available
   models on the **Models** tab.

   **Note**: If you want to use your own Data Science Experience
   project and models instead of the samples, you must persist a
   custom model in the {{site.data.keyword.pm_short}} repository. You can do
   this by either using the REST API or client libraries. For
   details, see Development and persistence of the custom model.

5. From the **Action** menu, click **Create Deployment** to deploy the
   Product Line Prediction model as an online deployment.
6. Specify the deployment name (for example, Product line
   prediction) and click Save.You should now see the Product line
   prediction deployment in the Deployments list.
7. From the Action menu, select View Details for your deployment.
   The Scoring Endpoint presented in the details pane will be
   used to run scoring requests.
8. To run scoring requests through the REST API, a scoring
   endpoint and an authorization token are required. For more
   information, see Deploying online models.
9. Experiment with the sample Node.js application, which is located at the following location:
   https://github.com/pmservice/product-line-prediction.

   To automatically deploy sample application code to your
   {{site.data.keyword.Bluemix_short}} space, go to the Samples tab, and in the Sample
   Applications section, select the Product Line Prediction
   application and deploy it by clicking the (+) icon.
   Authenticate on the DeployToBluemix form if prompted.

   You should now see the sample application on the {{site.data.keyword.Bluemix_short}}
   Dashboard in the All Apps section.

10. Click the application to view its details. Here you can
    modify application details, such as the number of instances or
    memory allocation.
11. Bind your application with the {{site.data.keyword.pm_short}}
    instance. On the **Connections** tab, click **Connect existing**.
    After restaging, your application is connected to the service
    instance.
12. Run the application by using the route address.
13. In the application, select your Product Line Prediction
    deployment. You are now ready to use the sample records and
    prediction results.
    
## Learn more

For more information about the API, see [Service API for Spark and Python models](pm_service_api_spark.html) or [Service
API for IBM® SPSS® models](pm_service_api_spss.html).

For more information about IBM® SPSS® Modeler and the modeling algorithms it
provides, see [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

For more information about IBM Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).