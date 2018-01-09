---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 设置机器学习环境并检索凭证

要使用 IBM Watson Machine Learning，您必须能够创建适当的机器学习环境并检索特定于该环境的凭证。

## 设置环境

要使用 IBM Watson Machine Learning，必须在 IBM Cloud 仪表板中设置以下各项的实例：

- Watson Machine Learning
- Object Storage
- Apache Spark

除了必需的实例外，我们的一些配置页和教程还会使用以下实例。要使用教程，还必须设置以下服务。

- Python Flask
- Natural Language Understanding
- 深度学习试验

### 创建 IBM Cloud 实例

请观看以下视频，了解如何创建 IBM Cloud 实例并查看凭证。然后，按照视频下面列出的步骤来设置自己的环境。请参阅<a href="#retrieving-your-credentials">检索凭证</a>部分以了解查看凭证的步骤。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>图 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="如果无法访问此页面中嵌入的视频，可以通过 YouTube Web 站点来访问此视频。（在新选项卡或窗口中打开）">    <img src="images/video.png" alt="“视频”图标"></a>IBM Watson Machine Learning：入门</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>此视频概述了如何设置机器学习环境。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

要创建 Watson Data Platform 实例，必须执行以下步骤：

1. [登录到 IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 在导航面板中，单击**数据和分析**。

   您将看到以各数据服务为中心的屏幕。您可以经常返回此处，以利用这一易用型页面来使用数据和分析服务。

3. 单击**分析**选项卡，然后单击**实例**。
4. 单击**新建实例**。
5. 配置实例和数据存储。

   输入实例的描述性名称，选择空间，然后选择数据套餐（在此页面上查找套餐比较和定价详细信息）。IBM Cloud 会自动使用您的实例名称填充 Object Storage 名称。Spark 项目可能需要用到数据存储，因此也请在此处选择所需的空间和套餐。

6. 单击**创建实例**。

这会打开您的实例。

### 向 Data Science Experience 中的项目添加现有 Bluemix 实例

请观看以下视频，了解如何创建项目并将其设置为使用 Watson Machine Learning。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>图 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="如果无法访问此页面中嵌入的视频，可以通过 YouTube Web 站点来访问此视频。（在新选项卡或窗口中打开）">    <img src="images/video.png" alt="“视频”图标"></a>IBM Watson Machine Learning：为 Watson Machine Learning 创建项目</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>此视频概述了如何设置机器学习环境。</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

如果您已有实例，但尚未将其链接到 Data Science Experience 中的项目，那么必须执行以下步骤：

1. [登录到 IBM Data Science Experience](https://datascience.ibm.com)。
2. 单击**项目** > **查看所有项目**。
3. 单击**设置**选项卡。
4. 要添加服务，请在**关联的服务**面板中，单击**添加关联服务**，选择服务，填写配置信息，然后单击**保存**。
5. 要添加访问令牌，请执行以下步骤：
   6. 在**访问令牌**面板中，单击**新建令牌**。
   7. 在**名称**框中，输入名称。
   8. 在**项目的访问角色**框中，选择角色（**查看者**或**编辑者**）。
   9. 单击**添加**。

6. 要连接到 GitHub 存储库，请在**存储库 URL** 中，输入存储库位置。如果尚未设置用于 GitHub 访问的个人访问令牌，那么现在必须通过单击**设置**来创建该令牌。完成后，单击**添加**。


## 检索凭证

要在配置页和应用程序中使用 Bluemix 实例、服务和模型，您必须能够将凭证、令牌和评分端点插入到用于处理的代码中。

1. [登录到 Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 滚动到**服务**部分，然后单击要检索其凭证的服务名称。
3. 在导航窗格中，单击**服务凭证**。
4. 在凭证列表中，单击**查看凭证**。
5. 如果不存在此服务的凭证，那么可以通过单击**创建凭证**进行创建。
6. 要复制凭证，请单击“复制”图标。

根据服务不同，可能有不同的字段，例如 `username` 和 `password`。必须复制所显示的值以用于代码。

## 了解更多信息

[快速深度学习教程](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

