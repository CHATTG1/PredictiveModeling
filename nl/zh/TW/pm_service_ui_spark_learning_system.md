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

# 持續學習系統

{{site.data.keyword.pm_full}} 持續學習系統提供模型效能的自動監視、重新訓練及重新部署，以確保預測品質。
{: shortdesc}

**情境名稱**：心臟治療的最佳藥物選擇。

**情境說明**：生產心臟藥物的生醫公司想要部署一個選擇心臟治療最佳藥物的模型。當藥物有效性出現新的證明時，應考慮進行模型更新。資料科學家準備了一套預測模型，並將其與您（開發人員）分享。您的作業是部署模型、設定學習配置，並且執行學習反覆運算，以評估、重新訓練及重新部署模型。

## 必要條件

若要使用此範例，您需要下列服務：

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)，用來儲存回饋資料
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服務

   如果您還沒有 Apache Spark 實例，請使用[此鏈結](https://console.bluemix.net/catalog/services/apache-spark)來建立 Apache Spark 實例。

## 使用範例模型

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**範例**標籤。
2. 在**範例模型**區段中，尋找**心臟藥物選擇**磚，然後按一下**新增模型**圖示 (+)。

範例「心臟藥物選擇」模型即會出現在**模型**標籤的可用模型清單中。


## 針對已發佈的模型設定持續學習系統

1.  移至「{{site.data.keyword.pm_full}} 儀表板」的**模型**標籤。
2.  從**動作**功能表選取**檢視詳細資料**。
3.  向下捲動至**重新訓練詳細資料**，然後按下**編輯**。
4.  您必須提供下列輸入：

    **回饋連線**：{{site.data.keyword.dashdbshort}} 詳細資料，用來作為進行模型評估及重新訓練的輸入（回饋資料）。

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
        "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    請使用下列指示，在 {{site.data.keyword.dashdbshort}} 中準備 `DRUG_FEEDBACK_DATA` 表格。
    
    - 建立 [{{site.data.keyword.dashdbshort}} 服務](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)實例（已提供入門方案）。
    - 在 **{{site.data.keyword.dashdbshort_notm}}** 中建立 **DRUG_FEEDBACK_DATA** 表格。
      + 從 GitHub 儲存庫中，下載 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 檔案。
      + 按一下**開啟主控台**，以開始使用 **{{site.data.keyword.dashdbshort_notm}}**。
      + 按一下**載入資料**，然後選取**桌面**載入類型。
      + 將先前下載的檔案**拖曳**至載入區域，然後按**下一步**。
      + 選取**綱目**以匯入資料，然後按一下**新建表格**。
      + 鍵入新表格的名稱，然後按**下一步**。
      + 使用分號 (;) 作為**欄位分隔字元**。
      + 按**下一步**，以建立具有上傳資料的表格。

     **附註**：您可以使用 [REST API 端點](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)，將新的回饋記錄新增至回饋資料庫。

     **資料大小下限**：回饋資料集中的記錄數下限，以開始評估模型（學習系統反覆運算的一部分）。

     ```
     20
     ```
     {: codeblock}

     **Spark 連線**：Spark 服務認證可以在 {{site.data.keyword.Bluemix_notm}} Spark 服務儀表板的**服務認證**標籤上找到。

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

     **臨界值**：觸發重新訓練程序的臨界值。如果度量值（例如，在模型評估期間計算的精確度值）小於臨界值，則會觸發重新訓練。

     ```
     0.8
     ```

     **自動部署重新訓練模型**：如果重新訓練模型的度量值比已部署的模型好，則會部署重新訓練模型。

5.  按一下**設定**。

## 執行持續學習系統反覆運算

已發佈的模型會以反覆運算方式進行評估。如果度量值（例如，在模型評估期間計算的精確度值）小於臨界值，則會觸發重新訓練。

     兩個資料集（訓練及回饋）都用於這個反覆運算的重新訓練及評估程序。

1. 若要開始新的反覆運算，請從模型**詳細資料**表單，在**監視**標籤上按一下**評估**。
3. 若要檢查反覆運算結果，請移至「評估事件」表格或「圖表」視圖。 

   您可以檢視根據回饋資料（監視）計算之模型品質的相關資訊。您也可以檢視重新訓練模型品質的相關資訊，這是所建立及評估的新模型版本。

4. 若要查看自動訓練模型及其品質的清單，請按一下**檢視詳細資料** > **概觀**，然後移至**版本歷程**區段。

## 回饋資料儲存庫

您可以使用[回饋端點](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)，將新記錄傳送至您在機器學習配置中所定義的回饋儲存庫。{{site.data.keyword.dashdbshort}} 目前支援作為回饋資料儲存庫。如果回饋表格不存在，則服務會予以建立。如果表格已存在，則會驗證綱目符合訓練表格的綱目，並將名為 `_training` 的額外直欄附加至表格。`_training` 直欄是用來標示在重新訓練程序期間所耗用的記錄。

如需回饋資料儲存庫的相關資訊及範例，請參閱 [API 文件](pm_service_api_spark_learning_system.html)。

**附註：**{{site.data.keyword.pm_full}} 服務會管理、修改及使用回饋表格。

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
