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

# Déploiement de modèles de traitement par lots

Grâce au service {{site.data.keyword.pm_full}}, vous pouvez déployer un modèle et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.
{: shortdesc}


**Nom du scénario **: Pronostic de satisfaction des clients.

**Description du scénario **: Une entreprise de télécommunications désire savoir quels clients risquent de les abandonner. Le modèle présenté pronostique une attrition de la clientèle. Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

## Conditions requises

Pour pouvoir utiliser cet exemple, vous avez besoin des services suivants :

* Des informations détaillées sur l'instance [Object Storage](https://console.bluemix.net/catalog/services/object-storage), qui seront utilisées comme entrées (données client auxquelles attribuer un score) pour le modèle et lieu de stockage de la sortie du modèle. Téléchargez l'exemple de fichier .csv de données d'entrée [ici](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv). Ajoutez le fichier d'entrée dans votre instance Object Storage.
* Des données d'identification de l'instance de service [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilisez [ce lien](https://console.bluemix.net/catalog/services/apache-spark) pour en créer une.


## Utilisation de l'exemple de modèle

1.  Accédez à l'onglet Exemples du tableau de bord {{site.data.keyword.pm_full}}.
2.  Dans la section Exemples de modèles, recherchez la vignette Pronostic de satisfaction des clients et cliquez sur l'icône Ajouter le modèle (+).

L'exemple de modèle Pronostic de satisfaction des clients s'affiche dans la liste des modèles disponibles sous l'onglet Modèles.

## Création d'un déploiement par lots à l'aide d'Object Storage

1.  Accédez à l'onglet Modèles du tableau de bord {{site.data.keyword.pm_full}}.
2.  Dans le menu **Actions**, cliquez sur **Créer un déploiement**.
3.  Dans le formulaire Créer un déploiement, renseignez les zones Nom, Description et Type de lot.
4.  Vous devez fournir les informations suivantes :

    **Connexion d'entrée** : Informations sur Object Storage qui seront utilisées comme entrées (données client auxquelles attribuer un score) pour le modèle et lieu de stockage pour la sortie du modèle (results.csv, en l'occurrence, qui est créé automatiquement).

    ```
       {
          "source":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Connexion de sortie**

    ```
       {
          "target":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Connexion Spark** : Les données d'identification du service Spark sont disponibles sous l'onglet Données d'identification pour le service du tableau de bord du service {{site.data.keyword.Bluemix_short}} Spark.

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

5.  Cliquez sur **Sauvegarder**.

Le résultat du pronostic est enregistré dans un fichier .csv dans IBM Object Storage. Un exemple de lignes figure ci-dessous.

Aperçu du fichier d'entrée :

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Output file preview:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## Obtention des détails du déploiement

Vous pouvez vérifier le statut et les paramètres associés au modèle déployé.

1. Accédez à l'onglet Déploiements du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans le menu **Actions**, cliquez sur **Afficher les détails**.

## Suppression d'un déploiement par lots

Si vous n'en avez plus besoin, vous pouvez supprimer le déploiement à l'aide d'une requête similaire à l'exemple suivant.

1. Accédez à l'onglet Déploiements du tableau de bord {{site.data.keyword.pm_full}}.
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
