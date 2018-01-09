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

# Politique de quotas

Le moteur {{site.data.keyword.pm_full}} applique des quotas par instance afin de garantir une allocation et une utilisation adéquates des ressources. Ces politiques varient en fonction
des profils de compte, de l'historique d'utilisation du service et de la disponibilité des ressources. La politique des quotas décrit les limites qui ne sont pas définies par le barème de prix
et qui sont **susceptibles d'être modifiés sans préavis**. Les limites de quota de service suivantes sont actives actuellement.
{: shortdesc}

## Utilisation des ressources

Les limites maximum des ressources sont les suivantes :

-  **Nombre maximal de modèles publiés** : 200 (forfait Lite) 1 000 (forfaits standard & professionnel)
-  **Taille maximale du modèle Spark ML** : 200 Mo
-  **Nombre maximal de modèles déployés** : 1 000 (forfaits standard & professionnel)

**Remarque** : Pour comprendre les limites du forfait Lite, consultez la description du [Plan Lite](https://console.bluemix.net/catalog/services/machine-learning) dans le [catalogue {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/catalog/).

## Limites des demandes de service

Le nombre de demandes que vous êtes autorisé à effectuer par minute dans certaines API est limité. Si votre application nécessite des limites plus élevées, [contactez le support {{site.data.keyword.Bluemix_notm}}](https://support.ng.bluemix.net/) pour augmenter votre quota.

## Demandes de déploiement

Les limites suivantes sont applicables aux demandes d'API de déploiement :

-  **Période** : 60 secondes
-  **Limite par défaut** : 10

## Demandes de prévision en ligne

Les limites suivantes s'appliquent aux demandes de [prévisions en ligne](pm_service_api_spark_building.html) :

-  **Période** : 60 secondes
-  **Limite par défaut** : 500

## Demandes de gestion des ressources

Les limites suivantes s'appliquent aux demandes totales combinées de cette liste :

[Jeton](https://watson-ml-api.mybluemix.net/#/Token) : requêtes post, get, delete, put, patch

[Déploiements](https://watson-ml-api.mybluemix.net/#/Deployments): requêtes post, get, delete, put, patch

-  **Période** : 60 secondes
-  **Limite par défaut** : 100

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM® Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
