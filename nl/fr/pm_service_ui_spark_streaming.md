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

# Déploiement de modèles de données en flux

Le service {{site.data.keyword.pm_full}} permet de déployer le modèle et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle en flux déployé.
{: shortdesc}

**Nom du scénario **: analyse d'opinions.

**Description du scénario **: une agence de marketing désire connaître les opinions sur un sujet spécifique. L'agence nous demande de développer un modèle qui classifie une instruction comme étant POSITIVE ou NEGATIVE. Un spécialiste des données prépare un
modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

Voir ce [document](https://github.com/pmservice/tweet-sentiment-prediction) pour plus d'informations.

## Conditions requises

Pour pouvoir utiliser cet exemple, vous devez disposer des ressources suivantes :

* Des informations de la rubrique [Message Hub](https://console.bluemix.net/catalog/services/message-hub) qui seront utilisées comme entrées (tweets) pour le modèle et lieu de stockage pour la sortie du modèle (résultats de la prévision). Assurez-vous que deux rubriques soient créées : une rubrique d'entrée avec des tweets et une rubrique de sortie.
* Des données d'identification de l'instance de service [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilisez [ce lien](https://console.bluemix.net/catalog/services/apache-spark) pour en créer une.


## Utilisation de l'exemple de modèle

1. Accédez à l'onglet **Exemples** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans la section **Exemples de modèles**, recherchez la vignette **Pronostics des opinions** et cliquez sur l'icône Ajouter le modèle (+).

L'exemple de modèle Pronostics des opinions s'affiche dans la liste des modèles disponibles sous l'onglet Modèles.


## Création d'un déploiement en flux à l'aide d'IBM Message Hub

1.  Accédez à l'onglet **Modèles** du tableau de bord {{site.data.keyword.pm_full}}.
2.  Dans le menu **Actions**, cliquez sur **Créer un déploiement**.
3.  Dans le formulaire **Créer un déploiement**, renseignez les zones **Nom**, **Description** et **Type de flux**.
4.  Vous devez fournir les informations suivantes :

    **Connexion d'entrée** : Informations de la rubrique IBM Message Hub qui sont utilisées comme entrées (tweets) pour le modèle et lieu de stockage pour la sortie du modèle (résultats de la prévision).

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
   "source":{
        "topic":"sinput",
         "type":"kafka"
      }
  }
    ```
    {: codeblock}

    **Connexion de sortie**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
   "target":{
       "topic":"soutput",
         "type":"kafka"
      }
 }
    ```
    {: codeblock}

    **Connexion Spark** : Les données d'identification du service Spark sont disponibles sous l'onglet Données d'identification pour le service du tableau de bord du service {{site.data.keyword.Bluemix_notm}} Spark.

     ```
{
     "credentials": {
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "cluster_master_url": "https://spark.bluemix.net",
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
      "plan": "ibm.SparkService.PayGoPersonal"
     },
         "version":"2.0"
}
     ```
     {: codeblock}

5. Cliquez sur **Sauvegarder**.

Les résultats de prévision sont envoyés à une rubrique MessageHub définie (connexion de sortie).

## Obtention des détails de déploiement

Vous pouvez vérifier le statut et les paramètres associés au modèle déployé.

1. Accédez à l'onglet **Déploiements** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans le menu **Actions**, cliquez sur **Afficher les détails**.

## Suppression d'un déploiement en flux

Si vous n'en avez plus besoin, vous pouvez supprimer le déploiement en exécutant une requête similaire à l'exemple suivant.

1. Accédez à l'onglet **Déploiements** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans le menu **Actions**, cliquez sur **Supprimer**.

## En savoir plus

Prêt à commencer ? Pour créer une instance de service ou lier
une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou
[Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API
de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).
Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation
qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
