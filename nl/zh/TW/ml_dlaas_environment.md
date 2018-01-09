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

# 安裝指令行介面 (CLI)

若要使用 {{site.data.keyword.pm_full}} 來處理深度學習實驗，請先[設定環境](ml_getting_access.html)，然後再安裝支援建立及監視訓練執行的指令行介面 (CLI)。
{: shortdesc}

## 安裝指令行介面

1.  安裝[指令行介面](http://clis.ng.bluemix.net/ui/home.html)，如其指示所述，您需要先安裝 [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html)。
2.  從此儲存庫的 [ML CLI 版本](https://github.ibm.com/NGP-TWC/wml-cli/releases)頁面中，下載 CLI 外掛程式的一個版本。請務必取得平台適用的正確版本。
3. 安裝 CLI 外掛程式（例如，在 OSX 上，這下載為 ml_cli_plugin_osx）

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   輸出範例：

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  驗證已安裝 CLI 外掛程式

   ```
   bx ml help
   ```
   {: codeblock}

輸出範例：

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

## 使用環境資訊配置 CLI

若要從 CLI 使用其他指令，您必須在作業系統中設定一些環境變數。如果您是在 Linux 中運作，請在將 ML_USERNAME 及 ML_PASSWORD 的值取代為您的認證之後，執行以下指令。您可以遵循指示以[擷取存取認證](ml_getting_access.html#retrieving-your-credentials)至 {{site.data.keyword.pm_full}} 實例，來取得必要值。  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## 啟動模型訓練執行

您現在已準備好，可以使用 CLI 來[建立及啟動訓練執行](ml_dlaas_working_with_new_models.html)。或者，若要進一步瞭解，您可以[透過訓練範例類神經網路模型開始](ml_dlaas_working_with_sample_models.html)。

### CLI 升級

如需新版本的 ML CLI，請檢查 [ML CLI 版本](https://github.ibm.com/NGP-TWC/wml-cli/releases)。建議您下載最新版的 CLI，以取得最新特性。針對您的作業系統下載最新版的 ML CLI 之後，需要先解除安裝外掛程式（如果您已安裝舊版本），然後再安裝新版本。

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

輸出範例：

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

執行下列指令來安裝新版本：

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

輸出範例：

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
