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

# Sviluppo e persistenza del modello personalizzato

Utilizzando il servizio {{site.data.keyword.pm_full}}, puoi distribuire
un modello e generare l'analisi predittiva effettuando richieste di punteggio sul modello distribuito.
{: shortdesc}

## Utilizzo di modelli personalizzati

### Nome scenario: Previsione linea di prodotti.

### Descrizione scenario:

Il nostro cliente sta realizzando una delle più famose catene di negozi
in Europa. Ci richiede di scoprire gli interessi dei propri clienti in merito alla propria linea di prodotti,
come Accessori personali, Attrezzature da campeggio e Protezione all'aperto. Un data scientist
sviluppa un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di distribuire
il modello e di generare l'analisi predittiva effettuando richieste di punteggio sul modello distribuito.

### Sviluppo e persistenza del modello personalizzato

#### Utilizzo di Data Science Experience

Utilizza [Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience) per creare modelli personalizzati. Dopo esserti registrato, devi accedere per completare la seguente procedura.

1. Crea un'organizzazione e uno spazio. La prima volta che accedi devi fornire queste informazioni. Fai clic su
**Continua** per accettare i valori predefiniti. 
2. Dopo aver creato l'organizzazione, vai a **Progetti** e fai clic su
   **Nuovo progetto**.
3. Specifica un nome e una descrizione per il progetto e fai clic su **Crea**. Il nome del progetto che hai specificato viene utilizzato come
   il nome del contenitore di destinazione.
4. Una volta creato il progetto, puoi eseguire una delle seguenti attività: 
   
   *  **aggiungere blocchi appunti** e iniziare a sviluppare i tuoi propri modelli o caricare uno dei seguenti blocchi appunti:
        *  [Sviluppo di modelli Spark MLlib con Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Sviluppo di modelli Spark MLlib con Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Sviluppo di modelli scikit-learn con Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **aggiungere modelli** e iniziare a sviluppare i tuoi propri modelli utilizzando la procedura guidata del modello 


#### Utilizzo dell'ambiente locale

Puoi inoltre utilizzare un ambiente di tua scelta per sviluppare il modello e in seguito pubblicarlo, distribuirlo e calcolarlo utilizzando la [libreria client API comune]() Watson Machine Learning disponibile per pypi.
Per ulteriori informazioni sulla libreria client, vedi [notebook](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125) e [documentation](pm_service_client_library.html) di esempio.

**Nota:** la libreria client API comune è nella **beta**.

### Distribuzione e punteggio del modello personalizzato

Puoi utilizzare l'API per distribuire e calcolare i modelli streaming, batch e online.

*  [Distribuzione di modelli online](pm_service_api_spark_online.html)
*  [Distribuzione di modelli batch](pm_service_api_spark_batch.html)
*  [Distribuzione di modelli streaming](pm_service_api_spark_streaming.html)

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
