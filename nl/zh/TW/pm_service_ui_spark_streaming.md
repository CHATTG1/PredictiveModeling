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

# 部署串流模型

您可以使用 {{site.data.keyword.pm_full}} 服務來部署模型，並且針對已部署串流模型提出評分要求來產生預測分析。
{: shortdesc}

**情境名稱**：觀感分析。

**情境說明**：行銷機構想要瞭解有關特定主題的觀感。該機構想要我們開發出一種將表達方式分類為 POSITIVE 或 NEGATIVE 的模型。資料科學家準備了一套預測模型，並將其與您（開發人員）分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測分析模型。

如需相關資訊，請參閱此[文件](https://github.com/pmservice/tweet-sentiment-prediction)。

## 必要條件

若要使用此範例，您需要下列資源：

* [Message Hub](https://console.bluemix.net/catalog/services/message-hub) 主題詳細資料，用來作為模型的輸入（推文），以及模型輸出（預測結果）儲存空間。請確定已建立兩個主題：具有推文及輸出主題的輸入。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服務實例認證。請使用[此鏈結](https://console.bluemix.net/catalog/services/apache-spark)予以建立。


## 使用範例模型

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**範例**標籤。
2. 在**範例模型**區段中，尋找**觀感預測**磚，然後按一下「新增模型」圖示 (+)。

「觀感預測」範例模型即會出現在「模型」標籤的可用模型清單中。


## 使用 IBM Message Hub 來建立串流部署

1.  移至「{{site.data.keyword.pm_full}} 儀表板」的**模型**標籤。
2.  從**動作**功能表選取**建立部署**。
3.  在**建立部署**表單中，完成**名稱**、**說明**及**串流類型**欄位。
4.  您必須提供下列輸入：

    **輸入連線**：IBM Message Hub 主題詳細資料，用來作為模型的輸入（推文），以及模型輸出（預測結果）儲存空間。

    ```
  {
     "connection":{
      "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
      ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
   "source":{
      "topic":"sinput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **輸出連線**

    ```
 {
    "connection":{
      "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
      ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
   "target":{
      "topic":"soutput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **Spark 連線**：Spark 服務認證可以在 {{site.data.keyword.Bluemix_notm}} Spark 服務儀表板的「服務認證」標籤上找到。

     ```
    {
         "credentials":{
      "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
            "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
            "cluster_master_url": "https://spark.bluemix.net",
            "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
            "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
            "plan": "ibm.SparkService.PayGoPersonal"
           },
          "version":"2.0"
       }
    ```
     {: codeblock}

5. 按一下**儲存**。

預測結果會傳送至已定義的 MessageHub 主題（輸出連線）。

## 取得部署詳細資料

您可以檢查與已部署模型相關的狀態和參數。

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**部署**標籤。
2. 從**動作**功能表選取**檢視詳細資料**。

## 刪除串流部署

您可以執行查詢來刪除不再需要的部署，如下列範例所示。

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**部署**標籤。
2. 從**動作**功能表中，按一下**刪除**。

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
