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

# Risoluzione dei problemi

Queste sono le risposte alle domande sulla risoluzione dei problemi comuni riguardanti l'utilizzo di {{site.data.keyword.pm_full}}.
{: shortdesc}

## Come ottenere aiuto e supporto per Machine Learning
{: #gettinghelp}

Se hai dei problemi o delle domande quando utilizzi {{site.data.keyword.pm_short}}, puoi ottenere aiuto ricercando le informazioni o facendo delle domande in un forum. Puoi anche aprire un ticket di supporto.

Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.pm_short}}. 

Se hai domande tecniche su {{site.data.keyword.pm_short}}, inserisci la tua domanda in <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="External link icon"></a> e contrassegnala con le tag "ibm-bluemix" e "machine-learning".

Per domande sul servizio e le istruzioni introduttive, utilizza il forum <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Includi le tag  "machine-learning" e "bluemix".
Per ulteriori dettagli sull'utilizzo dei forum, vedi [Come ottenere supporto](https://console.bluemix.net/docs/support/index.html#getting-help).

Per informazioni su come aprire un ticket di supporto IBM o sui livelli di supporto e sulla gravità dei ticket, consulta [Come contattare il supporto](https://console.bluemix.net/docs/support/index.html#contacting-support).

## Token di autorizzazione non fornito.
{: #ts_missing_authorization_token}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Il token di autorizzazione non è stato fornito nell'intestazione `Authorization`.
{: tsCauses}
 
Passa il token di autorizzazione nell'intestazione `Authorization`.
{: tsResolve}
       
## Token di autorizzazione non valido.
{: #ts_invalid_authorization_token}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Il token di autorizzazione che è stato fornito non può essere decodificato o analizzato.
{: tsCauses}
 
Passa il token di autorizzazione corretto nell'intestazione `Authorization`.
{: tsResolve}
       
## Il token di autorizzazione e l'instance_id utilizzato nella richiesta non sono uguali.
{: #ts_not_matching_authorization_token}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Il token di autorizzazione utilizzato non viene generato per l'istanza del servizio in cui è stato usato.
{: tsCauses}
 
Passa il token di autorizzazione nell'intestazione `Authorization` che corrisponde all'istanza del servizio da utilizzare.
{: tsResolve}
       
## Il token di autorizzazione è scaduto.
{: #ts_expired_authorization_token}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Il token di autorizzazione è scaduto.
{: tsCauses}
 
Passa un token di autorizzazione non scaduto nell'intestazione `Authorization`.
{: tsResolve}
       
## La chiave pubblica necessaria per l'autenticazione non è disponibile.
{: #ts_missing_public_key}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si tratta di un problema interno del servizio.
{: tsCauses}
 
Il problema deve essere risolto dal team di supporto.
{: tsResolve}
       
## Operazione scaduta dopo {{timeout}}
{: #ts_operation_timeout}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Timeout durante l'esecuzione dell'operazione richiesta.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata.
{: tsResolve}
       
## Eccezione non gestita di tipo {{type}} con {{status}}
{: #ts_unhandled_exception_with_status}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si tratta di un problema interno del servizio.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata. Se si verifica più volte, il problema dovrà essere risolto dal team di supporto.
{: tsResolve}
       
## Eccezione non gestita di tipo {{type}} con {{response}}
{: #ts_unhandled_exception_with_response}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si tratta di un problema interno del servizio.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata. Se si verifica più volte, il problema dovrà essere risolto dal team di supporto.
{: tsResolve}
       
## Eccezione non gestita di tipo {{type}} con {{json}}
{: #ts_unhandled_exception_with_json}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si tratta di un problema interno del servizio.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata. Se si verifica più volte, il problema dovrà essere risolto dal team di supporto.
{: tsResolve}
       
## Eccezione non gestita di tipo {{type}} con {{message}}
{: #ts_unhandled_exception_with_message}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si tratta di un problema interno del servizio.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata. Se si verifica più volte, il problema dovrà essere risolto dal team di supporto.
{: tsResolve}
       
## Non è stato possibile trovare l'oggetto richiesto.
{: #ts_not_found}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Non è stato possibile trovare la risorsa richiesta.
{: tsCauses}
 
Assicurati di fare riferimento alla risorsa esistente.
{: tsResolve}
       
## Il database sottostante ha riportato troppe richieste.
{: #ts_too_many_cloudant_requests}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
L'utente ha inviato troppe richieste in un determinato intervallo di tempo.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata.
{: tsResolve}
       
 ## La definizione della valutazione non è definita né nell'artifactModelVersion né nella distribuzione. Deve essere specificata \" +\n      \" almeno in una della posizioni.
{: #ts_missing_evaluation_definition}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Configurazione di apprendimento non contiene tutte le informazioni richieste
{: tsCauses}
 
Fornisci la `definizione` in `configurazione di apprendimento`
{: tsResolve}
       
## La valutazione richiede che sia specificata la configurazione di apprendimento per il modello.
{: #ts_missing_learning_configuration}
 
Non c'è alcuna possibilità di creare l'`iterazione di apprendimento`.
{: tsSymptoms}
 
Non c'è alcuna `configurazione di apprendimento` definita per il modello.
{: tsCauses}
 
Crea la `configurazione di apprendimento` e prova a creare di nuovo l'`iterazione di apprendimento`.
{: tsResolve}
       
## La valutazione richiede che l'istanza spark sia fornita nell'intestazione `X-Spark-Service-Instance`
{: #ts_missing_spark_definition_for_evaluation}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Nella `configurazione di apprendimento` mancano delle informazioni obbligatorie
{: tsCauses}
 
Fornisci `spark_service` nella configurazione di apprendimento o nell'instazione `X-Spark-Service-Instance`
{: tsResolve}
       
## Il modello non contiene alcuna versione.
{: #ts_missing_latest_model_version}
 
Non c'è alcuna possibilità né di creare la distribuzione né di impostare la configurazione di apprendimento.
{: tsSymptoms}
 
Esiste un'incongruenza relativa alla persistenza del modello.
{: tsCauses}
 
Prova a rendere di nuovo persistente il modello e a rieseguire l'operazione.
{: tsResolve}
       
## L'operazione patch può modificare solo la configurazione di apprendimento esistente.
{: #ts_patch_non_existing_learning_configuration}
 
Non c'è alcuna possibilità di richiamare il metodo REST API patch per correggere la configurazione di apprendimento.
{: tsSymptoms}
 
Non c'è alcuna `configurazione di apprendimento` impostata per questo modello o il modello non esiste.
{: tsCauses}
 
Assicurati che il modello esista e che abbia già la configurazione di apprendimento impostata.
{: tsResolve}
       
## L'operazione patch prevede esattamente un'operazione di sostituzione.
{: #ts_patch_multiple_ops}
 
Non è possibile correggere la distribuzione.
{: tsSymptoms}
 
Il payload di patch contiene una o più operazioni o l'operazione patch è diversa da `replace`.
{: tsCauses}
 
Utilizza una sola operazione nel payload di patch che sia l'operazione `replace`
{: tsResolve}
       
## Nel payload fornito mancano dei campi obbligatori: FIELD o i valori dei campi sono danneggiati.
{: #ts_invalid_request_payload}
 
Non c'è alcuna possibilità di elaborare l'azione correlata all'accesso al dataset sottostante.
{: tsSymptoms}
 
L'accesso al dataset non è definito correttamente.
{: tsCauses}
 
Correggi la definizione di accesso per il dataset.
{: tsResolve}
       
## Il metodo di valutazione fornito: METHOD non è supportato. Valori supportati: VALUE.
{: #ts_evaluation_method_not_supported}
 
Non c'è alcuna possibilità di creare la configurazione di apprendimento.
{: tsSymptoms}
 
È stato utilizzato un metodo di valutazione errato per creare la configurazione di apprendimento.
{: tsCauses}
 
Utilizza un metodo di valutazione supportato scegliendolo tra i seguenti: `regression`, `binary`, `multiclass`.
{: tsResolve}
       
## Ci può essere una sola valutazione attiva per ogni modello. Non è stato possibile completare la richiesta a causa della valutazione attiva esistente: {{url}}
{: #ts_active_evaluation_conflict}
 
Non c'è alcuna possibilità di creare un'altra iterazione di apprendimento
{: tsSymptoms}
 
Ci può essere una sola valutazione in esecuzione per il modello.
{: tsCauses}
 
Vedi la valutazione già in esecuzione o attendi che termini e avvia quella nuova.
{: tsResolve}
       
## Il tipo di distribuzione {{type}} non è supportato.
{: #ts_not_supported_deployment_type}
 
Non c'è alcuna possibilità di creare la distribuzione.
{: tsSymptoms}
 
È stato utilizzato un tipo di distribuzione non supportato.
{: tsCauses}
 
È necessario utilizzare un tipo di distribuzione supportato.
{: tsResolve}
       
## Input non corretto: ({{message}})
{: #ts_deserialization_error}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si è verificato un problema con l'analisi del json.
{: tsCauses}
 
Assicurati che nella richiesta venga passato il json corretto.
{: tsResolve}
       
## Dati non sufficienti - non è stato possibile calcolare la metrica {{name}}
{: #ts_missing_metric}
 
Iterazione di apprendimento non riuscita
{: tsSymptoms}
 
Non è stato possibile calcolare il valore per la metrica con la soglia definita a causa di dati di feedback insufficienti
{: tsCauses}
 
Rivedi e migliora i dati nell'origine dati `feedback_data_ref` in `configurazione di apprendimento`
{: tsResolve}
       
## Per il tipo {{type}}, è necessario fornire l'istanza spark nell'intestazione `X-Spark-Service-Instance`
{: #ts_missing_prediction_spark_definition}
 
Non è possibile creare la distribuzione
{: tsSymptoms}
 
Le distribuzioni `batch` e `streaming` richiedono che sia fornita l'istanza spark
{: tsCauses}
 
Fornisci l'istanza spark nell'intestazione `X-Spark-Service-Instance`
{: tsResolve}
       
## Azione {{action}} non riuscita con il messaggio {{message}}
{: #ts_http_client_error}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si è verificato un problema con il richiamo del servizio sottostante.
{: tsCauses}
 
Se c'è un suggerimento su come risolvere il problema, attieni ad esso. Contatta il team di supporto se nel messaggio non c'è alcun suggerimento o se il suggerimento non aiuta a risolvere il problema.
{: tsResolve}
       
## Il percorso `{{path}}` non è consentito. L'unico percorso consentito per il flusso di patch è `/status`
{: #ts_wrong_stream_patch_path}
 
Non c'è alcuna possibilità di correggere la distribuzione stream.
{: tsSymptoms}
 
È stato utilizzato un percorso errato per correggere la distribuzione `stream`.
{: tsCauses}
 
Correggi la distribuzione `stream` con l'opzione percorso supportata che è `/status` (consente di avviare/arrestare l'elaborazione del flusso).
{: tsResolve}
       
## L'operazione patch non è consentita per l'istanza di tipo `{{$type}}`
{: #ts_patch_not_supported}
 
Non c'è alcuna possibilità di correggere la distribuzione.
{: tsSymptoms}
 
Viene corretto il tipo di distribuzione errato.
{: tsCauses}
 
Correggi il tipo di distribuzione `stream`.
{: tsResolve}
       
## La connessione dati `{{data}}` non è valida per feedback_data_ref
{: #ts_invalid_feedback_data_connection}
 
Non c'è alcuna possibilità di creare la `configurazione di apprendimento` per il modello.
{: tsSymptoms}
 
È stata utilizzata un'origine dati non supportata durante la definizione di feedback_data_ref.
{: tsCauses}
 
Utilizza solo il tipo di origine dati supportato, che è `dashdb`
{: tsResolve}
       
## Il percorso {{path}} non è consentito. L'unico percorso consentito per il modello patch è `/deployed_version/url` o `/deployed_version/href` per V2
{: #ts_patch_model_path_not_allowed}
 
Non c'è alcuna opzione per correggere il modello.
{: tsSymptoms}
 
È stato utilizzato un percorso errato durante la correzione del modello.
{: tsCauses}
 
Correggi il modello con il percorso supportato che consente di aggiornare la versione del modello distribuito.
{: tsResolve}
       
## Errore di analisi: {{msg}}
{: #ts_parsing_error}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Non è stato possibile analizzare correttamente il payload richiesto.
{: tsCauses}
 
Assicurati che il payload richiesto sia corretto e che possa essere analizzato correttamente.
{: tsResolve}
       
## L'ambiente di runtime per il modello selezionato: {{env}} non è supportato per la `configurazione di apprendimento`. Ambienti supportati: [{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
Non c'è alcuna opzione per creare la `configurazione di apprendimento`
{: tsSymptoms}
 
Il modello per il quale è stato tentato di creare la `configurazione_di_apprendimento` non è supportato.
{: tsCauses}
 
Crea la `configurazione di apprendimento` per il modello con il runtime supportato.
{: tsResolve}
       
## Il piano corrente \'{{plan}}\' consente solo {{limit}} distribuzioni
{: #ts_deployments_plan_limit_reached}
 
Non c'è alcuna possibilità di creare la distribuzione.
{: tsSymptoms}
 
È stato raggiunto il limite del numero di distribuzioni per il piano corrente.
{: tsCauses}
 
Esegui l'aggiornamento al piano che non prevede tale limitazione.
{: tsResolve}
       
## La definizione della connessione al database non è valida ({{code}})
{: #ts_sql_error}
 
Non c'è alcuna possibilità di utilizzare la funzionalità `configurazione di apprendimento`.
{: tsSymptoms}
 
La definizione della connessione al database non è valida.
{: tsCauses}
 
Prova a risolvere il problema descritto dal `codice` restituito dal database sottostante.
{: tsResolve}
       
## Si sono verificati dei problemi durante la connessione al {{system}} sottostante
{: #ts_stream_tcp_error}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si è verificato un problema durante la connessione al sistema sottostante. Potrebbe trattarsi di un problema di rete temporaneo.
{: tsCauses}
 
Prova a chiamare di nuovo l'operazione desiderata. Se si verifica più volte, contatta il team di supporto.
{: tsResolve}
       
## Errore durante l'estrazione dell'intestazione X-Spark-Service-Instance: ({{message}})
{: #ts_spark_header_deserialization_error}
 
Non c'è alcuna possibilità di richiamare l'API REST che richiede le credenziali Spark
{: tsSymptoms}
 
Si è verificato un problema con la decodifica in base-64 o con l'analisi delle credenziali Spark.
{: tsCauses}
 
Assicurati che le credenziali Spark valide siano codificate correttamente in base-64. Per ulteriori informazioni, consulta la documentazione.
{: tsResolve}
       
## Questa funzionalità non è consentita per gli utenti non beta.
{: #ts_not_beta_user}
 
Impossibile richiamare correttamente l'API REST desiderata.
{: tsSymptoms}
 
L'API REST che è stata richiamata è attualmente in beta.
{: tsCauses}
 
Se sei interessato a partecipare, aggiungi il tuo nome alla lista di attesa. Puoi trovare i dettagli nella documentazione.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
Impossibile richiamare correttamente l'API REST.
{: tsSymptoms}
 
Si è verificato un problema con il richiamo del servizio sottostante.
{: tsCauses}
 
Se c'è un suggerimento su come risolvere il problema, attieni ad esso. Contatta il team di supporto se nel messaggio non c'è alcun suggerimento o se il suggerimento non aiuta a risolvere il problema.
{: tsResolve}
       
## Limite di frequenza superato.
{: #ts_rate_limit_exceeded}
 
Limite di frequenza superato.
{: tsSymptoms}
 
Il limite di frequenza per il piano corrente è stato superato.
{: tsCauses}
 
Per risolvere questo problema, acquisisci un altro piano con un limite di frequenza maggiore
{: tsResolve}
       
## Parametro `{{paramName}}` di query non valido con valore: {{value}}
{: #ts_invalid_query_parameter_value}
 
Errore di convalida poiché è stato passato un valore non corretto per il parametro di query.
{: tsSymptoms}
 
Errore nel richiamo del risultato per la query.
{: tsCauses}
 
Correggi il valore del parametro di query. Puoi trovare i dettagli nella documentazione.
{: tsResolve}
       
## Tipo di token non valido: {{type}}
{: #ts_invalid_token_type}
 
Errore relativo al tipo di token.
{: tsSymptoms}
 
Errore di autorizzazione.
{: tsCauses}
 
Il token deve iniziare con il prefisso `Bearer`
{: tsResolve}
       
## Formato token non valido. È necessario utilizzare il formato token Bearer.
{: #ts_invalid_token_format}
 
Errore relativo al formato di token.
{: tsSymptoms}
 
Errore di autorizzazione.
{: tsCauses}
 
Il token deve essere un token di connessione e deve iniziare con il prefisso `Bearer`
{: tsResolve}

## File JSON di input mancante o non valido: 400
{: #os_invalid_input}

Quando tenti di calcolare il punteggio online viene visualizzato il seguente messaggio: **File JSON di input mancante o non valido**.
{: tsSymptoms}

Questo messaggio viene visualizzato quando il payload di input di punteggio non corrisponde al tipo di input previsto che è richiesto per il calcolo del punteggio del modello. In particolare, possono essere applicati i seguenti motivi:

- Il payload di input è vuoto.
- Lo schema del payload di input non è valido.
- I tipi di dati di input non corrispondono a tipi di dati previsti.
{: tsCauses}

Correggi il payload di input. Assicurati che il payload abbia una sintassi corretta, uno schema valido e tipi di dati corretti. Dopo aver apportato le correzioni, prova a ripetere il calcolo del punteggio online. Per i problemi di sintassi, verifica il file JSON utilizzando il comando `jsonlint`.
{: tsResolve}

## Il token di autorizzazione è scaduto: 401
{: #os_expired_authorization_token}

Quando tenti di calcolare il punteggio online viene visualizzato il seguente messaggio: **Autorizzazione non riuscita**.
{: tsSymptoms}

Questo messaggio viene visualizzato quando il token utilizzato per il calcolo del punteggio è scaduto.
{: tsCauses}

Rigenera il token per questa istanza di {{site.data.keyword.pm_full}} e ritenta l'operazione. Se riscontri ancora questo problema, contatta il supporto IBM.
{: tsResolve}

## Identificazione di distribuzione sconosciuta:404
{: #os_unkown_depid}

Quando tenti di calcolare il punteggio online viene visualizzato il seguente messaggio:  **Identificazione di distribuzione sconosciuta**. {: osSymptoms}

Questo messaggio viene visualizzato quando l'ID di distribuzione utilizzato per il calcolo del punteggio non esiste.
{: tsCauses}

Assicurati di fornire l'ID di distribuzione corretto. Altrimenti, distribuisci il modello con l'ID di distribuzione e prova a ripetere il calcolo del punteggio.
{: tsResolve}

## Errore interno del server:500
{: #os_internal_error}

Quando tenti di calcolare il punteggio online viene visualizzato il seguente messaggio: **Errore interno del server**  {: osSymptoms}

Questo messaggio viene visualizzato se si produce un errore nel flusso di dati downstream da cui dipende il calcolo del punteggio online.
{: tsCauses}

Dopo aver atteso per un certo periodo di tempo, prova a ripetere il calcolo del punteggio online. Se ancora non riesce, contatta il supporto IBM.
{: tsResolve}
              
