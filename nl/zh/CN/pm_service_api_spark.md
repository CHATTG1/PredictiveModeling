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

# REST API

{{site.data.keyword.pm_short}} 服务是可通过任何编程语言调用的一组 REST API，支持将 Data Science Experience 分析集成到应用程序中。将 {{site.data.keyword.Bluemix_short}} 应用程序绑定到 {{site.data.keyword.pm_short}} 服务实例，然后生成预测性分析，您的应用程序需要这些分析来为用户提供更高的价值。在[管理仪表板](pm_service_ui_spark.html)中管理模型，并使用该仪表板来创建与应用程序集成的联机、批量或流式部署。
{: shortdesc}

基于通过强大的 [REST API](https://watson-ml-api.mybluemix.net/) 部署在服务实例上的 Spark、Python 和 IBM® SPSS® 模型开发应用程序：

*  检索给定预测模型的元数据
*  部署模型和管理已部署的模型
    *  联机部署（评分）
    *  批量部署（支持 IBM Cloud Object Storage 和 {{site.data.keyword.dashdbshort}}）
    *  流式部署（支持 IBM Cloud Message Hub）
*  检索给定部署的元数据
*  使用持续学习系统监视和重新培训已部署的模型
*  通过对已部署模型发出评分请求来生成预测性分析

有关更多信息，请参阅以下 REST API 用法示例：

*  [部署联机模型](pm_service_api_spark_online.html)
*  [联机模型评分](pm_service_api_develop_score.html)
*  [部署批处理模型](pm_service_api_spark_batch.html)
*  [部署流式模型](pm_service_api_spark_streaming.html)
*  [持续学习系统](pm_service_api_spark_learning_system.html)

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
