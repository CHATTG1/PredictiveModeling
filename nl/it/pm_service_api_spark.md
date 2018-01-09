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

# API REST

Il servizio {{site.data.keyword.pm_short}} consiste in una serie di
API REST che puoi richiamare da qualsiasi linguaggio di programmazione, il che permette l'integrazione dell'analisi Data Science Experience
nelle tue applicazioni. Esegui il bind delle tue applicazioni
{{site.data.keyword.Bluemix_short}} all'istanza del servizio {{site.data.keyword.pm_short}} e genera l'analitica
predittiva di cui hanno bisogno le tue applicazioni per offrire un valore più elevato ai tuoi utenti. Gestisci i modelli nel
[dashboard di amministrazione](pm_service_ui_spark.html) e utilizza il dashboard per creare distribuzioni online,
batch o streaming integrate con le tue
applicazioni.
{: shortdesc}

Sviluppa le applicazioni nei modelli Spark, Python e IBM® SPSS® che vengono distribuiti su un'istanza del servizio
attraverso le potenti [API REST](https://watson-ml-api.mybluemix.net/):

*  Richiama i metadati per uno specifico modello predittivo
*  Distribuisci i modelli e gestisci i modelli distribuiti
    *  Distribuzione online (punteggio)
    *  Distribuzione batch (supporta IBM Cloud Object Storage e {{site.data.keyword.dashdbshort}})
    *  Distribuzione flusso (supporta IBM Cloud Message Hub)
*  Richiama i metadati per una specifica distribuzione
*  Monitora e riaggiorna i modelli distribuiti utilizzando il sistema di apprendimento continuo
*  Genera l'analisi predittiva effettuando richieste di punteggio sui modelli distribuiti

Per ulteriori informazioni, consulta gli esempi di utilizzo dell'API REST:

*  [Distribuzione di modelli online](pm_service_api_spark_online.html)
*  [Calcolo dei modelli online](pm_service_api_develop_score.html)
*  [Distribuzione di modelli batch](pm_service_api_spark_batch.html)
*  [Distribuzione dei modelli di flusso](pm_service_api_spark_streaming.html)
*  [Sistema di apprendimento continuo](pm_service_api_spark_learning_system.html)

## Ulteriori informazioni

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli IBM® SPSS®](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) o [API del
servizio per i modelli IBM® SPSS®](pm_service_api_spss.html).

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
