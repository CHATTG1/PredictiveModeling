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

# Bibliothèques client

## Bibliothèque client d'API commune - <span class='tag--beta'>Bêta</span>

Pour utiliser le service IBM Watson Machine Learning avec le langage de programmation Python, exécutez PyPI pour installer la bibliothèque client `watson-machine-learning-client`.

Pour en savoir plus sur la manière de procéder, consultez les notebooks des rubriques [Spark MLlib notebook](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) ou [scikit-learn notebook ](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a) illustrant les techniques ci-dessous :

* Persistance de modèle
* Déploiement en ligne
* Evaluation à l'aide de l'API commune

La documentation relative à la bibliothèque d'API commune est accessible [ici](http://wml-api-pyclient.mybluemix.net/).

### Restrictions

* Le client d'API commune est en version **bêta**.
* Avant de pouvoir utiliser le client d'API commune, installez au moins l'un des packages suivants : XGboost, scikit-learn ou pyspark.
* Seul Python 3 est pris en charge.

## Bibliothèque client d'API de référentiel

Des encapsuleurs de bibliothèque client pour les API REST de référentiel du service Machine Learning sont développés en langage de programmation Python et Scala.

Pour plus d'informations sur l'utilisation de bibliothèques client de type `repository` afin de présenter la persistance de modèle au référentiel Watson Machine Learning, consultez les [exemples de notebooks](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7).

Pour plus d'informations sur les bibliothèques de référentiel, voir les rubriques ci-dessous :

* [Scala Repository Module documentation](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python Repository Module documentation](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### Restrictions

Les bibliothèques de référentiel sont disponibles uniquement lorsque vous utilisez [IBM Data Science Experience](https://datascience.ibm.com).
