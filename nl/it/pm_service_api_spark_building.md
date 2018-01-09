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

# Creazione di applicazioni di analisi predittiva

Puoi utilizzare il servizio {{site.data.keyword.pm_full}} per distribuire un'applicazione Node.js.
{: shortdesc}

**Nome scenario**: Previsione linea di prodotti.

**Descrizione scenario**: il nostro cliente sta realizzando una delle più famose catene di negozi
in Europa. Ci richiede di scoprire gli interessi dei propri clienti in merito alla propria linea di prodotti,
come Accessori personali, Attrezzature da campeggio e Protezione all'aperto.
Un data scientist sviluppa un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di
preparare e distribuire l'applicazione Node.js che consiglia linee di prodotti sportivi effettuando
richieste di punteggio sul modello distribuito.

1. Accedi a {{site.data.keyword.Bluemix_short}} e crea un'istanza di {{site.data.keyword.pm_full}}.
2. Avvia il dashboard {{site.data.keyword.pm_full}}.
3. Vai alla scheda **Esempi**.
4. Nella sezione **Modelli di esempio**, cerca il tile **Previsione linea
di prodotti** e fai clic sull'icona (+) **Aggiungi modello**. Viene visualizzato il modello
di esempio Previsione linea di prodotti nell'elenco di modelli
disponibili nella scheda **Modelli**. 

   **Nota**: se anziché gli esempi, vuoi utilizzare i tuoi propri progetti e modelli
   di Data Science Experience, devi rendere persistente un modello personalizzato
   nel repository {{site.data.keyword.pm_short}}. Per farlo, puoi utilizzare
l'API REST o le librerie del client. Per i
   dettagli, vedi Sviluppo e persistenza del modello personalizzato.

5. Dal menu **Azione**, fai clic su **Crea distribuzione** per
distribuire il modello Previsione linea di prodotti come distribuzione online. 
6. Specifica il nome della distribuzione (ad esempio, Previsione linea
   di prodotti) e fai clic su Salva. Dovresti ora visualizzare la distribuzione Previsione linea di
   prodotti nell'elenco Distribuzioni.
7. Dal menu Azione, seleziona Visualizza dettagli per la tua distribuzione.
   L'Endpoint di calcolo del punteggio presentato nel riquadro dei dettagli sarà utilizzato per
   eseguire le richieste di calcolo di punteggio.
8. Per eseguire le richieste di calcolo di punteggio attraverso l'API REST, sono necessari un endpoint di calcolo del punteggio e un token
di autorizzazione. Per ulteriori informazioni, vedi
   Distribuzione di modelli online.
9. Sperimenta l'applicazione Node.js di esempio, ubicata nella seguente posizione:
   https://github.com/pmservice/product-line-prediction.

   Per distribuire automaticamente il codice dell'applicazione di esempio al tuo
   spazio {{site.data.keyword.Bluemix_short}}, vai alla scheda Esempi e, nella sezione
   Applicazioni di esempio, seleziona l'applicazione Previsione linea di
   prodotti e distribuiscila facendo clic sull'icona (+).
   Se richiesto, effettua l'autenticazione nel modulo DeployToBluemix.

   Dovresti ora vedere l'applicazione di esempio sul Dashboard {{site.data.keyword.Bluemix_short}}
   nella sezione Tutte le applicazioni.

10. Fai clic sull'applicazione per vederne i dettagli. Qui puoi modificare i dettagli dell'applicazione, come ad esempio il
numero di istanze o l'assegnazione di memoria.
11. Esegui il bind della tua applicazione con l'istanza {{site.data.keyword.pm_short}}. Sulla scheda
**Connessioni**, fai clic su **Connetti esistente**.
    Al termine della
ripreparazione, la tua applicazione viene connessa all'istanza del servizio.
12. Esegui l'applicazione utilizzando l'indirizzo della rotta. 
13. Nell'applicazione, seleziona la tua distribuzione Previsione linea di prodotti. Sei ora pronto a utilizzare
i record di esempio e i risultati di previsione. 
    
## Ulteriori informazioni

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli IBM® SPSS®](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) o [API del
servizio per i modelli IBM® SPSS®](pm_service_api_spss.html).

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
