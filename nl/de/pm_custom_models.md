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

# Entwicklung und Persistenz des angepassten Modells

Mit dem {{site.data.keyword.pm_full}}-Service können Sie ein Modell bereitstellen und
prädiktive Analysen generieren, indem Sie Scoring-Anforderungen für
das bereitgestellte Modell erstellen.
{: shortdesc}

## Mit angepassten Modellen arbeiten

### Szenarioname: Produktlinienvorhersage.

### Szenariobeschreibung:

Unser Kunde führt eine der
bekanntesten Handelsketten in Europa. Er möchte, dass wir herausfinden,
an welcher Produktlinie die Kunden jeweils interessiert sind, z. B.
persönliche Accessoires, Campingausrüstung oder Outdoorschutz.
Ein Data-Scientist entwickelt ein Vorhersagemodell und teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, das Modell bereitzustellen und eine
Vorhersageanalyse zu generieren, indem Sie Scoring-Anforderungen für
das bereitgestellte Modell erstellen.

### Entwicklung und Persistenz des angepassten Modells

#### Data Science Experience verwenden

Verwenden Sie [Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience), um angepasste Modelle zu erstellen. Nach Ihrer Anmeldung müssen Sie sich anmelden, um die folgenden Schritte durchführen zu können.

1. Eine Organisation und einen Bereich erstellen. Wenn Sie sich zum ersten Mal anmelden, müssen Sie diese Informationen angeben. Klicken Sie auf **Weiter**, um die Standardwerte zu akzeptieren.
2. Wechseln Sie, nachdem die Organisation erstellt wurde, zu **Projekte** und klicken Sie auf **Neues Projekt**.
3. Geben Sie einen Namen und eine Beschreibung für Ihr
Projekt an und klicken Sie auf **Erstellen**. Der Projektname, den Sie angegeben haben, wird als
   Name des Zielcontainers verwendet.
4. Nach der Erstellung des Projekts können Sie eine der folgenden Tasks ausführen:
   
   *  **Notizbücher hinzufügen** und mit der Entwicklung eigener Modelle beginnen oder eines der folgenden Beispiel-Notizbücher hochladen:
        *  [Entwickeln von Spark MLlib-Modellen mit Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Entwickeln von Spark MLlib-Modellen mit Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Entwickeln von scikit-learn-Modellen mit Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **Modelle hinzufügen** und mit der Entwicklung eigener Modelle mithilfe des Modellassistenten beginnen.


#### Lokale Umgebung verwenden

Sie können auch eine Umgebung Ihrer Wahl verwenden, um ein Modell zu entwickeln und später zu veröffentlichen, bereitzustellen und zu bewerten, indem Sie die [allgemeine API-Clientbibliothek]() von Watson Machine Learning verwenden, die auf PyPI zur Verfügung steht.
Weitere Informationen zur Clientbibliothek finden Sie im [Beispielnotizbuch](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token = 4b39718f9e1f1de55e6e67e8dcbbb5f0cac848f390d73478d0dea9c1a8af24550 &cm_mc_uid=30670837705115063231884 &cm_mc_sid_50200000 = 1509364125) und der [Dokumentation](pm_service_client_library.html).

**Hinweis:** Die allgemeine API-Clientbibliothek ist in der **Betaversion** verfügbar.

### Bereitstellung und Scoring des angepassten Modells

Sie können die API verwenden, um Online-, Stapel- und Streamingmodelle zu implementieren und zu bewerten.

*  [Onlinemodelle bereitstellen](pm_service_api_spark_online.html)
*  [Stapelmodelle bereitstellen](pm_service_api_spark_batch.html)
*  [Streamingmodelle bereitstellen](pm_service_api_spark_streaming.html)

## Weitere Informationen

Sind Sie bereit? Informationen zum Erstellen einer Serviceinstanz oder zum Binden
einer Anwendung finden Sie unter [Service mit Spark- und Python-Modellen verwenden](using_pm_service_dsx.html) oder
[Service mit IBM® SPSS®-Modellen verwenden](using_pm_service.html).

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Modelle](pm_service_api_spark.html) oder [Service-
API für IBM® SPSS® Modelle] (pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie im [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM® Data Science Experience und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
