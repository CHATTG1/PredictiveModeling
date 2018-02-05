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

# Deploying online models

To deploy a model and generate predictive analytics by making score requests against the deployed model, use the {{site.data.keyword.pm_full}} service. The following scenario provides with an example of how to do this.
{: shortdesc}

**Scenario name**: Product line prediction.

**Scenario description**: A company that sells outdoor equipment
wants to build a model that predicts client interest in their
product line. A data scientist prepared a predictive model and
shares it with you (the developer). Your task is to deploy the
model and generate predictions of customer interest by making
score requests against the deployed model.

## Using the sample model

1. Go to the **Samples** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. In the **Sample Models** section, find the **Product Line Prediction**
   tile and click the **Add model** icon (+).

The sample **Product Line Prediction** model appears in the
list of available models on the **Models** tab.


## Creating the online deployment

1. Go to the **Models** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. From the **Actions** menu, click **Create Deployment**.
3. In the **Create Deployment** form complete the **Name**, **Description**, and **Online Type** fields.
4. Click **Save**.

The online deployment appears in the list of available deployments on the **Deployments** tab.

## Obtaining deployment details

You can check the status, scoring endpoint address (`Scoring Endpoint`),
and parameters related to the deployed model.

1. Go to the **Deployments** tab of the {{site.data.keyword.pm_full}}
   Dashboard.
2. From the **Actions** menu, click **View Details**.

Note the `Scoring Endpoint` value, which is needed to make scoring requests.


## Making score requests

After you create a scoring endpoint, you can generate predictions by making score requests. In the following scenario, customer records are passed to the predictive model and a sport product prediction is returned.

Sample record header:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Sample record:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Request example:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Output example:

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

We can see, for example, that a 55-year-old executive is
interested in Mountaineering Equipment, while a 23-year-old
student is interested in Personal Accessories.

## Learn more

For more information about the API, see [Service API for Spark and Python models](pm_service_api_spark.html) or [Service
API for IBM® SPSS® models](pm_service_api_spss.html).

For more information about IBM® SPSS® Modeler and the modeling algorithms it
provides, see [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

For more information about IBM® Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).