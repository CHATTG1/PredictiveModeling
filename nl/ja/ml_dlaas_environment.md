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

# コマンド・ライン・インターフェース (CLI) のインストール

{{site.data.keyword.pm_full}} を使用して深層学習試験を実施するには、最初に[環境のセットアップ](ml_getting_access.html)を行い、次に、トレーニング実行の作成とモニターをサポートするコマンド・ライン・インターフェース (CLI) をインストールします。
{: shortdesc}

## コマンド・ライン・インターフェースのインストール

1.  [コマンド・ライン・インターフェース](http://clis.ng.bluemix.net/ui/home.html)をインストールします。これには、手順で説明されているように、まず [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html) をインストールすることが必要です。
2.  [ML CLI リリース](https://github.ibm.com/NGP-TWC/wml-cli/releases)のページを使用して、このリポジトリーからいずれかのバージョンの CLI プラグインをダウンロードします。ご使用のプラットフォームに合った正しいバージョンを取得するよう注意してください。
3. CLI プラグイン (例えば OSX では、これは ml_cli_plugin_osx という名前でダウンロードされています) をインストールします。

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   サンプル出力:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  CLI プラグインがインストールされたことを確認します。

   ```
   bx ml help
   ```
   {: codeblock}

サンプル出力:

```
NAME:
   bx ml - Manage machine learning lifecycle on Bluemix
USAGE:
   bx ml command [arguments...] [command options]
COMMANDS:
   show      Get detailed information about models/deployments/trained-models
   train     Start training a model
   delete    Delete a deployment/trained-model
   list      List all models/deployments/frameworks/trained-models
   version   show git hash and build time of cli
   deploy    Deploy a model for scoring
   score     Score the model. Sample scoring json format -  {"modelId": "sample", "deploymentId": "sample","payload": {"fields": [],"values": []}}
   help      Enter 'bx ml help [command]' for more information about a command.
```
{: codeblock}

## 環境情報を使用した CLI の構成

CLI からその他のコマンドを使用するには、オペレーティング・システムでいくつかの環境変数を設定する必要があります。Linux で作業している場合は、以下のコマンドを実行します。その際、ML_USERNAME および ML_PASSWORD の値は実際の資格情報に置き換えてください。{{site.data.keyword.pm_full}} のインスタンスへの[アクセス資格情報の取得手順](ml_getting_access.html#retrieving-your-credentials)に従って、必要な値を取得できます。  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## モデル・トレーニング実行の開始

これで、CLI を使用して[トレーニング実行の作成と開始](ml_dlaas_working_with_new_models.html)を行う準備ができました。または、[ニューラル・ネットワーク・モデルのサンプルをトレーニングする](ml_dlaas_working_with_sample_models.html)ことから始めて、さらに詳細を学習することもできます。

### CLI アップグレード

[ML CLI リリース](https://github.ibm.com/NGP-TWC/wml-cli/releases)で、ML CLI の新しいバージョンがないか確認してください。最新機能を利用するために最新リリースの CLI をダウンロードすることをお勧めします。ご使用のオペレーティング・システムに対応する最新バージョンの ML CLI をダウンロードした後、プラグインをアンインストールし (インストール済みの古いバージョンが既にある場合)、その後で新バージョンをインストールする必要があります。

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

サンプル出力:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

次のコマンドを実行して新バージョンをインストールします。

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

サンプル出力:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```
{: codeblock}
