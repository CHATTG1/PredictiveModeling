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

# API REST

Le service {{site.data.keyword.pm_short}} est un ensemble d'API REST que vous pouvez appeler depuis n'importe quel langage de programmation et qui permet l'intégration d'analyses Data Science
Experience dans vos applications. Liez vos applications {{site.data.keyword.Bluemix_short}} à l'instance de service {{site.data.keyword.pm_short}} et générez les analyses prédictives dont vos applications ont besoin pour offrir aux utilisateurs une meilleure valeur ajoutée. Gérez
des modèles dans le [tableau de bord d'administration](pm_service_ui_spark.html) et utilisez ce dernier pour créer des déploiements en ligne, par lots ou en flux intégrés à vos
applications.
{: shortdesc}

Développez des applications basées sur des modèles Spark, Python et IBM® SPSS® déployés sur une instance de service grâce aux puissantes API REST [](https://watson-ml-api.mybluemix.net/) :

*  Extrayez les métadonnées d'un modèle prédictif spécifique
*  Déployez des modèles et gérez les modèles déployés
    *  Déploiement en ligne (évaluation)
    *  Déploiement par lots (compatible avec IBM Cloud Object Storage et {{site.data.keyword.dashdbshort}})
    *  Déploiement en flux (compatible avec IBM Cloud Message Hub)
*  Extrayez les métadonnées d'un déploiement spécifique
*  Surveillez et formez à nouveau les modèles déployés en vous appuyant sur le système d'apprentissage continu
*  Générez des analyses prédictives en soumettant des requêtes de score aux modèles déployés

Pour plus d'informations, consultez les exemples d'utilisation d'API REST suivants :

*  [Déploiement de modèles en ligne](pm_service_api_spark_online.html)
*  [Evaluation de modèles en ligne](pm_service_api_develop_score.html)
*  [Déploiement de modèles par lots](pm_service_api_spark_batch.html)
*  [Déploiement de modèles en flux](pm_service_api_spark_streaming.html)
*  [Système d'apprentissage continu](pm_service_api_spark_learning_system.html)

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
