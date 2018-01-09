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

# Liste aller momentan bereitgestellten Modelle abrufen

Rufen Sie eine Zusammenfassung aller Modelle ab, die aktuell in
dieser {{site.data.keyword.pm_full}}-Serviceinstanz bereitgestellt sind.
{: shortdesc}

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}
```
{: codeblock}

Anforderungsbeispiel:

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
