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

# Setting up your environment

**This content has moved to a [new location](https://dataplatform.ibm.com/docs/content/analyze-data/wml-setup.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


To use {{site.data.keyword.pm_full}}, you must be able to create the proper machine learning environment and retrieve the credentials that are specific to that environment. You can create the {{site.data.keyword.Bluemix}} instance either by using the [Cloud Foundry command line interface](https://github.com/cloudfoundry/cli#getting-started) or through the graphical user interface in the {{site.data.keyword.Bluemix}} Dashboard.
{: shortdesc}

## Using IBM Cloud Dashboard

To use {{site.data.keyword.pm_full}}, you must [create an instance](https://console.bluemix.net/catalog/services/machine-learning) of it in your {{site.data.keyword.Bluemix_notm}} Dashboard.

Depending on your scenario, you may also need the following resources:

- Object Storage (batch deployment)
- Db2 Warehouse on Cloud (batch deployment)
- Apache Spark (batch, stream deployment and continuous learning system)
- Message Hub (stream deployment)

To see how to work with the Dashboard to create the {{site.data.keyword.Bluemix_notm}} instances and view credentials, watch the following video, which supplements the written steps, which follow the video.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figure 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="If you cannot access the video that is embedded in this page, you can access the video from the YouTube website. (Opens in a new tab or window)">    <img src="images/video.png" alt="Video icon"></a>IBM Watson Machine Learning: Get Started</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>This video provides an overview of how to set up your machine learning environment.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## Creating {{site.data.keyword.Bluemix_notm}} instances

To create the {{site.data.keyword.pm_full}} instance, you must perform the following steps:

1. Open the {{site.data.keyword.pm_full}} [service page](https://console.bluemix.net/catalog/services/machine-learning).
2. Sign up or log in to create the service instance.
3. Enter a descriptive name for your instance, choose a space, and select your data plan.
4. Click **Create**.

Your instance opens.

## Retrieving your credentials

To use your {{site.data.keyword.Bluemix_notm}} (previously known as Bluemix) instance, you need service instance credentials. You can create and access your credentials either through the CF CLI or the {{site.data.keyword.Bluemix_notm}} Dashboard. After you bind the {{site.data.keyword.pm_short}} service instance to your {{site.data.keyword.Bluemix_notm}} application, the {{site.data.keyword.pm_short}} credentials are added to the `VCAP_SERVICES` environment variable.

To retrieve {{site.data.keyword.pm_full}} instance credentials, you must perform the following steps:

1. [Log in to {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. From the **Services** section, click the service name whose credentials you want to retrieve.
3. In the navigation pane, click **Service credentials**.
4. In the list of credentials, click **View credentials**.
5. If no credentials exist for this service, create them by clicking **Create credentials**.
6. To copy the credentials, click the copy icon.

To use the {{site.data.keyword.pm_short}} service in your application, you must bind the service to the {{site.data.keyword.Bluemix_notm}} application and the necessary credentials to run the application are inserted into the `VCAP_SERVICES` environmental variable. For more information, see [Cloud Foundry command line interface](#cloud-foundry-command-line-interface).

## Using Cloud Foundry CLI (command line interface)

### Steps to bind the service with {{site.data.keyword.Bluemix_notm}} application

You can download Node.js [sample code](https://github.com/pmservice/product-line-prediction/blob/master/README.md) to try the Machine
Learning service. Complete the following steps to create your {{site.data.keyword.Bluemix_notm}} application and bind the {{site.data.keyword.pm_short}} service. These examples use Node.js because it is a popular runtime. Others can be used, such as iOS, Ruby, Perl, or Java.

You can also perform steps 1 - 3 by using the {{site.data.keyword.Bluemix_notm}} Graphical Interface instead of the Cloud Foundry tool (cf).

1. Use the `cf create-service` command to create a service instance:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   For example, the following command creates one {{site.data.keyword.pm_short}} service instance
   with the lite plan named `my_wml_lite` in your {{site.data.keyword.Bluemix_notm}} space:

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Use the `cf create-service-key` command to create service
   credentials:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   For example, the following command creates {{site.data.keyword.pm_short}} service credentials:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1"
   ```
   {: codeblock}

3. Use the `cf bind-service` command to bind the service instance
   `my_wml_lite` to your application.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   For example, the following command binds the {{site.data.keyword.pm_short}} service instance
   `my_wml_lite` to the {{site.data.keyword.Bluemix_notm}} application `my_app1`:

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Accessing credentials through the `VCAP_SERVICES` variable

After you bind the {{site.data.keyword.pm_short}} service instance to your {{site.data.keyword.Bluemix}} application, the {{site.data.keyword.pm_short}} credentials are added to the `VCAP_SERVICES` environment variable:

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   The `VCAP_SERVICES` environment variable includes the following
   information:

<dl>
<dt>plan</dt>
<dd>the {{site.data.keyword.pm_short}} plan that is used in the service provisioning.</dd>
<dt>url</dt><dd>the address of the {{site.data.keyword.pm_short}} service instance.
<dt>access_key</dt><dd>for IBM® SPSS® Modeler streams only.</dd>
<dt>instance_id</dt><dd>{{site.data.keyword.pm_short}} instance unique identifier.</dd>
<dt>username</dt><dd>the user name, which is part of the basic authorization that is needed to generate an access token to pass in all requests to this service instance.</dd>
<dt>password</dt><dd>the password, which is part of the basic authorization that is needed to generate an access token to pass in all requests to this service instance. </dd>
</dl>

For example, you can generate an access token by using the following `curl` command:

Request example:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Output example:

       {"token":"**********"}
```
{: codeblock}

   The following Node.js code is an example of how to obtain the
   `username` and `password` values from the `VCAP_SERVICES` environment
   variable:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
