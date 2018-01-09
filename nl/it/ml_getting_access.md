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

# Configurazione del tuo ambiente

Per utilizzare {{site.data.keyword.pm_full}}, devi poter creare l'ambiente machine learning corretto e richiamare le credenziali specifiche per tale ambiente. Puoi creare l'istanza {{site.data.keyword.Bluemix}} utilizzando l'[interfaccia riga di comando Cloud Foundry](https://github.com/cloudfoundry/cli#getting-started) o tramite l'interfaccia utente grafica nel dashboard {{site.data.keyword.Bluemix}}.
{: shortdesc}

## Utilizzo del dashboard IBM Cloud

Per utilizzare {{site.data.keyword.pm_full}}, devi [creare un'istanza](https://console.bluemix.net/catalog/services/machine-learning) nel tuo dashboard {{site.data.keyword.Bluemix_notm}}.

A seconda del tuo scenario, puoi anche utilizzare le seguenti risorse:

- Object Storage (distribuzione batch)
- Db2 Warehouse on Cloud (distribuzione batch)
- Apache Spark (distribuzione flusso, batch e sistema di apprendimento continuo)
- Message Hub distribuzione flusso)

Per vedere come utilizzare il dashboard per creare le istanze {{site.data.keyword.Bluemix_notm}} e visualizzare le credenziali, guarda il seguente video, che integra la procedura scritta, che segue il video.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Se non puoi accedere al video incorporato in questa pagina, puoi accedervi dal sito web di YouTube. (Si apre in una nuova finestra o scheda)">    <img src="images/video.png" alt="Icona video"></a>IBM Watson Machine Learning: Introduzione</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Questo video fornisce una panoramica su come configurare il tuo ambiente machine learning.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## Creazione delle istanze {{site.data.keyword.Bluemix_notm}}

Per creare l'istanza {{site.data.keyword.pm_full}}, devi eseguire la seguente procedura:

1. Apri la {{site.data.keyword.pm_full}} pagina del servizio [](https://console.bluemix.net/catalog/services/machine-learning).
2. Registrati o accedi per creare l'istanza del servizio.
3. Immetti un nome descrittivo per la tua istanza, scegli uno spazio e seleziona il tuo piano di dati. 
4. Fai clic su **Create Instance**.

Viene aperta la tua istanza.

## Richiamo delle tue credenziali

Per utilizzare la tua istanza {{site.data.keyword.Bluemix_notm}} (precedentemente conosciuta come Bluemix), hai bisogno delle credenziali dell'istanza del servizio. Puoi creare e accedere alle tue credenziali tramite la [CLI CF](using_pm_service.html) o il dashboard {{site.data.keyword.Bluemix_notm}}. Dopo che hai eseguito il bind dell'istanza del servizio {{site.data.keyword.pm_short}} alla tua applicazione {{site.data.keyword.Bluemix_notm}} le credenziali {{site.data.keyword.pm_short}} vengono aggiunte alla variabile di ambiente `VCAP_SERVICES`. Per ulteriori informazioni, vedi [utilizzo del servizio con l'applicazione](using_pm_service.html).

Per richiamare le credenziali dell'istanza {{site.data.keyword.pm_full}}, devi eseguire la seguente procedura:

1. [Accedi a {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Dalla sezione **Services**, fai clic sul nome del servizio per cui desideri richiamare tali credenziali.
3. Nel pannello di navigazione, fai clic su **Service credentials**.
4. Nell'elenco delle credenziali, fai clic su **View credentials**.
5. Se non esistono credenziali per questo servizio, creale facendo clic su **Create credentials**. 
6. Per copiare le credenziali, fai clic sull'icona di copia.

Per utilizzare il servizio {{site.data.keyword.pm_short}} nella tua applicazione, devi associarlo all'applicazione {{site.data.keyword.Bluemix_notm}} e le credenziali necessarie per eseguire l'applicazione vengono inserite nella variabile di ambiente `VCAP_SERVICES`. Per ulteriori informazioni, vedi [Interfaccia riga di comando Cloud Foundry](#cloud-foundry-command-line-interface).

## Utilizzo della CLI Cloud Foundry (interfaccia riga di comando)

### Procedura per eseguire il bind del servizio all'applicazione {{site.data.keyword.Bluemix_notm}} 

Puoi scaricare il [codice di esempio](https://github.com/pmservice/product-line-prediction/blob/master/README.md) Node.js per provare il servizio Machine
Learning. Completare la seguente procedura per creare la tua applicazione {{site.data.keyword.Bluemix_notm}} ed eseguire il bind del servizio {{site.data.keyword.pm_short}}. Questi esempi utilizzano Node.js perché è un runtime di uso comune. È possibile utilizzarne degli altri come iOS, Ruby, Perl o Java. 

Puoi anche effettuare i passi da 1 a 3 utilizzando l'interfaccia grafica {{site.data.keyword.Bluemix_notm}} al posto dello strumento Cloud Foundry (cf).

1. Utilizza il comando `cf create-service` per creare un'istanza del servizio:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Ad esempio, il seguente comando crea un'istanza del servizio {{site.data.keyword.pm_short}}
   con il piano lite denominato `my_wml_lite` nel tuo spazio {{site.data.keyword.Bluemix_notm}}:

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Utilizza il comando `cf create-service-key` per creare le credenziali del servizio:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Ad esempio, il seguente comando crea le credenziali del servizio {{site.data.keyword.pm_short}}:

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. Utilizza il comando `cf bind-service` per eseguire il bind dell'istanza del servizio
   `my_wml_lite` alla tua applicazione.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   Ad esempio, il seguente comando associa l'istanza del servizio {{site.data.keyword.pm_short}}
   `my_wml_lite` all'applicazione {{site.data.keyword.Bluemix_notm}} `my_app1`:

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Accesso alle credenziali tramite la variabile `VCAP_SERVICES` 

Dopo che hai eseguito il bind dell'istanza del servizio {{site.data.keyword.pm_short}} alla tua applicazione
{{site.data.keyword.Bluemix}}, le credenziali {{site.data.keyword.pm_short}} vengono aggiunte alla variabile di ambiente `VCAP_SERVICES`.

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   La variabile di ambiente `VCAP_SERVICES` include le seguenti informazioni:

<dl>
<dt>piano</dt>
<dd>il piano {{site.data.keyword.pm_short}} utilizzato nel provisioning del servizio.</dd>
<dt>url</dt><dd>l'indirizzo dell'istanza del servizio {{site.data.keyword.pm_short}}. <dt>access_key</dt><dd>solo per i flussi IBM® SPSS® Modeler.</dd>
<dt>instance_id</dt><dd>identificativo univoco dell'istanza {{site.data.keyword.pm_short}}.</dd>
<dt>username</dt><dd>il nome utente che è parte dell'autorizzazione di base necessaria per generare un token di accesso per passare tutte le richieste a questa istanza del servizio.</dd>
<dt>password</dt><dd>la password che è parte dell'autorizzazione di base necessaria per generare un token di accesso per passare tutte le richieste a questa istanza del servizio. </dd>
</dl>

Ad esempio, puoi generare un token di accesso utilizzando il seguente comando `curl`:

Esempio di
richiesta:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Esempio di
output:

       {"token":"**********"}
```
{: codeblock}

   Il seguente codice Node.js è un esempio di come ottenere i valori
   `username` e `password` dalla variabile di ambiente `VCAP_SERVICES`: 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
