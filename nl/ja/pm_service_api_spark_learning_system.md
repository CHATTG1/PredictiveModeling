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

# 継続学習システム

{{site.data.keyword.pm_full}} サービスには継続学習システムが組み込まれています。継続学習システムは、予測品質を確保するため、モデル・パフォーマンスの自動モニタリング、リトレーニング、および再デプロイメントを提供します。
{: shortdesc}

**シナリオ名**: 最適な心臓治療薬品の選択。

**シナリオの説明**: 心臓用の医薬品を製造するバイオ医薬品会社が、心臓治療のために最適な薬品を選択するモデルをデプロイしたいと考えています。薬品の有効性に関する新しいエビデンスが出現した際には、モデル更新が検討される必要があります。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、learning configuration を設定し、learning iteration を実行して、モデルの評価、リトレーニング、および再デプロイを行うことです。

**注:** [サンプルの python ノートブック](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325)を試してみることもできます。これは、サンプル・モデルを作成し、学習システムを構成し、learning iteration を実行します。

## 前提条件

サンプルを使用して作業するには、以下のサービスが必要です。

* フィードバック・データを保管するための [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) サービス・インスタンス資格情報。[このリンク](https://console.bluemix.net/catalog/services/apache-spark)を使用して作成できます。

## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル」**セクションで「Heart Drug Selection」タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。

**「モデル」**タブの使用可能なモデルのリストに、サンプルの**「Heart Drug Selection」**モデルが表示されます。

## アクセス・トークンの生成

{{site.data.keyword.pm_full}} サービス・インスタンスの「サービス資格情報」タブで提供されているユーザーおよびパスワードを使用するアクセス・トークンを生成します。

要求の例:

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

出力例:

```
{"token":"**********"}
```
{: codeblock}

以下の端末コマンドを使用して、トークン値を環境変数 token に割り当てます。

```
token="<token_value>"
```
{: codeblock}

## 公開モデルの処理

次のようなインスタンス詳細を取得するには、以下の API 呼び出しを使用します。

* 公開モデルの `url` アドレス
* デプロイメントの `url` アドレス
* 使用情報

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

出力例:

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

**published_models** `url` アドレスを取得した後、次の API 呼び出しを使用してモデルの詳細を取得します。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

出力例:

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


## 公開されたモデルの継続学習システムのセットアップ

モデルの継続学習システムを構成するには、以下のタスクを実行する必要があります。

1.  [許可ヘッダーの作成](#prepare-the-authorization-header)。
2.  [フィードバック・データ・セットの作成](#prepare-the-feedback-data-set)。
3.  [構成ペイロードの作成](#prepare-the-configuration-payload)。

許可ヘッダー、フィードバック・データ・セット、および構成ペイロードの作成が完了した後、継続学習システムのイテレーションの実行を始めることができます。詳しくは、『[継続学習システムのイテレーションの実行](#run-continuous-learning-system-iteration)』を参照してください。

### 許可ヘッダーの作成

{{site.data.keyword.pm_full}} トークンと Spark インスタンス資格情報を結合する許可ヘッダーを作成するには、以下の詳細を指定します。

*  直前のステップで作成されたアクセス・トークン。
*  {{site.data.keyword.Bluemix_notm}} Spark サービス・ダッシュボードの「サービス資格情報」タブにある Spark サービス資格情報。デプロイメント要求を行う前に、Spark 資格情報が base64 としてエンコードされる必要があります。それらは、要求ヘッダーの X-Spark-Service-Instance フィールドで渡されます。

   使用しているオペレーティング・システムに応じて、以下のいずれかの端末コマンドを発行して base64 エンコードを実行し、それを環境変数に割り当てる必要があります。

   **macOS** オペレーティング・システムでは、以下のコマンドを使用します。

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net", "instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   **Microsoft Windows** または **Linux** オペレーティング・システムでは、以下のように `base64` コマンドで `--wrap=0` パラメーターを使用して、base64 エンコードを実行する必要があります。

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### フィードバック・データ・セットの作成

学習システムには、トレーニング・データ (モデルのトレーニングで使用されるデータ) およびフィードバック・データへの接続が必要です。フィードバック・データ・ストアは、必要なときにモデルをモニターおよびリトレーニングするために使用されます。フィードバック・データ・ストアとして {{site.data.keyword.dashdbshort}} がサポートされています。フィードバック表は、{{site.data.keyword.pm_short}} サービスによって、管理、変更、および使用されます。
**{{site.data.keyword.dashdbshort}}** に **DRUG_FEEDBACK_DATA** 表を作成するには、以下のステップを実行する必要があります。

1. [{{site.data.keyword.dashdbshort}} サービス](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)のインスタンスを作成します (入門プランが提供されています)。
2. **{{site.data.keyword.dashdbshort}}** に `DRUG_FEEDBACK_DATA` 表を作成します。
   1. GitHub リポジトリーから [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) ファイルをダウンロードします。
   2. **{{site.data.keyword.dashdbshort_notm}}** の使用を開始するため、**「コンソールを開く」**アイコンをクリックします。
   3. **「データのロード」**および**「デスクトップ」**ロード・タイプを選択します。
   4. 前にダウンロードしたファイルを**ドラッグ**し、**「次へ」**を押します。
   5. データをインポートするため、**「スキーマ」**をクリックし、**「新規表」**をクリックします。
   6. **「新規表」**フィールドに名前を入力し、**「次へ」**をクリックします。
   7. **「フィールド分離文字」**にはセミコロン (;) を使用します。
   8. **「次へ」**をクリックして、アップロードしたデータで表を作成します。

**注:** この [REST API エンドポイント](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)を使用して、新規フィードバック・レコードをフィードバック・データベースに追加できます。詳しくは、『[フィードバック・データ・ストア](#feedback-data-store)』セクションを参照してください。

### 構成ペイロードの作成

learning configuration を指定するには、以下のエンドポイントを使用する必要があります。

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

ペイロードをファイナライズするには、以下のパラメーターの値を定義する必要があります。

<dl><dt>feedback_data_reference</dt>
<dd>フィードバック・データ・ストアの接続およびソース</dd>
<dt>min_feedback_data_size</dt>
<dd>継続学習システムのイテレーションを開始するための、フィードバック・データ・セット内のレコードの最小数
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] は、リトレーニング・プロセスがトリガーされるタイミングを指定します。[conditionally] に設定されている場合、モデル品質が指定のしきい値を下回るとリトレーニング・プロセスがトリガーされます。
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] は、リトレーニングされたモデルがデプロイされるタイミングを指定します。[conditionally] に設定されている場合、新しくトレーニングされたモデルの品質が現在デプロイされているものの品質を上回ると再デプロイがトリガーされます。</dd></dl>

要求の例:

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

出力例:

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
         "feedback_data_reference": {
           "connection":{
         "db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
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

**注:** この例では、`auto_retrain` パラメーターおよび `auto_redeploy` パラメーターにデフォルト値が使用されています。これらのパラメーターについて詳しくは、[REST API 資料](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration)で `learning_configuration` パラメーターの説明を参照してください。

## 継続学習システムのイテレーションの実行

学習システムのイテレーションを開始するには、以下の REST API メソッドを使用します。イテレーション中に、公開されたモデルが評価されます。評価された正確度が指定のしきい値を下回っている場合、モデルのリトレーニングがトリガーされます。トレーニングとフィードバックの両方のデータ・セットが、リトレーニングおよび評価のために使用されます。

要求の例:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

出力例:

```
The request has been fulfilled and resulted in a new resource being created.
```
{: codeblock}

イテレーションの状況を確認するには、以下の要求を実行します。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

出力例:

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
      "credentials":{
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

以下の要求を実行することによって、評価メトリックおよび値のリストを取得することもできます。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

出力例:

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

## フィードバック・データ・ストア

[フィードバック・エンドポイント](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)を使用して、learning configuration に定義されているフィードバック・ストアに新規レコード (トレーニング・プロセスで使用されなかったレコード) を送信できます。フィードバック・データ・ストアは、必要なときにモデルをモニターおよびリトレーニングするために使用されます。フィードバック・データ・ストアとして {{site.data.keyword.dashdbshort}} がサポートされています。フィードバック表が存在しない場合、当サービスによって作成されます。表が存在する場合、スキーマが検証されてトレーニング表のスキーマと一致しているか確認され、`_training` という名前の列が表に付加されます。この追加列は、リトレーニング・プロセスでコンシュームされたレコードをマークするために使用されます。

**注:** フィードバック表は、{{site.data.keyword.pm_short}} サービスによって、管理、変更、および使用されます。

以下の要求を実行することによって、新規フィードバック・レコードをフィードバック・データ・ストアに送信できます。

要求の例:

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

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
