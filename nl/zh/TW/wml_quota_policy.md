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

# 配額原則

{{site.data.keyword.pm_full}} 引擎會對每個實例施行配額，以確保適當的資源配置及使用。這些原則會根據帳戶設定檔、歷程服務用量及資源的可用性而變。配額原則說明定價方案未定義的限制，且**會視情況變更，恕不另行通知**。下列服務配額限制目前在作用中。
{: shortdesc}

## 資源用量

資源受限於下列最大值：

-  **已發佈模型數上限**：200（精簡方案）1000（標準及專業方案）
-  **Spark ML 模型大小上限**：200 MB
-  **已部署模型數上限**：1000（標準及專業方案）

**附註**：若要瞭解「精簡方案」限制，請在 [{{site.data.keyword.Bluemix_notm}} 型錄](https://console.bluemix.net/catalog/)中參閱[精簡方案](https://console.bluemix.net/catalog/services/machine-learning)的說明。

## 服務要求限制

您受限於每分鐘可對特定 API 提出的要求數目。如果您的應用程式需要較高的限制，您必須[與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](https://support.ng.bluemix.net/)，以增加您的配額。

## 部署要求

下列限制適用於部署 API 要求：

-  **期間**：60 秒
-  **預設限制**：10

## 線上預測要求

下列限制適用於[線上預測](pm_service_api_spark_building.html)要求：

-  **期間**：60 秒
-  **預設限制**：500

## 資源管理要求

下列限制適用於此清單中的結合要求總數：

[記號](https://watson-ml-api.mybluemix.net/#/Token)：post、get、delete、put、patch 要求

[部署](https://watson-ml-api.mybluemix.net/#/Deployments)：post、get、delete、put、patch 要求

-  **期間**：60 秒
-  **預設限制**：100

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM® Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
