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

# Librerie client

## Libreria client API comune <span class='tag--beta'>Beta</span>

Per utilizzare il servizio IBM Watson Machine Learning con il linguaggio di programmazione Python, puoi utilizzare PyPI per installare la libreria client `watson-machine-learning-client`.

Per ulteriori dettagli su come farlo, consulta i notebook di esempio [Spark MLlib notebook](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) o [scikit-learn notebook ](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a) che illustrano le seguenti tecniche:

* persistenza del modello 
* distribuzione online
* calcolo utilizzando il client API comune

La documentazione per la libreria API comune è disponibile [qui](http://wml-api-pyclient.mybluemix.net/).

### Limitazioni

* Il client API comune è nella **beta**.
* Prima di poter utilizzare il client API comune, devi installare almeno uno dei seguenti pacchetti: XGboost, scikit-learn o pyspark.
* È supportato solo Python 3.

## Libreria client API del repository

I wrapper della libreria client per le API REST del repository del servizio Machine Learning sono distribuiti nei linguaggi di programmazione Python e Scala.

Per ulteriori informazioni sull'utilizzo delle librerie client del `repository` per fornire la persistenza del modello al repository di Watson Machine Learning, consulta i [notebook di esempio](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7).

Per ulteriori informazioni sulle librerie del repository, consulta i seguenti argomenti:

* [Documentazione del modulo del repository Scala](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Documentazione del modulo del repository Python](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### Limitazioni

Le librerie del repository sono disponibili solo quando utilizzi [IBM Data Science Experience](https://datascience.ibm.com).
