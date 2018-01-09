---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} è un servizio IBM Cloud che abilita gli utenti ad eseguire due operazioni fondamentali di machine learning: formazione e calcolo.
{: shortdesc}

- **Formazione** è il processo di rifinitura di un algoritmo in modo che possa imparare da un dataset. L'output di questa operazione viene chiamato modello. Un modello include i coefficienti appresi delle espressioni matematiche.
- **Calcolo** è l'operazione di predizione di un risultato utilizzando un modello eseguito. L'output dell'operazione di calcolo è un altro dataset che contiene i valori previsti.

{{site.data.keyword.pm_full}} è progettato per soddisfare i bisogni di due tipi di utenti principali:

- Data Scientist: creano le pipeline di machine learning che utilizzano le trasformazioni dei dati e gli algoritmi di machine learning. Normalmente utilizzano notebook o strumenti esterni per formare e valutare i propri modelli. I data scientist spesso collaborano con i data engineer per esplorare e comprendere i dati.
- Sviluppatori: creano applicazioni intelligenti che utilizzano l'output delle predizioni dai modelli di machine learning.

Anche se la formazione è un passo critico nel processo di machine learning, {{site.data.keyword.pm_full}} ti permette di ottimizzare il funzionamento dei tuoi modelli distribuendoli e ottenendo il relativo valore di business effettivo nel tempo e attraverso tutte le loro interazioni.

## Prerequisiti

Per utilizzare {{site.data.keyword.pm_full}}, dal catalogo {{site.data.keyword.Bluemix_short}}, devi creare l'[istanza del servizio qui](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/). Questa impostazione di abilita ad eseguire le seguenti attività: 

## Passi

1. [Configura il tuo ambiente Machine Learning.](ml_getting_access.html)
1. [Crea e memorizza un modello](pm_custom_models.html).
2. [Distribuisci un modello](pm_service_api_spark_online.html).
3. Utilizza il modello distribuito `endpoint di calcolo del punteggio` nella tua applicazione per [generare previsioni.](pm_service_api_spark_building.html)

## Utilizzo di Machine Learning con Data Science Experience

{{site.data.keyword.pm_full}} è integrato con IBM Data Science Experience. Puoi utilizzare le librerie client dell'API Machine Learning nei notebook Data Science Experience; devi disporre di un'istanza Machine Learning per utilizzare il builder del modello e l'editor del flusso.

## Utilizzo di Machine Learning con SPSS Modeler

{{site.data.keyword.pm_full}} è integrato con IBM® SPSS® Modeler. Puoi utilizzare l'API Machine Learning per utilizzare gli algoritmi matematici avanzati.


## Utilizzo di Machine Learning con il tuo ambiente

{{site.data.keyword.pm_full}} può essere utilizzato come una soluzione ibrida che collega il tuo ambiente locale al cloud. Puoi utilizzare l'API Machine Learning per pubblicare i tuoi modelli, distribuire e calcolare. Per ulteriori informazioni, consulta [DSX: Hybrid Mode](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b).

## Informazioni

Il servizio {{site.data.keyword.pm_full}} è una serie di API REST che è possibile
richiamare da qualsiasi linguaggio di programmazione.

L'obiettivo del servizio {{site.data.keyword.pm_full}} è la distribuzione, ma puoi utilizzare IBM® SPSS® Modeler
o IBM® Data Science Experience per creare e utilizzare i modelli e le pipeline. Sia SPSS® Modeler che Data Science Experience che utilizzano
Spark MLlib e Python scikit-learn, offrono diversi metodi di modellazione derivanti dal machine
learning, dall'intelligenza artificiale e dalle statistiche.

## Link correlati

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli SPSS](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) o [API del
servizio per i modelli SPSS](pm_service_api_spss.html). 

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com). 
