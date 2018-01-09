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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Fehlerbehebung

Hier sind die Antworten auf allgemeine Fragen zur Fehlerbehebung bei der Verwendung von {{site.data.keyword.pm_full}}.
{: shortdesc}

## Hilfe und Unterstützung für Machine Learning abrufen
{: #gettinghelp}

Bei Problemen oder Fragen zur Verwendung von {{site.data.keyword.pm_short}} können Sie Hilfe anfordern, indem Sie nach Informationen suchen oder über ein Forum Fragen stellen. Sie haben auch die Möglichkeit, ein Support-Ticket zu öffnen.

Wenn Sie eine Frage über die Foren stellen, kennzeichnen Sie Ihre Frage, sodass sie von den Entwicklungsteams für {{site.data.keyword.pm_short}} gesehen wird. 

Bei technischen Fragen zu {{site.data.keyword.pm_short}} posten Sie Ihre Frage unter <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow<img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> und kennzeichnen Sie Ihre Frage mit "ibm-bluemix" und "machine-learning". 

Bei Fragen zum Service sowie zu einführenden Anweisungen, verwenden Sie das Forum von <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers<img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Schließen Sie die Tags "machine-learning" und "bluemix" ein.
Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](https://console.bluemix.net/docs/support/index.html#getting-help).

Weitere Informationen zum Öffnen eines IBM Support Tickets oder zu Support-Levels und Schweregrad der Tickets finden Sie unter [Contacting Support](https://console.bluemix.net/docs/support/index.html#contacting-support).

## Berechtigungstoken wurde nicht angegeben.
{: #ts_missing_authorization_token}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Berechtigungstoken wurde im Header `Berechtigung` nicht zur Verfügung gestellt.
{: tsCauses}
 
Berechtigungstoken im Header `Berechtigung` übergeben.
{: tsResolve}
       
## Ungültiges Berechtigungstoken.
{: #ts_invalid_authorization_token}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Berechtigungstoken, das bereitgestellt wurde, kann nicht dekodiert oder geparst werden.
{: tsCauses}
 
Korrektes Berechtigungstoken an den Header `Berechtigung` übergeben.
{: tsResolve}
       
## Berechtigungstoken und Instanz-ID (instance_id), die in der Anforderung verwendet wurde, sind nicht identisch.
{: #ts_not_matching_authorization_token}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Das Berechtigungstoken, das verwendet wurde, wurde nicht für die Serviceinstanz generiert, für die es verwendet wurde.
{: tsCauses}
 
Berechtigungstoken im Header `Berechtigung`, das der verwendeten Serviceinstanz entspricht, übergeben.
{: tsResolve}
       
## Berechtigungstoken ist abgelaufen.
{: #ts_expired_authorization_token}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Berechtigungstoken ist abgelaufen.
{: tsCauses}
 
Ein nicht abgelaufenes Berechtigungstoken im Header `Berechtigung` übergeben.
{: tsResolve}
       
## Der für die Authentifizierung erforderliche öffentliche Schlüssel ist nicht verfügbar.
{: #ts_missing_public_key}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Dies ist ein internes Serviceproblem.
{: tsCauses}
 
Das Problem muss vom Support-Team behoben werden.
{: tsResolve}
       
## Zeitlimit der Operation nach {{timeout}} abgelaufen
{: #ts_operation_timeout}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Das Zeitlimit wurde während der Ausführung der angeforderten Operation überschritten.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen.
{: tsResolve}
       
## Nicht behandelte Ausnahme vom Typ {{type}} mit {{status}}
{: #ts_unhandled_exception_with_status}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Dies ist ein internes Serviceproblem.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen. Wenn das Problem mehrfach auftritt, muss es vom Support-Team behoben werden.
{: tsResolve}
       
## Nicht behandelte Ausnahme vom Typ {{type}} mit {{response}}
{: #ts_unhandled_exception_with_response}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Dies ist ein internes Serviceproblem.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen. Wenn das Problem mehrfach auftritt, muss es vom Support-Team behoben werden.
{: tsResolve}
       
## Nicht behandelte Ausnahme vom Typ {{type}} mit {{json}}
{: #ts_unhandled_exception_with_json}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Dies ist ein internes Serviceproblem.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen. Wenn das Problem mehrfach auftritt, muss es vom Support-Team behoben werden.
{: tsResolve}
       
## Nicht behandelte Ausnahme vom Typ {{type}} mit {{message}}
{: #ts_unhandled_exception_with_message}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Dies ist ein internes Serviceproblem.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen. Wenn das Problem mehrfach auftritt, muss es vom Support-Team behoben werden.
{: tsResolve}
       
## Das angeforderte Objekt konnte nicht gefunden werden.
{: #ts_not_found}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Die Anforderungsressource konnte nicht gefunden werden.
{: tsCauses}
 
Stellen Sie sicher, dass Sie sich auf die vorhandene Ressource beziehen.
{: tsResolve}
       
## Die zugrunde liegende Datenbank berichtete zu viele Anforderungen.
{: #ts_too_many_cloudant_requests}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Der Benutzer hat zu viele Anforderungen in einem vorgegebenen Zeitraum gesendet.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen.
{: tsResolve}
       
 ## Die Definition der Auswertung wird weder in der artifactModelVersion noch in der Bereitstellung definiert. Sie muss mindestens an einem der Orte angegeben werden.
{: #ts_missing_evaluation_definition}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Die Lernkonfiguration enthält nicht alle erforderlichen Informationen.
{: tsCauses}
 
Eine `Definition` in der `Lernkonfiguration` bereitstellen
{: tsResolve}
       
## Bewertung erfordert die Angabe einer Lernkonfiguration für das Modell.
{: #ts_missing_learning_configuration}
 
Es gibt keine Möglichkeit, eine `Lernwiederholung` zu erstellen.
{: tsSymptoms}
 
Es gibt keine definierte `Lernkonfiguration` für das Modell.
{: tsCauses}
 
Erstellen Sie eine `Lernkonfiguration` und versuchen Sie erneut, eine `Lernwiederholung` zu erstellen.
{: tsResolve}
       
## Die Evaluierung erfordert die Bereitstellung einer spark-Instanz im Header `X-Spark-Serviceinstanz`.
{: #ts_missing_spark_definition_for_evaluation}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Es gibt nicht alle erforderlichen Informationen in der `Lernkonfiguration`.
{: tsCauses}
 
Stellen Sie `spark_service` in der Lernkonfiguration oder im `X-Spark-Serviceinstanz`-Header bereit.
{: tsResolve}
       
## Das Modell enthält keine Version.
{: #ts_missing_latest_model_version}
 
Es gibt weder die Möglichkeit, eine Bereitstellung zu erstellen noch eine Lernkonfiguration festzulegen.
{: tsSymptoms}
 
Es gibt Inkonsistenzen bezüglich der Persistenz des Modells.
{: tsCauses}
 
Versuchen Sie, das Modell erneut als persistent zu definieren und die Aktion erneut auszuführen.
{: tsResolve}
       
## Die Patch-Operation kann nur die vorhandene Lernkonfiguration ändern.
{: #ts_patch_non_existing_learning_configuration}
 
Es gibt keine Möglichkeit, die Patch-REST-API-Methode aufzurufen, um die Lernkonfiguration zu aktualisieren.
{: tsSymptoms}
 
Es wurde keine `Lernkonfiguration` für dieses Modell festgelegt oder das Modell ist nicht vorhanden.
{: tsCauses}
 
Zulassen, dass ein Modell vorhanden ist und bereits eine Lernkonfiguration festgelegt wurde.
{: tsResolve}
       
## Die Patchoperation erwartet genau eine Ersetzungsoperation.
{: #ts_patch_multiple_ops}
 
Die Bereitstellung kann nicht aktualisiert werden.
{: tsSymptoms}
 
Die Patchnutzdaten enthalten mehr als eine Operation oder die Patchoperation ist nicht die Operation `Ersetzen`.
{: tsCauses}
 
Verwenden Sie nur eine Operation in den Patchnutzdaten, wobei es sich um eine Operation `Ersetzen` handeln sollte.
{: tsResolve}
       
## Den angegebenen Nutzdaten fehlen erforderliche Felder: FIELD oder die Werte dieser Felder sind beschädigt.
{: #ts_invalid_request_payload}
 
Es gibt keine Möglichkeit, die Aktion zu verarbeiten, die sich auf den Zugriff auf den darunterliegenden Datenbestand bezieht.
{: tsSymptoms}
 
Der Zugriff auf den Datenbestand ist nicht ordnungsgemäß definiert.
{: tsCauses}
 
Korrigieren Sie die Zugriffsdefinition für den Datenbestand.
{: tsResolve}
       
## Bereitgestellte Evaluierungsmethode: METHOD wird nicht unterstützt. Unterstützte Werte: VALUE.
{: #ts_evaluation_method_not_supported}
 
Es gibt keine Möglichkeit, eine Lernkonfiguration zu erstellen.
{: tsSymptoms}
 
Es wurde eine falsche Evaluierungsmethode zum Erstellen der Lernkonfiguration verwendet.
{: tsCauses}
 
Die unterstütze Evaluierungsmethode ist eine der folgenden Methoden: `Regression` (regression), `Binär` (binary), `Multiklasse` (multiclass).
{: tsResolve}
       
## Es kann pro Modell nur eine aktive Evaluierung geben. Die Anforderung konnte aufgrund einer vorhandenen aktiven Evaluierung nicht abgeschlossen werden: {{url}}
{: #ts_active_evaluation_conflict}
 
Es gibt keine Möglichkeit, eine andere Lernwiederholung zu erstellen.
{: tsSymptoms}
 
Es kann nur eine aktive Evaluierung pro Modell geben.
{: tsCauses}
 
Betrachten Sie die bereits aktive Evaluierung oder warten Sie, bis diese endet und starten Sie dann eine neue Evaluierung.
{: tsResolve}
       
## Der Bereitstellungstyp {{type}} wird nicht unterstützt.
{: #ts_not_supported_deployment_type}
 
Es gibt keine Möglichkeit, eine Bereitstellung zu erstellen.
{: tsSymptoms}
 
Es wurde kein unterstützter Bereitstellungstyp verwendet.
{: tsCauses}
 
Der unterstützte Bereitstellungstyp sollte verwendet werden.
{: tsResolve}
       
## Falsche Eingabe: ({{message}})
{: #ts_deserialization_error}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Es gibt ein Problem mit dem Parsing von JSON.
{: tsCauses}
 
Stellen Sie sicher, dass die richtige JSON in der Anforderung übergeben wird.
{: tsResolve}
       
## Unzureichende Daten - Metrikwert {{name}} konnte nicht berechnet werden.
{: #ts_missing_metric}
 
Lernwiederholung ist fehlgeschlagen.
{: tsSymptoms}
 
Wert für Metrikeert mit definiertem Schwellenwert konnte nicht berechnet werden, da nicht ausreichend Feedbackdaten vorlagen.
{: tsCauses}
 
Überprüfen und verbessern Sie Daten in der Datenquelle `feedback_data_ref` in der `Lernkonfiguration`.
{: tsResolve}
       
## Für den Typ {{type}} muss eine Spark-Instanz im Header `X-Spark-Serviceinstanz` bereitgestellt werden.
{: #ts_missing_prediction_spark_definition}
 
Die Bereitstellung konnte nicht erstellt werden.
{: tsSymptoms}
 
`Stapel`-Bereitstellungen (batch) und `Streaming`-Bereitstellungen (streaming) erfordern die Bereitstellung einer Spark-Instanz.
{: tsCauses}
 
Stellen Sie eine Spark-Instanz im Header `X-Spark-Serviceinstanz` bereit.
{: tsResolve}
       
## Die Aktion {{action}} ist mit der Nachricht {{message}} fehlgeschlagen.
{: #ts_http_client_error}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Es gab ein Problem mit dem Aufrufen des zugrundeliegenden Service.
{: tsCauses}
 
Wenn es einen Vorschlag gibt, wie das Problem behoben werden kann, dann befolgen Sie den Vorschlag. Wenden Sie sich an das Support-Team, wenn kein Vorschlag in der Nachricht enthalten ist oder wenn der Vorschlag das Problem nicht löst.
{: tsResolve}
       
## Der Pfad `{{path}}` ist nicht zulässig. Der zulässige Pfad für den Patch-Datenstrom ist `/status`
{: #ts_wrong_stream_patch_path}
 
Es gibt keine Möglichkeit, die Datenstrombereitstellung zu aktualisieren.
{: tsSymptoms}
 
Beim Aktualisieren der `Datenstrom`bereitstellung wurde der falsche Pfad verwendet.
{: tsCauses}
 
Aktualisieren Sie die `Datenstrom`bereitstellung mit der unterstützten Pfadoption `/status`, die das Starten und Stoppen der Datenstromverarbeitung ermöglicht.
{: tsResolve}
       
## Die Patchoperation ist für Instanzen vom Typ `{{$type}}` nicht zulässig.
{: #ts_patch_not_supported}
 
Es gibt keine Möglichkeit, die Bereitstellung zu aktualisieren.
{: tsSymptoms}
 
Der falsche Bereitstellungstyp wurde aktualisiert.
{: tsCauses}
 
Aktualisieren Sie den `Datenstrom`bereitstellungstyp.
{: tsResolve}
       
## Die Datenverbindung `{{data}}` ist für feedback_data_ref ungültig.
{: #ts_invalid_feedback_data_connection}
 
Es gibt keine Möglichkeit, eine `Lernkonfiguration` für das Modell zu erstellen.
{: tsSymptoms}
 
Es wurde eine nicht unterstützte Datenquelle bei der Definition von feedback_data_ref verwendet.
{: tsCauses}
 
Verwenden Sie nur den unterstützten Datenquellentyp `dashdb`.
{: tsResolve}
       
## Der Pfad {{path}} ist nicht zulässig. Der zulässige Pfad für das Patchmodell ist `/deployed_version/url` oder `/deployed_version/href` für V2.
{: #ts_patch_model_path_not_allowed}
 
Es gibt keine Möglichkeit das Modell zu aktualisieren.
{: tsSymptoms}
 
Während der Aktualisierung des Modells wurde der falsche Pfad verwendet.
{: tsCauses}
 
Aktualisieren Sie das Modell mit dem unterstützen Pfad, der eine Aktualisierung der Version des bereitgestellten Modells ermöglicht.
{: tsResolve}
       
## Fehler beim Parsing: {{msg}}
{: #ts_parsing_error}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Die angeforderte Nutzlast konnte nicht erfolgreich geparst werden.
{: tsCauses}
 
Stellen Sie sicher, dass Ihre Anforderungsnutzlast korrekt ist und korrekt geparst werden kann.
{: tsResolve}
       
## Laufzeitumgebung für ausgewähltes Modell: {{env}} wird nicht für die `Lernkonfiguration` unterstützt. Unterstützte Umgebungen: [{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
Es gibt keine Möglichkeit, eine `Lernkonfiguration` zu erstellen.
{: tsSymptoms}
 
Das Modell, für das versucht wurde, die `Lernkonfiguration` zu erstellen, wird nicht unterstützt.
{: tsCauses}
 
Erstellen Sie eine `Lernkonfiguration` für das Modell, dessen Laufzeit unterstützt wird.
{: tsResolve}
       
## Der aktuelle Plan \'{{plan}}\' erlaubt nur {{limit}} Bereitstellungen
{: #ts_deployments_plan_limit_reached}
 
Es gibt keine Möglichkeit, eine Bereitstellung zu erstellen.
{: tsSymptoms}
 
Der Grenzwert für die Anzahl der Bereitstellungen für den aktuellen Plan wurde erreicht.
{: tsCauses}
 
Führen Sie ein Upgrade auf einen Plan durch, der nicht über diese Einschränkung verfügt.
{: tsResolve}
       
## Die Definition der Datenbankverbindung ist nicht gültig ({{code}}).
{: #ts_sql_error}
 
Es gibt keine Möglichkeit, die Funktion `Lernkonfiguration` zu nutzen.
{: tsSymptoms}
 
Die Definition der Datenbankverbindung ist nicht gültig.
{: tsCauses}
 
Versuchen Sie, das Problem so zu lösen, wie es im `Code` beschrieben ist, der von der zugrundeliegenden Datenbank zurückgegeben wurde.
{: tsResolve}
       
## Es gab Probleme beim Herstellen der Verbindung zum zugrundeliegenden {{system}}
{: #ts_stream_tcp_error}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Es gab ein Problem während der Verbindung zum darunterliegenden System. Es kann sich um ein temporäres Netzproblem handeln.
{: tsCauses}
 
Versuchen Sie, die gewünschte Operation erneut aufzurufen. Wenn das Problem mehrfach auftritt, setzen Sie sich mit dem Support-Team in Verbindung.
{: tsResolve}
       
## Fehler beim Extrahieren des Headers der X-Spark-Serviceinstanz: ({{message}})
{: #ts_spark_header_deserialization_error}
 
Es gibt keine Möglichkeit, die REST-API aufzurufen, die Spark-Berechtigungsnachweise erfordert.
{: tsSymptoms}
 
Es gibt ein Problem mit der Base-64-Decodierung oder beim Parsing der Spark-Berechtigungsnachweise.
{: tsCauses}
 
Stellen Sie sicher, dass die richtigen Spark-Berechtigungsnachweise korrekt mit Base-64 codiert wurden. Weitere Informationen finden Sie in der Dokumentation.{: tsResolve}
       
## Diese Funktion ist für Benutzer ohne Beta verboten.
{: #ts_not_beta_user}
 
Die gewünschte REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Die aufgerufene REST-API ist aktuell in der Betaversion.
{: tsCauses}
 
Wenn Sie an der Teilnahme interessiert sind, fügen Sie sich selbst zur Warteliste hinzu. Die Details finden Sie in der Dokumentation.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
Die REST-API kann nicht erfolgreich aufgerufen werden.
{: tsSymptoms}
 
Es gab ein Problem mit dem Aufrufen des zugrundeliegenden Service.
{: tsCauses}
 
Wenn es einen Vorschlag gibt, wie das Problem behoben werden kann, dann befolgen Sie den Vorschlag. Wenden Sie sich an das Support-Team, wenn kein Vorschlag in der Nachricht enthalten ist oder wenn der Vorschlag das Problem nicht löst.
{: tsResolve}
       
## Der Grenzwert der Rate wurde überschritten.
{: #ts_rate_limit_exceeded}
 
Der Grenzwert der Rate wurde überschritten.
{: tsSymptoms}
 
Der Grenzwert der Rate wurde für den aktuellen Plan überschritten.
{: tsCauses}
 
Um dieses Problem zu lösen, fordern Sie einen neuen Plan mit einem höheren Grenzwert für die Rate an.
{: tsResolve}
       
## Ungültiger Abfrageparameter `{{paramName}}` Wert: {{value}}
{: #ts_invalid_query_parameter_value}
 
Gültigkeitsfehler da ein falscher Wert für den Abfrageparameter übergeben wurde.
{: tsSymptoms}
 
Fehler beim Abrufen des Ergebnisses der Abfrage.
{: tsCauses}
 
Korrigieren Sie den Abfrageparameterwert. Die Details finden Sie in der Dokumentation.
{: tsResolve}
       
## Ungültiger Tokentyp: {{type}}
{: #ts_invalid_token_type}
 
Fehler beim Tokentyp.
{: tsSymptoms}
 
Fehler bei der Autorisierung.
{: tsCauses}
 
Das Token sollte mit dem Präfix `Bearer` beginnen.
{: tsResolve}
       
## Ungültiges Tokenformat. Das Trägertoken (Bearer) sollte verwendet werden.
{: #ts_invalid_token_format}
 
Fehler beim Tokenformat.
{: tsSymptoms}
 
Fehler bei der Autorisierung.
{: tsCauses}
 
Das Token sollte ein Trägertoken (Bearer) sein und mit dem Präfix `Bearer` beginnen.
{: tsResolve}

## Die JSON-Eingabedatei fehlt oder ist ungültig: 400
{: #os_invalid_input}

Die folgende Nachricht wird angezeigt, wenn Sie versuchen, online zu bewerten: **JSON-Eingabedatei fehlt oder ist ungültig**.
{: tsSymptoms}

Diese Nachricht wird angezeigt, wenn die Scoring-Eingabenutzdaten nicht mit dem erwarteten Eingabetyp übereinstimmen, der für das Scoring des Modells erforderlich ist. Dabei können insbesondere die folgenden Gründe eine Rolle spielen:

- Die Eingabenutzdaten sind leer.
- Das Schema der Eingabenutzdaten ist nicht gültig.
- Die Eingabedatentypen stimmen nicht mit den erwarteten Datentypen überein.
{: tsCauses}

Korrigieren Sie die Eingabenutzdaten. Stellen Sie sicher, dass die Nutzdaten eine korrekte Syntax, ein gültiges Schema und entsprechende Datentypen aufweisen. Nachdem Sie Korrekturen vorgenommen haben, versuchen Sie erneut, online zu bewerten. Bei Syntaxproblemen überprüfen Sie die JSON-Datei mithilfe des Befehls `jsonlint`.
{: tsResolve}

## Das Berechtigungstoken ist abgelaufen: 401
{: #os_expired_authorization_token}

Die folgende Nachricht wird angezeigt, wenn Sie versuchen, online zu bewerten: **Berechtigung fehlgeschlagen**.
{: tsSymptoms}

Diese Nachricht wird angezeigt, wenn das Token, das für das Scoring verwendet wird, abgelaufen ist.
{: tsCauses}

Generieren Sie das Token für diese {{site.data.keyword.pm_full}}-Instanz neu und versuchen Sie es dann erneut. Wenn das Problem weiterhin bestehen bleibt, wenden Sie sich an IBM Support.
{: tsResolve}

## Unbekannte Bereitstellungsidentifikation: 404
{: #os_unkown_depid}

Die folgende Nachricht wird angezeigt, wenn Sie versuchen, online zu bewerten: **Unbekannte Bereitstellungsidentifikation**. {: osSymptoms}

Diese Nachricht wird angezeigt, wenn die Bereitstellungs-ID, die für das Scoring verwendet wird, nicht vorhanden ist.
{: tsCauses}

Stellen Sie sicher, dass Sie die richtige Bereitstellungs-ID bereitstellen. Ist dies nicht der Fall, stellen Sie das Modell mit der richtigen Bereitstellungs-ID bereit und versuchen Sie dann erneut, ein Scoring durchzuführen.
{: tsResolve}

## Interner Serverfehler: 500
{: #os_internal_error}

Die folgende Nachricht wird angezeigt, wenn Sie versuchen, online zu bewerten: **Interner Serverfehler**.  {: osSymptoms}

Diese Nachricht wird angezeigt, wenn der nachfolgende Datenfluss, von dem das Online-Scoring abhängt, fehlschlägt.
{: tsCauses}

Nachdem Sie eine Weile gewartet haben, versuchen Sie erneut, online zu bewerten. Wenn das Scoring erneut fehlschlägt, wenden Sie sich an IBM Support.
{: tsResolve}
              
