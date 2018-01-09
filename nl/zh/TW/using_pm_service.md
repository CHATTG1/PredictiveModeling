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

# 使用此服務

使用 IBM® SPSS® Modeler 建模選用區上的建模方法，即可從資料中衍生新資訊，以及開發預測模型。每一種方法都有特定強度，最適合特定類型的機器學習問題。
{: .shortdesc}

如需 IBM® SPSS® Modeler 及其提供之建模演算法的詳細資料，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

在實作 {{site.data.keyword.Bluemix_notm}} 應用程式及 IBM® SPSS® Modeler 評分分支設計的輸入及輸出需求之後，「資料分析師」可以變更評分分支的任何內部層面。「資料分析師」甚至可以變更某個重新整理作業中使用的模型演算法，確保您有能力細部調整預測分析，而不需要重寫應用程式。


## 連結服務與 {{site.data.keyword.Bluemix_notm}} 應用程式的步驟

請完成下列步驟來建立 {{site.data.keyword.Bluemix_notm}} 應用程式，並將它連結至 {{site.data.keyword.pm_short}} 服務。

1. 從 [GitHub 儲存庫](https://github.com/pmservice/customer-satisfaction-prediction)下載 Node.js 範例應用程式碼。

2. 使用 `cf create-service` 指令來建立服務實例：

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例如：

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   此指令會在 {{site.data.keyword.Bluemix_notm}} 空間中建立一個具有精簡方案 named my_pm_lite 的 {{site.data.keyword.pm_short}} 服務實例。

3. 使用 `cf create-service-key` 指令來建立服務認證：

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   例如：

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   這個指令會建立 {{site.data.keyword.pm_short}} 服務認證。

4. 使用 `cf bind-service` 指令，將服務實例 `my_pm_lite` 連結至應用程式。

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   例如：

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   此指令會將 {{site.data.keyword.pm_short}} 服務實例 `my_pm_lite` 連結至 {{site.data.keyword.Bluemix_notm}} 應用程式 my_app1。

5. {{site.data.keyword.pm_short}} 認證：

   將 {{site.data.keyword.pm_short}} 服務實例連結至 {{site.data.keyword.Bluemix_notm}} 應用程式之後，{{site.data.keyword.pm_short}} 認證即會新增至 `VCAP_SERVICES` 環境變數：

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

   `VCAP_SERVICES` 環境變數包括下列資訊：

   <dl>

   <dt>plan</dt>
   <dd>使用於服務供應的 {{site.data.keyword.pm_short}} 方案。</dd>

   <dt>url</dt>
   <dd>{{site.data.keyword.pm_short}} 服務實例的位址。</dd>

   <dt>access_key</dt>
   <dd>查詢參數 accessKey，它能將所有要求傳入這個服務實例。</dd>

   </dl>

例如：             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   下列範例 Node.js 程式碼顯示如何從 `VCAP_SERVICES` 環境變數取得 accessKey：

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
