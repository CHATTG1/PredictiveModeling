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

# 配额策略

{{site.data.keyword.pm_full}} 引擎按实例强制实施配额，以确保相应的资源分配和使用量。这些策略因帐户概要文件、历史服务使用情况和资源可用性而异。配额策略描述了定价套餐未定义的限制**，可随时更改而不另行通知**。当前有效的服务配额限制如下。
{: shortdesc}

## 资源使用情况

资源最大值限制如下：

-  **最大已发布的模型数**：200（轻量套餐），1000（标准和专业套餐）
-  **最大 Spark ML 模型大小**：200 MB
-  **最大已部署的模型数**：1000（标准和专业套餐）

**注**：要了解轻量套餐限制，请参阅 [{{site.data.keyword.Bluemix_notm}} 目录](https://console.bluemix.net/catalog/)中关于[轻量套餐](https://console.bluemix.net/catalog/services/machine-learning)的描述。

## 服务请求限制

您每分钟可以对特定 API 发出的请求数是有限的。如果应用程序需要更高的限制，那么您必须[联系 {{site.data.keyword.Bluemix_notm}} 支持](https://support.ng.bluemix.net/)来增加配额。

## 部署请求

以下限制适用于部署 API 请求：

-  **周期**：60 秒
-  **缺省限制**：10

## 联机预测请求

以下限制适用于[联机预测](pm_service_api_spark_building.html)请求：

-  **周期**：60 秒
-  **缺省限制**：500

## 资源管理请求

以下限制适用于此列表中的组合总请求数：

[令牌](https://watson-ml-api.mybluemix.net/#/Token)：post、get、delete、put 和 patch 请求

[部署](https://watson-ml-api.mybluemix.net/#/Deployments)：post、get、delete、put 和 patch 请求

-  **周期**：60 秒
-  **缺省限制**：100

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM® Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
