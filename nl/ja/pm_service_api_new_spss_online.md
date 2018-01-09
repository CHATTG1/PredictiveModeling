---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# REST API (新規) <span class='tag--beta'>ベータ</span>

{{site.data.keyword.pm_full}} サービスでは、単一ユーザー・エクスペリエンスを実現するために、他のサポートされるフレームワークに準拠する [REST API](http://watson-ml-api.mybluemix.net/) が提供されています。SPSS® モデルにも、その他のフレームワーク (scikit-learn、XGboost、Spark MLlib など) にも、同じ API 要求を使用できます。
{: shortdesc}

**注:** このフィーチャーはベータ版です。参加をご希望の場合は、ご自身を待機リストに追加してください。
詳しくは、[https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist) を参照してください。

{{site.data.keyword.pm_full}} サービス REST API を使用して、オンライン・デプロイメントを作成し、デプロイされたモデルに対してスコアリング要求を行うことによって予測分析を生成することができます。この REST API をよく理解するために、以下の顧客満足度シナリオを使用し、`bash` コマンドを使用してシナリオを実行する [サンプル Jupyter ノートブック](https://dataplatform.ibm.com/analytics/notebooks/3c2ef65c-7d0e-4ff7-ab11-578ef2a46d66/view?access_token=21517d9a2f59b1ff5e386982b3fa03d7b41cff53a22bd30b5dc7785535b0b80d)を使用してください。

**シナリオ名**: 顧客満足度予測

**シナリオの説明**: ある通信会社が、解約する恐れがあるのはどの顧客であるかを知りたいと考えています。提供されたモデルは、顧客のチャーン (契約/解約を繰り返す顧客の流動現象) を予測します。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、予測分析を生成することです。

## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル (Sample Models)」**セクションで**「Customer Satisfaction Prediction (SPSS MODEL)」**タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。

**「モデル (Models)」**タブの使用可能なモデルのリストに、サンプルの「Customer Satisfaction Prediction」モデルが表示されます。

## アクセス・トークンの生成

IBM Watson Machine Learning サービス・インスタンスの「サービス資格情報」タブに提供されているユーザー名およびパスワードを使用して、アクセス・トークンを生成します。

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

インスタンス詳細を取得するには、以下の API 呼び出しを使用します。詳細には次の値が含まれます。

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

モデル詳細を取得するには、**published_models** `url` を指定して、以下の API を実行します。

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
"guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "created_at":"2017-11-06T11:34:17.991Z",
            "modified_at":"2017-11-06T11:34:18.227Z"
         },
         "entity":{
            "runtime_environment":"spss-modeler-17.0",
            "learning_configuration_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_configuration",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts customer satisfaction in telco churn model",
            "label_col":"Churn",
            "learning_iterations_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_iterations",
            "training_data_schema":{
               "type":"struct",
               "fields":[
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
            "feedback_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/feedback",
            "latest_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments"
            },
            "evaluation_metrics_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/evaluation_metrics",
            "input_data_schema":{
               "type":"struct",
               "fields":[
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

**deployments** `url` は次のステップでオンライン・デプロイメントを作成するために必要なのでメモしておいてください。

## オンライン・デプロイメントの作成

以下の API 呼び出しを使用して、予測モデルのオンライン・デプロイメントを作成します。

要求の例:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Customer Satisfaction Prediction", "description": "Customer Satisfaction Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


出力例:

```
{
   "metadata":{ 
"guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
            "created_at":"2017-11-06T12:03:05.080Z",
            "modified_at":"2017-11-06T12:04:10.766Z"
         },
   "entity":{
      "runtime_environment":"spss-modeler-17.0",
            "name":"Customer Satisfaction Deployment",
            "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
            "description":"My 1st Deployment",
            "published_model":{
               "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "description":"Predicts customer satisfaction in telco churn model",
         "created_at":"2017-11-06T12:01:04.394Z"
      },
      "model_type":"spss-modeler-model-17.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "created_at":"2017-11-06T11:34:18.227Z"
      }
   }
}
```
{: codeblock}

## デプロイメントの詳細の取得

デプロイされたモデルに関連する状況、スコアリング・エンドポイント・アドレス (`scoring_url`)、およびパラメーターを確認できます。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

出力例:

```
{
   "count":1,
   "resources":[
      {
         "metadata":{ 
"guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
            "created_at":"2017-11-06T12:03:05.080Z",
            "modified_at":"2017-11-06T12:04:10.766Z"
         },
         "entity":{
            "runtime_environment":"spss-modeler-17.0",
            "name":"Customer Satisfaction Deployment",
            "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
            "description":"My 1st Deployment",
            "published_model":{
               "author":{
"name":"IBM",
               "email":""
            },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "description":"Predicts customer satisfaction in telco churn model",
         "created_at":"2017-11-06T12:01:04.394Z"
      },
            "model_type":"spss-modeler-model-17.0",
            "status":"ACTIVE",
            "type":"online",
            "deployed_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "created_at":"2017-11-06T11:34:18.227Z"
      }
   }
      }
   ]
}
```
{: codeblock}

## スコアリング要求の実行

スコアリング・エンドポイント (`scoring_url`) が作成されたため、スコアリング要求を行うことによって、予測を生成できるようになりました。次のシナリオでは、顧客レコードが予測モデルに渡され、満足度予測が返されます。

サンプル・レコード・ヘッダー:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges
```
{: codeblock}


サンプル顧客レコード:

```
"3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768
```
{: codeblock}


要求の例:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"values":[["3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

出力例:

```
{
   "fields":[ 
"customerID",
      "Churn",
      "Predicted Churn",
      "Probability of Churn"
   ],
   "values":[
      [
         "3638-WEABW",
         "No",
         "No",
         0.0526309571556145
      ]
   ]
}

```
{: codeblock}

この結果から、この顧客は満足していることが分かります。

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM® Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
