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

# モデルの永続化

{{site.data.keyword.pm_full}} サービスを使用して、モデルのバージョン管理を提供し、機械学習プロセスを適切に整理された状態に保つことができます。{{site.data.keyword.pm_short}} サービスのバージョン管理、メタデータ、およびアクセス・トークンの機能を使用することによって、これを行うことができます。
{: shortdesc}

## モデルの永続性とバージョン管理

調査と開発の一環としてモデルを改善することに常に取り組んでいると、既存のモデルに新しいフィーチャーを追加したり、モデルのパラメーターを最適化したりすることがよくあります。
このように反復的にモデルを開発していると、継続的に変更を追跡することはすぐに難しくなります。{{site.data.keyword.pm_short}} の以下のデータ・モデル・バージョン管理フィーチャーを使用することによって、プロセス全体を適切に整理された状態に保つことができます。 

*  [アクセス・トークンの生成](#generating-the-access-token)
*  [パイプライン・メタデータの作成](#creating-pipeline-metadata)
*  [パイプライン・バージョンの作成](#creating-pipeline-version)
*  [パイプライン・コンテンツのアップロード](#uploading-pipeline-content)
*  [パイプライン・モデル・メタデータの作成](#creating-pipeline-model-metadata)
*  [パイプライン・モデル・バージョンの作成](#creating-pipeline-model-version)
*  [パイプライン・モデル・コンテンツのアップロード](#uploading-pipeline-model-content)

### アクセス・トークンの生成

{{site.data.keyword.pm_full}} サービス・インスタンスの「サービス資格情報」タブで提供されているユーザーおよびパスワードを使用して、アクセス・トークンを生成します。

要求の例:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

出力例:

```
{"token":"**********"}```
{: codeblock}

以下の端末コマンドを使用して、環境変数 `access_token` にトークン値を割り当てます。

```
access_token="<token_value>"```
{: codeblock}

パイプラインとパイプライン・モデルを保存します。パイプラインとパイプライン・モデルのメタデータを作成し、パイプラインとパイプライン・モデルのバージョンを作成し、パイプラインとパイプライン・モデルのバージョンをアップロードします。 

### パイプライン・メタデータの作成

パイプラインのメタデータを作成するには、以下の例に示すように、
`curl` 要求内にパイプラインの基本的なプロパティーを記述します。

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "name": "sample pipeline",
  "description": "sample description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "type": "sparkml-pipeline-2.0",
  "runtimeEnvironment": "spark-2.0"
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines
```
{: codeblock}

応答の例:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:46:16 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3172115887
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### パイプライン・バージョンの作成

パイプラインのバージョンを作成するには、以下の例に示すように、
`curl` 要求内にパイプラインの `parentVersionHref` 値を指定します。

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

応答の例:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:47:56 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7/versions/0bb830fc-bc1e-439a-b929-685d9fc7712d
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3510865585
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### パイプライン・コンテンツのアップロード

パイプライン・コンテンツをアップロードするには、パイプラインがバイナリー形式でなければなりません。これを実行するには、SparkML パイプラインから
`save` メソッドを呼び出します。以下の例に示すように、エンドポイントでパイプラインの ID とバージョンを指定することによって、バイナリー・コンテンツをアップロードできます。

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

応答の例:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:50:53 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 980490895
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

パイプラインの保存後、続けてパイプライン・モデルの保存を実行できます。

### パイプライン・モデル・メタデータの作成

以下の例に示すように、パイプライン・モデルのデータ・スキーマを記述する必要があります。

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
  "name": "sample-model",
  "description": "sample description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "pipelineVersionHref": "string",
  "type": "sparkml-model-2.0",
  "runtimeEnvironment": "spark-2.0",
  "trainingDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "label",
        "type": "int",
        "nullable": true,
        "metadata": {}
      }
    ]
  },
  "labelCol": "string",
  "inputDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models
```
{: codeblock}

応答の例:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:58:29 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 437931687
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### パイプライン・モデル・バージョンの作成

パイプライン・モデルのバージョンを作成するには、`curl` 要求を使用して、トレーニング・データを格納する場所やモデル評価方式などの詳細を指定します。次の例を参照してください。

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
  "trainingDataRef": {
"connection": {
"auth_url": "https://identity.open.softlayer.com",
      "project": "object_storage_008c8b63_b3d6_47c4_9da2_6c3ea6cfa713",
      "projectId": "f771e5d082e24857adb7554d15fc357c",
      "region": "dallas",
      "userId": "9b2e44721b91471ab86ecc41aeb7d37f",
      "username": "admin_926e1098fc72ff3c59d9222a2ab31a8a22143497",
      "password": "xxxxxxxxxxxxxxxx",
      "domainId": "a7b64174a5d74134b73b1533f6b99ae6",
      "domainName": "1143537",
      "role": "admin"
    },
    "source": {
      "container": "notebooks",
      "filename": "WA_Fn-UseC_-Telco-Customer-Churn.csv",
      "fileformat": "csv",
      "inferschema": 1,
      "firstlineheader": true
    }
  },
  "evaluation": {
    "method": "Binary",
    "metrics": [
      {
        "name": "areaUnderROC",
        "threshold": 0.8
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/30ed894b-4018-4c88-9e53-86005548317a/versions
```
{: codeblock}

応答の例:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:10:12 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb/versions/8be22c6e-e214-45a4-8cfb-be4cade109ef
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 285978151
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### パイプライン・モデル・コンテンツのアップロード

パイプライン・モデル・コンテンツをアップロードするには、パイプライン・モデルがバイナリー形式でなければなりません。これを実行するには、SparkML パイプライン・モデルから
`save` メソッドを呼び出します。以下の例に示すように、エンドポイントでパイプラインの ID とバージョンを指定することによって、バイナリー・コンテンツをアップロードできます。

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

応答の例:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:11:23 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 605948113
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

## 詳細はこちら

モデル開発について詳しくは、以下のノートブックを参照してください。

*  [Developing SparkML models with Python](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d)
*  [Developing SparkML models with Scala](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93)
