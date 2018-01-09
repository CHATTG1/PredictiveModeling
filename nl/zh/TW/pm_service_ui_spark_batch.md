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

# 部署批次模型

使用 {{site.data.keyword.pm_full}} 服務，您可以部署模型，並且針對已部署模型提出評分要求來產生預測分析。
{: shortdesc}


**情境名稱**：客戶滿意度預測。

**情境說明**：電信公司想要知道哪些客戶有離開的風險。呈現的模型預測客戶流失。資料科學家開發出一套預測模型，並將其與您（開發人員）分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測分析模型。

## 必要條件

若要使用此範例，您需要下列服務：

* [Object Storage](https://console.bluemix.net/catalog/services/object-storage) 實例詳細資料，用來作為模型的輸入（要評分的客戶資料），以及模型輸出的儲存空間。在[這裡](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv)，下載範例輸入資料 .csv 檔案。您應該將輸入檔新增至 Object Storage 實例。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服務實例認證。您可以使用[此鏈結](https://console.bluemix.net/catalog/services/apache-spark)進行建立。


## 使用範例模型

1.  移至「{{site.data.keyword.pm_full}} 儀表板」的「範例」標籤。
2.  在「範例模型」區段中，尋找「客戶滿意度預測」磚，然後按一下「新增模型」圖示 (+)。

範例「客戶滿意度預測」模型會出現在「模型」標籤的可用模型清單中。

## 使用 Object Storage 來建立批次部署

1.  移至「{{site.data.keyword.pm_full}} 儀表板」的「模型」標籤。
2.  從**動作**功能表選取**建立部署**。
3.  在「建立部署」表單中，提供「名稱」、「說明」及「批次類型」。
4.  您必須提供下列輸入：

    **輸入連線**：Object Storage 詳細資料，用來作為模型的輸入（要評分的客戶資料），以及模型輸出的儲存空間（在此案例中為 results.csv，這是自動建立的檔案）。

    ```
       {
          "source":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"TelcoCustomerData.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **輸出連線**

    ```
       {
          "target":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"result.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Spark 連線**：Spark 服務認證可以在 {{site.data.keyword.Bluemix_short}} Spark 服務儀表板的「服務認證」標籤上找到。

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

5.  按一下**儲存**。

預測結果會儲存至 IBM Object Storage 中的 .csv 檔案。以下是範例列。

輸入檔預覽：

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

輸出檔預覽：

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## 取得部署詳細資料

您可以檢查與已部署模型相關的狀態及參數。

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的「部署」標籤。
2. 從**動作**功能表選取**檢視詳細資料**。

## 刪除批次部署

您可以使用查詢來刪除不再需要的部署，如下列範例所示。

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的「部署」標籤。

2. 從**動作**功能表中，按一下**刪除**。

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
