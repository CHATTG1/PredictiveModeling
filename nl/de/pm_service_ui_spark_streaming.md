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

# Streamingmodelle bereitstellen

Sie können den {{site.data.keyword.pm_full}}-Service verwenden, um das Modell bereitzustellen und
prädiktive Analysen zu generieren, indem Sie Scoring-Anforderungen für das bereitgestellte Streamingmodell stellen.
{: shortdesc}

**Szenarioname:** Stimmungsanalyse.

**Szenariobeschreibung:** Eine Marketing-Agentur möchten die
Stimmung zu einem bestimmten Thema erfassen. Die Agentur möchte, dass wir ein
Modell entwickeln, das eine Aussage als "POSITIVE" oder
"NEGATIVE" einstuft. Ein Data-Scientist bereitet ein Vorhersagemodell vor und teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, das Modell bereitzustellen und eine
Vorhersageanalyse zu generieren, indem Sie Scoring-Anforderungen für
das bereitgestellte Modell erstellen.

In diesem [Dokument](https://github.com/pmservice/tweet-sentiment-prediction) finden Sie weitere Informationen.

## Voraussetzungen

Um mit diesem Beispiel arbeiten zu können, müssen Sie über die folgenden Ressourcen verfügen:

* Details zu [Message Hub](https://console.bluemix.net/catalog/services/message-hub)-Themen, die als Eingabe (Tweet-Text) für das Modell und den Speicherort für die Modellausgabe (Vorhersageergebnisse) verwendet werden. Stellen Sie sicher, dass zwei Themen erstellt werden: Eingabe mit Tweet-Text und Ausgabe-Thema.
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark): Berechtigungsnachweise für die Serviceinstanz. Verwenden Sie [diesen Link](https://console.bluemix.net/catalog/services/apache-spark), um einen zu erstellen.


## Beispielmodell verwenden

1. Wechseln Sie zur Registerkarte **Beispiele** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Suchen Sie im Abschnitt **Beispielmodelle** die Kachel **Stimmungsvorhersage**
   und klicken Sie auf das Symbol 'Modell hinzufügen' (+).

Das Beispielmodell 'Stimmungsvorhersage' wird in der Liste
der verfügbaren Modelle in der Registerkarte 'Modelle' angezeigt.


## Streamingbereitstellung mit IBM Message Hub erstellen

1.  Wechseln Sie zur Registerkarte **Modelle** des {{site.data.keyword.pm_full}}-Dashboards.
2.  Klicken Sie im Menü **Aktionen** auf **Bereitstellung erstellen**.
3.  Geben Sie im Formular **Bereitstellung erstellen** **Name**, **Beschreibung** und **Datenstromtyp** an.
4.  Sie müssen die folgenden Eingaben bereitstellen:

    **Eingabeverbindung**: Details zu IBM Message Hub-Themen, die als Eingabe (Tweets) für das Modell und Speicherort für die Modellausgabe (Vorhersageergebnisse) verwendet werden.

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
   "source":{
        "topic":"sinput",
         "type":"kafka"
      }
  }
    ```
    {: codeblock}

    **Ausgabeverbindung**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
   "target":{
       "topic":"soutput",
         "type":"kafka"
      }
 }
    ```
    {: codeblock}

    **Spark-Verbindung**: Berechtigungsnachweise für den Spark-Service finden Sie auf der Registerkarte 'Serviceberechtigungsnachweise' des {{site.data.keyword.Bluemix_notm}} Spark-Servicedashboards.

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

5. Klicken Sie auf **Speichern**.

Das Vorhersageergebnis wird an das definierte MessageHub-Thema (Ausgabeverbindung) gesendet.

## Bereitstellungsdetails abrufen

Sie können den Status und Parameter im Zusammenhang mit den bereitgestellten Modellen überprüfen.

1. Wechseln Sie zur Registerkarte **Bereitstellungen** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Klicken Sie im Menü **Aktionen** auf **Details anzeigen**.

## Eine Streamingbereitstellung löschen

Sie können die Bereitstellung löschen, wenn sie nicht mehr benötigt wird, indem Sie eine
Abfrage wie die im folgenden Beispiel ausführen.

1. Wechseln Sie zur Registerkarte **Bereitstellungen** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Klicken Sie im Menü **Aktionen** auf **Löschen**.

## Weitere Informationen

Sind Sie bereit? Informationen zum Erstellen einer Serviceinstanz oder zum Binden
einer Anwendung finden Sie unter [Service mit Spark- und Python-Modellen verwenden](using_pm_service_dsx.html) oder
[Service mit IBM® SPSS®-Modellen verwenden](using_pm_service.html).

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Model](pm_service_api_spark.html) oder [Service
API für IBM® SPSS®-Modelle](pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM Data Science Experience und den von ihm bereitgestellten
Modellierungsalgorithmen finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
