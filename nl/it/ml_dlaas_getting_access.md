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

# Configurazione del tuo ambiente machine learning e richiamo delle tue credenziali

Per utilizzare IBM Watson Machine Learning, devi poter creare l'ambiente machine learning corretto e richiamare le credenziali specifiche per tale ambiente.

## Configurazione del tuo ambiente 

Per utilizzare IBM Watson Machine Learning, devi configurare le istanze dei seguenti elementi nel tuo dashboard IBM Cloud: dashboard:

- Watson Machine Learning
- Object Storage
- Apache Spark

In aggiunta alle istanze obbligatorie, alcuni dei tuoi notebook o esercitazioni usano anche le seguenti istanze. Per utilizzare le esercitazioni, devi configurare anche questi servizi.

- Python Flask
- Natural Language Understanding
- Esperimenti di apprendimento approfondito

### Creazione delle istanze di IBM Cloud

Guarda questo video per vedere come creare le istanze di IBM Cloud e visualizzare le credenziali. Quindi, segui le istruzioni dopo il video per configurare il tuo proprio ambiente. Consulta la sezione <a href="#retrieving-your-credentials">Richiamo delle tue credenziali</a> per le istruzioni per visualizzare le tue credenziali.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Se non puoi accedere al video incorporato in questa pagina, puoi accedervi dal sito web di YouTube. (Si apre in una nuova finestra o scheda)">    <img src="images/video.png" alt="Icona video"></a>IBM Watson Machine Learning: Introduzione</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Questo video fornisce una panoramica su come configurare il tuo ambiente machine learning.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Per creare le istanze Watson Data Platform, devi eseguire la seguente procedura:

1. [Accedi a IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Dal pannello di navigazione, fai clic su **Data & Analytics**.

   Visualizzi una schermata centrata sui servizi di dati. Puoi ritornare qui regolarmente per utilizzare i tuoi servizi di dati e analisi da una pagina facile da utilizzare.

3. Fai clic sulla scheda **Analytics** e su **Instances**.
4. Fai clic su **New Instance**.
5. Configura le istanze e l'archivio dati.

   Immetti un nome descrittivo per la tua istanza, scegli uno spazio e seleziona il tuo piano di dati (trovi il confronto tra i piani e i dettagli sui prezzi in questa pagina). IBM Cloud popola automaticamente il nome dell'archivio dell'oggetto con il tuo nome dell'istanza. È probabile che avrai bisogno dell'archivio dati per il tuo progetto Spark, per cui scegli qui anche uno spazio e un piano.

6. Fai clic su **Create Instance**.

Viene aperta la tua istanza.

### Aggiunta di istanze Bluemix esistenti a un progetto in Data Science Experience

Guarda questo video per vedere come creare un progetto e configuralo ad utilizzare Watson Machine Learning.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Figura 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="Se non puoi accedere al video incorporato in questa pagina, puoi accedervi dal sito web di YouTube. (Si apre in una nuova finestra o scheda)">    <img src="images/video.png" alt="Video icon"></a>IBM Watson Machine Learning: Crea un progetto per Watson Machine Learning</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>Questo video fornisce una panoramica su come configurare il tuo ambiente machine learning.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Se già disponi di istanze, ma non le hai collegate a un progetto in Data Science Experience, devi eseguire la seguente procedura:

1. [Accedi a IBM Data Science Experience](https://datascience.ibm.com).
2. Fai clic su **Projects** > **View All Projects**.
3. Fai clic sulla scheda **Settings**.
4. Per aggiungere un servizio nel pannello **Associated Services**, fai clic su **add associated service**, seleziona un servizio, completa le informazioni sulla configurazione e fai clic su **Save**.
5. Per aggiungere un token di accesso, esegui la seguente procedura:
   6. Nel pannello **Access Tokens**, fai clic su **create new token**.
   7. Nella casella **Name**, immetti un nome.
   8. Nella casella **Access Role For Project**, seleziona un ruolo, **Viewer** o **Editor**.
   9. Fai clic su **Add**.

6. Per collegarti a un repository GitHub, in **Repository URL** immetti l'ubicazione del tuo repository. Se non disponi di un token di accesso personale già configurato per l'accesso GitHub, devi crearlo ora facendo clic su **Settings**. Quando hai finito, fai clic su **Add**.


## Richiamo delle tue credenziali

Per utilizzare i tuoi modelli, servizi e istanze Bluemix nei notebook e nelle applicazioni, devi poter inserire le credenziali, i token e gli endpoint di calcolo nel codice utilizzato per l'elaborazione.

1. [Accedi a Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Scorri fino alla sezione **Services** e fai clic sul nome del servizio per cui desideri richiamare tali credenziali.
3. Nel pannello di navigazione, fai clic su **Service credentials**.
4. Nell'elenco delle credenziali, fai clic su **View credentials**.
5. Se non esistono credenziali per questo servizio, puoi crearle facendo clic su **Create credentials**.
6. Per copiare le credenziali, fai clic sull'icona di copia.

A seconda del servizio, potresti visualizzare campi diversi, come `username` e `password`. Devi copiare i valori così come sono visualizzati per utilizzarli nel tuo codice.

## Ulteriori informazioni

[A quick deep learning tutorial](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

