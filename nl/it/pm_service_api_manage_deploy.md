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

# Distribuzione o aggiornamento di un modello predittivo

Per distribuire o aggiornare un modello predittivo utilizzando il servizio, utilizza una chiamata API per caricare un file che contiene il ramo di calcolo che è stato distribuito utilizzando IBM® SPSS®
Modeler. Viene reso disponibile per il calcolo del punteggio dei
        dati nelle tue applicazioni. A ogni
file di modello viene dato un ID contesto come un pratico alias da utilizzare per
fare riferimento al modello distribuito nelle successive chiamate di servizio. Se esiste
un modello per un ID contesto, esso viene sostituito da questa chiamata PUT come
un modo per aggiornare l'analisi predittiva utilizzata dalle tue
applicazioni.
{: shortdesc}

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Esempio di
richiesta:

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
