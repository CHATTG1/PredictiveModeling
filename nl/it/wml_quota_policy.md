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

# Politica delle quote

Il motore di {{site.data.keyword.pm_full}} applica delle quote sulla base di ogni istanza per garantire l'assegnazione e l'utilizzo appropriati delle risorse. Queste politiche variano in base ai profili dell'account, all'uso storico dei servizi e alla disponibilità delle risorse. La politica delle quote descrive i limiti che non sono definiti dal piano dei prezzi e che **sono soggetti a modifiche senza preavviso**. Attualmente, sono attivi i seguenti limiti di quota dei servizi.
{: shortdesc}

## Utilizzo delle risorse

Le risorse sono limitate ai seguenti valori massimi: 

-  **Numero massimo di modelli pubblicati**: 200 (piano Lite) 1000 (piani Standard & Professional) 
-  **Dimensione massima del modello Spark ML**: 200 MB
-  **Numero massimo di modelli distribuiti**: 1000 (piani Standard & Professional) 

**Nota**: per comprendere i limiti del piano Lite, consulta la descrizione del [piano Lite](https://console.bluemix.net/catalog/services/machine-learning) nel [{{site.data.keyword.Bluemix_notm}}catalogo](https://console.bluemix.net/catalog/).

## Limiti di richieste del servizio

Esistono dei limiti nel numero di richieste che puoi effettuare al minuto a specifiche API. Se la tua applicazione ha bisogno di limiti superiori, devi [contattare il supporto {{site.data.keyword.Bluemix_notm}}](https://support.ng.bluemix.net/) per aumentare la tua quota.

## Richieste di distribuzione

Alle richieste API di distribuzione si applicano i seguenti limiti:

-  **Periodo**: 60 secondi
-  **Limite predefinito**: 10

## Richieste di previsione online

Alle richieste di [previsioni online](pm_service_api_spark_building.html) si applicano i seguenti limiti:

-  **Periodo**: 60 secondi
-  **Limite predefinito**: 500

## Richieste di gestione risorse

Alle richieste totali combinate in questo elenco si applicano i seguenti limiti:

[Token](https://watson-ml-api.mybluemix.net/#/Token): richieste post, get, delete, put, patch

[Distribuzioni](https://watson-ml-api.mybluemix.net/#/Deployments): richieste post, get, delete, put, patch

-  **Periodo**: 60 secondi
-  **Limite predefinito**: 100

## Ulteriori informazioni

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli IBM® SPSS®](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) o [API del
servizio per i modelli IBM® SPSS®](pm_service_api_spss.html).

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM® Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
