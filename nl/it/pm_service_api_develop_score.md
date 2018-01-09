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

# Calcolo del punteggio con un modello predittivo distribuito

Puoi utilizzare il servizio {{site.data.keyword.pm_full}} per inserire i dati di input che devono essere utilizzati dal modello distribuito tramite l'utilizzo di una chiamata API. Puoi utilizzare questo metodo per generare e restituire l'analisi predittiva nei risultati di punteggio.
{: shortdesc}

```
POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Esempio di
richiesta:

```
    Content-Type: application/json;charset=UTF-8
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello distribuito da utilizzare per elaborare questa richiesta di punteggio
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
        Corpo: i dati di input, stringa json, ad es.
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Esempio di una risposta positiva alla richiesta precedente:

```
    Content-Type: application/json;charset=UTF-8
    Codice di stato: 200
    corpo: il risultato di punteggio, un array json, ad es.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Risposta quando la richiesta di calcolo di punteggio non riesce:

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
