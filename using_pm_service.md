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

# Using the service

By using the modeling methods on the IBM® SPSS® Modeler modeling
palette you can derive new information from your data and
develop predictive models. Each method has certain strengths
and is best suited for particular types of machine learning problems.
{: .shortdesc}

For details
about IBM® SPSS® Modeler and the modeling algorithms it provides, see
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

After the input and output requirements of your {{site.data.keyword.Bluemix_notm}}
application and IBM® SPSS® Modeler scoring branch design are
implemented, your Data Analyst can change any internal aspect of
the scoring branch. The Data Analyst can even change the model
algorithm(s) used in a refresh operation, ensuring your ability
to fine-tune your predictive analytics without needing to rewrite
your applications.


## Steps to bind the service with {{site.data.keyword.Bluemix_notm}} application

Complete the following steps to create your {{site.data.keyword.Bluemix_notm}} application and bind it to the {{site.data.keyword.pm_short}} service.

1. Download Node.js sample application code from [GitHub repository](https://github.com/pmservice/customer-satisfaction-prediction).

2. Use the `cf create-service` command to create a service
   instance:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   For example:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   This command creates one {{site.data.keyword.pm_short}} service instance
   with lite plan named my_pm_lite in your {{site.data.keyword.Bluemix_notm}} space.

3. Use the `cf create-service-key` command to create service
   credentials:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   For example:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1"
   ```
   {: codeblock}

   This command creates {{site.data.keyword.pm_short}} service credentials.

4. Use the `cf bind-service` command to bind the service instance
   `my_pm_lite` to your application.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   For example:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   This command binds the {{site.data.keyword.pm_short}} service instance
   `my_pm_lite` to the {{site.data.keyword.Bluemix_notm}} application my_app1.

5. {{site.data.keyword.pm_short}} credentials:

   After you bind the {{site.data.keyword.pm_short}} service instance to your
   {{site.data.keyword.Bluemix_notm}} application, the {{site.data.keyword.pm_short}} credentials are
   added to the `VCAP_SERVICES` environment variable:

```
    {   
        "pm-20": {      
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   The `VCAP_SERVICES` environment variable includes the following
   information:

   <dl>

   <dt>plan</dt>
   <dd>The {{site.data.keyword.pm_short}} plan that is used in the service provisioning.</dd>

   <dt>url</dt>
   <dd>The address of the {{site.data.keyword.pm_short}} service instance.</dd>

   <dt>access_key</dt>
   <dd>The query parameter accessKey to pass in all requests
            to this service instance.</dd>

   </dl>

For example:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Example Node.js code that shows how to obtain the accessKey
   from the `VCAP_SERVICES` environment variable:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
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
