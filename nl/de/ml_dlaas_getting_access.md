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

# Umgebung für maschinelles Lernen einrichten und Berechtigungsnachweise abrufen

Um IBM Watson Machine Learning verwenden zu können, müssen Sie in der Lage sein, die richtige Maschinenlernumgebung zu erstellen und die Berechtigungsnachweise abzurufen, die für diese Umgebung spezifisch sind.

## Umgebung einrichten

Um IBM Watson Machine Learning verwenden zu können, müssen Sie Instanzen der folgenden Elemente in Ihrem IBM Cloud-Dashboard einrichten:

- Watson Machine Learning
- Object Storage
- Apache Spark

Zusätzlich zu den erforderlichen Instanzen verwenden einige unserer Notizbücher und Lernprogramme auch die folgenden Instanzen. Wenn Sie die Lernprogramme verwenden möchten, müssen Sie auch diese Services konfigurieren.

- Python-Flask
- Verständnis natürlicher Sprache
- Deep-Learning-Experimente

### IBM Cloud-Instanzen erstellen

Sehen Sie sich dieses Video an, um zu erfahren, wie die IBM Cloud-Instanzen erstellt und die Berechtigungsnachweise angezeigt werden. Führen Sie anschließend die Schritte nach dem Video aus, um Ihre eigene Umgebung einzurichten. Die Schritte zum Anzeigen Ihrer Berechtigungsnachweise finden Sie im Abschnitt <a href="#retrieving-your-credentials">Berechtigungsnachweise abrufen</a>.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Abb. 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Wenn Sie das auf dieser Seite eingebettete Video nicht ansehen können, können Sie über die YouTube-Website darauf zugreifen. (Wird in einer neuen Registerkarte oder einem neuen Fenster geöffnet)">    <img src="images/video.png" alt="Video-Symbol"></a>IBM Watson Machine Learning: Erste Schritte</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Dieses Video gibt einen Überblick über die Vorgehensweise zum Einrichten Ihrer Umgebung für maschinelles Lernen.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Zum Erstellen von Watson Data Platform-Instanzen müssen Sie die folgenden Schritte ausführen:

1. [Melden Sie sich bei der IBM Cloud an](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Klicken Sie im Navigationsfenster auf **Data & Analytics**.

   Der angezeigte Bildschirm ist auf Datenservices zentriert. Hierher können Sie regelmäßig zurückkehren, um von einer einzigen benutzerfreundlichen Seite aus mit Ihren Daten- und Analysediensten zu arbeiten.

3. Klicken Sie auf die Registerkarte **Analyse** und anschließend auf **Instanzen**.
4. Klicken Sie auf **Neue Instanz**.
5. Konfigurieren Sie Instanzen und Datenspeicher.

   Geben Sie einen beschreibenden Namen für Ihre Instanz ein, wählen Sie einen Bereich und Ihren Datenplan aus (Planvergleiche und Preisangaben finden Sie auf dieser Seite). IBM Cloud belegt den Objektspeichernamen automatisch mit Ihrem Instanznamen. Es ist wahrscheinlich, dass Sie Datenspeicher für Ihr Spark-Projekt benötigen. Wählen Sie deshalb einen Raum aus und planen Sie ihn auch hier.

6. Klicken Sie auf **Instanz erstellen**.

Ihre Instanz wird geöffnet.

### Vorhandene Bluemix-Instanzen zu einem Projekt in Data Science Experience hinzufügen

Sehen Sie sich dieses Video an, um zu erfahren, wie ein Projekt erstellt und für die Verwendung von Watson Machine Learning eingerichtet wird.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Abb. 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="Wenn Sie das auf dieser Seite eingebettete Video nicht ansehen können, können Sie über die YouTube-Website darauf zugreifen. (Wird in einer neuen Registerkarte oder einem neuen Fenster geöffnet)">    <img src="images/video.png" alt="Video-Symbol"></a>IBM Watson Machine Learning: Projekt für Watson Machine Learning erstellen</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>Dieses Video gibt einen Überblick über die Vorgehensweise zum Einrichten Ihrer Umgebung für maschinelles Lernen.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Wenn Sie bereits über Instanzen verfügen, sie aber nicht mit einem Projekt in Data Science Experience verknüpft haben, müssen Sie die folgenden Schritte ausführen:

1. [Melden Sie sich bei IBM Data Science Experience an](https://datascience.ibm.com).
2. Klicken Sie auf **Projekte** > **Alle Projekte anzeigen**.
3. Klicken Sie auf die Registerkarte **Einstellungen**.
4. Zum Hinzufügen eines Service klicken Sie im Fenster **Zugehörige Services** auf **Zugehörigen Service hinzufügen**, wählen Sie einen Service aus, geben Sie die Konfigurationsdaten ein und klicken Sie auf **Speichern**.
5. Zum Hinzufügen eines Zugriffstokens führen Sie die folgenden Schritte aus:
   6. Klicken Sie im Fenster **Zugriffstoken** auf **Neues Token erstellen**.
   7. Geben Sie im Feld **Name** einen Namen ein.
   8. Wählen Sie im Feld **Zugriffsrolle für Projekt** eine Rolle aus, entweder **Anzeigeberechtigter** oder **Bearbeiter**.
   9. Klicken Sie auf **Hinzufügen**.

6. Um eine Verbindung zu einem GitHub-Repository herzustellen, geben Sie im Feld **Repository-URL** die Position Ihres Repositorys ein. Wenn Sie noch kein persönliches Zugriffstoken für den GitHub-Zugriff konfiguriert haben, müssen Sie es jetzt erstellen, indem Sie auf **Einstellungen** klicken. Wenn Sie fertig sind, klicken Sie auf **Hinzufügen**.


## Berechtigungsnachweise abrufen

Wenn Sie Ihre Bluemix-Instanzen, -Services und -Modelle in Notizbüchern und Anwendungen verwenden möchten, müssen Sie in der Lage sein, Ihre Berechtigungsnachweise, Token und Scoring-Endpunkte in den Code einzufügen, der für die Verarbeitung verwendet wird.

1. [Melden Sie sich bei Bluemix an](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Blättern Sie zum Abschnitt **Services** und klicken Sie auf den Namen des Service, für den Sie Berechtigungsnachweise abrufen möchten.
3. Klicken Sie im Navigationsfenster auf **Serviceberechtigungsnachweise**.
4. Klicken Sie in der Liste der Berechtigungsnachweise auf **Berechtigungsnachweise anzeigen**.
5. Wenn für diesen Service keine Berechtigungsnachweise vorhanden sind, können Sie sie erstellen, indem Sie auf **Berechtigungsnachweise erstellen** klicken.
6. Zum Kopieren der Berechtigungsnachweise klicken Sie auf das Symbol für Kopieren.

Je nach Service können Sie verschiedene Felder verwenden, z. B. `Benutzername` und `Kennwort`. Sie müssen die Werte so kopieren, wie sie in Ihrem Code verwendet werden.

## Weitere Informationen

[Ein Lernprogramm für schnelles Deep Learning](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

