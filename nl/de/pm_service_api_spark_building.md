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

# Vorhersageanalyseanwendungen erstellen

Sie können den {{site.data.keyword.pm_full}}-Service verwenden, um eine Node.js-Anwendung bereitzustellen.
{: shortdesc}

**Szenarioname:** Produktlinienvorhersage.

**Szenariobeschreibung:** Unser Kunde führt
eine der bekanntesten Handelsketten in Europa. Er möchte, dass wir herausfinden, an welcher Produktlinie die Kunden jeweils interessiert sind, z. B. persönliche Accessoires, Campingausrüstung oder Outdoorschutz.
Ein Data-Scientist entwickelt ein Vorhersagemodell und
teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, die
Node.js-Anwendung vorzubereiten und zu implementieren,
die Sportproduktlinien empfiehlt, indem Scoring-Anforderungen an das
bereitgestellte Modell gestellt werden.

1. Melden Sie sich bei {{site.data.keyword.Bluemix_short}} an und erstellen Sie eine Instanz von {{site.data.keyword.pm_full}}.
2. Starten Sie das
{{site.data.keyword.pm_full}}
Dashboard.
3. Wechseln Sie zur Registerkarte
**Beispiele**.
4. Suchen Sie im Abschnitt **Beispielmodelle** die Kachel **Produktlinienvorhersage**
   und klicken Sie auf das Symbol **Modell hinzufügen** (+). Das
   Beispielmodell zur Produktlinienvorhersage wird in der Liste der verfügbaren
   Modelle in der Registerkarte **Modelle** angezeigt.

   **Hinweis**: Wenn Sie statt der Beispiele Ihr eigenes Data Science Experience-
   Projekt und zugehörige Modelle verwenden möchten, müssen Sie ein
   angepasstes Modell im {{site.data.keyword.pm_short}}-Repository als persistent definieren. Dies können Sie entweder mithilfe der
REST-API oder mithilfe der Clientbibliotheken realisieren. Detaillierte Informationen hierzu finden Sie unter
Entwicklung und Persistenz des angepassten Modells.

5. Klicken Sie im Menü **Aktion** auf **Bereitstellung erstellen**, um das
   Modell zur Produktlinienvorhersage als Onlinebereitstellung bereitzustellen.
6. Geben Sie den Bereitstellungsnamen an (z. B. Product Line Prediction) und klicken Sie auf
Speichern. Sie sollten jetzt die Bereitstellung Product Line Prediction in
der Liste von Bereitstellungen sehen.
7. Wählen Sie im Menü Aktion den Eintrag Details anzeigen für Ihre Bereitstellung aus.
   Der im Detailteilfenster angezeigte
Scoring-Endpunkt wird zum Ausführen von Scoring-Anforderungen verwendet.
8. Zum Ausführen von Scoring-Anforderungen über die REST-API, sind
ein Scoring-Endpunkt und ein Berechtigungstoken erforderlich. Weitere Informationen
finden Sie unter Online-Modelle bereitstellen.
9. Experimentieren Sie mit der Node.js-Beispielanwendung, die sich an folgendem Speicherort befindet:
   https://github.com/pmservice/product-line-prediction.

   Zum automatischen Bereitstellen von Beispielcode in Ihrem
   {{site.data.keyword.Bluemix_short}}-Bereich wechseln Sie zur Registerkarte 'Beispiele', wählen im Abschnitt
   'Beispielanwendungen' die Anwendung 'Produktlinienvorhersage'
   aus und implementieren sie, indem Sie auf das Symbol (+) klicken.
   Authentifizieren Sie sich im DeployToBluemix-Formular, falls Sie dazu
aufgefordert werden.

   Jetzt sollte die Beispielanwendung im {{site.data.keyword.Bluemix_short}}-
   Dashboard im Abschnitt 'Alle Apps' angezeigt werden.

10. Klicken Sie auf die Anwendung, um Details einzublenden. Hier können Sie
    Anwendungsdetails wie z. B. die Anzahl der Instanzen oder die
    Speicherzuordnung ändern.
11. Binden Sie Ihre Anwendung an die {{site.data.keyword.pm_short}}-
    Instanz. Klicken Sie auf der Registerkarte
**Verbindungen** auf **Vorhandenen verbinden**.
    Nach
dem erneuten Staging ist Ihre Anwendung mit der Serviceinstanz
verbunden.
12. Führen Sie die Anwendung mithilfe der Routenadresse aus.
13. Wählen Sie in der Anwendung Ihre Bereitstellung 'Product Line
Prediction' aus. Jetzt können Sie die Beispieldatensätze und
    Vorhersageergebnisse verwenden.
    
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
