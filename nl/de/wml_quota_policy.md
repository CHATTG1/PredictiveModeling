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

# Kontingentrichtlinie

Die {{site.data.keyword.pm_full}}-Engine erzwingt bestimmte Kontingente pro Instanz, um eine entsprechende Ressourcenzuordnung und -nutzung zu gewährleisten. Diese Richtlinien variieren je nach Kontoprofil, Protokollserviceverwendung und Verfügbarkeit der Ressourcen. Die Kontingentrichtlinie beschreibt Grenzwerte, die nicht durch die Preisstruktur definiert sind und **ohne vorherige Ankündigung geändert werden können**. Die folgenden Servicekontingentgrenzwerte sind momentan aktiv.
{: shortdesc}

## Ressourcennutzung

Ressourcen sind auf die folgenden Maximalwerte begrenzt:

-  **Maximal veröffentlichte Modelle**: 200 (Lite-Plan) 1000 (Standard- und Professional-Plan)
-  **Maximale Spark-ML-Modellgröße**: 200 MB
-  **Maximal bereitgestellte Modelle**: 1000 (Standard- und Professional-Plan)

**Hinweis**: Um die Grenzwerte des Lite-Plans zu verstehen, lesen Sie die Beschreibung zum [Lite-Plan](https://console.bluemix.net/catalog/services/machine-learning) im [{{site.data.keyword.Bluemix_notm}}-Katalog](https://console.bluemix.net/catalog/).

## Serviceanforderungsgrenzen

Die Anzahl der Anforderungen, die Sie pro Minute an eine bestimmte API senden können, ist begrenzt. Wenn Ihre Anwendung höhere Grenzwerte erfordert, [wenden Sie sich an den {{site.data.keyword.Bluemix_notm}} Support](https://support.ng.bluemix.net/), um Ihr Kontingent zu erweitern.

## Bereitstellungsanforderungen

Die folgenden Grenzwerte gelten für die Anforderungen an die Bereitstellungs-API:

-  **Zeitraum**: 60 Sekunden
-  **Standardgrenzwert**: 10

## Online-Prognoseanforderungen

Die folgenden Grenzwerte gelten für Anforderungen für eine [Online-Prognose](pm_service_api_spark_building.html):

-  **Zeitraum**: 60 Sekunden
-  **Standardgrenzwert**: 500

## Ressourcenmanagementanforderungen

Die folgenden Grenzwerte gelten für die kombinierten Gesamtanforderungen in dieser Liste:

[Token](https://watson-ml-api.mybluemix.net/#/Token): Anforderungen: post, get, delete, put, patch

[Bereitstellungen](https://watson-ml-api.mybluemix.net/#/Deployments): Anforderungen post, get, delete, put, patch

-  **Zeitraum**: 60 Sekunden
-  **Standardgrenzwert**: 100

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
