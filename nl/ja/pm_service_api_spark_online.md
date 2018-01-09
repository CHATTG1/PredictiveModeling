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

# オンライン・モデルのデプロイ

{{site.data.keyword.pm_full}} サービスを使用して、モデルをデプロイし、デプロイされたモデルに対してスコアリング要求を行うことによって予測分析を生成することができます。
{: shortdesc}

**シナリオ名**: 製品ライン予測。

**シナリオの説明**: アウトドア用の備品を販売する企業が、特定の製品ラインに対する顧客の興味を予測するモデルを作成したいと考えています。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、顧客の興味の予測を生成することです。

## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの「サンプル」タブに移動します。

2. 「サンプル・モデル」セクションで「Product Line Prediction」タイルを見つけて、「モデルの追加」アイコン (+) をクリックします。

「モデル (Models)」タブの使用可能なモデルのリストに、サンプルの「Product Line Prediction」モデルが表示されます。

## アクセス・トークンの生成

{{site.data.keyword.pm_full}} サービス・インスタンスの**「サービス資格情報」**タブにある `user` および `password` を使用して、アクセス・トークンを生成します。

要求の例:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

出力例:

```
{"token":"**********"}```
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
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
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


**published_models** の `url` が分かったので、次の API 呼び出しを使用してモデルの詳細を取得します。

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
            "guid":"7715dfcc-3005-4bc2-8bee-58ebdc9a43f3",
            "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publishepublished_model_id}",
            "created_at":"2017-07-10T12:45:56.623Z",
            "modified_at":"2017-07-10T12:45:56.710Z"
         },
         "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Product Line Prediction",
            "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
            "label_col":"PRODUCT_LINE",
            "training_data_schema":{ },
            "latest_version":{ },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publipublished_model_id}/deployments"
            },
            "input_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"GENDER",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {
                     "metadata":{
            },
                     "type":"string",
                     "name":"MARITAL_STATUS",
                     "nullable":true
                  },
                  {
                     "metadata":{
            },
                     "type":"string",
                     "name":"PROFESSION",
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


**deployments** `url` 値は以下のオンライン・デプロイメントを作成するために必要なのでメモしておいてください。


## オンライン・デプロイメントの作成

以下の API 呼び出しを使用して、予測モデルのオンライン・デプロイメントを作成します。

要求の例:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


出力例:

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
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
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"ACTIVE",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## スコアリング要求の実行

スコアリング・エンドポイント (`scoring_url`) が作成されたため、スコアリング要求を行うことによって、予測を生成できるようになりました。このシナリオでは、顧客レコードが予測モデルに渡され、スポーツ製品予測が返されます。

サンプル・レコード・ヘッダー:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION```
{: codeblock}

サンプル・レコード:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

要求の例:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

出力例:

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "values":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

例えば、55 歳の会社役員は「登山用備品 (Mountaineering Equipment)」に関心があり、一方、23 歳の学生は「個人用アクセサリー (Personal Accessories)」に関心があることが分かります。

**注:** scikit-learn モデルおよび XGBoost モデルの場合、スコアリング・ペイロード内では `values` フィールドのみが必須です。

要求の例:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"values": [[0.0,1.0],[4.0,15.0]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
