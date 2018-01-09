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

# 自訂模型的開發和持續性

使用 {{site.data.keyword.pm_full}} 服務，您可以部署模型，並且針對已部署模型提出評分要求來產生預測分析。
{: shortdesc}

## 使用自訂模型

### 情境名稱：產品線預測。

### 情境說明：

我們的客戶經營歐洲最著名的連鎖店之一。他們想要我們依據其產品線（例如個人配件、露營設備和戶外防護器材），找出其客戶的興趣所在。
資料科學家開發出一套預測模型，並將其與您（開發人員）分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測分析模型。

### 自訂模型的開發和持續性

#### 使用 Data Science Experience

使用 [Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience) 可建立自訂模型。註冊之後，您必須登入，才能完成下列步驟。

1. 建立組織和空間。您第一次登入時，必須提供此資訊。請按一下**繼續**，以接受預設值。
2. 建立組織之後，移至**專案**，然後按一下**新建專案**。
3. 指定專案的名稱和說明，然後按一下**建立**。您指定的專案名稱是用來作為「目標容器」名稱。
4. 建立專案之後，您可以執行下列其中一項作業：
   
   *  **新增記事本**，並開始開發自己的模型，或上傳下列其中一個範例記事本：
        *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **新增模型**，並開始使用模型精靈來開發自己的模型。


#### 使用本端環境

您也可以使用您選擇的環境來開發模型，稍後再使用 pypi 上可用的 Watson Machine Learning [一般 API 用戶端程式庫]()進行發佈、部署及評分。
如需用戶端程式庫的相關資訊，請參閱範例[記事本](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125)及[文件](pm_service_client_library.html)。

**附註：**一般 API 用戶端程式庫為**測試版**。

### 自訂模型的部署和評分

您可以使用 API 來部署及評分線上、批次及串流模型。

*  [部署線上模型](pm_service_api_spark_online.html)
*  [部署批次模型](pm_service_api_spark_batch.html)
*  [部署串流模型](pm_service_api_spark_streaming.html)

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM® Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
