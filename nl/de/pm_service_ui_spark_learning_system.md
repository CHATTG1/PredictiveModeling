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

Das {{site.data.keyword.pm_full}} fortlaufende Ausbildungssystem bietet eine automatisierte Überwachung des Modellerfolgs, des erneuten Trainings und der erneuten Bereitstellung, um die Vorhersagequalität sicherzustellen.
{: shortdesc}

**Szenarioname**: Beste Auswahl für ein Medikament zur Herzbehandlung.

**Szenariobeschreibung**: Ein Biomedizinisches Unternehmen, das Herzmedikamente herstellt, möchte ein Modell bereitstellen, das das jeweils beste Medikament für die Herzbehandlung auswählt. Wenn neue Erkenntnisse über die Wirksamkeit von Arzneimitteln entstehen, sollte eine Modellaktualisierung in Betracht gezogen werden. Ein Data-Scientist bereitet ein Vorhersagemodell vor und teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, das Modell bereitzustellen, die Lernkonfiguration festzulegen und die Lerniteration auszuführen, um das Modell zu evaluieren, neu zu trainieren und erneut bereitzustellen.

## Voraussetzungen

Um mit diesem Beispiel arbeiten zu können, benötigen Sie die folgenden Services:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) zum Speichern der Feedbackdaten
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark)-Service

   Verwenden Sie [diesen Link](https://console.bluemix.net/catalog/services/apache-spark), um eine Apache Spark-Instanz zu erstellen, wenn Sie noch nicht über eine Spark-Instanz verfügen.

## Beispielmodell verwenden

1. Wechseln Sie zur Registerkarte **Beispiele** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Suchen Sie im Abschnitt **Beispielmodelle** die Kachel **Heart Drug Selection**
   (Auswahl eines Herzmedikaments) und klicken Sie auf das Symbol **Modell hinzufügen**(+).

Das Beispielmodell 'Heart Drug Selection' wird in der Liste der verfügbaren Modelle auf der Registerkarte **Modelle** angezeigt.


## Fortlaufendes Ausbildungssystem für das veröffentlichte Modell festlegen

1.  Wechseln Sie zur Registerkarte **Modelle** des {{site.data.keyword.pm_full}}-Dashboards.
2.  Klicken Sie im Menü **Aktionen** auf **Details anzeigen**.
3.  Blättern Sie zu **Erneutes Training - Details** und drücken Sie **Bearbeiten**.
4.  Sie müssen die folgenden Eingaben bereitstellen.

    **Feedback-Verbindung**: Details zu {{site.data.keyword.dashdbshort}}, die als Eingabe (Feedbackdaten) für die Modellevaluation und das erneute Training verwendet werden.

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Verwenden Sie die folgenden Anweisungen, um die Tabelle `DRUG_FEEDBACK_DATA` in {{site.data.keyword.dashdbshort}} vorzubereiten.
    
    - Erstellen Sie eine Instanz des [{{site.data.keyword.dashdbshort}}-Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (ein Eintragsplan wird angeboten).
    - Erstellen Sie die Tabelle **DRUG_FEEDBACK_DATA** in **{{site.data.keyword.dashdbshort_notm}}**.
      + Laden Sie die Datei [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) vom GitHub-Repository herunter.
      + Klicken Sie auf **Konsole öffnen**, um mit**{{site.data.keyword.dashdbshort_notm}}** zu beginnen.
      + Klicken Sie auf **Daten laden** und wählen Sie den Ladetyp **Desktop** aus.
      + **Ziehen** Sie die zuvor heruntergeladene Datei in den Ladebereich und klicken Sie auf **Weiter**.
      + Wählen Sie **Schema** aus, um Daten zu importieren und klicken Sie auf **Neue Tabelle**.
      + Geben Sie einen Namen für die neue Tabelle ein und klicken Sie auf **Weiter**.
      + Verwenden Sie ein Semikolon (;) als **Feldtrennzeichen**.
      + Klicken Sie auf **Weiter**, um eine Tabelle mit den hochgeladenen Daten zu erstellen.

     **Hinweis**: Sie können neue Feedbackdatensätze zur Feedbackdatenbank hinzufügen, indem Sie den [REST-API-Endpunkt](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) verwenden.

     **Minimale Datengröße**: Die minimale Anzahl der Datensätze im Feedbackdatensatz zum Starten der Evaluierung des Modells (Teil der Iteration des Ausbildungssystems).

     ```
     20
     ```
     {: codeblock}

     **Spark-Verbindung**: Berechtigungsnachweise für den Spark-Service finden Sie auf der Registerkarte **Serviceberechtigungsnachweise** des {{site.data.keyword.Bluemix_notm}} Spark-Servicedashboards.

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

     **Schwellenwerte**: Der Schwellenwert, der den Prozess des erneuten Trainings auslöst. Wenn der Metrikwert, z. B. der Genauigkeitswert, der während der Modellevaluierung berechnet wird, unter dem Schwellenwert liegt, löst er ein erneutes Training aus.

     ```
     0.8
     ```

     **Erneut trainiertes Modell automatisch bereitstellen**: Erneut trainiertes Modell bereitstellen, wenn es einen besseren Metrikwert aufweist als das bereitgestellte.

5.  Klicken Sie auf die Option zum **Konfigurieren**.

## Führen Sie die fortlaufende Iteration des Ausbildungssystems aus.

Ein veröffentlichtes Modell wird iterativ evaluiert. Wenn der Metrikwert, z. B. der Genauigkeitswert, der während der Modellevaluierung berechnet wird, unter dem Schwellenwert liegt, löst er ein erneutes Training aus. Sowohl Training- als auch Feedbackdatensatz werden für diesen iterativen Prozess des erneuten Trainings und der Evaluierung verwendet.

1. Zum Starten einer neuen Wiederholung klicken Sie im Formular **Details** auf der Registerkarte **Überwachen** auf **Evaluieren**.
3. Um der Ergebnis der Iteration zu prüfen, wechseln Sie entweder zur Tabelle für Evaluierungsereignisse oder zur Diagrammansicht. 

   Sie können Informationen zur Modellqualität anzeigen, die basierend auf den Feedbackdaten (Überwachung) berechnet werden. Sie können auch Informationen über die Qualität des neu trainierten Modells anzeigen. Dies ist die neue Modellversion, die erstellt und evaluiert wurde.

4. Zum Anzeigen einer Liste automatisch trainierter Modelle und ihrer Qualität klicken Sie auf **Details anzeigen** > **Übersicht** und wechseln Sie zum Abschnitt **Versionsverlauf**.

## Feedbackdatenspeicher

Sie können einen [Feedback-Endpunkt](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) verwenden, um neue Datensätze an den Feedbackspeicher zu senden, den Sie in der Konfiguration für maschinelles Lernen definiert haben. {{site.data.keyword.dashdbshort}} wird aktuell als Feedbackdatenspeicher unterstützt. Wenn die Feedbacktabelle nicht vorhanden ist, wird sie vom Service erstellt. Wenn die Tabelle vorhanden ist, wird das Schema darauf hin geprüft, ob es mit dem der Trainingstabelle übereinstimmt, und eine zusätzliche Spalte mit dem Namen `_training` wird an die Tabelle angehängt. Die Spalte `_training` wird verwendet, um Datensätze zu markieren, die im Prozess für das erneute Training verwendet werden.

Weitere Informationen und Beispiele für einen Feedbackdatenspeicher finden Sie in der [API-Dokumentation](pm_service_api_spark_learning_system.html).

**Hinweis:** Die Feedbacktabelle wird vom {{site.data.keyword.pm_full}}-Service verwaltet, geändert und verwendet.

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
