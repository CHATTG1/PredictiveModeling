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

# デプロイされた SPSS モデルの管理

{{site.data.keyword.pm_full}} サービス API を使用して、デプロイ対象の IBM® SPSS® Modeler スコアリング枝が含まれているファイルをアップロードできます。そのファイルは、アップロードされた後、アプリケーションでデータをスコアリングするために使用可能にされます。
{: shortdesc}

具体的には、以下のタスクを実行できます。

*  [予測モデルのデプロイまたは最新表示](#deploying-or-refreshing-a-predictive-model)
*  [現在デプロイされている全モデルのリストの取得](#retrieving-a-list-of-all-currently-deployed-models)
*  [デプロイされた特定モデル・ファイルのコピーをダウンロードする](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [デプロイされた予測モデルの削除](#deleting-a-deployed-predictive-model)

## 予測モデルのデプロイまたは最新表示

各モデル・ファイルには、以降のサービス呼び出しでデプロイ済みモデルを参照するために使用する便利な別名として context ID が与えられます。特定の context ID のモデルが存在する場合、そのモデルは、アプリケーションで使用中の予測分析を最新表示する手段として、以下の `PUT` 呼び出しによって置き換えられます。

要求の例:

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}```
{: codeblock}

要求パラメーター:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

デプロイメントが成功した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

デプロイメントが失敗した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
        }
```
{: codeblock}

## 現在デプロイされている全モデルのリストの取得

このサービス・インスタンスに現在デプロイされているすべてのモデルのサマリーを取得するには、以下の API 呼び出しを使用します。

要求の例:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}```
{: codeblock}

要求パラメーター:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

デプロイされたモデル・サマリーの要求が成功した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

デプロイされたモデル・サマリーの要求が失敗した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
         }
```
{: codeblock}

## デプロイされた特定モデル・ファイルのコピーをダウンロードする

以下の API 呼び出しを使用して、デプロイされた特定のモデル・ファイルのコピーをダウンロードします。

要求の例:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}```
{: codeblock}

要求パラメーター:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

ダウンロード要求が成功した場合の応答:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

ダウンロード要求が失敗した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":String // failure reason
        }
```
{: codeblock}

## デプロイされた予測モデルの削除

以下の API 呼び出しを使用して、Machine Learning サービス・インスタンスから予測モデルを削除します。この呼び出しの実行後は、予測モデルはアプリケーション内のダウンロードにもデータのスコアリングにも使用できなくなります。

要求の例:

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}```
{: codeblock}

要求パラメーター:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

アンデプロイが成功した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

アンデプロイが失敗した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
        }
```
{: codeblock}

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
