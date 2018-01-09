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

# Umgebung einrichten

Um {{site.data.keyword.pm_full}} verwenden zu können, müssen Sie in der Lage sein, die richtige Maschinenlernumgebung zu erstellen und die Berechtigungsnachweise abzurufen, die für diese Umgebung spezifisch sind. Sie können die {{site.data.keyword.Bluemix}}-Instanz entweder über die [Befehlszeilenschnittstelle von Cloud Foundry](https://github.com/cloudfoundry/cli#getting-started) oder über die grafische Benutzerschnittstelle im {{site.data.keyword.Bluemix}}-Dashboard erstellen.
{: shortdesc}

## IBM Cloud-Dashboard verwenden

Für die Verwendung von {{site.data.keyword.pm_full}} müssen Sie davon [eine Instanz erstellen](https://console.bluemix.net/catalog/services/machine-learning). Dies geschieht über Ihr {{site.data.keyword.Bluemix_notm}}-Dashboard.

Abhängig von Ihrem Szenario benötigen Sie möglicherweise auch die folgenden Ressourcen:

- Object Storage (Stapelbereitstellung)
- Db2 Warehouse on Cloud (Stapelbereitstellung)
- Apache Spark (Stapel- oder Datenstrombereitstellung sowie fortlaufendes Ausbildungssystem)
- Message Hub (Datenstrombereitstellung)

Wenn Sie sehen möchten, wie Sie über das Dashboard die {{site.data.keyword.Bluemix_notm}}-Instanzen erstellen und Berechtigungsnachweise anzeigen können, sehen Sie sich das folgende Video an, das die im Text beschriebenen Schritte ergänzt, die dem Video folgen.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Abb. 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Wenn Sie das auf dieser Seite eingebettete Video nicht ansehen können, können Sie über die YouTube-Website darauf zugreifen. (Wird in einer neuen Registerkarte oder einem neuen Fenster geöffnet)">    <img src="images/video.png" alt="Video-Symbol"></a>IBM Watson Machine Learning: Erste Schritte</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Dieses Video gibt einen Überblick über die Vorgehensweise zum Einrichten Ihrer Umgebung für maschinelles Lernen.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## {{site.data.keyword.Bluemix_notm}}-Instanzen erstellen

Um die {{site.data.keyword.pm_full}}-Instanz zu erstellen, müssen Sie die folgenden Schritte ausführen:

1. Öffnen Sie die {{site.data.keyword.pm_full}} [Serviceseite ](https://console.bluemix.net/catalog/services/machine-learning).
2. Registrieren Sie sich oder melden Sie sich an, um die Serviceinstanz zu erstellen.
3. Geben Sie einen beschreibenden Namen für Ihre Instanz ein, wählen Sie einen Speicherbereich aus und wählen Sie Ihren Datenplan aus.
4. Klicken Sie auf **Instanz erstellen**.

Ihre Instanz wird geöffnet.

## Berechtigungsnachweise abrufen

Um Ihre {{site.data.keyword.Bluemix_notm}}- (bisher als Bluemix bezeichnete) Instanz verwenden zu können, benötigen Sie Berechtigungsnachweise für die Serviceinstanz. Sie können Ihre Berechtigungsnachweise entweder über die [CF-CLI](using_pm_service.html) oder über das {{site.data.keyword.Bluemix_notm}}-Dashboard erstellen bzw. darauf zugreifen. Nach dem Binden der {{site.data.keyword.pm_short}}-Serviceinstanz an Ihre {{site.data.keyword.Bluemix_notm}}-Anwendung werden die {{site.data.keyword.pm_short}}-Berechtigungsnachweise zur Umgebungsvariablen `VCAP_SERVICES` hinzugefügt. Weitere Informationen finden Sie im Abschnitt [Den Service mit einer Anwendung verwenden](using_pm_service.html).

Zum Abrufen von {{site.data.keyword.pm_full}}-Instanzberechtigungsnachweisen müssen Sie die folgenden Schritte ausführen:

1. [Melden Sie sich bei {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter) an.
2. Klicken Sie im Abschnitt **Services** auf den Servicenamen, dessen Berechtigungsnachweise abgerufen werden sollen.
3. Klicken Sie im Navigationsfenster auf **Serviceberechtigungsnachweise**.
4. Klicken Sie in der Liste der Berechtigungsnachweise auf **Berechtigungsnachweise anzeigen**.
5. Wenn für diesen Service keine Berechtigungsnachweise vorhanden sind, erstellen Sie sie, indem Sie auf **Berechtigungsnachweise erstellen** klicken.
6. Zum Kopieren der Berechtigungsnachweise klicken Sie auf das Symbol für Kopieren.

Um den {{site.data.keyword.pm_short}}-Service in Ihrer Anwendung verwenden zu können, müssen Sie den Service an die {{site.data.keyword.Bluemix_notm}}-Anwendung binden und die erforderlichen Berechtigungsnachweise für die Ausführung der Anwendung in die Umgebungsvariable `VCAP_SERVICES` einfügen. Weitere Informationen finden Sie im Abschnitt [Befehlszeilenschnittstelle von Cloud Foundry](#cloud-foundry-command-line-interface).

## Cloud Foundry-CLI (Befehlszeilenschnittstelle)

### Schritte zum Binden des Service an eine {{site.data.keyword.Bluemix_notm}}-Anwendung

Sie können Node.js-[Beispielcode](https://github.com/pmservice/product-line-prediction/blob/master/README.md) herunterladen, um den
Machine Learning-Service zu testen. Führen Sie die folgenden Schritte aus, um Ihre {{site.data.keyword.Bluemix_notm}}-Anwendung zu erstellen und den {{site.data.keyword.pm_short}}-Service anzubinden. Diese Beispiele verwenden Node.js, da es eine gängige Laufzeit ist. Es können auch andere verwendet werden, wie z. B. iOS, Ruby, Perl oder Java.

Sie können die Schritte 1 bis 3 auch über die grafische Schnittstelle von {{site.data.keyword.Bluemix_notm}} statt über das Cloud Foundry-Werkzeug ('cf') ausführen.

1. Verwenden Sie den Befehl `cf create-service`, um eine Serviceinstanz zu erstellen:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Der folgende Befehl beispielsweise erstellt genau eine {{site.data.keyword.pm_short}}-Serviceinstanz
   mit dem Lite-Plan mit der Bezeichnung `my_wml_lite` in Ihrem {{site.data.keyword.Bluemix_notm}}-Bereich:

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Verwenden Sie den Befehl `cf
create-service-key`, um Serviceberechtigungsnachweise zu
erstellen:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Der folgende Befehl beispielsweise erstellt {{site.data.keyword.pm_short}}-Berechtigungsnachweise:

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. Verwenden Sie den Befehl `cf bind-service` zum Binden der Serviceinstanz
   `my_wml_lite` an Ihre Anwendung.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   Der folgende Befehl beispielsweise bindet die {{site.data.keyword.pm_short}}-Serviceinstanz
   `my_wml_lite` an die {{site.data.keyword.Bluemix_notm}}-Anwendung `my_app1`:

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Über die Variable `VCAP_SERVICES` auf Berechtigungsnachweise zugreifen

Nach dem Binden der {{site.data.keyword.pm_short}}-Serviceinstanz an Ihre {{site.data.keyword.Bluemix}}-Anwendung werden die {{site.data.keyword.pm_short}}-Berechtigungsnachweise zur Umgebungsvariablen `VCAP_SERVICES` hinzugefügt:

```
    {
     "pm-20-dev": [
       {
         "credentials": {
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

   Die Umgebungsvariable `VCAP_SERVICES` umfasst die folgenden Informationen:

<dl>
<dt>plan</dt>
<dd>Der {{site.data.keyword.pm_short}}-Plan, der in der Servicebereitstellung verwendet wird.</dd>
<dt>url</dt><dd>Die Adresse der {{site.data.keyword.pm_short}}-Serviceinstanz.
<dt>access_key</dt><dd>Nur für IBM® SPSS® Modeler-Datenströme.</dd>
<dt>instance_id</dt><dd>Die eindeutige ID der {{site.data.keyword.pm_short}}-Instanz.</dd>
<dt>username</dt><dd>Der Benutzername, der Teil der grundlegenden Berechtigung ist, die zur Generierung eines Zugriffstokens erforderlich ist, das in allen Anforderungen an diese Serviceinstanz übergeben wird.</dd>
<dt>password</dt><dd>Das Kennwort, das Teil der grundlegenden Berechtigung ist, die zur Generierung eines Zugriffstokens erforderlich ist, das in allen Anforderungen an diese Serviceinstanz übergeben wird. </dd>
</dl>

Sie können z. B. ein Zugriffstoken mit dem folgenden `curl`-Befehl generieren:

Anforderungsbeispiel:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Ausgabebeispiel:

       {"token":"**********"}
```
{: codeblock}

   Der folgende Node.js-Code ist ein Beispiel für das Abrufen der Werte für Benutzernamen
   (`username`) und Kennwort (`password`) aus der Umgebungsvariablen
   `VCAP_SERVICES`:

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
