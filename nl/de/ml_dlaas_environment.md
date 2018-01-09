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

# Befehlszeilenschnittstelle (CLI) installieren

Um Deep-Learning-Experimente mithilfe von {{site.data.keyword.pm_full}} durchzuführen, [richten Sie zuerst Ihre Umgebung ein](ml_getting_access.html) und installieren dann die Befehlszeilenschnittstelle (CLI), die das Erstellen und Überwachen von Trainingsläufen unterstützt.
{: shortdesc}

## Befehlszeilenschnittstelle installieren

1.  Installieren Sie die [Befehlszeilenschnittstelle](http://clis.ng.bluemix.net/ui/home.html), die, wie in den Anweisungen erklärt, zunächst die Installation der [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html) erfordert.
2.  Laden Sie eine Version des CLI-Plug-ins von der Seite [ML-CLI-Releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) dieses Repositorys herunter. Stellen Sie sicher, dass Sie die richtige Version für Ihre Plattform erhalten.
3. Installieren Sie das CLI-Plug-in (zum Beispiel wurde dieses unter OSX als ml_cli_plugin_osx heruntergeladen).

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Beispielausgabe:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Verwenden Sie 'bx plugin show machine-learning', um seine Details anzuzeigen.
   ```

4.  Überprüfen Sie, ob das CLI-Plug-in installiert wurde.

   ```
   bx ml help
   ```
   {: codeblock}

Beispielausgabe:

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

## CLI mit eigenen Umgebungsinformationen konfigurieren

Wenn Sie die anderen Befehle über die Befehlszeilenschnittstelle verwenden möchten, müssen Sie einige Umgebungsvariablen in Ihrem Betriebssystem festlegen. Wenn Sie unter Linux arbeiten, führen Sie die folgenden Befehle aus, wobei Sie die Werte für ML_USERNAME und ML_PASSWORD durch Ihre Berechtigungsnachweise ersetzen. Sie können die erforderlichen Werte abrufen, indem Sie die [Anweisungen zum Abrufen der Zugriffsberechtigungsnachweise](ml_getting_access.html#retrieving-your-credentials) auf Ihre Instanz von {{site.data.keyword.pm_full}} befolgen.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## Modell-Trainingslauf starten

Sie können jetzt mithilfe der CLI [eigene Trainingsläufe erstellen und starten](ml_dlaas_working_with_new_models.html). Alternativ können Sie aber auch, um mehr zu erfahren, [starten, indem Sie ein Beispielmodell für ein neuronales Netz trainieren](ml_dlaas_working_with_sample_models.html).

### CLI-Upgrade

Überprüfen Sie die [ML-CLI-Releases](https://github.ibm.com/NGP-TWC/wml-cli/releases) auf neue Versionen der ML-CLI. Wir empfehlen Ihnen, das neueste Release der CLI herunterzuladen, um die aktuellsten Funktionen zu erhalten. Nach dem Download
der neuesten Version der ML-CLI für Ihr Betriebssystem müssen Sie das Plug-in deinstallieren (wenn Sie bereits eine ältere
Version installiert hatten) und dann die neue Version installieren.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Beispielausgabe:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Installieren Sie die neue Version, indem Sie den folgenden Befehl ausführen:

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Beispielausgabe:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Verwenden Sie 'bx plugin show machine-learning', um seine Details anzuzeigen.
```
{: codeblock}
