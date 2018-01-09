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

# Machine Learning-Service-Stapeljob-API für IBM SPSS Modeler-Modelle

Die Stapeljob-API für den {{site.data.keyword.pm_full}}-Service unterstützt die
lange laufenden Tasks im Zusammenhang mit Modelltraining, Modellevaluierung
und Stapel-Scoring. Diese API verwaltet zwei Assettypen: die IBM® SPSS®
Modeler-Datenstromdateien, die in den Stapeljobs verwendet werden, und die
Jobdefinitionen, die übergeben werden.
{: shortdesc}

Für jede SPSS® Modeler-Datenstromdatei, die Sie hochladen,
kann es viele Jobs mit vielen Typen geben, die übergeben werden. Falls ein Job das Potenzial
hat, den Inhalt der Modeler-Datenstromdatei zu aktualisieren, wird die
geänderte Datei in den Anhang der Jobergebnisse einbezogen. Beim Jobtyp
der Modellevaluierung befinden sich alle Auswertungsdateien, die generiert werden, in den Anhängen der Jobergebnisse.

**Hinweis**: Die im Dashboard angezeigten Daten beziehen sich nur auf Echtzeitvorhersagen, z. B. die Daten aus der Upload-Funktion.

## Jobs löschen

Jobs können gelöscht werden. Wenn ein Job ausgeführt wird, bricht das Löschen eines Jobs zuerst den Job ab. Verwenden Sie den Befehl `LÖSCHEN` mit der `Job-ID`. Sie können durch Übergeben mehrerer IDs mehr als einen Job gleichzeitig abbrechen.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

Die Rückgabe zeigt, wie viele der Jobs, die durch die ID in der Anforderung referenziert werden, gelöscht werden. Wenn diese Zahl nicht mit der Liste übereinstimmt, die Sie in der Anforderung übergeben haben, müssen Sie den Status
der einzelnen Jobs überprüfen.

## Status eines Jobs überprüfen

Sie können den Status Ihrer `Job-ID` zu jedem beliebigen Zeitpunkt mit dem Befehl `GET` abrufen:


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

Die zurückgegebene JSON-Datei gibt den Jobstatus an und,
falls der Job erfolgreich ausgeführt wurde, eine Daten-URL, mit der Sie alle
generierten Dateiinhalte abrufen können.

## Job erneut übergeben

Um einen Job erneut abzuschicken, verwenden Sie den Befehl `PUT` mit der `Job-ID`. Zum erneuten Abschicken eines Jobs muss dieser Job nicht ausgeführt werden.

Wenn die ID, auf die Sie verweisen, nicht vorhanden ist, wird durch den folgenden Aufruf ein neuer Job erstellt. Der Rückgabestatus ist 201 versus 200 (womit
angegeben wird, was davon aufgetreten ist). Sie müssen die JSON-Datei für die Jobdefinition, die in dieser
Ausführung verwendet werden soll, übergeben.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Übergeben Sie den Job erneut mit dem Wert `content_type`, der auf "application/json" gesetzt ist. Sie müssen die neue oder aktualisierte JSON-Datei für die Jobdefinition in den Hauptteil der Anforderung einschließen.

## Job für eine hochgeladene Modeler-Datenstromdatei übergeben

Verwenden Sie den Befehl `PUT`, um einen Job in die Ausführungswarteschlange zu stellen:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Übergeben Sie den Job, wobei der Wert `content_type` auf 'application/json' festgelegt ist. Sie müssen die neue oder aktualisierte JSON-Datei für die Jobdefinition in den Hauptteil der Anforderung einschließen.

Diese Anforderung wird sofort zurückgegeben und zeigt den Erfolg an, wenn die
Jobdefinition in die Warteschlange gestellt wurde. Es gibt keinen Test der möglichen erfolgreichen Ausführung des Modeler-Datenstroms so wie dieser in Ihrer Jobdefinition konfiguriert ist. Der Job wird von einem der {{site.data.keyword.pm_short}}-Job-Server
in der Cloud ausgeführt und Sie können seinen
Status überwachen. Wenn Sie ein Modelltraining ausführen und angeben,
dass Sie ein automatisches Aktualisieren wünschen, wird der Job in der
ursprünglichen Modeler-Datenstromdatei ersetzt, wenn er erfolgreich ausgeführt wurde.

## Datenstromdatei zur Verwendung in Ihren Jobs hochladen

**Hinweis**: Das {{site.data.keyword.pm_short}}-Dashboard dient nur dem Echtzeitscoring. Sie können es nicht zur Ausführung von Jobs (Stapel-Scoring) verwenden.

Um eine Modeler-Datenstromdatei für Jobs zugänglich zu machen, verwenden Sie den Befehl `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

wobei content_type "multipart/form-data" die Datei an die Anforderung übergibt.

Der verwendete eindeutige Name (`Datei-ID` im Aufruf `PUT`) ist Ihre Referenz im Aufruf `DELETE` an die Datei-API und dient ferner als Modellreferenz in Ihren Jobdefinitionen. Beachten Sie, dass der Namensbereich von Dateien, die in Ihren Jobs verwendet werden, sich vollständig in Ihrer Kontrolle befindet. Der Befehl `PUT` für eine Datei unter derselben `Datei-ID` ersetzt implizit die aktuell gehaltene Kopie.

Um eine Liste aller für Ihre Jobs hochgeladenen Dateien zu generieren, verwenden Sie den Befehl `GET`, lassen jedoch den Parameter `Datei-ID` aus. Um eine spezifische Datei abzurufen, verwenden Sie den Befehl `GET` mit dem Parameter `Datei-ID`. Sie können auch eine hochgeladene Datei durch Ausgabe des Befehls `DELETE` löschen. Dies führt zu Fehlern in allen ausstehenden Jobausführungen, die auf die Datei verweisen.

Anforderungsbeispiel:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Implementierung:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }      
```
{: codeblock}

Antwort bei fehlgeschlagener Implementierung:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
        }
```
{: codeblock}

## Jobtypen

**Training**: Dieser Jobtyp gibt an, dass alle Endknoten von "Model Builder"
im Modeler-Datenstrom ausgeführt werden sollten. Nach erfolgreichem Jobabschluss wird sich ein aktualisierter Modeler-Datenstrom mit neu trainierten Modellnuggets in den Jobergebnissen befinden, die abgerufen werden können. Falls die Modeler-Datenstromdatei über Links von Modellerstellungsknoten zu den trainierten Modellnuggets in der Evaluierung und den Scoring-Verzweigungen verfügt, führt dies zu einer Aktualisierung dieser Knoten.

**Evaluierung**: Dieser Jobtyp löst die Ausführung aller Endknoten für "Dokumenterstellungsprogramme"
aus (hauptsächlich über die Registerkarten Grafiken und Ausgabe im Modeler-Client), die statischen Inhalt für Berichtsdateien generieren, der an den Aufrufer zurück übergeben werden kann. Die Scoring-Verzweigung wird nicht als Teil dieses Jobtyps betrachtet.

**Automatische Aktualisierung**: Eine Version des Jobtyps `TRAINING`, dessen
ursprüngliche Modeler-Datenstromdatei in der Stapeldateiliste aktualisiert wird,
wenn der Job erfolgreich ausgeführt wurde. Evaluierung und explizite Entscheidung bezüglich eines Aktualisierungsereignisses für den Modeler-Datenstrom, der für Echtzeit-Scoring bereitgestellt wurde, wird vorausgesetzt und zu diesem Zeitpunkt nicht in der automatischen Aktualisierung abgedeckt.

**Stapel-Score**: Ausführung des Endknotens, dem Sie die Option Als Scoring-Verzweigung
verwenden zugewiesen haben, womit Sie darauf hinweisen, dass dies die Scoring-Verzweigung
in diesem Modeler-Datenstromdesign ist. Die Jobdefinition muss die Angabe Export sowie
Details zur Quelle enthalten.

**Datenstrom ausführen**: Die Ausführung ist ähnlich wie das Klicken auf das grüne Symbol
für die Ausführung ("run") in Modeler, wobei die Option 'Dieses Script ausführen' auf der Registerkarte
'Ausführung' der Datenstromeigenschaften ausgewählt ist. Die Verwendung deckt die Notwendigkeit der Scriptausführung eines Modelltrainings oder anderer Jobtypen ab. Alle dynamischen Steuerelemente des Scripts müssen von Datenstromparametern gehandhabt werden, deren Parameterwerte in der Jobdefinition übergeben wurden.

## Jobstatus

**Anstehend**: Die Jobdefinition wurde übergeben jedoch noch nicht von einem Job-Server zur Ausführung übernommen.

**Aktiv**: Die Jobdefinition wurde von einem Job-Server übernommen
und wird ausgeführt.

**Wird abgebrochen**: Der Job wird gerade abgebrochen.

**Abgebrochen**: Der Job wurde abgebrochen.

**Fehlgeschlagen**: Der Job ist fehlgeschlagen. Details über die Ursache des Fehlers
werden zurückgegeben, wenn der Jobstatus mit GET abgerufen wird.

**Erfolg**: Der Job wurde erfolgreich ausgeführt. Alle Ergebnisse, die für dieses Ereignis
kommuniziert wurden, werden in der JSON beim Abrufen des Jobstatus mit GET zurückgegeben.

## Job-API - Details

`POST /v1/jobs/{id}`

Beschreibung: Übergibt eine Jobdefinition für die Ausführung. Es kommt zu einem Fehler, wenn die `Job-ID` bereits vorhanden ist.

Inhaltstypen:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Benutzerdefinierte `Job-ID`. Muss eindeutig für die {{site.data.keyword.pm_short}}-Serviceinstanz sein:

```
@PathParam("id")
```
{: codeblock}

JSON-Jobdefinition (siehe Jobdefinition JSON):

```
@BodyParam
```
{: codeblock}

Antworten:

Erfolg. Job wurde zur Ausführung übergeben und gibt die JSON-Beschreibung des übergebenen Jobs zurück.

```
@ApiResponse(code = 200)
```
{: codeblock}

Fehler, da Job bereits vorhanden ist. Ein Job mit dieser ID wurde bereits übergeben. Die zurückgegebene Nachricht beschreibt diesen Fehler.

```
@ApiResponse(code = 409)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

Beschreibung: Job erstellen oder aktualisieren. Wenn ein Job mit dieser ID nicht vorhanden ist, erstellen Sie ihn. Andernfalls aktualisieren Sie ihn (wodurch er wirksam erneut für die Ausführung übergeben wird).

Inhaltstypen:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Benutzerdefinierte `Job-ID`:

```
@PathParam("id")
```
{: codeblock}

JSON-Jobdefinition (siehe Jobdefinition JSON):

```
@BodyParam
```
{: codeblock}

Antworten:

Erfolg. Job wurde aktualisiert und für die Ausführung übergeben. Gibt die JSON-Beschreibung des übergebenen Jobs zurück.

```
@ApiResponse(code = 200)
```
{: codeblock}

Erfolg. Job wurde erstellt und für die Ausführung übergeben. Gibt die JSON-Beschreibung des übergebenen Jobs zurück.

```
@ApiResponse(code = 201)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

Beschreibung: Gibt eine Liste aller definierten Jobs auf dieser Machine
Learning-Serviceinstanz zurück.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Antworten:

Erfolg. Gibt den JSON-Array aller Jobdefinitionen zurück:

```
@ApiResponse(code = 200)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

Beschreibung: Anforderung zur Rückgabe einer bestimmten Jobdefinition.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Übergebene `Job-ID`:

```
@PathParam("id")
```
{: codeblock}

Antworten:

Erfolg. Gibt die JSON mit der Beschreibung des referenzierten Jobs zurück:

```
@ApiResponse(code = 200)
```
{: codeblock}

Fehler, da kein Job gefunden wurde. Details in Nachricht zurückgegeben:

```
@ApiResponse(code = 404)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

Beschreibung: Status eines bestimmten Jobs abrufen.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Übergebene `Job-ID`:

```
@PathParam("id")
```
{: codeblock}

Antworten:

Erfolg. Gibt die JSON-Beschreibung des Jobstatus zurück.

```
@ApiResponse(code = 200)
```
{: codeblock}

Fehler, da Job nicht gefunden wurde. Details in der zurückgegebenen Nachricht:

```
@ApiResponse(code = 404)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

Beschreibung: Löschen Sie einen oder mehrere Jobs. Bricht Job ab, wenn er momentan ausgeführt wird.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Übergebene `Job-ID` oder `Job-ID`-Liste mit ID-Werten, die durch ein Komma (,) voneinander getrennt sind.

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Antworten:

Erfolg. Gibt die Anzahl der gelöschten Jobs zurück:

```
@ApiResponse(code = 200)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

## Jobdefinition JSON

Die Jobdefinition JSON enthält die folgenden allgemeinen Abschnitte:

Jobtyp und Vorhersagemodellreferenz

**Hinweis**: Die im Dashboard angezeigten Daten einschließlich der Daten der Hochladefunktion beziehen sich nur auf Echtzeitprognosen.

### Aktionstypen

**TRAINING** – Führt das Modellerstellungsknotentraining oder Aktualisierungsmodellnuggets aus. Der aktualisierte Modeler-Datenstrom wird in Jobergebnissen abgerufen.

**EVALUATION** – Führt das Dokumenterstellungsprogramm aus und analysiert Knoten, die das Trainingsmodell evaluieren.

**AUTO_REFRESH** – Führt TRAINING aus und aktualisiert Dateiinhalte im Hochladebereich der Stapeldatei.

**BATCH_SCORE** – Führt die Scoring-Verzweigung aus und exportiert die Ergebnisdaten als von der Jobdefinition gesteuert.

**RUN_STREAM** – Führt den Modeler-Datenstrom wie in den Datenstromeigenschaften angegeben aus; wird meistens verwendet, wenn eine Scriptausführung erforderlich ist.

Modell: ID wie in der Aktion zum Hochladen der Datei für die Stapeljobreferenz
angegeben. Weitere Informationen finden Sie unter Datenstromdatei zur Verwendung in Ihren Jobs hochladen. Beachten Sie, dass `/pm/v1/file` verwendet wird.

```
"action": "TRAINING",
"model": {
     "id": "str2"
     "name":"stream1.str" 
},
```
{: codeblock}

Beachten Sie, dass die ID mit der `Datei-ID` übereinstimmen sollte, die in der API `PUT` verwendet wird. Der Name ist nicht erforderlich. Beim Modelltraining und beim
automatischen Aktualisieren wird das Jobergebnis jedoch mithilfe des hier definierten
Namens gespeichert. Wenn der Name nicht definiert ist, generiert der {{site.data.keyword.pm_short}}-Service
das Ergebnis entsprechend den vordefinierten Namensregeln.

### Jobeinstellungen

Alle erforderlichen Einstellungen für die Ausführung dieses Jobs.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### Definitionen der Datenbankkonnektivität

Database type: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL, ProgressOpenEdge, Salesforce,  SQLServer, Sybase, SybaseIQ, Teradata.

So wie die Konnektivität in der DB-Serviceinstanz angegeben ist, sind für viele Datenbanktypen bestimmte Einstellungen erforderlich, die mit den 'Optionen' übergeben werden müssen.

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### Quellenknoteneinstellungen

Referenzieren Sie die Datenbankkonnektivität und die verwendete Tabelle, um einen gegebenen Zweig im Modeler-Datenstrom als Quelle zu verwenden, so wie dies vom Quellenknotennamen angegeben wurde.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

Dabei ist table der Name der Datenbanktabelle. Diese Tabelle wird zum Überschreiben des Datenstromquellenknotens verwendet. Der Knoten node ist der
Datenquellenname für den Datenstrom. Ferner referenziert dbRef die Datenbankkonnektivität, die durch
das Objekt dbDefinitions definiert ist, und table ist die Datenbanktabelle, die als Eingabe für
den gegebenen Zweig im Modeler-Datenstrom verwendet wird. Der Knoten node gibt den ursprünglichen
Quellenknoten im Modeler-Datenstrom an, der durch einen DB-Quellenknoten ersetzt wird,
der durch die bereitgestellten Parameter konstruiert wird.

### Knoteneinstellungen exportieren

Referenzieren Sie DB-Konnektivität und die verwendete Tabelle, um Daten für einen gegebenen Zweig im Modeler-Datenstrom als persistent zu definieren so wie dies durch den Endknotennamen angegeben wurde.

Steuerungsmethode, die verwendet wird, wenn als persistent definierte Daten
über das Attribut insertMode kommuniziert werden:

**Append** – die Tabelle muss vorhanden und für das Einfügen kompatibel sein

**Create** – die Tabelle wurde erstellt (ein Fehler wird zurückgegeben, wenn er bereits vorhanden ist)

**Drop** – falls die referenzierte Tabelle vorhanden ist, wird sie gelöscht und erneut erstellt

**Refresh** – vorhandene Zeilen aus der Tabelle werden gelöscht, bevor sie in neue Zeilen eingefügt werden
   
Die Tabelle table ist der Name der Datenbanktabelle, in die die Jobergebnisse geschrieben werden.
Der Knoten node ist der Name des Endknotens für den
Datenstrom. Ähnlich wie bei Quellenknoteneinstellungen gibt node den ursprünglichen Ausgabeknoten
im Modeler-Datenstrom an, der durch einen DB-Exportknoten ersetzt wird, der mit den zur Verfügung
gestellten Parametern konstruiert wurde.

**Hinweis**: Die Einstellungen des Exportknotens und die Quellenknoteneinstellungen sind entscheidend dafür, dass Ihr Job erfolgreich ausgeführt wird.

### Überschreibungen von Parameterwerten

Zum Überschreiben von Standardwerten für die Parameter auf Datenstromebene vor der Jobausführung können Sie die Name-/Wertpaare angeben, die in der Jobdefinition verwendet werden sollen.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Gewünschte Berichtsformate für das Evaluierungsdokument, das bei der Ausführung eines Evaluierungsjobtyps zurückgegeben wird

Alle generierten Dokumente der Ausführung des Evaluierungsjobs
werden in einer .zip-Datei gebündelt. Es werden nur Dokumenterstellungsprogrammknoten
ausgeführt, die in der Lage sind, das angezeigte reportFormat zu unterstützen und deren
Inhalt nach einer erfolgreichen Ausführung des Jobs zurückgegeben wurde.

Die folgenden Formate werden unterstützt: HTML, JPG, PNG, RTF, SAV, TAB und XML.

```
"reportFormat": "HTML"
```
{: codeblock}

### Vollständiges JSON-Beispiel

```
{ 
     "action": "TRAINING",
     "model": { 
          "id": "str2"
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
                    "port":50001,
                    "db":"BLUDB",
                    "username":"bluadmin",
                    "password":"NmI4MDViMzBkNDUz",
                    "options":"Security=SSL"
               }
     },
     "setting": {
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Stapeljob-API - Details

In den folgenden Abschnitten finden Sie Details zur IBM® SPSS® Modeler-Dateiverwaltungs-API für Stapeljobs.

`PUT /v1/file/{id}`

Beschreibung: Lädt eine IBM® SPSS®-Modeler-Datenstromdatei für die Verwendung in Stapeljobs hoch.

**Hinweis**: Wenn bereits eine Datei unter der angegebenen ID gespeichert ist, wird diese überschrieben.

### Inhaltstypen

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### Parameter

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey") 
```
{: codeblock}

Benutzerdefinierte `Datei-ID`. Die ID muss im Serviceinstanzrepository eindeutig sein:

```
@PathParam("id") 
```
{: codeblock}

### Antworten

Erfolg. Gibt die URL zurück, die für das Herunterladen der Datei aus dem Repository der Serviceinstanz verwendet werden kann.

```
@ApiResponse(code = 200)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

Beschreibung: Löscht die Datei, die unter der angegebenen ID gespeichert ist,
aus dem Serviceinstanzrepository.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")
```
{: codeblock}

Benutzerdefinierte ID für die Datei nach dem Hochladen:

```
@PathParam("id") 
```
{: codeblock}

Antworten:

Erfolg. Die angegebene Datei wurde gelöscht:

```
@ApiResponse(code = 200)
```
{: codeblock}

Fehler. Eine Datei, die unter dieser ID gespeichert wurde, ist im Serviceinstanzrepository nicht vorhanden.

```
@ApiResponse(code = 404)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

Beschreibung: Gibt eine Liste der IDs aller Dateien zurück, die im
Repository der Serviceinstanz für die Stapeljobverarbeitung verwaltet werden.

Inhaltstypen:

```
@Produces({“application/json”})
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")  
```
{: codeblock}

Antworten:

Erfolg. Gibt einen JSON-Bereich mit Werten für die `Datei-ID` zurück:

```
@ApiResponse(code = 200)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

Beschreibung: Ruft die IBM® SPSS®-Datenstromdatei ab, die für die Verwendung in der Stapeljobverarbeitung unter der angegebenen ID gespeichert wurde.

Inhaltstypen:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parameter:

Zugriffsschlüssel wurde als Berechtigungsnachweis in der
Bereitstellung oder Bindung zurückgegeben:

```
@QueryParam("accesskey")  
```
{: codeblock}

Benutzerdefinierte ID für die Datei nach dem Hochladen:

```
@PathParam("id") 
```
{: codeblock}

Antworten:

Erfolg. Gibt die IBM® SPSS® Modeler-Datei zurück:

```
@ApiResponse(code = 200)
```
{: codeblock}

Fehler. Eine Datei, die unter dieser ID gespeichert wurde, ist im Serviceinstanzrepository nicht vorhanden.

```
@ApiResponse(code = 404)
```
{: codeblock}

Anderer Fehler. JSON der Ausnahmebedingung zurückgegeben.

```
@ApiResponse(code = 500)
```
{: codeblock}

## Weitere Informationen

Ein Beispiel für eine Stapeljobakzeptanz in einem Notizbuch finden Sie unter [Vom SPSS-Datenstrom zum Stapel-Scoring mit Python
](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

Weitere Informationen zur Jobdefinition JSON finden Sie unter [Jobdefinition JSON](#job-definition-json).
