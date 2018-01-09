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

# Développement et persistance du modèle personnalisé

Grâce au service {{site.data.keyword.pm_full}}, vous pouvez déployer un modèle et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.
{: shortdesc}

## Utilisation de modèles personnalisés

### Nom du scénario : Pronostic de ligne de produits.

### Description du scénario :

Notre client gère l'une des chaînes de magasins les plus connues en Europe. Il souhaite que nous identifions les intérêts de ses clients en ce qui concerne leurs lignes de produits, par exemple des accessoires personnels, du matériel de camping ou des vêtements de plein air.
Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

### Développement et persistance du modèle personnalisé

#### Utilisation de Data Science Experience

Utilisez [Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience) pour créer des modèles personnalisés. Une fois que vous êtes inscrit, connectez-vous pour pouvoir exécuter la procédure ci-dessous.

1. Créez une organisation et un espace. Ces informations sont requises lors de votre toute première connexion. Cliquez sur **Continuer** pour accepter les valeurs par défaut.
2. Après avoir créé l'organisation, accédez à **Projets** et cliquez sur **Nouveau projet**.
3. Indiquez un nom et une description pour votre projet et cliquez sur **Créer**. Le nom de projet que vous avez indiqué est utilisé comme nom de conteneur cible.
4. Une fois le projet créé, vous pouvez effectuer n'importe laquelle des tâches suivantes :
   
   *  **ajouter des notebooks** et commencer à développer vos propres modèles, ou bien télécharger l'un des exemples de notebooks suivants :
        *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7) (Développement de modèles Spark MLlib avec Python)
        *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf) (Développement de modèles Spark MLlib avec Scala)
        *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e) (Développement de modèles scikit-learn avec Python)
   * **ajouter des modèles** et commencer à développer vos propres modèles à l'aide de l'assistant de modèles.


#### Utilisation de l'environnement local

Vous pouvez également faire appel à l'environnement de votre choix pour développer des modèles à des fins ultérieures de publication, de déploiement et d'évaluation à l'aide de la [bibliothèque client d'API commune]() Watson Machine Learning, disponible sur pypi.
Pour en savoir plus sur la bibliothèque client, consultez l'exemple de [notebook](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125) ainsi que la [documentation](pm_service_client_library.html).

**Remarque :** La bibliothèque client d'API commune est en version **bêta**.

### Déploiement et évaluation du modèle personnalisé

Vous pouvez vous servir de l'API pour déployer et évaluer des modèles en ligne, par lots et en flux.

*  [Déploiement de modèles en ligne](pm_service_api_spark_online.html)
*  [Déploiement de modèles de traitement par lots](pm_service_api_spark_batch.html)
*  [Déploiement de modèles de données en flux](pm_service_api_spark_streaming.html)

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM® Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
