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

# Utilizzo del servizio

Utilizzando i metodi di modellazione nella tavolozza di modellazione IBM® SPSS®
puoi ricavare nuove
informazioni dai tuoi dati e sviluppare modelli predittivi. Ogni metodo ha determinati punti di forza e si presta meglio per particolari tipi di problemi machine learning.
{: .shortdesc}

Per i dettagli su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7). 

Dopo che sono stati implementati i requisiti di input e di output della progettazione della tua applicazione {{site.data.keyword.Bluemix_notm}} e
del ramo di calcolo del punteggio IBM® SPSS® Modeler, il tuo analista dei dati può modificare
qualsiasi aspetto interno del ramo di calcolo del punteggio. L'analista dei dati può anche modificare uno o più algoritmi di modello utilizzati in un'operazione di aggiornamento, assicurandoti di poter ottimizzare la tua analisi predittiva senza dover riscrivere le tue applicazioni.


## Procedura per eseguire il bind del servizio all'applicazione {{site.data.keyword.Bluemix_notm}}

Completa la seguente procedura per creare la tua applicazione {{site.data.keyword.Bluemix_notm}} ed eseguirne il bind al servizio {{site.data.keyword.pm_short}}.

1. Scarica il codice dell'applicazione di esempio Node.js dal [repository GitHub](https://github.com/pmservice/customer-satisfaction-prediction).

2. Utilizza il comando `cf create-service` per creare un'istanza del servizio:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Ad esempio:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   Questo comando crea un'istanza del servizio {{site.data.keyword.pm_short}}
   con il piano lite denominato my_pm_lite nel tuo spazio {{site.data.keyword.Bluemix_notm}}.

3. Utilizza il comando `cf create-service-key` per creare le credenziali del servizio:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Ad esempio:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Questo comando crea le credenziali del servizio
{{site.data.keyword.pm_short}}.

4. Utilizza il comando `cf bind-service` per eseguire il bind dell'istanza del servizio
   `my_pm_lite` alla tua applicazione.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   Ad esempio:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   Questo comando esegue il bind dell'istanza del servizio {{site.data.keyword.pm_short}}
   `my_pm_lite` all'applicazione my_app1 di {{site.data.keyword.Bluemix_notm}}.

5. Credenziali {{site.data.keyword.pm_short}}:

   Dopo che hai eseguito il bind dell'istanza del servizio {{site.data.keyword.pm_short}} alla tua applicazione
{{site.data.keyword.Bluemix_notm}}, le credenziali {{site.data.keyword.pm_short}} vengono aggiunte alla variabile di ambiente `VCAP_SERVICES`.

```
    {   
        "pm-20": {      
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   La variabile di ambiente `VCAP_SERVICES` include le seguenti informazioni:

   <dl>

   <dt>piano</dt>
   <dd>Il piano {{site.data.keyword.pm_short}} utilizzato nel provisioning del servizio.</dd>

   <dt>url</dt>
   <dd>L'indirizzo dell'istanza del servizio {{site.data.keyword.pm_short}}.</dd>

   <dt>access_key</dt>
   <dd>Il parametro di query accessKey da passare in tutte le richieste
            a questa istanza del servizio.</dd>

   </dl>

Ad esempio:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Codice Node.js di esempio che mostra come ottenere l'accessKey dalla
   variabile di ambiente `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
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
