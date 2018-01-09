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

# Système d'apprentissage continu

Le service d'apprentissage continu d'{{site.data.keyword.pm_full}} permet de surveiller les performances du modèle de manière automatique, et d'effectuer un réapprentissage et un redéploiement afin de garantir la qualité des prévisions.
{: shortdesc}

**Scenario name** : Meilleur médicament pour la sélection de traitement du coeur.

**Scenario description** : Une société biomédicale qui produit des médicaments pour le coeur souhaite déployer un modèle qui sélectionne le meilleur médicament pour
le traitement du coeur. Lorsque de nouvelles informations sont collectées sur l'efficacité d'un médicament, une mise à jour du modèle doit être envisagée. Un spécialiste des données prépare un
modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle, à définir la configuration d'apprentissage et d'exécuter l'itération d'apprentissage afin d'évaluer, de ré-entraîner et de redéployer le modèle.

## Conditions requises

Pour pouvoir utiliser cet exemple, vous avez besoin des services suivants :

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) à des fins de stockage des données de commentaires
* Le service [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark)

   Cliquez sur ce [lien](https://console.bluemix.net/catalog/services/apache-spark) pour créer une instance Apache Spark si vous n'en possédez pas déjà une.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet **Exemples** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans la section **Exemples de modèles**, recherchez la vignette **Sélection de médicaments pour le coeur** et cliquez sur l'icône **Ajouter le modèle** (+).

L'exemple de modèle Sélection de médicaments pour le coeur s'affiche dans la liste des modèles disponibles sous l'onglet **Modèles**.


## Définissez le système d'apprentissage continu pour le modèle publié

1.  Accédez à l'onglet **Modèles** du tableau de bord {{site.data.keyword.pm_full}}.
2.  Dans le menu **Actions**, cliquez sur **View Details**.
3.  Faites défiler la liste jusqu'à **Retraining Details** et appuyez sur **Edit**.
4.  Vous devez fournir les informations suivantes :

    **Feedback Connection** : Informations relatives à {{site.data.keyword.dashdbshort}} qui sont utilisées comme entrées (données de commentaires) pour l'évaluation et le réapprentissage du modèle.

    ```
    {
        "connection":{
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
   "source":{
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Utilisez les instructions suivantes pour préparer la table `DRUG_FEEDBACK_DATA` dans {{site.data.keyword.dashdbshort}}.
    
    - Créez une instance [{{site.data.keyword.dashdbshort}} Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (un plan d'entrée est offert).
    - Créez la table **DRUG_FEEDBACK_DATA** dans **{{site.data.keyword.dashdbshort_notm}}**.
      + Téléchargez le fichier [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) depuis le référentiel GitHub.
      + Cliquez sur l'icône permettant d'**ouvrir la console** pour vous initier à **{{site.data.keyword.dashdbshort_notm}}**.
      + Cliquez sur **Load Data** et sélectionnez le type de chargement **Desktop**.
      + **Faites-glisser** le fichier précédemment téléchargé vers la zone de chargement et cliquez sur **Next**.
      + Sélectionnez **Schema** pour importer les données et cliquez sur **New Table**.
      + Attribuez un nom à la nouvelle table et cliquez sur **Next**.
      + Utilisez un point-virgule (;) comme **séparateur de zone**.
      + Cliquez sur **Next** pour créer une table avec les données téléchargées.

     **Remarque** : Vous pouvez ajouter de nouveaux enregistrements de commentaires à la base de données des commentaires en utilisant le [noeud final d'API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback).

     **Minimal Data Size** : Nombre minimal d'enregistrements requis dans le jeu de données de commentaires pour commencer l'évaluation du modèle (partie de l'itération du système d'apprentissage).

     ```
     20
     ```
     {: codeblock}

     **Spark Connection** : Les données d'identification du service Spark sont disponibles sous l'onglet **Données d'identification pour le service** du tableau de bord du service {{site.data.keyword.Bluemix_notm}} Spark.

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

     **Seuils** : La valeur de seuil qui déclenche le processus de réapprentissage. Si la mesure, par exemple la valeur de précision calculée pendant l'évaluation du modèle, est inférieure à la valeur de seuil, le réapprentissage est déclenché.

     ```
     0.8
     ```

     **Automatically deploy the retrained model** : Déploiement du modèle à nouveau formé si la mesure de celui-ci est meilleure mesure que celle du modèle déployé.
5.  Cliquez sur **Set up**.

## Exécution de l'itération du système d'apprentissage continu

Un modèle publié est évalué de manière itérative. Si la mesure, par exemple la valeur de précision calculée pendant l'évaluation du modèle, est inférieure à la valeur de seuil, le réapprentissage est déclenché.

     Les jeux de données d'apprentissage et de commentaires sont tous deux utilisés pour ce processus itératif de réapprentissage de d'évaluation.

1. Pour démarrer une nouvelle itération, sur le formulaire **Details** du modèle, sur l'onglet **Monitor**, cliquez sur **Evaluate**.
3. Pour vérifier le résultat de l'itération, consultez soit la table Evaluation Events ou la vue graphique. 

   Vous pouvez visualiser des informations sur la qualité du modèle qui est calculée d'après les données de commentaires (surveillance). Vous pouvez aussi afficher des informations sur la qualité du modèle à nouveau formé, qui correspond à la nouvelle version du modèle qui a été créé et évalué.

4. Pour afficher la liste des modèles formés automatiquement et leur qualité, cliquez sur **View Details** > **Overview**, et accédez à la section **Version History**.

## Magasin de données de commentaires

Vous pouvez faire appel à un [noeud final de commentaires](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) pour envoyer de nouveaux enregistrements au magasin de commentaires que vous avez défini dans la configuration d'apprentissage automatique. {{site.data.keyword.dashdbshort}} est actuellement pris en charge comme magasin de données de commentaires. Si la table de commentaires n'existe pas, le service la crée. Si elle existe, le schéma est vérifié pour voir s'il correspond à celui de la table d'apprentissage et une colonne supplémentaire nommée `_training` est ajoutée à la table. La colonne `_training` est utilisée pour signaler les enregistrements qui sont utilisés dans le cadre du processus de réapprentissage.

Pour obtenir des informations supplémentaires et des exemples sur un magasin de données de commentaires , consultez la [documentation d'API](pm_service_api_spark_learning_system.html).

**Remarque :** La table de commentaires est gérée, modifiée et utilisée par le service {{site.data.keyword.pm_full}}.

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
