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

# 安装命令行界面 (CLI)

要使用 {{site.data.keyword.pm_full}} 执行深度学习试验，请首先[设置环境](ml_getting_access.html)，然后安装支持创建和监视培训运行的命令行界面 (CLI)。
{: shortdesc}

## 安装命令行界面

1.  安装[命令行界面](http://clis.ng.bluemix.net/ui/home.html)，根据其指示信息说明，需要您首先安装 [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html)。
2.  从此存储库的 [ML CLI 发行版](https://github.ibm.com/NGP-TWC/wml-cli/releases)页面下载 CLI 插件的版本。确保获取适合您平台的正确版本。
3. 安装 CLI 插件（例如，在 OSX 上，下载的此插件为 ml_cli_plugin_osx）

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   样本输出：

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  验证 CLI 插件是否已安装

   ```
   bx ml help
   ```
   {: codeblock}

样本输出：

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

## 使用环境信息配置 CLI

要通过 CLI 使用其他命令，必须在操作系统中设置一些环境变量。如果是在 Linux 中工作，请运行以下命令并将 ML_USERNAME 和 ML_PASSWORD 的值替换为您的凭证。您可以按照[检索访问凭证的指导信息](ml_getting_access.html#retrieving-your-credentials)（{{site.data.keyword.pm_full}} 实例的访问凭证）来获取必需的值。  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## 启动模型培训运行

您现在已准备就绪，可以使用 CLI [创建并启动培训运行](ml_dlaas_working_with_new_models.html)。或者，要了解更多信息，可以[从培训样本神经网络模型开始](ml_dlaas_working_with_sample_models.html)。

### CLI 升级

检查 [ML CLI 发行版](https://github.ibm.com/NGP-TWC/wml-cli/releases)以了解 ML CLI 的新版本。我们鼓励您下载 CLI 的最新发行版，以获得最新的功能。下载适合操作系统的最新版本 ML CLI 后，需要先卸载该插件（如果已安装较旧的版本），然后再安装新版本。

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

样本输出：

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

通过运行以下命令来安装新版本：

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

样本输出：

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
