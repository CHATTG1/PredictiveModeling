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

# IBM SPSS Modeler モデル用の Machine Learning サービスのバッチ・ジョブ API

{{site.data.keyword.pm_full}} サービスのバッチ・ジョブ API は、モデル学習、モデル評価、およびバッチ・スコアリングに関連する長時間実行タスクをサポートします。この API は、バッチ・ジョブで使用される IBM® SPSS® Modeler ストリーム・ファイルと、実行依頼されるジョブ定義の 2 つの資産タイプを管理します。
{: shortdesc}

アップロードする SPSS® Modeler ストリーム・ファイルごとに、実行依頼される多くのタイプの多数のジョブがある可能性があります。ジョブで Modeler ストリーム・ファイルの内容が更新される可能性がある場合、変更されたファイルがジョブ結果の添付ファイルに含められます。モデル評価ジョブ・タイプでは、生成されたすべての評価ファイルがジョブ結果の添付ファイルに含められます。

**注:** ダッシュボードに表示されるデータは、アップロード・フィーチャーからのデータなど、リアルタイム予測にのみ関連しています。

## ジョブの削除

ジョブを削除できます。ジョブが実行中の場合、ジョブを削除すると、まずジョブのキャンセルが行われます。`job ID` を指定して `DELETE` コマンドを使用してください。複数の ID を渡すことによって、一度に複数のジョブを取り消すことができます。

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx```
{: codeblock}

削除されたジョブの数が返されます (それらのジョブは要求内で ID によって表されたものです)。この数が要求で渡したリストと一致しない場合は、個々のジョブの状況を検査する必要があります。

## ジョブの状況の確認

`GET` コマンドを使用することによって、任意の時点で `job ID` の状況を取得できます。


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx```
{: codeblock}

返される JSON ファイルでは、ジョブ状況が示され、ジョブが正常に完了した場合は、生成されたすべてのファイル内容を取得するために使用できるデータ Url も示されます。

## ジョブの再実行依頼

ジョブを再実行依頼するには、`job ID` を指定して `PUT` コマンドを使用します。再実行依頼するジョブは稼働中であってはなりません。

表している ID が存在しない場合、以下の呼び出しは新規ジョブを作成します。戻り状況は 201 か 200 です (発生した結果を、これらのいずれかで示します)。この実行で使用するジョブ定義 JSON ファイルを渡す必要があります。

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx```
{: codeblock}

`content_type` 値を「application/json」に設定してジョブを再実行依頼します。要求の本体に、新規または更新されたジョブ定義 JSON ファイルを含める必要があります。

## アップロードされた Modeler ストリーム・ファイルに対するジョブの実行依頼

実行キューにジョブを入れるには、`PUT` コマンドを使用します。

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx```
{: codeblock}

`content_type` 値を「application/json」に設定してジョブを実行依頼します。要求の本体に、新規または更新されたジョブ定義 JSON ファイルを含める必要があります。

この要求は、ジョブ定義が実行キューに入れられた場合、即時に戻り、成功を示します。ジョブ定義で構成されたように Modeler ストリームを正常に実行できるかどうかのテストはありません。ジョブは、クラウドで {{site.data.keyword.pm_short}} ジョブ・サーバーの 1 つによって実行され、ユーザーはその状況をモニターできます。モデル学習を実行するときに、自動最新表示を行うことを指定した場合、ジョブは、ジョブが正常に実行されると元の Modeler ストリーム・ファイルを置き換えます。

## ジョブで使用するストリーム・ファイルのアップロード

**注:** {{site.data.keyword.pm_short}} ダッシュボードはリアルタイム・スコアリング専用です。これは、実行中のジョブ (バッチ・スコアリング) には使用できません。

Modeler ストリーム・ファイルをジョブでアクセス可能にするには、以下のように `PUT` コマンドを使用します。

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx```
{: codeblock}

要求で content_type "multipart/form-data" を使用して、ファイルを渡します。

使用される固有の名前 (`PUT` 呼び出しの `file ID`) は、ファイル API に対する `DELETE` 呼び出しでの参照先であり、ジョブ定義でのモデル参照でもあります。ユーザーはジョブで使用されるファイルの名前空間を完全に制御できることに注意してください。
同じ `file ID` でのファイルの `PUT` コマンドは、現行コピーを暗黙的に置き換えます。

ジョブのためにアップロードされたすべてのファイルのリストを生成するには、`file ID` パラメーターを省略して `GET` コマンドを使用します。特定のファイルを取り出すには、`file ID` パラメーターを指定して `GET` コマンドを使用します。`DELETE` コマンドを実行することによって、アップロードされたファイルを削除することもできます。これによって、そのファイルを参照する保留中のジョブ実行ではエラーが生じます。

要求の例:

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

## ジョブ・タイプ

**学習 (Training)**: このジョブ・タイプは、すべての「モデル・ビルダー」端末ノードを Modeler ストリームで実行する必要があることを示します。ジョブが正常に完了した後で、新たに学習されたモデル・ナゲットで更新された Modeler ストリームが、取得可能なジョブ結果内に入れられます。Modeler ストリーム・ファイルに、モデル・ビルド・ノードから、評価枝とスコアリング枝内で学習されたモデル・ナゲットへのリンクが含まれている場合、これによってこれらのノードが最新表示されます。

**評価 (Evaluation)**: このジョブ・タイプは、呼び出し元に戻すことができる静的レポート・ファイル内容を生成する、すべての「文書ビルダー」端末ノードの実行を (主に、Modeler クライアントの 「グラフ」タブと「出力」タブから) トリガーします。スコアリング枝は、このジョブ・タイプの一部とはみなされません。

**自動最新表示 (Auto-Refresh)**: `TRAINING` ジョブ・タイプの 1 つのバージョンであり、ジョブが正常に完了したときに、バッチ・ファイル・リスト内の元の Modeler ストリーム・ファイルが更新されます。リアルタイム・スコアリングのためにデプロイされた Modeler ストリームの最新表示イベントに関する評価と明示的な決定が想定され、この時点では自動最新表示の対象となりません。

**バッチ・スコア (Batch Score)**: 端末ノードがこの Modeler ストリーム設計のスコアリング枝であることを示す、「スコアリング枝として使用」オプションを適用した端末ノードの実行。ジョブ定義では、「エクスポート」および「ソース」の詳細を指定する必要があります。

**ストリームの実行 (Run Stream)**: 実行は、ストリーム・プロパティーの「実行」タブで「このスクリプトを実行」オプションを選択した状態で、Modeler の緑色の「実行 (run)」アイコンをクリックすることと似ています。使用法では、モデル学習やその他のジョブ・タイプのスクリプト化された実行の必要性がカバーされています。スクリプトの動的な制御はすべて、ジョブ定義でパラメーター値を渡してストリーム・パラメーターによって処理する必要があります。

## ジョブ状況

**保留中 (Pending)**: ジョブ定義は実行依頼されましたが、ジョブ・サーバーによって実行がまだ要求されていません。

**実行中 (Running)**: ジョブ定義がジョブ・サーバーによって要求され、実行中です。

**キャンセル中 (Canceling)**: ジョブはキャンセル中です。

**キャンセル済み (Canceled)**: ジョブはキャンセルされました。

**失敗 (Failed)**: ジョブは失敗しました。失敗の原因に関する詳細が、ジョブ状況の GET 時に返されます。

**成功 (Success)**: ジョブは正常に実行されました。このイベントについて通信されたすべての結果が、ジョブ状況の GET の JSON で返されます。

## ジョブ API の詳細

`POST /v1/jobs/{id}`

説明: ジョブ定義を実行依頼します。`job ID` が既に存在する場合はエラーになります。

コンテンツ・タイプ:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定の `job ID`。{{site.data.keyword.pm_short}} サービス・インスタンスに固有でなければなりません。

```
@PathParam("id") ```
{: codeblock}

JSON ジョブ定義 (『ジョブ定義 JSON』を参照):

```
@BodyParam```
{: codeblock}

応答:

成功。ジョブは実行依頼され、実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブは既に存在しているというエラー。この ID を持つジョブは既に実行依頼されています。返されるメッセージではこのエラーが説明されています。

```
@ApiResponse(code = 409)```
{: codeblock}

その他のエラー。返される例外の JSON

```
@ApiResponse(code = 500)```
{: codeblock}

`PUT /v1/jobs/{id}`

説明: ジョブを作成または更新します。この ID を持つジョブが存在しない場合は作成します。存在する場合は更新します (事実上、再実行依頼します)。

コンテンツ・タイプ:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定の `job ID`:

```
@PathParam("id") ```
{: codeblock}

JSON ジョブ定義 (『ジョブ定義 JSON』を参照):

```
@BodyParam```
{: codeblock}

応答:

成功。ジョブが更新され、実行依頼されます。実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

成功。ジョブが作成され、実行依頼されます。実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 201)```
{: codeblock}

その他のエラー。返される例外の JSON。

```
@ApiResponse(code = 500)```
{: codeblock}

`GET /v1/jobs`

説明: この Machine Learning サービス・インスタンスで定義されたすべてのジョブのリストを返します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

応答:

成功。すべてのジョブ定義の JSON 配列を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`GET /v1/jobs/{id}`

説明: 特定のジョブ定義の戻りを要求します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼された `job ID`:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。参照されるジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブが見つからないというエラー。返されるメッセージの詳細:

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`GET /v1/jobs/{id}/status`

説明: 特定のジョブの状況を取得します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼された `job ID`:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。ジョブ状況を説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブが見つからないというエラー。返されるメッセージの詳細:

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`DELETE /v1/jobs/{ids}`

説明: 1 つ以上のジョブを削除します。現在ジョブが実行中の場合は、そのジョブを取り消します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼された `job ID`、または ID 値をコンマ (,) で区切った `job ID` リスト

```
@PathParam("id" or “id,id,id”)```
{: codeblock}

応答:

成功。削除されたジョブの数を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

## ジョブ定義 JSON

ジョブ定義 JSON には、以下の一般セクションが含まれています。

ジョブ・タイプおよび予測モデル参照

**注:** ダッシュボードに表示されるデータは、アップロードされたフィーチャーからのデータも含めて、リアルタイム予測にのみ関連しています。

### アクション・タイプ

**TRAINING** – モデル・ナゲットを学習または最新表示するモデル・ビルダー・ノードを実行します。更新された Modeler ストリームがジョブ結果で取得されます。

**EVALUATION** – 学習されたモデルを評価する文書ビルダー・ノードおよび分析ノードを実行します。

**AUTO_REFRESH** – TRAINING を実行し、バッチ・ファイルのアップロード・スペースでファイル内容を更新します。

**BATCH_SCORE** – スコアリング枝を実行し、ジョブ定義で指示されたとおりに結果のデータをエクスポートします。

**RUN_STREAM** – ストリーム・プロパティーで示されたとおりに Modeler ストリームを実行します。ほとんどの場合、スクリプト化された実行が必要な場合に使用されます。

model: バッチ・ジョブ参照のためにファイルのアップロード・アクションで指定された ID。詳しくは、『ジョブで使用するストリーム・ファイルのアップロード』を参照してください。`/pm/v1/file` が使用されることに注意してください。

```
"action": "TRAINING",
"model": {
     "id": "str2"
     "name":"stream1.str"
},
```
{: codeblock}

ID は `PUT` API で使用される `file ID` と同じでなければならないことに注意してください。name は必須ではありませんが、モデルの学習と自動最新表示のために、ジョブ結果はここで定義された名前を使用して保存されます。name が定義されていない場合、{{site.data.keyword.pm_short}} サービスは、定義済みの命名規則に従って結果を生成します。

### ジョブ設定

このジョブを実行するために必要なすべての設定。

```
"setting": {          
… Export settings
… Import settings
… Parameter value settings
… Document control settings
}
```
{: codeblock}

### データベース接続定義

データベース・タイプ: ApacheHive、DashDB、DB2、Greenplum、Impala、Informix、MySQL、Oracle、PostgreSQL、ProgressOpenEdge、Salesforce、SQLServer、Sybase、SybaseIQ、Teradata。

DB サービス・インスタンスで指定された接続。多くの DB タイプでは、特定の設定を「Options」で渡す必要があります。

```
"dbDefinitions":{
"db1":{
"type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### ソース・ノード設定

Modeler ストリームの、ソース・ノード名で指定された特定の枝を提供するために使用される参照 DB 接続と表。

```
"inputs": [
     {
          "odbc": {
"dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

table はデータベース表名です。この表は、ストリーム・ソース・ノードをオーバーライドするために使用されます。node は、ストリームのデータ・ソース名です。dbRef は、dbDefinitions オブジェクトで定義された DB 接続を参照し、table は、Modeler ストリーム内の特定の枝の入力として使用されるデータベース表です。node は、指定されたこれらのパラメーターを使用して構成される DB ソース・ノードによって置き換えられる、Modeler ストリーム内の元のソース・ノードを示しています。

### エクスポート・ノード設定

Modeler ストリームの、端末ノード名で指定された特定の枝のデータを保持するために使用される参照 DB 接続と表。

データの保持中に使用される制御方法は、以下の insertMode 属性を使用して伝えられます。

**Append** – 表が存在し、挿入のための互換性がある必要があります

**Create** – 表が作成されます (表が既に存在する場合はエラーが返されます)

**Drop** – 参照される表が存在する場合は除去され、再作成されます

**Refresh** – 表の既存の行は、新しい行が挿入される前に削除されます
   
table は、ジョブ結果の書き込み先のデータベース表名です。
node は、ストリームの端末ノード名です。ソース・ノード設定と同様に、node は、指定されたこれらのパラメーターを使用して構成される DB エクスポート・ノードによって置き換えられる、Modeler ストリーム内の元の出力ノードを示しています。

**注:** エクスポート・ノード設定とソース・ノード設定は、ジョブを正常に実行するために重要です。

### パラメーター値のオーバーライド

ジョブの実行前にストリーム・レベル・パラメーターのデフォルト値をオーバーライドするには、ジョブ定義で使用する名前/値のペアを指定します。

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

評価ジョブ・タイプの実行時に返される評価文書の望ましいレポート・フォーマット

評価ジョブの実行から生成されるすべての文書が 1 つの .zip ファイルに組み込まれます。示されている reportFormat をサポートできる文書ビルダー・ノードのみが実行され、ジョブが正常に実行された後でその内容が返されます。

サポートされるフォーマットは、HTML、JPG、PNG、RTF、SAV、TAB、および XML です。

```
"reportFormat": "HTML"```
{: codeblock}

### 完全な JSON の例

```
{ 
     "action": "TRAINING",
     "model": {
          "id": "str2"
          "name": "stream1.str"
     },
     "dbDefinitions":{
          "db":{
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
                    "port":50001,
                    "db":"BLUDB",
                    "username":"bluadmin",
                    "password":"NmI4MDViMzBkNDUz",
                    "options":"Security=SSL"
               }
     },
     "setting": {
          "inputs": [
                    {
                         "odbc": {
"dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## バッチ・ジョブ API の詳細

以降のセクションでは、バッチ・ジョブの IBM® SPSS® Modeler ファイル管理 API の詳細を示します。

`PUT /v1/file/{id}`

説明: バッチ・ジョブで使用するために IBM® SPSS® Modeler ストリーム・ファイルをアップロードします。

**注:** 指定された ID で既に保管されたファイルがある場合、そのファイルは上書きされます。

### コンテンツ・タイプ

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})```
{: codeblock}

### パラメーター

プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定の `file ID`。 サービス・インスタンス・リポジトリーで固有でなければなりません。

```
@PathParam("id") ```
{: codeblock}

### 応答

成功。サービス・インスタンスのリポジトリーからファイルをダウンロードするために使用できる URL を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`DELETE /v1/file/{id}`

説明: 指定された ID で保管されたファイルをサービス・インスタンス・リポジトリーから削除します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

アップロード時にユーザーが指定したファイルの ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。指定されたファイルは削除されています。

```
@ApiResponse(code = 200)```
{: codeblock}

エラー。この ID で保管されたファイルはサービス・インスタンス・リポジトリーに存在しませんでした。

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`GET /v1/file/`

説明: バッチ・ジョブの処理のためにサービス・インスタンス・リポジトリーで管理されるすべてのファイルの ID のリストを返します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

応答:

成功。`file ID` 値の JSON 配列を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

`GET /v1/file/{id}`

説明: バッチ・ジョブの処理で使用するために、指定された ID で保管された IBM® SPSS® Modeler ストリーム・ファイルを取得します。

コンテンツ・タイプ:

```
@Produces({ "application/octet-stream" })```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

アップロード時にユーザーが指定したファイルの ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。IBM® SPSS® Modeler ファイルを返します。

```
@ApiResponse(code = 200)```
{: codeblock}

エラー。この ID で保管されたファイルはサービス・インスタンス・リポジトリーに存在しませんでした。

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

## 詳細はこちら

ノートブックでのバッチ・ジョブの導入の例については、[From SPSS stream to batch scoring with Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37) を参照してください。

ジョブ定義 JSON の詳細については、[ジョブ定義 JSON](#job-definition-json) を参照してください。
