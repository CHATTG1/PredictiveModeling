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

# Vorhersagemodell implementieren oder aktualisieren

Zum Bereitstellen oder Aktualisieren eines Vorhersagemodells mithilfe des Service verwenden Sie einen API-Aufruf zum Hochladen einer Datei, die die Scoring-Verzweigung enthält, die mit IBM® SPSS® Modeler entwickelt
wurde. Sie wird für das Scoring von Daten in Ihren Anwendungen zur Verfügung gestellt. Jeder Modelldatei
wird eine Kontext-ID als Alias zugeordnet, der zum Referenzieren des bereitgestellten Modells
in nachfolgenden Serviceaufrufen verwendet wird. Wenn für eine Kontext-ID ein Modell vorhanden ist,
wird sie durch diesen PUT-Aufruf ersetzt, um die momentan von Ihren Anwendungen benutzte Vorhersageanalyse
zu aktualisieren.
{: shortdesc}

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Anforderungsbeispiel:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Implementierung:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

Antwort bei fehlgeschlagener Implementierung:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
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
