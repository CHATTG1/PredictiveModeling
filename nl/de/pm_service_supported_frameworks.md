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

# Unterstützte Frameworks

Der {{site.data.keyword.pm_full}}-Service unterstützt die folgenden Frameworks für Modellentwicklung und -überprüfung als Teil der Modelllebenszyklusverwaltung.
{: shortdesc}

## Spark MLlib

Der {{site.data.keyword.pm_full}}-Service unterstützt Spark MLlib für die Online-, Stapel (Betaversion) und Datenstrom- (Betaversion) Bereitstellung sowie für den Scoring-Service. Nur bestimmte Versionen und Laufzeitumgebungen werden unterstützt.

### Funktionalität

* Bereitstellungstypen:
  * Online
  * Stapel mit Object Storage, Db2 Warehouse on Cloud und Db2 on Cloud
  * Datenstrom mit Message Hub
*  Fortlaufendes Ausbildungssystem

### Unterstützte Versionen

*  Spark 2.0

### Einschränkungen

  *  Nur Klassifizierungs- und Regressionsmodelle werden unterstützt.
  *  Modelle, die Verweise zu angepassten Transformern, benutzerdefinierten Funktionen und Klassen enthalten, werden nicht unterstützt.
  * Stapel- und Datenstrombereitstellungen verwenden den IBM Cloud Apache Spark-Service. Beachten Sie in Abhängigkeit vom abonnierten Serviceplan die folgenden Einschränkungen in der Anzahl paralleler 'spark-submit'-Jobs (['spark-submit'-Dokumentation verwenden](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite: nur ein 'spark-submit'-Job zur Zeit
    * Reserved Enterprise: 150 'spark-submit'-Jobs gleichzeitig

## Scikit-learn

Es gibt Unterstützung für scikit-learn für den {{site.data.keyword.pm_full}} Online Deployment and Scoring-Service. Nur bestimmte Versionen und Laufzeitumgebungen werden unterstützt.

### Funktionalität

* Bereitstellungstypen:
  * Online

### Unterstützte Versionen

- Anaconda 4.2.x für Python 3.5 Runtime
- Scikit-Learn 0.17

### Python Runtime

Die Python-Laufzeit für Modelle, die mithilfe des WML Online Deployment and Scoring-Service bereitgestellt werden müssen, ist Python 3.5.

In einigen Fällen, bei denen Modelle in der Laufzeit Python 2.7 erstellt wurden, kann die Bereitstellung aufgrund von Inkompatibilitäten zwischen Python 2.7 und Python 3.5 fehlschlagen.

### Unterstützte Python-Pakete

Eine Liste der Python-Pakete, die vom WML Online Deployment and Scoring-Service unterstützt werden, finden Sie [hier](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Einschränkungen

Es gibt einige Einschränkungen, die einige oder alle der folgenden Begrenzungen enthalten können:

* Es werden nur Klassifizierungs- und Regressionsmodelle unterstützt.
* Modelle können nur Verweise auf die Pakete (und Paketversionen) enthalten, die von Anaconda 4.2.x für die Laufzeit Python 3.5 unterstützt werden.
* Der Endpunkt "schema" wurde zurückgegeben, während die Bereitstellungsanforderung nicht implementiert wurde.
* Modelle, die Pandas Dataframe als Eingabetyp für die API "predict()" erfordern, werden nicht unterstützt.
* Modelle, die Verweise zu angepassten Transformern, benutzerdefinierten Funktionen und Klassen enthalten, werden nicht unterstützt.

## XGBoost

Es gibt Unterstützung für XGBoost für den {{site.data.keyword.pm_full}} Online Deployment and Scoring-Service. Nur bestimmte Versionen und Laufzeitumgebungen werden unterstützt.

### Funktionalität

* Bereitstellungstypen:
  * Online

### Unterstützte Versionen

- XGBoost 0.6
- Anaconda 4.2.x für Python 3.5 Runtime
- Scikit-Learn 0.17

### Python Runtime

Die Python-Laufzeit für Modelle, die mithilfe des WML Online Deployment and Scoring-Service bereitgestellt werden müssen, ist Python 3.5.

In einigen Fällen, bei denen Modelle in der Laufzeit Python 2.7 erstellt wurden, kann die Bereitstellung aufgrund von Inkompatibilitäten zwischen Python 2.7 und Python 3.5 fehlschlagen.

### Unterstützte Python-Pakete

Eine Liste der Python-Pakete, die vom WML Online Deployment and Scoring-Service unterstützt werden, finden Sie [hier](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Einschränkungen

Es gibt einige Einschränkungen, die einige oder alle der folgenden Einschränkungen enthalten:

* Modelle können nur Verweise auf die Pakete (und Paketversionen) enthalten, die von Anaconda 4.2.x für die Laufzeit Python 3.5 unterstützt werden.
* Der Endpunkt "schema" wurde zurückgegeben, während die Bereitstellungsanforderung nicht implementiert wurde.
* Die Wahrscheinlichkeit der Prognose wird zu diesem Zeitpunkt nicht als Antwort der Scoring-Anforderung zurückgegeben.
* Modelle, die Verweise zu angepassten Transformern, benutzerdefinierten Funktionen und Klassen enthalten, werden nicht unterstützt.

## SPSS Modeler

Es gibt Unterstützung für IBM® SPSS® Modeler-Datenströme für den {{site.data.keyword.pm_full}} Online- bzw. Stapelbereitstellungs- und Scoring-Service. Nur bestimmte Versionen und Laufzeitumgebungen werden unterstützt.

### Funktionalität

* Bereitstellungstypen:
  * Online
  * Stapel
* Erneutes Trainine
* Datenstrom ausführen

### Unterstützte Versionen

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Einschränkungen

Die folgenden Einschränkungen gelten für IBM® SPSS® Modeler-Datenströme:

*  Datenströme, die mittels IBM® Data Science Experience-Ablaufeditor erstellt wurden, werden nicht unterstützt.
*  Beim Vorbereiten einer Scoring-Verzweigung für die Verwendung im Echtzeit-Scoring müssen Eingabedaten, die mit der Scoring-Anforderung eingehen, den Quellenknoten ersetzen, der in der Scoring-Verzweigung entworfen wurde, und die sich ergebene Vorhersageanalyse muss wieder in den Antwortablauf übergeben werden (wobei sie den Endknoten in der Gestaltung der Scoring-Verzweigung ersetzt).
*  Da die Scoring-Verzweigung für die Echtzeitausführung in {{site.data.keyword.Bluemix_short}} vorbereitet ist, kann sie keine Verbindung zu einem externen Service anfordern. Ein IBM Analytical Decision Management-Scoring-Verzweigungsdesign kann keine Referenzen auf Regeln oder andere Modelle enthalten, die in einem IBM® SPSS® Collaboration- oder Deployment Services-Repository gespeichert sind.
*  Die Ausführung einer Scoring-Verzweigung für Echtzeitscoring in {{site.data.keyword.Bluemix_short}} kann keinen externen Service erfordern. Sie können keine Modellalgorithmen bereitstellen oder bewerten, die einen IBM® SPSS® Analytic Server und Apache Hadoop-Data-Store in Echtzeit erfordern.
*  {{site.data.keyword.pm_short}} unterstützt Modeler-integriertes Python-Scripting. Es gibt einige Einschränkungen wegen der Methode, die für die Verarbeitung von Datenströmen vor deren Ausführung in {{site.data.keyword.pm_short}} verwendet wird. Wenn ein Benutzer auswählt, dass er die Ausführung des Datenstroms steuert, wird in der Regel der Endknoten des Zweigs referenziert. Wenn bei {{site.data.keyword.pm_short}} der Datenstrom verarbeitet wird, kennzeichnen wir die Knoten von JSON, die überschrieben werden, und ersetzen diese dann, bevor der Datenstrom ausgeführt wird. Auf diese Weise schlägt der Datenstrom im Script fehl, da die referenzierten Eingabe- und Ausgabeknoten nicht mehr vorhanden sind. Die Lösung ist die Verwendung der ID eines anderen Knotens, um den Zweig während der Ausführung eindeutig anzugeben. Dies stellt sicher, dass der Datenstrom wie im integrierten Python-Script definiert ausgeführt wird.

## Weitere Informationen

Weitere Informationen zur aktuellen Unterstützung für IBM® SPSS® Analytic Server-trainierte Vorhersagemodelle finden Sie im Abschnitt [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) des [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
