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

# Setting up your machine learning environment and retrieving your credentials

To use {{site.data.keyword.pm_full}}, you must be able to create the proper machine learning environment and retrieve the credentials that are specific to that environment.

## Setting up your environment

To use {{site.data.keyword.pm_full}}, you must set up instances of the following items in your {{site.data.keyword.Bluemix}} dashboard:

- {{site.data.keyword.pm_full}}
- Object Storage
- Apache Spark

In addition to the required instances, some of our notebooks and tutorials also use the following instances. To use the tutorials, you must set these services up as well.

- Python Flask
- Natural Language Understanding
- Deep learning command line interface (CLI)

### Creating IBM Cloud instances

When you create {{site.data.keyword.Bluemix}} resources, you must select a resource from the Catalog and then enter a descriptive name for your instance, choose a region, space and organization, and select your data plan. {{site.data.keyword.Bluemix}} automatically populates the Object storage name with your instance name. It’s likely you’ll need data storage for your Spark project, so choose a space and plan here too.

Watch this video to see how to create the {{site.data.keyword.Bluemix}} instances and view the credentials. Then, follow the steps after the video to set up your own environment. See the <a href="#retrieving-your-credentials">Retrieving your credentials</a> section for the steps to view your credentials.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figure 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="If you cannot access the video that is embedded in this page, you can access the video from the YouTube website. (Opens in a new tab or window)">    <img src="images/video.png" alt="Video icon"></a>IBM Watson Machine Learning: Get Started</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>This video provides an overview of how to set up your machine learning environment.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

To create Watson Data Platform instances, you must perform the following steps:

1. [Log in to IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
To create {{site.data.keyword.Bluemix_short}} instances, you must perform the following steps:

1. [Log in to {{site.data.keyword.Bluemix_short}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. From the navigation panel, click **Catalog**.

   You see a list of {{site.data.keyword.Bluemix_short}} services. You can return here regularly to add or update data and analytics services from one easy-to-use page.

3. From the {{site.data.keyword.Bluemix_short}} Catalog page, in the **Search** box, type **machine learning**.
4. Select **Machine Learning**, provide a unique name for the service, select a region, an organization, a space, and then, select a plan, and click **Create**. You must repeat these steps to create an instance of Apache Spark and Cloud Object Storage.
3. Next, search for and select **Apache Spark**, provide the required information and click **Create**.
3. Finally, search for and select **Object Storage**, provide the required information and click **Create**.

These instances now appear on your {{site.data.keyword.Bluemix_short}} Dashboard.

**Note** when provisioning an instance of {{site.data.keyword.pm_full}} to use in training Deep Learning models please be sure to select the **Region** as **US South**.  Deep learning functionality is not yet available in other regions.

### Adding existing IBM Cloud instances to a project in Data Science Experience

Watch this video to see how to create a project and set it up to use {{site.data.keyword.pm_full}}.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Figure 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="If you cannot access the video that is embedded in this page, you can access the video from the YouTube website. (Opens in a new tab or window)">    <img src="images/video.png" alt="Video icon"></a>IBM Watson Machine Learning: Create a project for Watson Machine Learning</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>This video provides an overview of how to set up your machine learning environment.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

If you already have instances, but have not linked them to a project in Data Science Experience, you must perform the following steps:

1. [Log on to IBM Data Science Experience](https://datascience.ibm.com).
2. Click **Projects** > **View All Projects**.
3. Click the **Settings** tab.
4. To add a service, in the **Associated Services** panel, click **add associated service**, select a service, complete the configuration information, and click **Save**.
5. To add an access token, perform the following steps:
   6. In the **Access Tokens** panel, click **create new token**.
   7. In the **Name** box, type a name.
   8. In the **Access Role For Project** box, select a role, either **Viewer** or **Editor**.
   9. Click **Add**.

6. To connect to a GitHub repository, in the **Repository URL** type your repository location. If you do not have a personal access token already set up for GitHub access, you must create it now by clicking **Settings**. When you have finished, click **Add**.

## Retrieving your credentials

To use your {{site.data.keyword.Bluemix}} instances, services, and models in notebooks and applications, you must be able to insert your credentials, tokens, and scoring endpoints into the code that is used for processing.

1. [Log in to IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Scroll to the **Services** section and click the service name whose credentials you want to retrieve.
3. In the navigation pane, click **Service credentials**.
4. In the list of credentials, click **View credentials**.
5. If no credentials exist for this service, you can create them by clicking **Create credentials**.
6. To copy the credentials, click the copy icon.

Depending on the service, you may have different fields, such as `username` and `password`. You must copy the values as they appear to use them in your code.

## Learn more

[A quick deep learning tutorial](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

