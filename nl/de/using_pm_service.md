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

# Service verwenden

Unter Verwendung der Modelliermethoden in der IBM® SPSS® Modeler-Modellierungs-
palette können Sie neue Informationen aus Ihren Daten ableiten und
Vorhersagemodelle entwickeln. Jede Methode weist bestimmte Stärken auf
und eignet sich für spezielle Typen von Problemen beim maschinellen Lernen.
{: .shortdesc}

Ausführliche Informationen
zu IBM® SPSS® Modeler und seinen Modellierungsalgorithmen finden Sie im
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Nachdem die Ein- und Ausgabeanforderungen Ihrer {{site.data.keyword.Bluemix_notm}}-
Anwendung und das Design der Scoring-Verzweigung für IBM® SPSS® Modeler
bereitgestellt wurden, kann der Datenanalyst jeden internen Aspekt
der Scoring-Verzweigung ändern. Der Datenanalyst kann sogar die Modellalgorithmen ändern, die in einer Aktualisierungsoperation verwendet werden und so sicherstellen, dass die Vorhersageanalyse optimiert werden kann, ohne dass die Anwendungen neu programmiert werden müssen.


## Schritte zum Binden des Service an eine {{site.data.keyword.Bluemix_notm}}-Anwendung

Führen Sie die folgenden Schritte aus, um Ihre {{site.data.keyword.Bluemix_notm}}-Anwendung zu erstellen und an den {{site.data.keyword.pm_short}}-Service zu binden.

1. Laden Sie den Beispielanwendungscode für Node.js aus dem [GitHub-Repository](https://github.com/pmservice/customer-satisfaction-prediction) herunter.

2. Verwenden Sie den Befehl `cf create-service`, um eine Serviceinstanz zu erstellen:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Beispiel:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   Dieser Befehl erstellt eine {{site.data.keyword.pm_short}}-Serviceinstanz
   mit einem Lite-Plan namens 'my_pm_lite' in Ihrem {{site.data.keyword.Bluemix_notm}}-Bereich.

3. Verwenden Sie den Befehl `cf
create-service-key`, um Serviceberechtigungsnachweise zu
erstellen:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Beispiel:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Dieser Befehl erstellt
{{site.data.keyword.pm_short}}-Serviceberechtigungsnachweise.

4. Verwenden Sie den Befehl `cf bind-service`, um die Serviceinstanz
   `my_pm_lite` an Ihre Anwendung zu binden.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   Beispiel:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   Dieser Befehl bindet die {{site.data.keyword.pm_short}}-Serviceinstanz
   `my_pm_lite` an die {{site.data.keyword.Bluemix_notm}}-Anwendung 'my_app1'.

5. {{site.data.keyword.pm_short}}-Berechtigungsnachweise:

   Nach dem Binden der {{site.data.keyword.pm_short}}-Serviceinstanz an Ihre
   {{site.data.keyword.Bluemix_notm}}-Anwendung werden die {{site.data.keyword.pm_short}}-Berechtigungsnachweise
   zur Umgebungsvariablen `VCAP_SERVICES` hinzugefügt:

```
    {   
        "pm-20": {
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   Die Umgebungsvariable `VCAP_SERVICES` umfasst die folgenden Informationen:

   <dl>

   <dt>plan</dt>
   <dd>Der {{site.data.keyword.pm_short}}-Plan, der für die Servicebereitstellung verwendet wird.</dd>

   <dt>url</dt>
   <dd>Die Adresse der {{site.data.keyword.pm_short}}-Serviceinstanz.</dd>

   <dt>access_key</dt>
   <dd>Der Abfrageparameter accessKey, der in allen Anforderungen an diese Serviceinstanz übergeben
werden muss.</dd>

   </dl>

Beispiel:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Node.js-Beispielcode, in dem dargestellt ist, wie der Zugriffsschlüssel (accessKey)
aus der Umgebungsvariablen `VCAP_SERVICES` abgerufen werden kann:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## Weitere Informationen

Sind Sie bereit? Informationen zum Erstellen einer Serviceinstanz oder zum Binden
einer Anwendung finden Sie unter [Service mit Spark- und Python-Modellen verwenden](using_pm_service_dsx.html) oder
[Service mit IBM® SPSS®-Modellen verwenden](using_pm_service.html).

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Modelle](pm_service_api_spark.html) oder [Service-
API für IBM® SPSS® Modelle] (pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie im [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM Data Science Experience und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
