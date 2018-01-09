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

# バッチ・モデルのデプロイ

{{site.data.keyword.pm_full}} サービスを使用して、モデルをデプロイし、デプロイされたモデルに対してスコアリング要求を行うことによって予測分析を生成することができます。
{: shortdesc}


**シナリオ名**: 顧客満足度予測。

**シナリオの説明**: ある通信会社が、解約する恐れがあるのはどの顧客であるかを知りたいと考えています。提供されたモデルは、顧客のチャーン (契約/解約を繰り返す顧客の流動現象) を予測します。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、予測分析を生成することです。

**注:** サンプルの python [ノートブック](https://apsportal.ibm.com/analytics/notebooks/5e4963d9-faea-455d-a7db-ff6302d1d8f5/view?access_token=5d23d36be72dea35ebbde9b4b5f4a16d0053ee898f1ab2ab73cf1301ce9322be)を試してみることもできます。これは、サンプル・モデルを作成し、顧客のチャーンを予測します。

## 前提条件

この例を使用して作業するには、以下のリソースが必要です。

* モデルの入力 (スコアリングする顧客データ)、およびモデルの出力のストレージとして使用される、[オブジェクト・ストレージ](https://console.bluemix.net/catalog/services/object-storage)・インスタンスの詳細。サンプル入力データ .csv ファイルは、[ここ](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv)からダウンロードできます。 入力ファイルはオブジェクト・ストレージ・インスタンスに追加する必要があります。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) サービス・インスタンス資格情報。[このリンク](https://console.bluemix.net/catalog/services/apache-spark)を使用して作成できます。



## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル (Sample Models)」**セクションで**「Customer Satisfaction Prediction」**タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。

**「モデル (Models)」**タブの使用可能なモデルのリストに、サンプルの「Customer Satisfaction Prediction」モデルが表示されます。

## アクセス・トークンの生成

{{site.data.keyword.pm_full}} サービス・インスタンスの「サービス資格情報」タブで提供されているユーザーおよびパスワードを使用して、アクセス・トークンを生成します。

要求の例:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
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

インスタンス詳細を取得するには、以下の API 呼び出しを使用します。詳細には次の項目が含まれます。

* 公開されたモデルの `url` 値
* デプロイメントの `url` 値
* 使用情報

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

出力例:

```
{
   "metadata":{
      "guid":"87452a37-6a8f-4d59-bf88-59c66b5463e4",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
      "created_at":"2017-06-23T08:31:52.026Z",
      "modified_at":"2017-06-23T08:31:52.026Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
      },
      "usage":{ },
      "plan_id":"5325f63a-683a-47f0-a04e-97e371385588",
      "account_id":"b56398ea52f470c3173f4cf3bef5cc7e",
      "status":"Active",
      "organization_guid":"3e658178-a60c-48b8-8be9-bf58cc821656",
      "region":"us-south",
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}}/deployments"
      },
      "space_guid":"c3ea6205-b895-48ad-bb55-6786bc712c24",
      "plan":"lite"
   }
}
```
{: codeblock}

**published_models** `url` 値を指定することによって、以下の API 呼び出しを使用してモデル詳細を取得できます。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

出力例:

```
{
   "count":1,
   "resources":[
      {
         "metadata":{
      "guid":"dc46315a-c30e-46a3-8e30-33518e6f7976",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976",
            "created_at":"2017-03-21T13:49:38.711Z",
            "modified_at":"2017-03-21T13:49:38.802Z"
         },
   "entity":{
      "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts Telco customer churn.",
            "label_col":"Churn",
            "training_data_schema":{
               "type": "struct",
    "fields": [
      {
        "metadata":{
      },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Churn",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/dc46315a-c30e-46a3-8e30-33518e6f7976/versions/658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "guid":"658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "created_at":"2017-03-21T13:49:38.802Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976/deployments"
            },
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
        "metadata":{
      },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  }
               ]
            }
         }
      }
   ]
}
```
{: codeblock}

**deployments** `url` 値は以下のバッチ・デプロイメントを作成するために必要なのでメモしておいてください。

## オブジェクト・ストレージを使用するバッチ・デプロイメントの作成

REST API 呼び出しを使用して予測モデルのバッチ・デプロイメントを作成するには、以下の詳細を指定します。

*  直前のステップで作成されたアクセス・トークン。
*  {{site.data.keyword.Bluemix_notm}} Spark サービス・ダッシュボードの「サービス資格情報」タブにある Spark サービス資格情報。デプロイメント要求を行う前に、Spark 資格情報を base64 としてデコードし、`curL` 要求のヘッダー内で X-Spark-Service-Instance として渡す必要があります。

   使用しているオペレーティング・システムに応じて、以下のいずれかの端末コマンドを発行して base64 デコードを実行し、それを環境変数に割り当てる必要があります。

   **macOS** オペレーティング・システムでは、以下のコマンドを使用します。

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   **Microsoft Windows** または **Linux** オペレーティング・システムでは、以下のように `base64` コマンドで `--wrap=0` パラメーターを使用して、base64 デコードを実行する必要があります。

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

*  オブジェクト・ストレージの詳細。モデルの入力 (スコアリングする顧客データ)、およびモデルの出力 (この場合は results.csv であり、自動的に作成される) のストレージとして、使用されます。
*  デプロイメントを作成するには、前のセクションからの **deployments** `url` 値を使用します。


要求の例:

```
curl -v -XPOST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"Customer Satisfaction Prediction",
      "type": "batch",
      "description": "Batch Deployment",
       "input":{
          "source":{
             "container":"batchjob",
             "filename":"TelcoCustomerData.csv",
             "fileformat":"csv",
             "inferschema":"1",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       },
       "output":{
          "target":{
             "container":"batchjob",
             "filename":"result.csv",
             "fileformat":"csv",
             "writemode":"write",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b283cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       }
    }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

出力例:

```
{
   "metadata":{
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials":{
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

**注**: ダッシュボードを使用してバッチ・デプロイメントを作成することもできます。

## デプロイメントの詳細の取得

デプロイメント・モデルに関連する状況およびパラメーターを **metadata** `url` 値を使用して確認できます。
要求の例:

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

出力例:

```
{
   "metadata":{
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"COMPLETED",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials":{
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

予測結果は、IBM オブジェクト・ストレージ内の .csv ファイルに保存されます。出力例については、以下のサンプル行を参照してください。

入力ファイルのプレビュー:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

出力ファイルのプレビュー:

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

## バッチ・デプロイメントの削除

デプロイメントを削除するには、以下の照会を使用します。

要求の例:

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

出力例:

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
