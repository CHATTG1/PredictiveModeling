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

# Einführung
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} ist ein IBM Cloud-Service, der Benutzern die Durchführung von zwei grundlegenden Operationen des maschinellen Lernens ermöglicht: Training und Scoring.
{: shortdesc}

- **Training** ist der Prozess der Feinanpassung eines Algorithmus, sodass er aus einem Datensatz lernen kann. Die Ausgabe dieser Operation wird als Modell bezeichnet. Ein Modell umfasst die erlernten Koeffizienten als mathematische Ausdrücke.
- **Scoring** ist die Operation der Vorhersage eines Ergebnisses mit einem trainierten Modell. Die Ausgabe der Scoring-Operation ist ein weiterer Datensatz, der die vorhergesagten Werte enthält.

{{site.data.keyword.pm_full}} soll den Anforderungen von zwei primären Personengruppen gerecht werden:

- Data-Scientists: Erstellen von Pipelines für maschinelles Lernen, die Datentransformationen und Algorithmen für maschinelles Lernen nutzen. In der Regel verwenden sie Notizbücher oder externe Tools, um ihre Modelle zu trainieren und zu evaluieren. Datenwissenschaftler arbeiten oft mit Datenentwicklern zusammen, um die Daten zu untersuchen und zu verstehen.
- Entwickler: Erstellen intelligenter Anwendungen, die die Vorhersage-Ausgabe der Modelle für maschinelles Lernen nutzen.

Obwohl das Training ein kritischer Schritt im maschinellen Lernprozess ist, ermöglicht {{site.data.keyword.pm_full}} es Ihnen, die Funktion Ihrer Modelle zu optimieren, indem Sie sie bereitstellen und durch diese und alle ihre Iterationen im Laufe der Zeit den tatsächlichen Geschäftswert erhalten.

## Voraussetzungen

Um {{site.data.keyword.pm_full}} aus dem {{site.data.keyword.Bluemix_short}}-Katalog zu verwenden, müssen Sie die [Serviceinstanz hier](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/) erstellen. Mit dieser Konfiguration können Sie die folgenden Tasks ausführen:

## Schritte

1. [Ihre Machine Learning-Umgebung konfigurieren](ml_getting_access.html)
1. [Ein Modell erstellen und speichern](pm_custom_models.html).
2. [Ein Modell bereitstellen](pm_service_api_spark_online.html).
3. Verwenden Sie das bereitgestellte Modell `Scoring-Endpunkt` in Ihrer Anwendung, um [Vorhersagen abzurufen.](pm_service_api_spark_building.html)

## Machine Learning mit Data Science Experience verwenden

{{site.data.keyword.pm_full}} wird in IBM Data Science Experience integriert. Sie können Clientbibliotheken für Machine Learning-APIs in Data Science Experience-Notizbüchern verwenden. Sie müssen über eine Machine Learning-Instanz verfügen, um die Modellerstellung und den Ablaufeditor verwenden zu können.

## Machine Learning mit SPSS Modeler verwenden

{{site.data.keyword.pm_full}} wird in IBM® SPSS® Modeler integriert. Sie können die Machine Learning-API verwenden, um fortgeschrittene mathematische Algorithmen zu nutzen.


## Machine Learning mit Ihrer Umgebung verwenden

{{site.data.keyword.pm_full}} kann als Hybridlösung verwendet werden, die Ihre lokale Umgebung mit der Cloud verknüpft. Sie können die Machine Learning-API verwenden, um Ihre Modelle zu veröffentlichen, bereitzustellen und zu bewerten. Weitere Informationen finden Sie im Abschnitt [DSX: Hybridmodus](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b).

## Produktinfo

Der {{site.data.keyword.pm_full}}-Service besteht aus einer Gruppe von REST-APIs, die über jede Programmiersprache aufgerufen werden können.

Der Schwerpunkt des {{site.data.keyword.pm_full}}-Service liegt zwar auf der Bereitstellung, aber Sie können IBM® SPSS® Modeler oder IBM® Data Science Experience nutzen, um Modelle und Pipelines zu erstellen und mit ihnen zu arbeiten. Sowohl SPSS® Modeler als auch Data Science Experience, die Spark MLlib und Python scikit-learn verwenden, bieten verschiedene Modellierungsmethoden, die aus maschinellem Lernen, künstlicher Intelligenz und Statistiken entnommen werden.

## Zugehörige Links

Sind Sie bereit? Informationen zum Erstellen einer Serviceinstanz oder zum Binden einer Anwendung finden unter
[Service mit Spark- und Python-Modellen verwenden](using_pm_service_dsx.html) oder
[Service mit SPSS-Modellen verwenden](using_pm_service.html).

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Modell](pm_service_api_spark.html) oder [Service-
API für SPSS-Modelle](pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie im [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM Data Science Experience und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
