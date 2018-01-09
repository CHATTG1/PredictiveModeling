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

# 使用服务

通过使用 IBM® SPSS® Modeler 建模选用板上提供的这些建模方法，可以从数据派生新信息和开发预测模型。每种方法各有所长，可针对特定类型的机器学习问题，选择最适用的方法。
{: .shortdesc}

有关 IBM® SPSS® Modeler 及其提供的建模算法的详细信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

在实现了 {{site.data.keyword.Bluemix_notm}} 应用程序和 IBM® SPSS® Modeler 评分分支设计的输入和输出需求后，数据分析人员可以更改评分分支的任何内部方面。数据分析人员甚至可以更改刷新操作中使用的模型算法，以确保您能够优化预测性分析，而无需重写应用程序。


## 将服务与 {{site.data.keyword.Bluemix_notm}} 应用程序绑定的步骤

要创建 {{site.data.keyword.Bluemix_notm}} 应用程序并将其绑定到 {{site.data.keyword.pm_short}} 服务，请完成以下步骤。

1. 从 [GitHub 存储库](https://github.com/pmservice/customer-satisfaction-prediction)下载 Node.js 样本应用程序代码。

2. 使用 `cf create-service` 命令创建服务实例：

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例如：

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   此命令在您的 {{site.data.keyword.Bluemix_notm}} 空间中使用名为 my_pm_lite 的轻量套餐创建一个 {{site.data.keyword.pm_short}} 服务实例：

3. 使用 `cf create-service-key` 命令创建服务凭证：

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   例如：

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   此命令创建 {{site.data.keyword.pm_short}} 服务凭证。

4. 使用 `cf bind-service` 命令将服务实例 `my_pm_lite` 绑定到应用程序。

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   例如：

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   此命令将 {{site.data.keyword.pm_short}} 服务实例 `my_pm_lite` 绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序 my_app1。

5. {{site.data.keyword.pm_short}} 凭证：

   将 {{site.data.keyword.pm_short}} 服务实例绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序后，系统会将 {{site.data.keyword.pm_short}} 凭证添加到 `VCAP_SERVICES` 环境变量中：

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

   `VCAP_SERVICES` 环境变量包含以下信息：

   <dl>

   <dt>plan</dt>
   <dd>服务供应中使用的 {{site.data.keyword.pm_short}} 套餐。</dd>

   <dt>url</dt>
   <dd>{{site.data.keyword.pm_short}} 服务实例的地址。</dd>

   <dt>access_key</dt>
   <dd>要在对此服务实例的所有请求中传递的查询参数 accessKey。</dd>

   </dl>

例如：             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   以下示例 Node.js 代码显示如何从 `VCAP_SERVICES` 环境变量获取 accessKey：

```
if (process.env.VCAP_SERVICES) {
var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
