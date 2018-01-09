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

# Framework supportati

Il servizio {{site.data.keyword.pm_full}} supporta i seguenti framework per lo sviluppo e la convalida dei modelli come parte della gestione della durata del modello.
{: shortdesc}

## Spark MLlib

Il servizio {{site.data.keyword.pm_full}} supporta Spark MLlib per il servizio online, batch (beta), flusso (beta) distribuzione e calcolo. Sono supportati solo specifici ambienti di runtime e versioni.

### Funzionalità

* Tipi di distribuzione: 
  * Online
  * Batch con Object Storage, DB2 Warehouse on Cloud e DB2 on Cloud
  * Flusso con Message Hub
*  Sistema di apprendimento continuo 

### Versioni supportate

*  Spark 2.0

### Limitazioni

  *  Sono supportati solo i modelli di classificazione e regressione.
  *  I modelli che contengono riferimenti a trasformatori personalizzati, funzioni definite dall'utente e classi non sono supportati. 
  * Le distribuzioni batch e flusso utilizzano il servizio IBM Cloud Apache Spark. In base al piano dei servizi sottoscritto, tieni presente le seguenti limitazioni nel numero di lavori spark-submit paralleli ([Using spark-submit documentation](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite: solo un lavoro spark-submit alla volta
    * Reserved Enterprise: 150 lavori spark-submit alla volta

## Scikit-learn

È disponibile il supporto di scikit-learn per il servizio Distribuzione e punteggio online di {{site.data.keyword.pm_full}}. Sono supportati solo specifici ambienti di runtime e versioni.

### Funzionalità

* Tipi di distribuzione: 
  * Online

### Versioni supportate

- Anaconda 4.2.x per il runtime Python 3.5
- Scikit-Learn 0.17

### Runtime Python

Il runtime Python per i modelli che devono essere distribuiti utilizzando il servizio Distribuzione e punteggio online WML è Python 3.5. 

In alcuni casi, dove i modelli sono stati creati utilizzando il runtime Python 2.7, la distribuzione potrebbe non riuscire a causa di incompatibilità tra Python 2.7 e Python 3.5.

### Pacchetti Python supportati

Un elenco di pacchetti Python supportati dal servizio Distribuzione e punteggio online WML è disponibile [qui](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Limitazioni

Esistono diverse limitazioni che possono includere alcune o tutte quelle riportate di seguito:

* Sono supportati solo i modelli di classificazione e regressione.
* I modelli possono contenere riferimenti solo ai pacchetti (e versione pacchetto) supportati da Anaconda 4.2.x per il runtime Python 3.5.
* L'endpoint "schema" restituito durante la richiesta di distribuzione non è implementato.
* I modelli che richiedono Pandas Dataframe come tipo di input per l'API "predict()" non sono supportati.
* I modelli che contengono riferimenti a trasformatori personalizzati, funzioni definite dall'utente e classi non sono supportati. 

## XGBoost

È disponibile il supporto di XGBoost per il servizio Distribuzione e punteggio online di {{site.data.keyword.pm_full}}. Sono supportati solo specifici ambienti di runtime e versioni.

### Funzionalità

* Tipi di distribuzione: 
  * Online

### Versioni supportate

- XGBoost 0.6
- Anaconda 4.2.x per il runtime Python 3.5
- Scikit-Learn 0.17

### Runtime Python

Il runtime Python per i modelli che devono essere distribuiti utilizzando il servizio Distribuzione e punteggio online WML è Python 3.5.

In alcuni casi, dove i modelli sono stati creati utilizzando il runtime Python 2.7, la distribuzione potrebbe non riuscire a causa di incompatibilità tra Python 2.7 e Python 3.5.

### Pacchetti Python supportati

Un elenco di pacchetti Python supportati dal servizio Distribuzione e punteggio online WML è disponibile [qui](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Limitazioni

Esistono diverse limitazioni che includono alcune o tutte quelle riportate di seguito: 

* I modelli possono contenere riferimenti solo ai pacchetti (e versione pacchetto) supportati da Anaconda 4.2.x per il runtime Python 3.5.
* L'endpoint "schema" restituito durante la richiesta di distribuzione non è implementato.
* La probabilità di previsione non viene restituita come risposta della richiesta di calcolo del punteggio in questa fase.
* I modelli che contengono riferimenti a trasformatori personalizzati, funzioni definite dall'utente e classi non sono supportati. 

## SPSS Modeler

È disponibile il supporto dei flussi IBM® SPSS® Modeler per il servizio Distribuzione e punteggio online e batch di {{site.data.keyword.pm_full}}. Sono supportati solo specifici ambienti di runtime e versioni.

### Funzionalità

* Tipi di distribuzione: 
  * Online
  * Batch
* Riaggiornamento 
* Esecuzione flusso 

### Versioni supportate

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Limitazioni

Per i flussi IBM® SPSS® Modeler esistono le seguenti limitazioni:

*  I flussi creati utilizzando l'editor del flusso IBM® Data Science Experience non sono supportati.
*  Quando un ramo di calcolo viene preparato per l'utilizzo per il calcolo in tempo reale, i dati di input ricevuti nella richiesta di calcolo
devono sostituire il nodo di origine creato nel ramo di calcolo e l'output di analisi predittiva risultante
deve ritornare al flusso della risposta (sostituendo di fatto il nodo del terminale
nella progettazione del ramo di calcolo).
*  Poiché il ramo di calcolo è stato preparato per l'esecuzione in tempo reale in {{site.data.keyword.Bluemix_short}}, non può richiedere una connessione a un servizio esterno. Ad esempio, una progettazione del ramo di calcolo di IBM Analytical Decision Management
non può contenere i riferimenti alle regole o ai modelli archiviati in un repository IBM® SPSS® Collaboration and Deployment Services.
*  L'esecuzione di un ramo di calcolo per il calcolo in tempo reale in {{site.data.keyword.Bluemix_short}} non può richiedere un servizio
esterno. Ad esempio, non puoi distribuire e calcolare gli algoritmi del modello che richiedono un server IBM® SPSS® Analytic e un archivio dati Apache Hadoop in tempo reale.
*  {{site.data.keyword.pm_short}} supporta lo script Modeler integrato con
Python. Esistono alcune restrizioni dovute al metodo utilizzato per l'elaborazione dei flussi prima di essere
eseguiti in {{site.data.keyword.pm_short}}. Normalmente, se un utente sceglie
di controllare l'esecuzione del flusso, fa riferimento al nodo del terminale del ramo. Per
{{site.data.keyword.pm_short}}, quando elaboriamo il flusso, identifichiamo i nodi da
JSON che saranno sovrascritti e quindi eseguiamo la sostituzione prima dell'esecuzione
del flusso. Questo causa il malfunzionamento del flusso nello script perché i nodi di esportazione e l'input di riferimento
non esistono più. La soluzione è di utilizzare l'ID di un altro nodo per identificare univocamente il ramo
durante l'esecuzione. Ciò assicura che il flusso eseguito sia definito nello script Python integrato.

## Ulteriori informazioni

Per ulteriori informazioni sul supporto corrente per i modelli predittivi eseguiti dal server IBM® SPSS® Analytic , consulta la sezione [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) di [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
