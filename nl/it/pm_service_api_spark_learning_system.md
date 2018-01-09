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

Il servizio {{site.data.keyword.pm_full}} include un sistema di apprendimento continuo. I sistemi di apprendimento continuo forniscono il monitoraggio automatizzato delle prestazioni, del riaggiornamento e della ridistribuzione del modello per garantire la qualità delle previsioni.
{: shortdesc}

**Nome scenario**: Selezione dei farmaci migliori per il trattamento di disfunzioni cardiache.

**Descrizione scenario**: una società biomedica che produce farmaci per il cuore vuole distribuire un modello che selezioni il farmaco migliore per il trattamento di disfunzioni cardiache. Quando emergono nuove prove sull'efficacia del farmaco, è opportuno prendere in considerazione un aggiornamento del modello. Un data scientist ha preparato
un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di distribuire il modello, impostare la configurazione di apprendimento ed eseguire l'iterazione di apprendimento per valutare, riaggiornare e ridistribuire il modello. 

**Nota**: puoi anche utilizzare [blocco appunti  python di esempio](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325) che crea un modello di esempio, configura il sistema di apprendimento ed esegue l'iterazione di apprendimento.

## Prerequisiti

Per utilizzare questi esempi, devi disporre dei seguenti servizi: 

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) per archiviare i dati di feedback
* Credenziali dell'istanza del servizio [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Puoi utilizzare [questo link](https://console.bluemix.net/catalog/services/apache-spark) per crearlo.

## Utilizzo del modello di esempio

1. Vai alla scheda **Esempi** del dashboard {{site.data.keyword.pm_full}}.
2. Nella sezione **Modelli di esempio**, cerca il tile Selezione di farmaci per il cuore e fai clic sull'icona **Aggiungi modello** (+).

Viene visualizzato il modello
di esempi **Selezione di farmaci per il cuore** nell'elenco di modelli
disponibili nella scheda **Modelli**.

## Generazione del token di accesso

Genera un token di accesso che utilizza l'Utente e
la Password disponibili dalla scheda Credenziali del servizio
dell'istanza del servizio {{site.data.keyword.pm_full}}. 

Esempio di
richiesta:

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Esempio di output:

```
{"token":"**********"}
```
{: codeblock}

Utilizza il seguente comando di terminale per assegnare il tuo valore token alla variabile di ambiente token:

```
token="<token_value>"
```
{: codeblock}

## Utilizzo di modelli pubblicati

Utilizza la seguente chiamata API per richiamare i dettagli della tua istanza, ad esempio:

* indirizzo `url` dei modelli pubblicati 
* indirizzo `url` delle distribuzioni
* informazioni di utilizzo

Esempio di
richiesta:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Esempio di output:

```
{
   "metadata":{
      "guid":"360c510b-012c-4793-ae3f-063410081c3e",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e",
      "created_at":"2017-08-04T09:15:48.344Z",
      "modified_at":"2017-08-22T08:34:47.759Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models"
      },
      "usage":{
         "expiration_date":"2017-09-01T00:00:00.000Z",
         "computation_time":{
            "limit":18000,
            "current":4
         },
         "model_count":{
            "limit":200,
            "current":2
         },
         "prediction_count":{
            "limit":5000,
            "current":16
         },
         "deployment_count":{
            "limit":5,
            "current":1
         }
      },
      "plan_id":"0f2a3c2c-456b-40f3-9b19-726d2740b11c",
      "status":"Active",
      "organization_guid":"b0e61605-a82e-4f03-9e9f-2767973c084d",
      "region":"us-south",
      "account":{
         "id":"f52968f3dbbe7b0b53e15743d45e5e90",
         "name":"Umit Cakmak's Account",
         "type":"TRIAL"
      },
      "owner":{
         "ibm_id":"31000292EV",
         "email":"umit.cakmak@pl.ibm.com",
         "user_id":"43e0ee0e-6bfb-48fc-bcd8-d61e40d19253",
         "country_code":"POL",
         "beta_user":true
      },
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/deployments"
      },
      "space_guid":"4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "plan":"standard"
   }
}
```
{: codeblock}

Dopo aver ottenuto l'indirizzo `url` dei **modelli_pubblicati**, utilizza la seguente chiamata API per richiamare i dettagli del modello: 

Esempio di
richiesta:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

Esempio di output:

```
{  
   "count":1,
   "resources":[  
      {  
         "metadata":{  
      "guid":"361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "created_at":"2017-08-22T08:34:47.597Z",
            "modified_at":"2017-08-22T08:34:47.691Z"
         },
   "entity":{  
      "runtime_environment":"spark-2.0",
            "author":{  
               "name":"IBM",
            "email":""
            },
            "name":"Best Heart Drug Selection",
            "label_col":"DRUG",
            "training_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"DRUG"
                     },
                     "type":"string",
                     "name":"DRUG",
                     "nullable":true
                  }
               ],
               "type":"struct"
            },
            "latest_version":{  
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "guid":"8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "created_at":"2017-08-22T08:34:47.691Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{  
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/deployments"
            },
            "input_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  }
               ],
               "type":"struct"
            }
         }
      }
}
```
{: codeblock}


## Imposta il sistema di apprendimento continuo per un modello pubblicato 

Per configurare il sistema di apprendimento continuo per il tuo modello, devi eseguire le seguenti attività:

1.  [Prepara l'intestazione di autorizzazione](#prepare-the-authorization-header).
2.  [Prepara la serie di dati di feedback](#prepare-the-feedback-data-set).
3.  [Prepa il payload di configurazione](#prepare-the-configuration-payload).

Dopo aver completato la preparazione dell'intestazione di autorizzazione, della serie di dati di feedback e del payload di configurazione, puoi iniziare ad interagire con il tuo sistema di apprendimento continuo. Per ulteriori informazioni, vedi [Esegui l'iterazione del sistema di apprendimento continuo](#run-continuous-learning-system-iteration).

### Prepara l'intestazione di autorizzazione 

Per preparare l'intestazione di autorizzazione che unisce il token di {{site.data.keyword.pm_full}} e le credenziali dell'istanza Spark, fornisci i seguenti dettagli:

*  Il token di accesso creato nel passo precedente
*  Le credenziali del servizio Spark, che è possibile trovare nella scheda Credenziali del servizio del dashboard del servizio {{site.data.keyword.Bluemix_notm}} Spark. Prima di effettuare la richiesta di distribuzione, le credenziali Spark devono essere codificate in base64. Queste vengono passate nell'intestazione della richiesta nel campo X-Spark-Service-Instance.

   A seconda del sistema operativo che stai utilizzando, devi immettere uno dei seguenti comandi di terminale per eseguire la codifica in base64 e assegnarlo alla variabile di ambiente.

   Sul sistema operativo **macOS**, utilizza il seguente comando:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Sui sistemi operativi **Microsoft Windows** o **Linux**, per eseguire la decodifica in base64 devi utilizzare il parametro `--wrap=0` con il comando `base64`: 

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### Prepara serie di dati di feedback

Il sistema di apprendimento richiede la connessione ai dati di formazione (i dati utilizzati nella formazione del modello) così come i dati di feedback. L'archivio dei dati di feedback viene utilizzato per monitorare e riaggiornare il tuo modello se necessario. {{site.data.keyword.dashdbshort}} è supportato come un archivio di dati di feedback. La tabella di feedback viene gestita, modificata ed utilizzata dal servizio {{site.data.keyword.pm_short}}.
Per preparare la tabella **DRUG_FEEDBACK_DATA** in **{{site.data.keyword.dashdbshort}}**, devi completare la seguente procedura:

1. Crea un'istanza del servizio [{{site.data.keyword.dashdbshort}}](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (viene offerto un piano di ingresso).
2. Crea la tabella `DRUG_FEEDBACK_DATA` in **{{site.data.keyword.dashdbshort}}**.
   1. Dal repository GitHub, scarica il file [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv).
   2. Per iniziare ad utilizzare **{{site.data.keyword.dashdbshort_notm}}**, fai clic sull'icona **Open the console**.
   3. Seleziona il tipo di caricamento **Carica dati** e **Desktop**.
   4. **Trascina** il file precedentemente scaricato e fai clic su **Avanti**.
   5. Per importare i dati, fai clic su **Schema** e su **Nuova tabella**.
   6. Immetti un nome nel campo **nuova tabella** e fai clic su **Avanti**.
   7. Per il **separatore campi**, utilizza un punto e virgola (;).
   8. Fai clic su **Avanti** per creare la tabella con i dati caricati.

**Nota**: puoi aggiungere nuovi record di feedback al database di feedback utilizzando questo [endpoint API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback). Per ulteriori informazioni, consulta [sezione archivio dati di feedback](#feedback-data-store).

### Prepara il payload di configurazione

Per specificare la configurazione di apprendimento, devi utilizzare il seguente endpoint: 

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Per finalizzare il payload, devi definire i valori dei seguenti parametri:

<dl><dt>feedback_data_reference</dt>
<dd>connessione e origine dell'archivio dei dati di feedback</dd>
<dt>min_feedback_data_size</dt>
<dd>numero minimo di record nella serie di dati di feedback per avviare l'iterazione del sistema di apprendimento continuo
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] specifica quando attiva il processo di riaggiornamento. Quando impostato su [conditionally], attiva il processo di riaggiornamento quando la qualità del modello è inferiore al valore di soglia specificato.
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] specifica quando deve essere distribuito il modello riaggiornato. Quando impostato su [conditionally], la ridistribuzione del modello quando la qualità del modello appena formato è superiore rispetto a quello attualmente distribuito.</dd></dl>

Esempio di
richiesta:

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
         "definition": {
           "method": "multiclass",
           "metrics": [
             {
               "name": "accuracy",
               "threshold": 0.8
             }
           ]
         },
         "feedback_data_reference": {
           "connection":{
            "db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
           },
    "source": {
            "tablename": "DRUG_FEEDBACK_DATA",
            "type": "dashdb"
           }
          },
          "min_feedback_data_size": 10,
          "auto_retrain": "conditionally",
          "auto_redeploy": "never",
          "last_training_record": 0
        }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Esempio di output:

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass",
    "metrics": [
      {
        "name": "accuracy",
        "threshold": 0.8
      }
    ]
   },
   "feedback_data_reference":{
      "connection":{
         "db":"BLUDB",
         "host":"awh-yp-small02.services.dal.bluemix.net",
         "username":"***",
         "password":"***"
      },
      "source":{
         "tablename":"DRUG_FEEDBACK_DATA",
         "type":"dashdb"
      }
   },
   "min_feedback_data_size":10,
   "spark_service":{
      "credentials": {
         "tenant_id":"***",
         "cluster_master_url":"https://spark.bluemix.net",
         "tenant_id_full":"***",
         "tenant_secret":"***",
         "instance_id":"***",
         "plan":"ibm.SparkService.PayGoPersonal"
      },
         "version":"2.0"
   },
   "training_data_reference": {
   "connection":{
     "db": "BLUDB",
     "host": "dashdb-entry-yp-dal09-08.services.dal.bluemix.net",
     "password": "***",
     "username": "***"
   },
    "source": {
     "tablename": "DRUG_TRAIN_DATA_UPDATED",
     "type": "dashdb"
    }
   }
}
```
{: codeblock}

**Nota**: l'esempio utilizza i valori predefiniti per i parametri `auto_retrain` e `auto_redeploy`. Per ulteriori informazioni su questi parametri, consulta la [documentazione dell'API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration) per il parametro `learning_configuration`.

## Esegui l'iterazione del sistema di apprendimento continuo

Per iniziare ad interagire con il sistema di apprendimento, utilizza il seguente metodo API REST. Durante l'iterazione il modello pubblicato viene valutato. Se l'accuratezza valutata è inferiore al valore di soglia specificato, il modello attiva il riaggiornamento. I dataset di feedback e formazione vengono entrambi utilizzati per il riaggiornamento e la valutazione.

Esempio di
richiesta:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Esempio di output:

```
The request has been fulfilled and resulted in a new resource being created.
```
{: codeblock}

Per controllare lo stato dell'iterazione, immetti la seguente richiesta:

Esempio di
richiesta:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Esempio di output:

```
{
  "count": 1,
  "resources": [
    {
      "entity":{
        "status": {
          "state": "INITIALIZED"
        },
        "model_version": {
          "url": "https://ibm-watson-ml-svt.stage1.mybluemix.net/v2/artifacts/models/71dc688a-ebda-4174-9574-e8805059e08f/versions/de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d",
          "created_at": "2017-10-30T15:45:12.926Z",
          "guid": "de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d"
        },
      "spark_service":{
          "credentials": {
            "tenant_id_full": "***",
            "tenant_secret": "***",
            "tenant_id": "***",
            "instance_id": "***",
            "plan": "ibm.SparkService.PayGoPersonal",
            "cluster_master_url": "https://spark.bluemix.net"
          },
         "version":"2.0"
        },
        "published_model": {
          "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f",
          "guid": "71dc688a-ebda-4174-9574-e8805059e08f"
        }
      },
      "metadata": {
        "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f/learning_iterations/a308838b-445f-45b8-9fbf-1c3dd1b392c1",
        "created_at": "2017-10-30T15:54:30.657Z",
        "guid": "a308838b-445f-45b8-9fbf-1c3dd1b392c1"
      }
    }
  ]
}
```
{: codeblock}

Puoi anche ottenere l'elenco delle metriche e dei valori di valutazione immettendo la seguente richiesta:

Esempio di
richiesta:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

Esempio di output:

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-07T10:11:02.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
      {
      "phase": "monitoring",
      "values": [
        {
          "name": "accuracy",
          "value": 0.75,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    },    
    {
      "phase": "training",
      "values": [
        {
          "name": "accuracy",
          "value": 0.88281694,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:22.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    }
  ]
}
```
{: codeblock}

## Archivio dei dati di feedback 

Puoi utilizzare un [endpoint di feedback](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) per inviare i nuovi record (i record che non stati utilizzati nel processo di formazione) all'archivio di feedback definito nella configurazione di apprendeimento. L'archivio dei dati di feedback viene utilizzato per monitorare e riaggiornare il tuo modello se necessario. {{site.data.keyword.dashdbshort}} è supportato come un archivio di dati di feedback. Se la tabella di feedback non esiste, il servizio la crea. Se la tabella esiste, viene verificato se lo schema corrisponde a quello della tabella di formazione e viene accodata un ulteriore colonna denominata `_training` alla tabella. La colonna aggiuntiva viene utilizzata per contrasegnare i record che sono utilizzati nel processo di riaggiornamento.

**Nota:** la tabella di feedback viene gestita, modificata ed utilizzata dal servizio {{site.data.keyword.pm_short}}.

Puoi inviare un nuovo record di feedback all'archivio di dati di feedback immettendo la seguente richiesta:

Esempio di
richiesta:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields": [
      "AGE",
      "SEX",
      "BP",
      "CHOLESTEROL",
      "NA",
      "K",
      "DRUG"
   ],
   "values":[
      [
         
         16,
         "M",
         "HIGH",
         "NORMAL",
         0.58301000000000003,
         0.033884999999999998,
         "drugY"
      ]
   ]
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/feedback
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
