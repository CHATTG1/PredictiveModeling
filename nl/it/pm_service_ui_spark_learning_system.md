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

# Sistema di apprendimento continuo

Il sistema di apprendimento continuo {{site.data.keyword.pm_full}} fornisce il monitoraggio automatizzato delle prestazioni, del riaggiornamento e della ridistribuzione del modello per garantire la qualità delle previsioni.
{: shortdesc}

**Nome scenario**: Selezione dei farmaci migliori per il trattamento di disfunzioni cardiache.

**Descrizione scenario**: una società biomedica che produce farmaci per il cuore vuole distribuire un modello che selezioni il farmaco migliore per il trattamento di disfunzioni cardiache. Quando emergono nuove prove sull'efficacia del farmaco, è opportuno prendere in considerazione un aggiornamento del modello. Un data scientist prepara un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di distribuire il modello, impostare la configurazione di apprendimento ed eseguire l'iterazione di apprendimento per valutare, riaggiornare e ridistribuire il modello. 

## Prerequisiti

Per utilizzare questo esempio, devi disporre dei seguenti servizi: 

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) per archiviare i dati di feedback
* Servizio [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 

   Utilizza [questo link](https://console.bluemix.net/catalog/services/apache-spark) per creare un'istanza Apache Spark se non ne hai già una.

## Utilizzo del modello di esempio

1. Vai alla scheda **Esempi** del dashboard {{site.data.keyword.pm_full}}.
2. Nella sezione **Modelli di esempio**, cerca il tile **Selezione
di farmaci per il cuore** e fai clic sull'icona (+) **Aggiungi modello**. 

Viene visualizzato il modello
di esempi Selezione di farmaci per il cuore nell'elenco di modelli
disponibili nella scheda **Modelli**. 


## Imposta il sistema di apprendimento continuo per il modello pubblicato

1.  Vai alla scheda **Modelli** del dashboard {{site.data.keyword.pm_full}}. 
2.  Dal menu **Azioni**, fai clic su **Visualizza dettagli**.
3.  Scorri fino a **Dettagli di riaggiornamento** e fai clic su **Modifica**.
4.  Devi fornire i seguenti input:

    **Connessione feedback**: i dettagli di {{site.data.keyword.dashdbshort}}, che vengono utilizzati come input (dati di feedback) per la valutazione e il riaggiornamento del modello.

    ```
    {
        "connection":{
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Utilizza le seguenti istruzioni per preparare la tabella `DRUG_FEEDBACK_DATA` in {{site.data.keyword.dashdbshort}}.
    
    - Crea un'istanza del servizio [{{site.data.keyword.dashdbshort}}](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (viene offerto un piano di ingresso).
    - Crea la tabella **DRUG_FEEDBACK_DATA** in **{{site.data.keyword.dashdbshort_notm}}**.
      + Scarica il file [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) dal repository GitHub.
      + Fai clic su **Apri la console** per iniziare a utilizzare **{{site.data.keyword.dashdbshort_notm}}**.
      + Fai clic su **Carica dati** e seleziona il tipo di caricamento **Desktop**.
      + **Trascina** il file precedentemente scaricato nell'area di caricamento e fai clic su **Avanti**.
      + Seleziona **Schema** per importare i dati e fai clic su **Nuova tabella**.
      + Immetti un nome per la nuova tabella e fai clic su **Avanti**.
      + Utilizza un punto e virgola (;) come **separatore di campo**.
      + Fai clic su **Avanti** per creare una tabella con i dati caricati. 

     **Nota**: puoi aggiungere nuovi record di feedback al database di feedback utilizzando l'[endpoint API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback).

     **Dimensione minima dei dati**: il numero minimo di record nel data di feedback per avviare la valutazione del modello (parte dell'iterazione del sistema di apprendimento). 

     ```
     20
     ```
     {: codeblock}

     **Connessione Spark**: le credenziali del servizio Spark possono essere trovate nella scheda **Credenziali del servizio** del dashboard del servizio {{site.data.keyword.Bluemix_notm}} Spark.

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

     **Soglie**: il valore di soglia che attiva il processo di riaggiornamento. Se il valore metrico, come il valore di accuratezza viene calcolato durante la valutazione del modello è inferiore del valore di soglia, viene attivato il riaggiornamento.

     ```
     0.8
     ```

     **Distribuisci automaticamente il modello riaggiornato**: distribuisci il modello riaggiornato se ha un valore metrico migliore rispetto a quello distribuito.

5.  Fai clic su **Configura**.

## Esegui l'iterazione del sistema di apprendimento continuo

Un modello pubblicato viene valutato ripetutamente. Se il valore metrico, come il valore di accuratezza viene calcolato durante la valutazione del modello è inferiore del valore di soglia, viene attivato il riaggiornamento. Entrambi i dataset, formazione e feedback, sono utilizzati per questo processo interattivo di formazione e valutazione.

1. Per avviare una nuova iterazione, dal modulo del modello **Dettagli**, sulla scheda **Monitora**, fai clic su **Valuta**.
3. Per controllare il risultato dell'iterazione, vai alla tabella Eventi di valutazione o alla vista Grafico.  

   Puoi visualizzare le informazioni sulla qualità del modello che viene calcolato in base ai dati di feedback (monitoraggio). Puoi anche visualizzare le informazioni sulla qualità del modello riaggiornato, che è la nuova versione del modello che è stato creato e valutato.

4. Per vedere l'elenco di modelli formati automaticamente e la loro qualità, fai clic su **Visualizza dettagli modello** > **Panoramica** e passa alla sezione **Cronologia delle versioni**.

## Archivio dei dati di feedback

Puoi utilizzare u [endpoint di feedback](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) per inviare i nuovi record all'archivio dei feedback che hai definito nella configurazione di machine learning. {{site.data.keyword.dashdbshort}} è al momento supportato come un archivio di dati di feedback. Se la tabella di feedback non esiste, il servizio la crea. Se la tabella esiste, viene verificato se lo schema corrisponde a quello della tabella di formazione e viene accodata un ulteriore colonna denominata `_training` alla tabella. La colonna `_training` viene utilizzata per contrassegnare i record che sono utilizzati durante il processo di riaggiornamento.

Per ulteriori informazioni ed esempi di un archivio dati feedback, vedi la [documentazione API](pm_service_api_spark_learning_system.html).

**Nota:** la tabella di feedback viene gestita, modificata ed utilizzata dal servizio {{site.data.keyword.pm_full}}. 

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
