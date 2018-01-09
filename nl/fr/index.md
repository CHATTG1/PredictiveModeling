---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} est un service IBM Cloud qui permet aux utilisateurs d'effectuer deux opérations essentielles de l'apprentissage automatique, à savoir l'apprentissage et l'évaluation.
{: shortdesc}

- L'**apprentissage** est le processus d'affinement des algorithmes à des fins d'apprentissage automatique à partir d'un jeu de données. Le résultat de cette opération s'appelle un modèle. Un modèle englobe les coefficients d'expressions mathématiques qui ont été appris.
- L'**évaluation** est l'opération de prévision d'un résultat à partir d'un modèle formé. Le résultat de l'opération d'évaluation correspond à un autre jeu de données comportant les valeurs prédites.

{{site.data.keyword.pm_full}} a été conçu pour répondre plus particulièrement aux besoins de deux catégories de personnes :

- Les analystes scientifiques : Ils créent des pipelines d'apprentissage automatique permettant d'optimiser les transformations de données et les algorithmes d'apprentissage automatique. Ils utilisent en général des notebooks ou des outils externes pour former et évaluer leurs modèles. Les analystes scientifiques collaborent souvent avec les ingénieurs en traitement de données afin d'explorer et de comprendre les données.
- Les développeurs : Ils créent des applications intelligentes qui utilisent les prévisions des modèles d'apprentissage automatique.

Bien que la formation soit une étape critique dans le processus d'apprentissage automatique, {{site.data.keyword.pm_full}} vous permet de rationaliser le fonctionnement de vos modèles en les déployant et en obtenant une valeur métier réelle au fil du temps et au fur et à mesure de leurs itérations.

## Conditions requises

Pour utiliser {{site.data.keyword.pm_full}}, à partir du catalogue {{site.data.keyword.Bluemix_short}}, créez l'[instance de service ici](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/). Ceci vous permet d'effectuer les tâches suivantes :

## Procédure

1. [Configuration de votre environnement Machine Learning](ml_getting_access.html)
1. [Création et stockage d'un modèle](pm_custom_models.html)
2. [Déploiement d'un modèle](pm_service_api_spark_online.html)
3. Utilisation du `noeud final d'évaluation` du modèle déployé dans votre application pour [obtenir des prévisions.](pm_service_api_spark_building.html)

## Utilisation de Machine Learning avec Data Science Experience

{{site.data.keyword.pm_full}} est intégré à IBM Data Science Experience. Vous pouvez utiliser les bibliothèques client d'API Machine Learning dans les notebooks Data Science Experience. Vous devez disposer d'une instance Machine Learning pour pouvoir utiliser le générateur de modèle et l'éditeur de flux.

## Utilisation de Machine Learning avec SPSS Modeler

{{site.data.keyword.pm_full}} est intégré à IBM® SPSS® Modeler. Vous pouvez utiliser l'API Machine Learning pour exploiter les algorithmes mathématiques avancés.


## Utilisation de Machine Learning avec votre environnement

{{site.data.keyword.pm_full}} peut être utilisé comme solution hybride en liant votre environnement local au cloud. Vous pouvez utiliser l'API Machine Learning pour publier vos modèles, les déployer et les évaluer. Pour plus d'informations, voir [DSX: Hybrid Mode](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b).

## A propos de Watson Machine Learning

Le service {{site.data.keyword.pm_full}} est un ensemble d'API REST pouvant être appelées à partir de n'importe quel langage de programmation.

La priorité du service {{site.data.keyword.pm_full}} porte sur le déploiement, mais vous pouvez parfaitement utiliser IBM® SPSS® Modeler ou IBM® Data Science Experience pour créer et utiliser des modèles et des pipelines. SPSS® Modeler et Data Science Experience (qui exploitent Spark MLlib et Python scikit-learn) proposent tous deux diverses méthodes de modélisation issues de l'apprentissage automatique, de l'intelligence artificielle et des statistiques.

## Liens connexes

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir
[Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou
[Utilisation du service avec des modèles SPSS](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles SPSS](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
