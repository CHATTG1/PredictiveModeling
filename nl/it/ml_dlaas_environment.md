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

# Installazione della CLI (Command line interface)

Per condurre esperimenti di apprendimento approfondito utilizzando {{site.data.keyword.pm_full}}, per prima cosa [configura il tuo ambiente](ml_getting_access.html) quindi installa la CLI (command line interface) che supporta la creazione e il monitoraggio delle esecuzioni di formazione.
{: shortdesc}

## Installa l'interfaccia riga di comando

1.  Installa l'[interfaccia riga di comando](http://clis.ng.bluemix.net/ui/home.html), che, come spiegato nelle relative istruzioni, richiede di aver prima installato la [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html).
2.  Scarica una versione del plugin della CLI dalla pagina [ML CLI releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) di questo repository. Assicurati di ottenere la versione corretta per la tua piattaforma.
3. Installa il plugin della CLI (ad esempio su OSX viene scaricato come ml_cli_plugin_osx)

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Output di
esempio:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  Verifica che il plugin della CLI sia installato

   ```
   bx ml help
   ```
   {: codeblock}

Output di
esempio:

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

## Configura la CLI con le informazioni del tuo ambiente

Per utilizzare gli altri comandi dalla CLI, devi configurare alcune variabili di ambiente nel tuo sistema operativo.  Se stai utilizzando Linux, immetti i seguenti comandi mentre sostituisci i valori per ML_USERNAME e ML_PASSWORD con le tue credenziali. Puoi ottenere i valori necessari seguendo le [indicazioni per richiamare le tue credenziali di accesso](ml_getting_access.html#retrieving-your-credentials) per la tua istanza di  {{site.data.keyword.pm_full}}.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## Avvia un'esecuzione di formazione del modello

Sei ora pronto per [creare e avviare le tue esecuzioni di formazione](ml_dlaas_working_with_new_models.html) utilizzando la CLI.  O, per ulteriori informazioni, puoi [iniziare formando un modello di rete neurale di esempio](ml_dlaas_working_with_sample_models.html).

### Aggiorna la CLI

Controlla [ML CLI releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) per le nuove versioni della CLI ML. Ti incoraggiamo a scaricare l'ultima release della CLI per avere le funzioni più aggiornate. Dopo aver scaricato
l'ultima versione della CLI ML per il tuo sistema operativo, devi disinstallare il plugin (se hai ancora una vecchia versione installata)
e quindi installare la nuova versione.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Output di
esempio:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Installa la nuova versione immettendo il seguente comando:

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Output di
esempio:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
