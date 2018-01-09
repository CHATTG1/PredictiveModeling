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

# Implementierte SPSS-Modelle verwalten

Sie können die {{site.data.keyword.pm_full}}-Service-API verwenden, um eine Datei hochzuladen, die die bereitzustellende Scoring-Verzweigung von IBM® SPSS® Modeler enthält. Nach dem Hochladen wird sie für die Scoring-Daten in Ihren Anwendungen verfügbar gemacht.
{: shortdesc}

Insbesondere können Sie die folgenden Tasks ausführen:

*  [Vorhersagemodell implementieren oder aktualisieren](#deploying-or-refreshing-a-predictive-model)
*  [Liste aller momentan bereitgestellten Modelle abrufen](#retrieving-a-list-of-all-currently-deployed-models)
*  [Kopie einer bestimmten bereitgestellten Modelldatei herunterladen](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [Implementiertes Vorhersagemodell löschen](#deleting-a-deployed-predictive-model)

## Vorhersagemodell implementieren oder aktualisieren

Jeder Modelldatei
wird eine Kontext-ID als Alias zugeordnet, der zum Referenzieren des bereitgestellten Modells
in nachfolgenden Serviceaufrufen verwendet wird. Wenn ein
Modell für eine Kontext-ID vorhanden ist, wird es durch den folgenden `PUT`-Aufruf
ersetzt, um die Vorhersageanalyse, die von Ihren Anwendungen verwendet wird,
zu aktualisieren.

Anforderungsbeispiel:

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Anforderungsparameter:

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

## Liste aller momentan bereitgestellten Modelle abrufen

Verwenden Sie den folgenden API-Aufruf, um alle Modelle abzurufen, die aktuell in dieser
Serviceinstanz bereitgestellt sind.

Anforderungsbeispiel:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}
```
{: codeblock}

Anforderungsparameter:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Anforderung einer Zusammenfassung für ein bereitgestelltes Modell:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Antwort bei fehlgeschlagener Anforderung einer Zusammenfassung für ein bereitgestelltes Modell:

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

## Kopie einer bestimmten bereitgestellten Modelldatei herunterladen

Verwenden Sie den folgenden API-Aufruf, um eine Kopie einer bestimmten bereitgestellten Modelldatei herunterzuladen.

Anforderungsbeispiel:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Anforderungsparameter:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Downloadanforderung:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Antwort bei fehlgeschlagener Downloadanforderung:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":String // failure reason
        }
```
{: codeblock}

## Implementiertes Vorhersagemodell löschen

Verwenden Sie den folgenden API-Aufruf, um das Vorhersagemodell aus der
Machine Learning-Serviceinstanz zu löschen. Nach diesem Aufruf steht das Vorhersagemodell
nicht mehr zum Download oder zum Scoring von Daten in Ihren
Anwendungen zur Verfügung.

Anforderungsbeispiel:

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}
```
{: codeblock}

Anforderungsparameter:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Deimplementierung:

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

Antwort bei fehlgeschlagener Deimplementierung:

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
