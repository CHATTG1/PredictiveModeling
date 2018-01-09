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

{{site.data.keyword.pm_full}} 服務包括持續學習系統。持續學習系統提供模型效能的自動監視、重新訓練及重新部署，以確保預測品質。
{: shortdesc}

**情境名稱**：心臟治療的最佳藥物選擇。

**情境說明**：生產心臟藥物的生醫公司想要部署一個選擇心臟治療最佳藥物的模型。當藥物有效性出現新的證明時，應考慮進行模型更新。資料科學家準備了一套預測模型，並將其與您（開發人員）分享。您的作業是部署模型、設定學習配置，並且執行學習反覆運算，以評估、重新訓練及重新部署模型。

**附註**：您也可以試用[範例 Python 記事本](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325)，它會建立範例模型、配置學習系統，並且執行學習反覆運算。

## 必要條件

若要使用範例，您必須具有下列服務：

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)，用來儲存回饋資料
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服務實例認證。您可以使用[此鏈結](https://console.bluemix.net/catalog/services/apache-spark)予以建立。

## 使用範例模型

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**範例**標籤。
2. 在**範例模型**區段中，尋找「心臟藥物選擇」磚，然後按一下**新增模型**圖示 (+)。

範例**心臟藥物選擇**模型即會出現在**模型**標籤的可用模型清單中。

## 產生存取記號

使用 {{site.data.keyword.pm_full}} 服務實例的「服務認證」標籤中所提供的使用者和密碼來產生存取記號。

要求範例：

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

輸出範例：

```
{"token":"**********"}
```
{: codeblock}

請使用下列終端機指令來指派記號值給 token 環境變數：


```
token="<token_value>"
```
{: codeblock}

## 使用已發佈的模型

使用下列 API 呼叫來取得您的實例詳細資料，例如：

* 已發佈模型 `url` 位址
* 部署 `url` 位址
* 用量資訊

要求範例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

輸出範例：

```
{
   "metadata":{
      "guid":"360c510b-012c-4793-ae3f-063410081c3e",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e",
      "created_at":"2017-08-04T09:15:48.344Z",
      "modified_at":"2017-08-22T08:34:47.759Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models"
      },
      "usage":{
         "expiration_date":"2017-09-01T00:00:00.000Z",
         "computation_time":{
            "limit":18000,
            "current":4
         },
         "model_count":{
            "limit":200,
            "current":2
         },
         "prediction_count":{
            "limit":5000,
            "current":16
         },
         "deployment_count":{
            "limit":5,
            "current":1
         }
      },
      "plan_id":"0f2a3c2c-456b-40f3-9b19-726d2740b11c",
      "status":"Active",
      "organization_guid":"b0e61605-a82e-4f03-9e9f-2767973c084d",
      "region":"us-south",
      "account":{
         "id":"f52968f3dbbe7b0b53e15743d45e5e90",
         "name":"Umit Cakmak's Account",
         "type":"TRIAL"
      },
      "owner":{
         "ibm_id":"31000292EV",
         "email":"umit.cakmak@pl.ibm.com",
         "user_id":"43e0ee0e-6bfb-48fc-bcd8-d61e40d19253",
         "country_code":"POL",
         "beta_user":true
      },
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/deployments"
      },
      "space_guid":"4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "plan":"standard"
   }
}
```
{: codeblock}

取得 **published_models** `url` 位址之後，請使用下列 API 呼叫來取得模型的詳細資料：

要求範例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

輸出範例：

```
{  
   "count":1,
   "resources":[  
      {  
         "metadata":{
                        "guid":"361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "created_at":"2017-08-22T08:34:47.597Z",
            "modified_at":"2017-08-22T08:34:47.691Z"
         },
         "entity":{
            "runtime_environment":"spark-2.0",
            "author":{  
               "name":"IBM",
            "email":""
         },
            "name":"Best Heart Drug Selection",
            "label_col":"DRUG",
            "training_data_schema":{  
               "fields":[
                  {
                     "metadata":{
                        "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"DRUG"
                     },
                     "type":"string",
                     "name":"DRUG",
                     "nullable":true
                  }
               ],
               "type":"struct"
            },
            "latest_version":{  
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "guid":"8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "created_at":"2017-08-22T08:34:47.691Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{  
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/deployments"
            },
            "input_data_schema":{  
               "fields":[
                  {
                     "metadata":{
                        "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{
                        "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  }
               ],
               "type":"struct"
            }
         }
      }
}
```
{: codeblock}


## 針對已發佈的模型設定持續學習系統

若要配置模型的持續學習系統，您必須執行下列作業：

1.  [準備授權標頭](#prepare-the-authorization-header)。
2.  [準備回饋資料集](#prepare-the-feedback-data-set)。
3.  [準備配置有效負載](#prepare-the-configuration-payload)。

完成授權標頭、回饋資料集及配置有效負載的準備之後，即可開始反覆運算持續學習系統。如需相關資訊，請參閱[執行持續學習系統反覆運算](#run-continuous-learning-system-iteration)。

### 準備授權標頭

若要準備結合 {{site.data.keyword.pm_full}} 記號與 Spark 實例認證的授權標頭，請提供下列詳細資料：

*  在前一個步驟中建立的存取記號
*  Spark 服務認證（可以在 {{site.data.keyword.Bluemix_notm}} Spark 服務儀表板的「服務認證」標籤上找到）。在提出部署要求之前，必須先將 Spark 認證編碼為 base64。它們會在 X-Spark-Service-Instance 欄位中的要求標頭中傳遞。

   視您正在使用的作業系統而定，您必須發出下列其中一個終端機指令來執行 base64 編碼，並將其指派至環境變數。

   在 **macOS** 作業系統上，請使用下列指令：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net", "instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   在 **Microsoft Windows** 或 **Linux** 作業系統上，您必須搭配使用 `--wrap=0` 參數與 `base64` 指令來執行 base64 編碼：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### 準備回饋資料集

學習系統需要與訓練資料（用於模型訓練的資料）以及回饋資料的連線。回饋資料儲存庫是用來監視及重新訓練您的模型（必要時）。{{site.data.keyword.dashdbshort}} 支援作為回饋資料儲存庫。{{site.data.keyword.pm_short}} 服務會管理、修改及使用回饋表格。
若要在 **{{site.data.keyword.dashdbshort}}** 中準備 **DRUG_FEEDBACK_DATA** 表格，您必須完成下列步驟：

1. 建立 [{{site.data.keyword.dashdbshort}} 服務](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)實例（已提供入門方案）。
2. 在 **{{site.data.keyword.dashdbshort}}** 中建立 `DRUG_FEEDBACK_DATA` 表格。
   1. 從 GitHub 儲存庫中，下載 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 檔案。
   2. 若要開始使用 **{{site.data.keyword.dashdbshort_notm}}**，請按一下**開啟主控台**圖示。
   3. 選取**載入資料**及**桌面**載入類型。
   4. **拖曳**先前下載的檔案，然後按**下一步**。
   5. 若要匯入資料，請按一下**綱目**，然後按一下**新建表格**。
   6. 在**新建表格**欄位中輸入名稱，然後按**下一步**。
   7. 針對**欄位分隔字元**使用分號 (;)。
   8. 按**下一步**以建立具有上傳資料的表格。

**附註**：您可以使用此 [REST API 端點](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)，將新的回饋記錄新增至回饋資料庫。如需相關資訊，請參閱[回饋資料儲存庫](#feedback-data-store)小節。

### 準備配置有效負載

若要指定學習配置，您必須使用下列端點：

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

若要終結有效負載，您必須定義下列參數的值：

<dl><dt>feedback_data_reference</dt>
<dd>回饋資料儲存庫的連線及來源</dd>
<dt>min_feedback_data_size</dt>
<dd>回饋資料集中的記錄數下限，以開始持續學習系統反覆運算
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] 指定何時觸發重新訓練程序。設為 [conditionally] 時，如果模型品質小於指定的臨界值，則會觸發重新訓練程序。
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] 指定何時應該部署重新訓練模型。設為 [conditionally] 時，若新訓練模型品質優於目前已部署模型的品質，則會觸發模型重新部署。</dd></dl>

要求範例：

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
          "definition": {
            "method": "multiclass",
           "metrics": [
             {
               "name": "accuracy",
               "threshold": 0.8
             }
           ]
         },
         "feedback_data_reference": {
           "connection": {
            "db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
           },
           "source": {
            "tablename": "DRUG_FEEDBACK_DATA",
            "type": "dashdb"
           }
          },
          "min_feedback_data_size": 10,
          "auto_retrain": "conditionally",
          "auto_redeploy": "never",
          "last_training_record": 0
        }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

輸出範例：

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass",
    "metrics": [
      {
        "name": "accuracy",
        "threshold": 0.8
      }
    ]
   },
   "feedback_data_reference":{
      "connection":{
         "db":"BLUDB",
         "host":"awh-yp-small02.services.dal.bluemix.net",
         "username":"***",
         "password":"***"
      },
      "source":{
         "tablename":"DRUG_FEEDBACK_DATA",
         "type":"dashdb"
      }
   },
   "min_feedback_data_size":10,
   "spark_service":{
      "credentials":{
         "tenant_id":"***",
         "cluster_master_url":"https://spark.bluemix.net",
         "tenant_id_full":"***",
         "tenant_secret":"***",
         "instance_id":"***",
         "plan":"ibm.SparkService.PayGoPersonal"
      },
      "version":"2.0"
   },
   "training_data_reference": {
   "connection": {
            "db": "BLUDB",
     "host": "dashdb-entry-yp-dal09-08.services.dal.bluemix.net",
     "password": "***",
     "username": "***"
   },
           "source": {
            "tablename": "DRUG_TRAIN_DATA_UPDATED",
     "type": "dashdb"
    }
   }
}
```
{: codeblock}

**附註**：此範例使用 `auto_retrain` 及 `auto_redeploy` 參數的預設值。如需這些參數的相關資訊，請參閱 [REST API 文件](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration)以尋找 `learning_configuration` 參數。

## 執行持續學習系統反覆運算

若要啟動學習系統的反覆運算，請使用下列 REST API 方法。在反覆運算期間，評估已發佈的模型。如果評估的精確度小於指定的臨界值，則會觸發模型重新訓練。訓練及回饋資料集都會用於重新訓練與評估。

要求範例：

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

輸出範例：

```
The request has been fulfilled and resulted in a new resource being created.
```
{: codeblock}

若要檢查反覆運算的狀態，請發出下列要求：

要求範例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

輸出範例：

```
{
  "count": 1,
  "resources": [
    {
      "entity":{
"status": {
          "state": "INITIALIZED"
        },
        "model_version": {
          "url": "https://ibm-watson-ml-svt.stage1.mybluemix.net/v2/artifacts/models/71dc688a-ebda-4174-9574-e8805059e08f/versions/de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d",
          "created_at": "2017-10-30T15:45:12.926Z",
          "guid": "de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d"
        },
   "spark_service":{
      "credentials": {
           "tenant_id_full": "***",
            "tenant_secret": "***",
            "tenant_id": "***",
            "instance_id": "***",
            "plan": "ibm.SparkService.PayGoPersonal",
            "cluster_master_url": "https://spark.bluemix.net"
          },
      "version":"2.0"
   },
        "published_model": {
          "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f",
          "guid": "71dc688a-ebda-4174-9574-e8805059e08f"
        }
      },
      "metadata": {
        "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f/learning_iterations/a308838b-445f-45b8-9fbf-1c3dd1b392c1",
        "created_at": "2017-10-30T15:54:30.657Z",
        "guid": "a308838b-445f-45b8-9fbf-1c3dd1b392c1"
      }
    }
  ]
}
```
{: codeblock}

您也可以發出下列要求，取得評估度量及值的清單：

要求範例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

輸出範例：

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-07T10:11:02.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
      {
      "phase": "monitoring",
      "values": [
        {
          "name": "accuracy",
          "value": 0.75,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    },    
    {
      "phase": "training",
      "values": [
        {
          "name": "accuracy",
          "value": 0.88281694,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:22.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    }
  ]
}
```
{: codeblock}

## 回饋資料儲存庫

您可以使用[回饋端點](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)，將新記錄（未用於訓練程序的記錄）傳送至學習配置中所定義的回饋儲存庫。回饋資料儲存庫是用來監視及重新訓練您的模型（必要時）。{{site.data.keyword.dashdbshort}} 支援作為回饋資料儲存庫。如果回饋表格不存在，則服務會予以建立。如果表格已存在，則會驗證綱目符合訓練表格的綱目，並將名為 `_training` 的額外直欄附加至表格。其他直欄是用來標示在重新訓練程序中所耗用的記錄。

**附註：**{{site.data.keyword.pm_short}} 服務會管理、修改及使用回饋表格。

您可以發出下列要求，將新的回饋記錄傳送至回饋資料儲存庫：

要求範例：

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields":[
"AGE",
      "SEX",
      "BP",
      "CHOLESTEROL",
      "NA",
      "K",
      "DRUG"
   ],
   "values":[
      [
         16,
         "M",
         "HIGH",
         "NORMAL",
         0.58301000000000003,
         0.033884999999999998,
         "drugY"
      ]
   ]
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/feedback
```
{: codeblock}

## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
