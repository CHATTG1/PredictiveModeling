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

# 環境のセットアップ

{{site.data.keyword.pm_full}} を使用するには、適切な機会学習環境を作成することと、その環境に固有の資格情報を取得することができなければなりません。{{site.data.keyword.Bluemix}} インスタンスの作成は、[Cloud Foundry コマンド・ライン・インターフェース](https://github.com/cloudfoundry/cli#getting-started)を使用することによって、または、{{site.data.keyword.Bluemix}} ダッシュボードでグラフィカル・ユーザー・インターフェースを介して行うことができます。
{: shortdesc}

## IBM Cloud ダッシュボードの使用

{{site.data.keyword.pm_full}} を使用するには、{{site.data.keyword.Bluemix_notm}} ダッシュボードにその[インスタンスを作成](https://console.bluemix.net/catalog/services/machine-learning)する必要があります。

シナリオによっては、以下のリソースが必要になることもあります。

- オブジェクト・ストレージ (バッチ・デプロイメント)
- Db2 Warehouse on Cloud (バッチ・デプロイメント)
- Apache Spark (バッチ、ストリーム・デプロイメント、および継続学習システム)
- Message Hub (ストリーム・デプロイメント)

ダッシュボードを使用して {{site.data.keyword.Bluemix_notm}} インスタンスの作成と資格情報の表示を行う方法については、次のビデオを視聴してください。このビデオは、下に記述された手順を補足しています。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>図 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="このページに組み込まれているビデオにアクセスできない場合、YouTube Web サイトからビデオにアクセスできます。(新しいタブまたはウィンドウで開きます)">    <img src="images/video.png" alt="ビデオ・アイコン"></a>IBM Watson Machine Learning: Get Started</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>このビデオでは、機械学習環境のセットアップ方法の概要が説明されています。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## {{site.data.keyword.Bluemix_notm}} インスタンスの作成

{{site.data.keyword.pm_full}} インスタンスを作成するには、以下の手順を実行する必要があります。

1. {{site.data.keyword.pm_full}} [サービス・ページ](https://console.bluemix.net/catalog/services/machine-learning)を開きます。
2. サービス・インスタンスを作成するために、登録またはログインします。
3. インスタンスの記述名を入力し、スペースを選択し、データ・プランを選択します。
4. **「インスタンスの作成 (Create Instance)」**をクリックします。

インスタンスが開きます。

## 資格情報の取得

{{site.data.keyword.Bluemix_notm}} (旧称 Bluemix) インスタンスを使用するには、サービス・インスタンス資格情報が必要です。[CF CLI](using_pm_service.html) または {{site.data.keyword.Bluemix_notm}} ダッシュボードのいずれかを使用して、資格情報を作成し、アクセスすることができます。{{site.data.keyword.pm_short}} サービス・インスタンスをお使いの {{site.data.keyword.Bluemix_notm}} アプリケーションにバインドすると、{{site.data.keyword.pm_short}} 資格情報が `VCAP_SERVICES` 環境変数に追加されます。詳しくは、[アプリケーションでのサービスの使用](using_pm_service.html)に関するセクションを参照してください。

{{site.data.keyword.pm_full}} インスタンス資格情報を取得するには、以下の手順を実行する必要があります。

1. [{{site.data.keyword.Bluemix_notm}} にログインします](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. **「サービス」**セクションで、資格情報を取得したいサービスの名前をクリックします。
3. ナビゲーション・ペインで**「サービス資格情報」**をクリックします。
4. 資格情報リストで**「資格情報の表示」**をクリックします。
5. このサービスの資格情報が存在しない場合、**「資格情報の作成 (Create credentials)」**をクリックして作成します。
6. 資格情報をコピーするには、コピー・アイコンをクリックします。

アプリケーション内で {{site.data.keyword.pm_short}} サービスを使用するには、サービスを {{site.data.keyword.Bluemix_notm}} アプリケーションにバインドする必要があります。そうすると、アプリケーションの実行に必要な資格情報が `VCAP_SERVICES` 環境変数に挿入されます。詳しくは、[Cloud Foundry コマンド・ライン・インターフェース](#cloud-foundry-command-line-interface)を参照してください。

## Cloud Foundry CLI (コマンド・ライン・インターフェース) の使用

### サービスを {{site.data.keyword.Bluemix_notm}} アプリケーションとバインドする手順

Node.js [サンプル・コード](https://github.com/pmservice/product-line-prediction/blob/master/README.md)をダウンロードして、Machine
Learning サービスを試してみることができます。以下のステップを実行して、{{site.data.keyword.Bluemix_notm}} アプリケーションを作成し、{{site.data.keyword.pm_short}} サービスをバインドします。これらの例では、一般的なランタイムである Node.js を使用しています。iOS、Ruby、Perl、あるいは Java など、他のものも使用できます。

Cloud Foundry ツール (cf) の代わりに {{site.data.keyword.Bluemix_notm}} グラフィカル・インターフェースを使用して、ステップ 1 から 3 までを実行することもできます。

1. `cf create-service` コマンドを使用して、サービス・インスタンスを作成します。


   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例えば、次のコマンドは、{{site.data.keyword.Bluemix_notm}} スペース内に、`my_wml_lite` という名前で、ライト・プランを使用する 1 つの {{site.data.keyword.pm_short}} サービス・インスタンスを作成します。

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. `cf create-service-key` コマンドを使用して、サービス資格情報を作成します。

   ```
cf create-service-key "{service instance name}" {vcap key name}```
   {: codeblock}

   例えば、次のコマンドは、{{site.data.keyword.pm_short}} サービス資格情報を作成します。

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. `cf bind-service` コマンドを使用して、サービス・インスタンス `my_wml_lite` をアプリケーションにバインドします。

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   例えば、次のコマンドは、{{site.data.keyword.pm_short}} サービス・インスタンス `my_wml_lite` を {{site.data.keyword.Bluemix_notm}} アプリケーション `my_app1` にバインドします。

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### `VCAP_SERVICES` 変数による資格情報へのアクセス

{{site.data.keyword.pm_short}} サービス・インスタンスをお使いの {{site.data.keyword.Bluemix}} アプリケーションにバインドすると、{{site.data.keyword.pm_short}} 資格情報が `VCAP_SERVICES` 環境変数に追加されます。

```
    {
     "pm-20-dev": [
       {
         "credentials":{
      "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   `VCAP_SERVICES` 環境変数には以下の情報が含まれます。


<dl>
<dt>plan</dt>
<dd>サービス・プロビジョニングで使用される {{site.data.keyword.pm_short}} プラン。</dd>
<dt>url</dt><dd>{{site.data.keyword.pm_short}} サービス・インスタンスのアドレス。
<dt>access_key</dt><dd>IBM® SPSS® Modeler ストリームの場合のみ。</dd>
<dt>instance_id</dt><dd>{{site.data.keyword.pm_short}} インスタンスの固有 ID。</dd>
<dt>username</dt><dd>ユーザー名。これは、このサービス・インスタンスへのすべての要求で渡されるアクセス・トークンを生成するために必要な基本的な許可の一部です。</dd>
<dt>password</dt><dd>パスワード。これは、このサービス・インスタンスへのすべての要求で渡されるアクセス・トークンを生成するために必要な基本的な許可の一部です。</dd>
</dl>

例えば、以下の `curl` コマンドを使用してアクセス・トークンを生成できます。

要求の例:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       出力例:

       {"token":"**********"}
```
{: codeblock}

   以下の Node.js コードは、`VCAP_SERVICES` 環境変数から `username` 値および `password` 値を取得する方法の例です。

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
