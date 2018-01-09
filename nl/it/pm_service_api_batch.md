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

# API del lavoro batch del servizio Machine Learning per i modelli IBM SPSS Modeler

L'API del lavoro batch per il servizio {{site.data.keyword.pm_full}} supporta
le attività a esecuzione prolungata correlate alla formazione modello, alla valutazione del modello e
al calcolo del punteggio batch. Questa API gestisce due tipi di asset: i file del flusso IBM® SPSS®
Modeler utilizzati nei lavori batch e
le definizioni del lavoro inviate.
{: shortdesc}

Per ogni file del flusso SPSS® Modeler che hai caricato,
possono esserci molti lavori di vari tipi che vengono inviati. Se un lavoro ha il
potenziale per aggiornare i contenuti del file del flusso Modeler, il file modificato
viene incluso negli allegati del risultato del lavoro. In un tipo di lavoro di valutazione del modello, tutti i file valutati
generati sono presenti negli allegati dei risultati del lavoro. 

**Nota**: i dati visualizzati nel dashboard sono correlati solo alle previsioni in tempo reale, come i dati della funzione di caricamento. 

## Eliminazione lavori

Puoi anche eliminare i lavori. Se un lavoro è in esecuzione, eliminarlo, annulla il lavoro. Utilizza il comando `DELETE` con `job ID`. Puoi annullare più di un lavoro alla volta passando più ID.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

Il valore restituito indica quanti lavori a cui fa riferimento l'ID nella richiesta sono stati eliminati. Se
questo numero non corrisponde all'elenco trasmesso nella richiesta, devi controllare lo stato
dei singoli lavori. 

## Controllo dello stato di un lavoro

Puoi ottenere lo stato del tuo `job ID` in qualsiasi momento utilizzando il comando `GET`:


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

Il file JSON indica lo stato del lavoro e, se il lavoro è terminato con esito
positivo, un URL dei dati che puoi utilizzare per ottenere tutto il contenuto del file
generato.

## Reinvia un lavoro

Per inviare nuovamente un lavoro, utilizza il comando `PUT` con `job ID`. Per reinviare un lavoro, deve essere in esecuzione.

Se l'ID a cui fai riferimento non esiste, la seguente chiamata crea un nuovo lavoro. Lo stato di ritorno è 201 invece di 200 (che indica
cosa è successo). Devi trasmettere il file JSON della definizione del lavoro da utilizzare in questa esecuzione. 

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Reinvia il lavoro con il valore `content_type` impostato su "application/json". Devi includere il file JSON della definizione del lavoro nuovo o aggiornato nel corpo della richiesta.

## Invia un lavoro in un file del flusso Modeler caricato

Per inserire un lavoro nella coda di esecuzione, utilizza il comando `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Invia il lavoro con il valore `content_type` impostato su "application/json". Devi includere il file JSON della definizione del lavoro nuovo o aggiornato nel corpo della richiesta.

Questa richiesta viene restituita immediatamente e indica un esito positivo se la definizione del lavoro è stata posizionata nella coda di
esecuzione. Non esiste una verifica della capacità di eseguire correttamente il flusso Modeler
come configurato nella tua definizione del lavoro. Il lavoro sarà eseguito da uno dei server del lavoro
{{site.data.keyword.pm_short}} nel cloud e puoi monitorarne
lo stato. Se stai eseguendo la formazione del modello e indichi che desideri eseguire l'aggiornamento automatico, il lavoro
sostituisce il file del flusso Modeler originale nel momento dell'esecuzione positiva del lavoro. 

## Carica un file del flusso da utilizzare nei tuoi lavori

**Nota**: il dashboard {{site.data.keyword.pm_short}} serve solo per il calcolo del punteggio
in tempo reale. Non puoi utilizzarlo per eseguire i lavori (calcolo batch).

Per rendere un file del flusso Modeler accessibile ai lavori, utilizza il comando `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

con content_type "multipart/form-data" che passa il file nella
richiesta.

Dovrai fare riferimento al nome univoco utilizzato (`file ID` in una chiamata `PUT`)
in una chiamata `DELETE` all'API del file così come al riferimento del modello
nelle tue definizioni del lavoro. Nota che lo spazio dei nomi dei file
utilizzato nei tuoi lavori è completamente sotto il tuo controllo. Il comando `PUT` di un file
sotto lo stesso `file ID` sostituisce implicitamente la copia
corrente.

Per generare un elenco di tutti i file caricati per i tuoi lavori, utilizza un comando `GET` omettendo però il parametro `file ID`. Per richiamare uno specifico file, utilizza un comando `GET` con il parametro `file ID`. Puoi anche eliminare un file caricato immettendo un comando `DELETE`. Ciò provoca errori in qualsiasi esecuzione di lavori in sospeso che fa riferimento al file.

Esempio di
richiesta:

```
    Content-Type: multipart/form-data
    Parametri:
        Parametri modulo:
            model_file: il file del modello da caricare
        Parametri percorso:
            contextId: l'identificativo univoco da assegnare al tuo modello o un riferimento al modello distribuito da aggiornare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la distribuzione riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":true,
           "message":"informazioni dettagliate"
         }      
```
{: codeblock}

Risposta quando la distribuzione non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
        }
```
{: codeblock}

## Tipi di lavoro

**Formazione**: questo tipo di lavoro indica che tutti i nodi del terminale "builder del modello"
devono essere eseguiti nel flusso Modeler. Dopo che il lavoro è stato completato correttamente, verrà creato un flusso Modeler aggiornato
con i nuovi nugget del modello eseguito nei risultati del lavoro che possono essere richiamati. Se il file del flusso Modeler
dispone di collegamenti dai nodi di build del modello ai nugget del modello eseguito nei rami di valutazione e calcolo,
sarà eseguito un aggiornamento di questi nodi.

**Valutazione**: questo tipo di lavoro attiva l'esecuzione di tutti i nodi del terminale "builder del
documento" (principalmente dalle schede Grafici e Output nel
client Modeler) che generano contenuti del file di report statico che
possono essere trasmessi al chiamante. Il ramo di calcolo non è considerato come parte di questo tipo di lavoro.

**Aggiornamento automatico**: una versione del tipo di lavoro `TRAINING` dove,
al completamento con esito positivo del lavoro, il file del flusso Modeler originale
nell'elenco di file batch sarà aggiornato. La valutazione e la decisione esplicita riguardante un evento di aggiornamento dei flussi Modeler
distribuiti per il calcolo in tempo reale si suppone non siano coperti dall'aggiornamento automatico in questo momento.

**Punteggio batch**: esecuzione del nodo del terminale a cui hai applicato l'opzione
Utilizza come ramo di calcolo, che indica che questo è il ramo di calcolo del punteggio
in questa progettazione del flusso Modeler. La definizione del lavoro deve specificare
i dettagli dell'esportazione così come i dettagli dell'origine.

**Esegui flusso**: l'esecuzione è simile al fare clic sull'icona di "esecuzione" verde in
Modeler con l'opzione Esegui questo script selezionata nella scheda
Esecuzione delle proprietà del flusso. L'utilizzo copre i bisogni dell'esecuzione con script
della formazione del modello o di altri tipi di lavoro. Tutto il controllo dinamico dello script deve essere gestito dai parametri del flusso,
con i valori del parametro trasmessi nella definizione del lavoro.

## Stato del lavoro

**In sospeso**: la definizione del lavoro è stata inviata ma non è stata
ancora richiesta da un server del lavoro per l'esecuzione.

**In esecuzione**: la definizione del lavoro è stata richiesta da un server del lavoro ed
è in esecuzione.

**In annullamento**: il lavoro è in fase di annullamento.

**Annullato**: il lavoro è stato annullato.

**Non riuscito**: il lavoro non è riuscito. I dettagli sulla causa dell'errore vengono
restituiti con GET nello stato del lavoro.

**Riuscito**: il lavoro è riuscito. Qualsiasi risultato comunicato per
questo evento viene restituito nel JSON di GET nello stato del lavoro.

## Dettagli API lavoro

`POST /v1/jobs/{id}`

Descrizione: invia una definizione del lavoro per l'esecuzione. Si verificherà un errore se
`job ID` già esiste.

Tipi di contenuto:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` specificato dall'utente. Deve essere univoco per un'istanza del servizio {{site.data.keyword.pm_short}}:

```
@PathParam("id")
```
{: codeblock}

Definizione lavoro JSON (consulta JSON della definizione del lavoro):

```
@BodyParam
```
{: codeblock}

Risposte:

Esito positivo. Il lavoro è stato inviato per l'esecuzione e restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore, il lavoro già esiste. Un lavoro con questo ID è già stato inviato. Il messaggio restituito descrive questo errore.

```
@ApiResponse(code = 409)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

Descrizione: crea o aggiorna un lavoro. Se un lavoro con questo ID non esiste,
lo crea; altrimenti, lo aggiorna (che, di fatto, lo reinvia per l'esecuzione).

Tipi di contenuto:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` specificato dall'utente:

```
@PathParam("id")
```
{: codeblock}

Definizione lavoro JSON (consulta JSON della definizione del lavoro):

```
@BodyParam
```
{: codeblock}

Risposte:

Esito positivo. Il lavoro è aggiornato ed è stato inviato per l'esecuzione. Restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 200)
```
{: codeblock}

Esito positivo. Il lavoro è stato creato ed è stato inviato per l'esecuzione. Restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 201)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

Descrizione: restituisce un elenco di tutti i lavori definiti su questa istanza del servizio Machine
Learning.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce l'array JSON di tutte le definizioni del lavoro:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

Descrizione: richiede la restituzione di una specifica definizione del lavoro.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` inviato:

```
@PathParam("id")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il JSON che descrive il lavoro di riferimento:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore, lavoro non trovato. Dettagli nel messaggio restituito:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

Descrizione: richiama lo stato di uno specifico lavoro.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` inviato:

```
@PathParam("id")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il JSON che descrive lo stato del lavoro:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore di lavoro non trovato, dettagli nel messaggio restituito:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

Descrizione: elimina uno o più lavori. Eliminerà il lavoro se è al momento in esecuzione.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` inviato o elenco di `job ID` con i valori ID suddivisi da una virgola (,)

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il numero di lavori eliminati:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON della definizione del lavoro

Il JSON della definizione del lavoro contiene le seguenti sezioni generali:

Riferimento al modello predittivo e tipo di lavoro

**Nota**: i dati visualizzati nel dashboard sono correlati solo alle previsioni in tempo reale, inclusi i dati della funzione di caricamento.

### Tipi di azione

**TRAINING** – esegue la formazione del nodo di builder del modello o aggiorna i nugget
del modello. Il flusso Modeler aggiornato viene richiamato nei risultati del lavoro.

**EVALUATION** – esegue il builder del documento e analizza i nodi valutando
l'ultimo modello eseguito.

**AUTO_REFRESH** – esegue TRAINING e aggiorna i contenuti del file
   nello spazio di caricamento del file batch.

**BATCH_SCORE** – esegue il ramo di calcolo e esporta i dati risultanti come
diretti da una definizione del lavoro.

**RUN_STREAM** – esegue il flusso Modeler come indicato nelle proprietà del flusso;
molto spesso utilizzato quando è richiesta l'esecuzione con script.

Modello: l'ID come specificato nell'azione di caricamento del file per il
riferimento del lavoro batch. Per ulteriori informazioni, vedi Caricamento di un file del flusso
da utilizzare nei tuoi lavori. Nota che viene utilizzato
`/pm/v1/file`.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Nota che l'ID deve essere uguale al `file ID` utilizzato
nell'API `PUT`. Il nome non è obbligatorio, ma per la formazione e l'aggiornamento automatico del modello
il risultato del lavoro verrà salvato utilizzando il nome definito
qui. Se name non è stato definito, il servizio {{site.data.keyword.pm_short}} genererà il risultato
in base alle regole di denominazione predefinite.

### Impostazioni lavoro

Tutte le impostazioni richiedono di eseguire questo lavoro.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### Definizioni di connettività del database

Tipo di database: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL, ProgressOpenEdge, Salesforce,  SQLServer, Sybase, SybaseIQ, Teradata.

La connettività come specificata nell'istanza del servizio del DB, molti tipi di DB richiedono la trasmissione di
configurazioni specifiche nelle
‘Opzioni’

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

### Impostazioni nodo di origine

La connettività del DB di riferimento e la tabella utilizzata per originare un ramo fornito nel flusso Modeler
come identificato dal nome del nodo di origine.

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

Il valore table è il nome della tabella del database. Questa tabella è utilizzata per sovrascrivere il nodo
di origine del flusso. Il valore node è il nome dell'origine dati
per il flusso. Il valore dbRef fa riferimento alla connettività del DB definita
dall'oggetto dbDefinitions e il valore table è la tabella del database utilizzata
come input del ramo fornito nel flusso Modeler. Il valore node
identifica il nodo di origine iniziale nel flusso Modeler da sostituire
con un nodo di origine del DB creato con questi parametri
forniti.

### Impostazioni del nodo di esportazione

La connettività del DB di riferimento e la tabella utilizzata per conservare i dati per un ramo fornito nel flusso Modeler
come identificato dal nome del nodo del terminale.

Il controllo del metodo utilizzato durante la conservazione dei dati viene comunicato tramite l'attributo
insertMode:

**Append** – la tabella deve esistere ed essere compatibile per l'inserimento

**Create** – la tabella è stata creata (è stato restituito un errore se già esiste)

**Drop** – se la tabella di riferimento già esiste, viene eliminata e ricreata

**Refresh** – le righe esistenti della tabella vengono eliminate prima di inserire nuove
righe
   
Il valore table è il nome della tabella del database in cui scrivere i risultati del lavoro.
Il valore node è il nome del nodo del terminale per il flusso. Analogamente alle impostazioni
del nodo di origine, node identifica il nodo di output originale
nel flusso Modeler da sostituire con un nodo di esportazione del DB creato
con questi parametri forniti.

**Nota**: le impostazioni del nodo di esportazione e di origine
sono importanti per la corretta esecuzione del lavoro.

### Sovrascritture del valore del parametro

Per sovrascrivere i valori predefiniti per i parametri al livello del flusso prima dell'esecuzione di un lavoro, puoi specificare
la coppia nome/valore da utilizzare nella definizione del lavoro.

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

Formati del report desiderati per il documento di valutazione restituito durante l'esecuzione di un tipo di lavoro di valutazione

Tutti i documenti generati dall'esecuzione del lavoro di valutazione vengono
compressi in un file .zip. Solo i nodi di builder del documento in grado di
supportare il reportFormat indicato vengono eseguiti e hanno i rispettivi
contenuti restituiti dopo un'esecuzione corretta del lavoro.

Sono supportati i seguenti formati: HTML, JPG, PNG, RTF, SAV, TAB e XML.

```
"reportFormat": "HTML"
```
{: codeblock}

### Esempio JSON completo

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

## Dettagli API del lavoro batch

Le seguenti sezioni forniscono i dettagli dell'API di gestione del file IBM® SPSS® Modeler del lavoro batch.

`PUT /v1/file/{id}`

Descrizione: carica un file del flusso IBM® SPSS® Modeler da utilizzare nei lavori
batch.

**Nota**: se c'è già un file memorizzato nell'ID specificato,
sarà sovrascritto.

### Tipi di contenuto

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### Parametri

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey") 
```
{: codeblock}

`file ID` specificato dall'utente. Deve essere univoco nel repository dell'istanza del servizio:

```
@PathParam("id") 
```
{: codeblock}

### Risposte

Esito positivo. Restituisce l'URL che può essere utilizzato per scaricare il file dal repository dell'istanza del servizio:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

Descrizione: elimina il file memorizzato nell'ID specificato dal
repository dell'istanza del servizio.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID specificato dall'utente per il file quando caricato:

```
@PathParam("id") 
```
{: codeblock}

Risposte:

Esito positivo. Il file specificato è stato eliminato:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore. Un file archiviato in questo ID non esiste nel repository dell'istanza del servizio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

Descrizione: restituisce un elenco di ID di tutti i file gestiti
nel repository dell'istanza del servizio per l'elaborazione del lavoro batch.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

Risposte:

Esito positivo. Restituisce un array JSON di valori `file ID`:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

Descrizione: richiama il file del flusso IBM® SPSS® Modeler memorizzato da
utilizzare nell'elaborazione del lavoro batch nell'ID specificato.

Tipi di contenuto:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

ID specificato dall'utente per il file quando caricato:

```
@PathParam("id") 
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il file IBM® SPSS® Modeler:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore. Un file archiviato in questo ID non esiste nel repository dell'istanza del servizio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

## Ulteriori informazioni

Per un esempio di adozione di lavoro batch in un notebook, consulta [From SPSS stream to batch scoring with
Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

Per ulteriori informazioni sul JSON della definizione del lavoro, consulta [JSON della
definizione del lavoro](#job-definition-json).
