---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installing the Command line interface (CLI)

To conduct deep learning modeling runs by using {{site.data.keyword.pm_full}}, first [set up your environment](ml_getting_access.html) then install the command line interface (CLI) which supports creating and monitoring training runs.
{: shortdesc}

## Install the command line interface

### Install the IBM Cloud CLI

First install the [IBM Cloud command line interface](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started)

You can then invoke the IBM Cloud CLI using `bx` or `bluemix`

### Install the Machine Learning CLI plugin

Install the latest version of the Machine Learning CLI plugin using the following command:

```
bx plugin install machine-learning
```
{: codeblock}
   
Sample output:
  
```
$ bx plugin install machine-learning
Looking up 'machine-learning' from repository 'Bluemix'...
Plug-in 'machine-learning 1.9.0' found in repository 'Bluemix'
Attempting to download the binary file...
13.08 MiB / 13.08 MiB [===========================================] 100.00% 4s
13715732 bytes downloaded
Installing binary...
OK
Plug-in 'machine-learning 1.9.0' was successfully installed into /Users/Smith/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}

Verify that the CLI plug-in is installed

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

## Start a model training run

You're now ready to [create and start your training runs](ml_dlaas_working_with_new_models.html) using the CLI.  Or, to learn more, you can [start by training an sample neural network model](ml_dlaas_working_with_sample_models.html).

### CLI Upgrade

Check this page for new versions of the ML CLI. After you download a new version of ML CLI for your operating system, you need to uninstall the plug-in (if you already have an older
version installed) and then install the new version.

```
bx plugin update machine-learning
```
{: codeblock}

Sample output:

```
Checking upgrades for plug-in 'machine-learning' from repository 'Bluemix'...
No updates are available.
```
{: codeblock}

### CLI Uninstall

Uninstall the new version by running the following command:

```
bx plugin uninstall machine-learning
```
{: codeblock}

Sample output:

```
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```
{: codeblock}

### Checking the CLI version

To display the version of the ML CLI that is currently installed use the following command:

```
bx ml version
```
{: codeblock}

Sample output:

```
Git Commit Hash: 90ee3d4a5c832a8c1a0d749c1a7c4883fd92b50b
UTC Build Time : 20180118.120022
Plugin Version : 1.9.0
```
{: codeblock}
