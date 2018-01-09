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

# Fortlaufendes Ausbildungssystem

Der {{site.data.keyword.pm_full}}-Service enthält ein fortlaufendes Ausbildungssystem. Fortlaufende Ausbildungssysteme bieten eine automatisierte Überwachung des Modellerfolgs, erneutes Training und erneute Bereitstellung zum Sicherstellen der Vorhersagequalität.
{: shortdesc}

**Szenarioname**: Beste Auswahl für ein Medikament zur Herzbehandlung.

**Szenariobeschreibung**: Ein Biomedizinisches Unternehmen, das Herzmedikamente herstellt, möchte ein Modell bereitstellen, das das jeweils beste Medikament für die Herzbehandlung auswählt. Wenn neue Erkenntnisse über die Wirksamkeit von Arzneimitteln entstehen, sollte eine Modellaktualisierung in Betracht gezogen werden. Ein Data-Scientist hat ein Vorhersagemodell vorbereitet und
teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, das Modell bereitzustellen, die Lernkonfiguration festzulegen und die Lerniteration auszuführen, um das Modell zu evaluieren, neu zu trainieren und erneut bereitzustellen.

**Hinweis**: Sie können sich auch mit einem [Beispiel-Notizbuch für Python](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325) vertraut machen, das ein Beispielmodell erstellt, ein Ausbildungssystem konfiguriert und die Lerniteration ausführt.

## Voraussetzungen

Um mit den Beispielen arbeiten zu können, müssen Sie über die folgenden Services verfügen:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) zum Speichern der Feedbackdaten.
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark): Berechtigungsnachweise für die Serviceinstanz. Sie können [diesen Link](https://console.bluemix.net/catalog/services/apache-spark) verwenden, um einen zu erstellen.

## Beispielmodell verwenden

1. Wechseln Sie zur Registerkarte **Beispiele** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Suchen Sie im Abschnitt **Beispielmodelle** das Beispiel mit der Kachel 'Heart Drug Selection'
   und klicken Sie auf das Symbol **Modell hinzufügen** (+).

Das Beispielmodell **Heart Drug Selection** (Auswahl eines Herzmedikaments) wird in der Liste der verfügbaren Modell in der Registerkarte **Modelle** angezeigt.

## Zugriffstoken generieren

Generieren Sie ein Zugriffstoken, das den Benutzer und das Kennwort verwendet, die auf der Registerkarte 'Berechtigungsnachweise' der {{site.data.keyword.pm_full}}-Serviceinstanz angegeben sind.

Anforderungsbeispiel:

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Ausgabebeispiel:

```
{"token":"**********"}
```
{: codeblock}

Verwenden Sie den folgenden Terminalbefehl, um Ihren Tokenwert dem
Umgebungsvariablentoken zuzuweisen:

```
token="<token_value>"
```
{: codeblock}

## Mit veröffentlichten Modellen arbeiten

Verwenden Sie den folgenden API-Aufruf, um Ihre Instanzdetails abzurufen. Zum Beispiel:

* `URL`-Adresse für veröffentlichte Modelle
* `URL`-Adresse für Bereitstellungen
* Nutzungsinformationen

Anforderungsbeispiel:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Ausgabebeispiel:

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

Nach dem Abrufen der `URL`-Adresse **published_models** verwenden Sie den folgenden API-Aufruf, um die Modelldetails abzurufen:

Anforderungsbeispiel:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

Ausgabebeispiel:

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


## Fortlaufendes Ausbildungssystem für ein veröffentlichtes Modell einrichten

Um ein kontinuierliches Ausbildungssystem für Ihr Modell zu konfigurieren, müssen Sie die folgenden Tasks ausführen:

1.  [Bereiten Sie den Berechtigungsheader vor](#prepare-the-authorization-header).
2.  [Bereiten Sie den Feedbackdatensatz vor](#prepare-the-feedback-data-set).
3.  [Bereiten Sie die Konfigurationsnutzdaten vor](#prepare-the-configuration-payload).

Nachdem Sie den Berechtigungsheader, den Feedbackdatensatz und die Konfigurationsnutzdaten vorbereitet haben, können Sie mit dem Iterieren des fortlaufenden Ausbildungssystems beginnen. Weitere Informationen finden Sie unter [Fortlaufende Iteration des Ausbildungssystems ausführen](#run-continuous-learning-system-iteration).

### Berechtigungsheader vorbereiten

Geben Sie die folgenden Details an, um den Berechtigungsheader vorzubereiten, der die Berechtigungsnachweise für das {{site.data.keyword.pm_full}}-Token und die Spark-Instanz kombiniert:

*  Das im vorherigen Schritt erstellte Zugriffstoken
*  Spark-Serviceberechtigungsnachweise, die Sie auf der Registerkarte 'Serviceberechtigungsnachweise' des {{site.data.keyword.Bluemix_notm}} Spark-Servicedashboards finden. Bevor die Bereitstellungsanforderung gestellt werden kann, müssen Spark-Berechtigungsnachweise als Base64 codiert werden. Sie werden im Header der Anforderung im Feld X-Spark-Serviceinstanz übergeben.

   Setzen Sie abhängig vom verwendeten Betriebssystem einen der folgenden Terminalbefehle ab, um die Base64-Codierung auszuführen, und weisen Sie ihn der Umgebungsvariablen zu.

   Verwenden Sie unter dem Betriebssystem **macOS** den folgenden Befehl:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net", "instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Unter den Betriebssystemen **Microsoft Windows** oder **Linux** müssen Sie den Parameter `--wrap=0` zusammen mit dem `base64`-Befehl verwenden, um die Base64-Codierung auszuführen:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### Feedbackdatensatz vorbereiten

Das Ausbildungssystem erfordert eine Verbindung zu Trainingsdaten (Daten, die für das Modelltraining verwendet werden) sowie Feedbackdaten. Der Feedbackdatenspeicher wird verwendet, um Ihr Modell bei Bedarf zu überwachen und neu zu trainieren. {{site.data.keyword.dashdbshort}} wird als Feedbackdatenspeicher unterstützt. Die Feedbacktabelle wird vom {{site.data.keyword.pm_short}}-Service verwaltet, geändert und verwendet.
Zum Vorbereiten der Tabelle **DRUG_FEEDBACK_DATA** in **{{site.data.keyword.dashdbshort}}** müssen Sie die folgenden Schritte ausführen:

1. Erstellen Sie eine Instanz des [{{site.data.keyword.dashdbshort}}-Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (ein Eintragsplan wird angeboten).
2. Erstellen Sie die Tabelle `DRUG_FEEDBACK_DATA` in **{{site.data.keyword.dashdbshort}}**.
   1. Laden Sie vom GitHub-Repository die Datei [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) herunter.
   2. Zum Beginnen der Arbeit mit **{{site.data.keyword.dashdbshort_notm}}** klicken Sie auf das Symbol **Konsole öffnen**.
   3. Wählen Sie die Option **Daten laden** und den Ladetyp **Desktop** aus.
   4. **Ziehen** Sie die zuvor heruntergeladene Datei und klicken Sie auf **Weiter**.
   5. Zum Importieren von Daten klicken Sie auf **Schema** und anschließend auf **Neue Tabelle**.
   6. Geben Sie im Feld **Neue Tabelle** einen Namen an und klicken Sie auf **Weiter**.
   7. Verwenden Sie als **Feldtrennzeichen** ein Semikolon (;).
   8. Klicken Sie auf **Weiter**, um eine Tabelle mit den hochgeladenen Daten zu erstellen.

**Hinweis**: Sie können der Feedbackdatenbank neue Feedbackdatensätze hinzufügen, indem Sie diesen [REST-API-Endpunkt](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) verwenden. Weitere Informationen finden Sie im Abschnitt [Feedbackdatenspeicher](#feedback-data-store).

### Konfigurationsnutzdaten vorbereiten

Zur Angabe der Ausbildungskonfiguration müssen Sie den folgenden Endpunkt verwenden:

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Zum Fertigstellen der Nutzdaten müssen Sie die Werte der folgenden Parameter definieren:

<dl><dt>feedback_data_reference</dt>
<dd>Verbindung und Quelle des Feedbackdatenspeichers</dd>
<dt>min_feedback_data_size</dt>
<dd>Mindestanzahl von Datensätzen im Feedbackdatensatz, um die Iteration des fortlaufenden Ausbildungssystems starten zu können
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] (nie, immer, bedingt) gibt an, wann der Prozess für erneutes Training ausgelöst wird. Ist der Wert auf [conditionally] festgelegt, wird der Prozess für erneutes Training ausgelöst, sobald die Modellqualität unter dem angegebenen Schwellenwert liegt.
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] (nie, immer, bedingt) gibt an, wann das neu trainierte Modell bereitgestellt werden soll. Ist der Wert auf [conditionally] festgelegt, wird die erneute Bereitstellung des Modells ausgelöst, sobald die Qualität des neu trainierten Modells über der Qualität des aktuell bereitgestellten Modells liegt.</dd></dl>

Anforderungsbeispiel:

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
           "connection": {
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

Ausgabebeispiel:

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
   "connection": {
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

**Hinweis**: Im Beispiel werden für die Parameter `auto_retrain` und `auto_redeploy` Standardwerte verwendet. Weitere Informationen zu diesen Parametern finden Sie im Abschnitt [REST-API-Dokumentation](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration) für den Parameter `learning_configuration`.

## Führen Sie die fortlaufende Iteration des Ausbildungssystems aus.

Zum Starten einer Iteration des Ausbildungssystems verwenden Sie die folgende REST-API-Methode. Während der Iteration wird das veröffentlichte Modell evaluiert. Liegt die evaluierte Genauigkeit unter dem angegebenen Schwellenwert, wird das erneute Training des Modells ausgelöst. Beide Datensätze, der für das Training als auch der für das Feedback, werden für das erneute Training und die Evaluierung verwendet.

Anforderungsbeispiel:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Ausgabebeispiel:

```
Die Anforderung wurde erfüllt und im Ergebnis wurde eine neue Ressource erstellt.
```
{: codeblock}

Geben Sie die folgende Anforderung aus, um den Status der Wiederholung zu überprüfen:

Anforderungsbeispiel:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Ausgabebeispiel:

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

Sie können auch die Liste der Evaluierungsmetriken und Werte abrufen, indem Sie die folgende Anforderung ausgeben:

Anforderungsbeispiel:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

Ausgabebeispiel:

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

## Feedbackdatenspeicher

Sie können einen [Feedback-Endpunkt](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) verwenden, um neue Datensätze (Datensätze, die nicht in einem Trainingsprozess verwendet wurden) an einen Feedbackdatenspeicher zu senden, der in der Lernkonfiguration definiert ist. Der Feedbackdatenspeicher wird verwendet, um Ihr Modell bei Bedarf zu überwachen und neu zu trainieren. {{site.data.keyword.dashdbshort}} wird als Feedbackdatenspeicher unterstützt. Wenn die Feedbacktabelle nicht vorhanden ist, wird sie vom Service erstellt. Wenn die Tabelle vorhanden ist, wird das Schema darauf hin geprüft, ob es mit dem der Trainingstabelle übereinstimmt, und eine zusätzliche Spalte mit dem Namen `_training` wird an die Tabelle angehängt. Die zusätzliche Spalte wird verwendet, um Datensätze zu markieren, die im Prozess für das erneute Training verwendet werden.

**Hinweis:** Die Feedbacktabelle wird vom {{site.data.keyword.pm_short}}-Service verwaltet, geändert und verwendet.

Sie können einen neuen Feedback-Datensatz an den Feedbackdatenspeicher senden, indem Sie die folgende Anforderung absetzen:

Anforderungsbeispiel:

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

## Weitere Informationen

Sind Sie bereit? Informationen zum Erstellen einer Serviceinstanz oder zum Binden
einer Anwendung finden Sie unter [Service mit Spark- und Python-Modellen verwenden](using_pm_service_dsx.html) oder
[Service mit IBM® SPSS®-Modellen verwenden](using_pm_service.html).

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Modelle](pm_service_api_spark.html) oder [Service-
API für IBM® SPSS® Modelle] (pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie im [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM Data Science Experience und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
