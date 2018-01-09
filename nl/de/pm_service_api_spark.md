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

# REST-API

Der {{site.data.keyword.pm_short}}-Service ist eine Gruppe von REST-APIs, die Sie über
jede Programmiersprache aufrufen können und die die Integration von Data Science
Experience-Analysen in Ihren Anwendungen zulassen. Binden Sie Ihre
{{site.data.keyword.Bluemix_short}}-Anwendungen an die {{site.data.keyword.pm_short}}-Serviceinstanz und
generieren Sie die Vorhersageanalyse, die Ihre Anwendungen benötigen, um
Ihren Benutzern einen Mehrwert zu bieten. Verwalten Sie Modelle im [Administrationsdashboard](pm_service_ui_spark.html) und
verwenden Sie das Dashboard, um Online-, Stapel- oder
Streamingbereitstellungen zu erstellen, die in Ihre Anwendungen
integriert sind.
{: shortdesc}

Entwickeln Sie Anwendungen für Spark-, Python- und IBM® SPSS®-Modelle, die über die leistungsfähigen
[REST-APIs](https://watson-ml-api.mybluemix.net/) in einer Serviceinstanz bereitgestellt werden:

*  Metadaten für ein gegebenes Vorhersagemodell abrufen
*  Modelle bereitstellen und bereitgestellte Modelle verwalten
    *  Onlinebereitstellung (Scoring)
    *  Stabelbereitstellung (unterstützt IBM Cloud Object Storage und {{site.data.keyword.dashdbshort}})
    *  Datenstrombereitstellung (unterstützt IBM Cloud Message Hub)
*  Metadaten für eine gegebene Bereitstellung abrufen
*  Bereitgestellte Modelle mittels fortlaufendem Ausbildungssystem überwachen und erneut trainieren
*  Vorhersageanalysen generieren, indem
Sie
Scoring-Anforderungen für bereitgestellte Vorhersagemodelle erstellen

Weitere Informationen finden Sie in den folgenden Beispielen für die Verwendung einer REST-API:

*  [Onlinemodelle bereitstellen](pm_service_api_spark_online.html)
*  [Onlinemodelle bewerten](pm_service_api_develop_score.html)
*  [Stapelmodelle bereitstellen](pm_service_api_spark_batch.html)
*  [Datenstrommodelle bereitstellen](pm_service_api_spark_streaming.html)
*  [Fortlaufendes Ausbildungssystem](pm_service_api_spark_learning_system.html)

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
