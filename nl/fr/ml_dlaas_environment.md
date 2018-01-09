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

# Installation de l'interface de ligne de commande (CLI)

Pour mener des expérimentations d'apprentissage en profondeur à l'aide d'{{site.data.keyword.pm_full}}, vous devez tout d'abord [configurer votre environnement](ml_getting_access.html), puis installer l'interface de ligne de commande qui prend en charge la création et la surveillance des sessions d'apprentissage.
{: shortdesc}

## Installation de l'interface de ligne de commande

1.  Installez l'[interface de ligne de commande](http://clis.ng.bluemix.net/ui/home.html), qui nécessite que vous ayez au préalable installé l'[interface CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html).
2.  Téléchargez une version du plug-in d'interface CLI à partir de la page [ML CLI releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) de ce référentiel. Veillez à sélectionner la version appropriée pour votre plateforme.
3. Installez le plug-in d'interface CLI (par exemple ml_cli_plugin_osx sous OSX)

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Exemple de sortie :

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  Vérifiez que le plug-in d'interface CLI est installé

   ```
   bx ml help
   ```
   {: codeblock}

Exemple de sortie :

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

## Configuration de l'interface de ligne de commande avec les informations de votre environnement

Pour pouvoir utiliser les autres commandes de l'interface de ligne de commande, vous devez définir quelques variables d'environnement dans votre système d'exploitation. Si vous travaillez sous Linux, exécutez les commandes ci-dessous en remplaçant les valeurs ML_USERNAME et ML_PASSWORD par vos données d'identification. Vous pouvez obtenir les valeurs nécessaires en suivant les [instructions d'extraction de vos données d'identification](ml_getting_access.html#retrieving-your-credentials) vers votre instance d'{{site.data.keyword.pm_full}}.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## Démarrage d'une session d'apprentissage de modèle

Vous êtes maintenant prêt à [créer et démarrer vos sessions d'apprentissage](ml_dlaas_working_with_new_models.html) à l'aide de l'interface de ligne de commande. Vous pouvez également en apprendre davantage en [formant un exemple de modèle de réseau de neurones](ml_dlaas_working_with_sample_models.html).

### Mise à niveau de l'interface de ligne de commande

Consultez la page [ML CLI releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) pour connaître les versions les plus récente de ML CLI. Nous vous conseillons de télécharger la version d'interface de ligne de commande la plus à jour pour disposer des dernières fonctionnalités. Une fois que vous avez téléchargé la dernière version d'interface de ligne de commande ML pour votre système d'exploitation, désinstallez le plug-in (si une version plus ancienne existe), puis installez la nouvelle version.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Exemple de sortie :

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Installez la nouvelle version à l'aide de la commande suivante :

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Exemple de sortie :

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
