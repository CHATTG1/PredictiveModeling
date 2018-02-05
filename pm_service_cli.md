---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Command line interface (CLI)

You can use CLI to work with {{site.data.keyword.pm_full}}.
{: shortdesc}

## Install the command line interface

1.  First install the [IBM Cloud command line interface](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started)
2.  Download the Machine Learning CLI v1.8 plug-in from the [ML CLI 1.8 release](https://github.ibm.com/NGP-TWC/wml-cli/releases/tag/v1.8) page. Be sure to get the right download for your platform.
3. Install the CLI plug-in (for example on OSX this downloaded as ml_cli_plugin_osx)

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Sample output:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  Verify that the CLI plug-in is installed

   ```
   bx ml help
   ```
   {: codeblock}

Sample output:

```
NAME:
   bx ml - Manage machine learning lifecycle on Bluemix
USAGE:
   bx ml command [arguments...] [command options]

COMMANDS:
   show      Get detailed information about models/deployments/training-runs
   train     Start training a model
   delete    Delete a model/deployment/training-run
   cancel    Cancel training a model
   list      List all models/deployments/training-runs
   monitor   Start fetching status messages of a training-run
   version   show git hash and build time of cli
   deploy    Deploy a model for scoring
   score     Score the model. Sample scoring json format -  {"modelId": "sample", "deploymentId": "sample","payload": {"fields": [],"values": []}}
   store     Store model to WML repostory
   help

Enter 'bx ml help [command]' for more information about a command.
```
{: codeblock}

## Configure the CLI with your environment info

To use the other commands from the CLI, you must set a few environment variables in your operating system.  If you are working in Linux, then run the commands below while replacing the values for ML_USERNAME and ML_PASSWORD with your credentials. You can get the necessary values by following the [directions to  retrieve your  access credentials](ml_getting_access.html#retrieving-your-credentials) to your instance of  {{site.data.keyword.pm_full}}.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

### CLI Upgrade

Check this page for new versions of the ML CLI. After you download a new version of ML CLI for your operating system, you need to uninstall the plug-in (if you already have an older
version installed) and then install the new version.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Sample output:

```
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Install the new version by running the following command:

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Sample output:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
