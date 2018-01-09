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

# 设置环境

要使用 {{site.data.keyword.pm_full}}，您必须能够创建适当的机器学习环境并检索特定于该环境的凭证。您可以使用 [Cloud Foundry 命令行界面](https://github.com/cloudfoundry/cli#getting-started)或通过 {{site.data.keyword.Bluemix}}“仪表板”中的图形用户界面来创建 {{site.data.keyword.Bluemix}} 实例。
{: shortdesc}

## 使用 IBM Cloud 仪表板

要使用 {{site.data.keyword.pm_full}}，您必须在 {{site.data.keyword.Bluemix_notm}}“仪表板”中为其[创建实例](https://console.bluemix.net/catalog/services/machine-learning)。

根据场景不同，您可能还需要以下资源：

- Object Storage（批量部署）
- Db2 Warehouse on Cloud（批量部署）
- Apache Spark（批量、流式部署和持续学习系统）
- Message Hub（流式部署）

要查看如何使用“仪表板”来创建 {{site.data.keyword.Bluemix_notm}} 实例并查看凭证，请观看以下视频，这是对视频下面所列文字步骤的补充。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>图 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="如果无法访问此页面中嵌入的视频，可以通过 YouTube Web 站点来访问此视频。（在新选项卡或窗口中打开）">    <img src="images/video.png" alt="“视频”图标"></a>IBM Watson Machine Learning：入门</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>此视频概述了如何设置机器学习环境。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## 创建 {{site.data.keyword.Bluemix_notm}} 实例

要创建 {{site.data.keyword.pm_full}} 实例，必须执行以下步骤：

1. 打开 {{site.data.keyword.pm_full}} [服务页面](https://console.bluemix.net/catalog/services/machine-learning)。
2. 注册或登录以创建服务实例。
3. 输入实例的描述性名称，选择空间，然后选择数据套餐。
4. 单击**创建实例**。

这会打开您的实例。

## 检索凭证

要使用 {{site.data.keyword.Bluemix_notm}}（先前称为 Bluemix）实例，您需要服务实例凭证。可以通过 [CF CLI](using_pm_service.html) 或 {{site.data.keyword.Bluemix_notm}}“仪表板”来创建和访问凭证。将 {{site.data.keyword.pm_short}} 服务实例绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序后，系统会将 {{site.data.keyword.pm_short}} 凭证添加到 `VCAP_SERVICES` 环境变量中。有关更多信息，请参阅[将服务与应用程序一起使用](using_pm_service.html)。

要检索 {{site.data.keyword.pm_full}} 实例凭证，必须执行以下步骤：

1. [登录到 {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 在**服务**部分中，单击要检索其凭证的服务名称。
3. 在导航窗格中，单击**服务凭证**。
4. 在凭证列表中，单击**查看凭证**。
5. 如果不存在此服务的凭证，请通过单击**创建凭证**进行创建。
6. 要复制凭证，请单击“复制”图标。

要在应用程序中使用 {{site.data.keyword.pm_short}} 服务，必须将该服务绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序，并将运行应用程序所需的凭证插入 `VCAP_SERVICES` 环境变量。有关更多信息，请参阅 [Cloud Foundry 命令行界面](#cloud-foundry-command-line-interface)。

## 使用 Cloud Foundry CLI（命令行界面）

### 将服务与 {{site.data.keyword.Bluemix_notm}} 应用程序绑定的步骤

您可以下载 Node.js [样本代码](https://github.com/pmservice/product-line-prediction/blob/master/README.md)来试用 Machine Learning 服务。要创建 {{site.data.keyword.Bluemix_notm}} 应用程序并绑定 {{site.data.keyword.pm_short}} 服务，请完成以下步骤。以下示例使用的是 Node.js，因为这是比较常用的运行时。可以使用其他运行时，例如 iOS、Ruby、Perl 或 Java。

还可以使用 {{site.data.keyword.Bluemix_notm}} 图形界面（而不使用 Cloud Foundry 工具 (cf)）来执行步骤 1-3。

1. 使用 `cf create-service` 命令创建服务实例：

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例如，以下命令在您的 {{site.data.keyword.Bluemix_notm}} 空间中使用名为 `my_wmllte` 的轻量套餐创建一个 {{site.data.keyword.pm_short}} 服务实例：

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. 使用 `cf create-service-key` 命令创建服务凭证：

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   例如，以下命令将创建 {{site.data.keyword.pm_short}} 服务凭证：

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. 使用 `cf bind-service` 命令将服务实例 `my_wmllte` 绑定到应用程序。

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   例如，以下命令将 {{site.data.keyword.pm_short}} 服务实例 `my_wmlLite` 绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序 `my_app1`：

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### 通过 `VCAP_SERVICES` 变量访问凭证

将 {{site.data.keyword.pm_short}} 服务实例绑定到 {{site.data.keyword.Bluemix}} 应用程序后，系统会将 {{site.data.keyword.pm_short}} 凭证添加到 `VCAP_SERVICES` 环境变量中：

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

   `VCAP_SERVICES` 环境变量包含以下信息：

<dl>
<dt>plan</dt>
<dd>服务供应中使用的 {{site.data.keyword.pm_short}} 套餐。</dd>
<dt>url</dt><dd>{{site.data.keyword.pm_short}} 服务实例的地址。<dt>access_key</dt><dd>仅限 IBM® SPSS® Modeler 流。</dd>
<dt>instance_id</dt><dd>{{site.data.keyword.pm_short}} 实例唯一标识。</dd>
<dt>username</dt><dd>用户名，这是生成访问令牌（用于在对此服务实例的所有请求中传递）所需的基本授权的组成部分。</dd>
<dt>password</dt><dd>密码，这是生成访问令牌（用于在对此服务实例的所有请求中传递）所需的基本授权的组成部分。</dd>
</dl>

例如，可以使用以下 `curl` 命令生成访问令牌：

请求示例：

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       输出示例：

       {"token":"**********"}
```
{: codeblock}

   以下 Node.js 代码是有关如何从 `VCAP_SERVICES` 环境变量获取 `username` 和 `password` 值的示例：

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
