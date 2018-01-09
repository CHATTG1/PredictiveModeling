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

# Gestione dei modelli SPSS distribuiti

Puoi utilizzare l'API del servizio {{site.data.keyword.pm_full}} per caricare un file che contiene il ramo di calcolo
IBM® SPSS® Modeler da distribuire. Dopo averlo caricato, viene reso disponibile per il calcolo del punteggio dei
dati nelle tue applicazioni. {: shortdesc}

Nello specifico, è possibile effettuare le attività riportate di seguito:

*  [Distribuzione o aggiornamento di un modello predittivo](#deploying-or-refreshing-a-predictive-model)
*  [Richiamo di un elenco di tutti i modelli attualmente distribuiti](#retrieving-a-list-of-all-currently-deployed-models)
*  [Download di una copia di uno specifico file modello distribuito](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [Eliminazione di un modello predittivo distribuito](#deleting-a-deployed-predictive-model)

## Distribuzione o aggiornamento di un modello predittivo

A ogni
file di modello viene dato un ID contesto come un pratico alias da utilizzare per
fare riferimento al modello distribuito nelle successive chiamate di servizio. Se esiste
un modello per un ID contesto, esso viene sostituito dalla seguente chiamata `PUT` come
un modo per aggiornare l'analisi predittiva utilizzata dalle tue
applicazioni.

Esempio di
richiesta:

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Parametri richiesti: 

```
    Content-Type: multipart/form-data
    Parametri:
        Parametri modulo:
            model_file: il file del modello da caricare
        Parametri percorso:
            contextId: l'identificativo univoco da assegnare al tuo modello o un riferimento al modello distribuito da aggiornare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la distribuzione riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":true,
           "message":"informazioni dettagliate"
         }
```
{: codeblock}

Risposta quando la distribuzione non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
        }
```
{: codeblock}

## Richiamo di un elenco di tutti i modelli attualmente distribuiti

Utilizza la seguente chiamata API per richiamare un riepilogo di tutti i modelli attualmente distribuiti su questa istanza del servizio. 

Esempio di
richiesta:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}
```
{: codeblock}

Parametri richiesti: 

```
    Content-Type: */*
    Parametri:
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la richiesta per un riepilogo di modello distribuito riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo: un array vuoto se non è stato distribuito alcun modello o un riepilogo dei modelli distribuiti...
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

Risposta quando la richiesta per un riepilogo di modello distribuito non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
         }
```
{: codeblock}

## Download di una copia di uno specifico file modello distribuito

Utilizza la seguente chiamata API per eseguire il download di uno specifico file modello distribuito. 

Esempio di
richiesta:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Parametri richiesti: 

```
    Content-Type: */*
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello predittivo distribuito di cui desideri eseguire il download
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la richiesta di download riesce:

```
    Content-Type: application/octet-stream
    Codice di stato: 200
    Intestazione:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Risposta quando la richiesta di download non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":String // motivo dell'errore
        }
```
{: codeblock}

## Eliminazione di un modello predittivo distribuito

Utilizza la seguente chiamata API per eliminare il modello predittivo dall'istanza del servizio Machine
Learning. Dopo aver eseguito questa chiamata, il modello
predittivo non sarà più disponibile per il download o il calcolo
del punteggio dei dati nelle tue applicazioni. 

Esempio di
richiesta:

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}
```
{: codeblock}

Parametri richiesti: 

```
    Content-Type: */*
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello distribuito di cui si desidera annullare la distribuzione e che si desidera eliminare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando l'annullamento della distribuzione riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":true,
           "message":"informazioni dettagliate"
         }
```
{: codeblock}

Risposta quando l'annullamento della distribuzione non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
        }
```
{: codeblock}

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
