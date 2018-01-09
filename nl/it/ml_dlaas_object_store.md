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

# Configura un archivio dell'oggetto per l'apprendimento approfondito

Per utilizzare il servizio {{site.data.keyword.pm_full}} e gli esperimenti di apprendimento approfondito che sono parte di questo servizio, devi avere accesso a un'istanza Cloud Object Storage (control.softlayer.com) o Object Storage OpenStack Swift (console.bluemix.net).  Devi definire i bucket di Cloud Object Storage per fornire i dati di formazione e per archiviare i log e il modello eseguito e puoi utilizzare istanze separate o le stesse di Cloud Object Storage a tale scopo.
{: shortdesc}

## Creazione di un'istanza di Cloud Object Storage

1. Vai alla pagina [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) e seleziona una delle seguenti opzioni:

   - Archiviazione non crittografata gratuita (Lite): [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - Servizio di crittografia non gratuito: [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. Configura la tua istanza di Object Storage e fai clic su **Create**.
3. Dopo aver creato il servizio, nel menu laterale, fai clic su **Service Credentials** per ottenere le credenziali, come l'URL API, il nome utente e la password per accedere all'istanza di Object Storage.

## Accesso all'istanza di Cloud Object Storage (IaaS)

Il servizio Cloud Object Storage (IaaS) fornisce API compatibili S3 ed è possibile accedervi utilizzando qualsiasi applicazione client compatibile.

## CLI (command line interface) AWS (Amazon Web Services)

1. Installa la [CLI AWS](https://aws.amazon.com/cli/) utilizzando `pip install awscli`
2. Configura le credenziali per Cloud Object Storage (IaaS) nel tuo file delle credenziali AWS (per linux, ubicato in ~/.aws/credentials).

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

Puoi utilizzare la CLI AWS per elencare e aggiornare i file e i bucket, specificando il precedente profilo e l'endpoint Cloud Object Storage dalle tue credenziali.  Ad esempio, per elencare i bucket in un'istanza, immetti il seguente comando:

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Accesso a Object Storage OpenStack Swift per l'istanza Bluemix

Puoi utilizzare le librerie client o la CLI swift (ad esempio la libreria client python swift per accedere a questo objectstore).  Per ulteriori dettagli fai riferimento alla [documentazione](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## Formattazione dati

Framework differenti richiedono la formazione e la verifica dei dataset in formati diversi. Ad esempio, Caffe richiede i dataset nel formato LevelDB o LMDB mentre Torch nel suo formato proprietario. Presupponiamo che i dati sono già nel formato necessario al framework specificato. I dettagli su come convertire i dati non elaborati nel formato specifico del framework è al di fuori dello scopo di questa documentazione. Consulta la documentazione per il framework di apprendimento approfondito se desideri avere ulteriori informazioni.
