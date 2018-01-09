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

# Distribuzione di modelli streaming

Puoi utilizzare il servizio {{site.data.keyword.pm_full}} per distribuire
il modello e generare l'analisi predittiva effettuando richieste di punteggio sul modello di streaming distribuito.
{: shortdesc}

**Nome scenario**: Analisi delle valutazioni.

**Descrizione scenario**: un'agenzia di marketing vuole conoscere le
valutazioni in merito a un determinato argomento. L'agenzia ci richiede di
sviluppare un modello che classifichi un'istruzione come POSITIVA o
NEGATIVA.  Un data scientist prepara un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di distribuire
il modello e di generare l'analisi predittiva effettuando richieste di punteggio sul modello distribuito.

Per ulteriori informazioni, vedi questo [documento](https://github.com/pmservice/tweet-sentiment-prediction).

## Prerequisiti

Per utilizzare questo esempio, devi disporre delle seguenti risorse: 

* I dettagli dell'argomento [Message Hub](https://console.bluemix.net/catalog/services/message-hub), che vengono utilizzati come input (testo del tweet) per il modello e archiviazione per l'output del modello (risultati della previsione). Assicurati che vengano creati due argomenti: l'argomento di output e di input con il testo del tweet.
* Credenziali dell'istanza del servizio [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilizza [questo link](https://console.bluemix.net/catalog/services/apache-spark) per crearne una.


## Utilizzo del modello di esempio

1. Vai alla scheda **Esempi** del dashboard {{site.data.keyword.pm_full}}.
2. Nella sezione **Modelli di esempio**, cerca il tile **Previsione linea
di prodotti** e fai clic sull'icona (+) Aggiungi modello. 

Viene visualizzato il modello di esempio Previsione valutazioni nell'elenco di modelli
disponibili nella scheda Modelli.


## Creazione di una distribuzione streaming con IBM Message Hub

1.  Vai alla scheda **Modelli** del dashboard {{site.data.keyword.pm_full}}. 
2.  Dal menu **Azioni**, fai clic su **Crea distribuzione**.
3.  Nel modulo **Crea distribuzione** compila i campi **Nome**, **Descrizione** e **Tipo flusso**. 
4.  Devi fornire i seguenti input:

    **Connessione di input**: i dettagli degli argomenti di IBM Message Hub che verranno utilizzati come input (tweet) per il modello e archiviazione per l'output del modello  (risultati della previsione). 

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
   "source":{
        "topic":"sinput",
         "type":"kafka"
      }
  }
    ```
    {: codeblock}

    **Connessione di output**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
   "target":{
       "topic":"soutput",
         "type":"kafka"
      }
 }
    ```
    {: codeblock}

    **Connessione Spark**: le credenziali del servizio Spark possono essere trovate nella scheda Credenziali del servizio del dashboard del servizi {{site.data.keyword.Bluemix_notm}} Spark.

     ```
{
     "credentials": {
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",  
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",  
      "cluster_master_url": "https://spark.bluemix.net",  
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",  
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",  
      "plan": "ibm.SparkService.PayGoPersonal"
     },
         "version":"2.0"
}
     ```
     {: codeblock}

5. Fai clic su **Salva**.

Il risultato della previsione viene inviato all'argomento MessageHub definito (connessione di output).

## Recupero dei dettagli di distribuzione

Puoi controllare lo stato e i parametri correlati al modello distribuito.

1. Vai alla scheda **Distribuzioni** del dashboard {{site.data.keyword.pm_full}}.
2. Dal menu **Azioni**, fai clic su **Visualizza dettagli**.

## Eliminazione di una distribuzione streaming

Puoi eliminare una distribuzione che non ti serve più eseguendo una query come nel seguente
esempio.

1. Vai alla scheda **Distribuzioni** del dashboard {{site.data.keyword.pm_full}}.
2. 2. Dal menu **Azioni**, fai clic su **Elimina**.

## Ulteriori informazioni

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli IBM® SPSS®](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) oppure [API del
servizio per i modelli IBM® SPSS®](pm_service_api_spss.html).

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
