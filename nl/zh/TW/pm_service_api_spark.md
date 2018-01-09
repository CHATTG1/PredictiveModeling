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

{{site.data.keyword.pm_short}} 服務是一組 REST API，可以從任何程式設計語言呼叫，並允許在應用程式中整合 Data Science Experience 分析。將 {{site.data.keyword.Bluemix_short}} 應用程式連結至 {{site.data.keyword.pm_short}} 服務實例，並產生應用程式需要的預測分析，來為使用者提供更高的價值。您可以在[管理儀表板](pm_service_ui_spark.html)中管理模型，並使用儀表板來建立與應用程式整合的線上、批次或串流部署。
{: shortdesc}

透過功能強大的 [REST API](https://watson-ml-api.mybluemix.net/)，針對服務實例上所部署的 Spark、Python 及 IBM® SPSS® 模型來開發應用程式：

*  針對給定的預測模型來擷取 meta 資料
*  部署模型以及管理所部署的模型
    *  線上部署（評分）
    *  批次部署（支援 IBM Cloud Object Storage 及 {{site.data.keyword.dashdbshort}}）
    *  串流部署（支援 IBM Cloud Message Hub）
*  針對給定的部署來擷取 meta 資料
*  使用持續學習系統來監視及重新訓練已部署模型
*  針對所部署的模型提出評分要求，以產生預測分析

如需相關資訊，請參閱下列 REST API 使用範例：

*  [部署線上模型](pm_service_api_spark_online.html)
*  [評分線上模型](pm_service_api_develop_score.html)
*  [部署批次模型](pm_service_api_spark_batch.html)
*  [部署串流模型](pm_service_api_spark_streaming.html)
*  [持續學習系統](pm_service_api_spark_learning_system.html)

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
