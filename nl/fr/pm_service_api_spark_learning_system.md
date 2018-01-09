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

Le service {{site.data.keyword.pm_full}} inclut un système d'apprentissage continu. Les systèmes d'apprentissage continu permettent un contrôle automatique des performances du modèle, un réapprentissage et un redéploiement afin de garantir la qualité des prévisions.
{: shortdesc}

**Scenario name** : Meilleur médicament pour la sélection de traitement du coeur.

**Scenario description** : Une société biomédicale qui produit des médicaments pour le coeur souhaite déployer un modèle qui sélectionne le meilleur médicament pour
le traitement du coeur. Lorsque de nouvelles informations sont collectées sur l'efficacité d'un médicament, une mise à jour du modèle doit être envisagée. Un spécialiste des données a développé un modèle prédictif et le partage avec vous (le développeur). Votre tâche est de déployer le modèle, de définir une configuration d'apprentissage et d'exécuter l'itération d'apprentissage pour évaluer, reformer et redéployer le modèle.

**Remarque** : Vous pouvez également vous exercer avec un [exemple de notebook Python](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325) qui crée un exemple de modèle, configure le système d'apprentissage et exécute ensuite l'itération d'apprentissage.

## Conditions requises

Pour pouvoir utiliser les exemples, vous devez disposer des services suivants :

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) à des fins de stockage des données de commentaires
* Des données d'identification de l'instance de service [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilisez [ce lien](https://console.bluemix.net/catalog/services/apache-spark) pour en créer une.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet **Exemples** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans la section **Exemples de modèles**, recherchez la vignette Sélection de médicaments pour le coeur et cliquez sur l'icône **Ajouter le modèle**
(+).

L'exemple de modèle **Sélection de médicaments** s'affiche dans la liste des modèles disponibles sous l'onglet **Modèles**.

## Génération du jeton d'accès

Générez un jeton d'accès à l'aide du nom d'utilisateur et du mot de passe disponibles sur l'onglet Données d'identification pour le service de l'instance de service {{site.data.keyword.pm_full}}.

Exemple de requête :

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Exemple de sortie :

```
{"token":"**********"}
```
{: codeblock}

Utilisez la commande de terminal suivante pour affecter votre valeur de jeton au jeton de la variable d'environnement :

```
token="<token_value>"
```
{: codeblock}

## Utilisation de modèles publiés

Utilisez l'appel API suivant pour obtenir les détails de votre instance, par exemple :

* l'adresse `url` des modèles publiés
* l'adresse `url` des déploiements
* des informations sur l'utilisation

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"360c510b-012c-4793-ae3f-063410081c3e",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e",
      "created_at":"2017-08-04T09:15:48.344Z",
      "modified_at":"2017-08-22T08:34:47.759Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models"
      },
      "usage":{
         "expiration_date":"2017-09-01T00:00:00.000Z",
         "computation_time":{
            "limit":18000,
            "current":4
         },
         "model_count":{
            "limit":200,
            "current":2
         },
         "prediction_count":{
            "limit":5000,
            "current":16
         },
         "deployment_count":{
            "limit":5,
            "current":1
         }
      },
      "plan_id":"0f2a3c2c-456b-40f3-9b19-726d2740b11c",
      "status":"Active",
      "organization_guid":"b0e61605-a82e-4f03-9e9f-2767973c084d",
      "region":"us-south",
      "account":{
         "id":"f52968f3dbbe7b0b53e15743d45e5e90",
         "name":"Umit Cakmak's Account",
         "type":"TRIAL"
      },
      "owner":{
         "ibm_id":"31000292EV",
         "email":"umit.cakmak@pl.ibm.com",
         "user_id":"43e0ee0e-6bfb-48fc-bcd8-d61e40d19253",
         "country_code":"POL",
         "beta_user":true
      },
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/deployments"
      },
      "space_guid":"4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "plan":"standard"
   }
}
```
{: codeblock}

Une fois que vous connaissez l'adresse `url` des modèles publiés (**published_models**), utilisez l'appel API suivant pour obtenir les détails du modèle :

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

Exemple de sortie :

```
{  
   "count":1,
   "resources":[  
      {  
         "metadata":{
      "guid":"361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "created_at":"2017-08-22T08:34:47.597Z",
            "modified_at":"2017-08-22T08:34:47.691Z"
         },
   "entity":{
      "runtime_environment":"spark-2.0",
            "author":{  
               "name":"IBM",
            "email":""
            },
            "name":"Best Heart Drug Selection",
            "label_col":"DRUG",
            "training_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"DRUG"
                     },
                     "type":"string",
                     "name":"DRUG",
                     "nullable":true
                  }
               ],
               "type":"struct"
            },
            "latest_version":{  
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "guid":"8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "created_at":"2017-08-22T08:34:47.691Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{  
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/deployments"
            },
            "input_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  }
               ],
               "type":"struct"
            }
         }
      }
}
```
{: codeblock}


## Configuration du système d'apprentissage continu pour un modèle publié

Pour configurer le système d'apprentissage continu de votre modèle, procédez comme suit :

1.  [Préparation de l'en-tête d'autorisation](#prepare-the-authorization-header).
2.  [Préparation du jeu de données de commentaires](#prepare-the-feedback-data-set).
3.  [Préparation du contenu de configuration](#prepare-the-configuration-payload).

Une fois que vous avez terminé la préparation de l'en-tête d'autorisation, du jeu de données de commentaires et du contenu de configuration, vous pouvez commencer à itérer votre système d'apprentissage continu. Pour plus d'informations, voir [Exécution de l'itération du système d'apprentissage continu](#run-continuous-learning-system-iteration).

### Préparation de l'en-tête d'autorisation

Pour préparer l'en-tête d'autorisation qui combine le jeton {{site.data.keyword.pm_full}} et les données d'identification d'instance Spark, entrez les informations suivantes :

*  Le jeton d'accès créé à l'étape précédente
*  Les données d'identification du service Spark, que vous pouvez trouver dans l'onglet Données d'identification du service du tableau de bord du service {{site.data.keyword.Bluemix_notm}} Spark. Avant d'effectuer la demande de déploiement, les données d'identification Spark doivent être codées en base64. Elles sont transmises dans l'en-tête de la demande dans la zone X-Spark-Service-Instance.

   Suivant le système d'exploitation que vous utilisez, vous devez émettre l'une des commandes de terminal suivantes pour effectuer un codage en base64 et l'affecter à la variable
d'environnement.

   Sur le système d'exploitation **macOS**, utilisez la commande suivante :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net", "instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Sur les systèmes d'exploitation **Microsoft Windows** ou **Linux**, utilisez le paramètre `--wrap=0` avec la commande `base64` pour effectuer un codage en base64 :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### Préparation du jeu de données de commentaires

Le système d'apprentissage nécessite une connexion aux données d'apprentissage (les données utilisées pour l'apprentissage du modèle) ainsi qu'aux données de commentaires en retour. Le magasin de données de commentaires est utilisé pour surveiller et reformer votre modèle, si nécessaire. {{site.data.keyword.dashdbshort}} est pris en chage en tant que magasin de données de commentaires. La table des commentaires est gérée, modifiée et utilisée par le service {{site.data.keyword.pm_short}}.
Pour préparer la table **DRUG_FEEDBACK_DATA** dans **{{site.data.keyword.dashdbshort}}**, procédez comme suit :

1. Créez une instance [{{site.data.keyword.dashdbshort}} Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (un plan d'entrée est offert).
2. Créez la table `DRUG_FEEDBACK_DATA` dans **{{site.data.keyword.dashdbshort}}**.
   1. A partir du référentiel GitHub, téléchargez le fichier [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv).
   2. Pour vous initier à **{{site.data.keyword.dashdbshort_notm}}**, cliquez sur l'icône permettant d'**ouvrir la console**.
   3. Sélectionnez **Load Data** et le type de chargement **Desktop**.
   4. **Faites glisser** le fichier téléchargé auparavant et appuyez sur **Suivant**.
   5. Pour importer les données, cliquez sur **Schéma**, puis sur **Nouvelle table**.
   6. Dans la zone **new table**, entrez un nom et cliquez sur **Next**.
   7. Utilisez le point-virgule (;) comme **séparateur de zone**.
   8. Cliquez sur **Next** pour créer une table avec les données téléchargées.

**Remarque** : Vous pouvez ajouter de nouveaux enregistrements de commentaires en retour à la base de données à l'aide de ce [noeud final d'API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback). Pour plus d'informations, voir la [section du magasin de données de commentaires](#feedback-data-store).

### Préparation du contenu de configuration

Pour pouvoir spécifier la configuration d'apprentissage, utilisez le noeud final suivant :

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Pour finaliser le contenu, définissez les valeurs des paramètres suivants :

<dl><dt>feedback_data_reference</dt>
<dd>connexion et source du magasin de données de commentaires</dd>
<dt>min_feedback_data_size</dt>
<dd>nombre minimal d'enregistrements requis dans le jeu de données de commentaires pour démarrer l'itération du système d'apprentissage continu
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] indique le déclenchement du processus de réapprentissage. Lorsque le paramètre est défini sur [conditionally], il déclenche le processus de réapprentissage losque la qualité du modèle est inférieure à la valeur de seuil spécifiée.
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] indique lorsque le modèle reformé doit être déployé. Lorsqu'il est défini sur [conditionally], le paramètre déclenche le rédéploiement du modèle lorsque la qualité du modèle reformé est meilleure que celle du modèle actuellement déployé.</dd></dl>

Exemple de requête :

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
         "definition": {
           "method": "multiclass",
           "metrics": [
             {
               "name": "accuracy",
               "threshold": 0.8
             }
           ]
         },
         "feedback_data_reference": {
           "connection":{
            "db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
           },
   "source":{
            "tablename": "DRUG_FEEDBACK_DATA",
            "type": "dashdb"
           }
          },
          "min_feedback_data_size": 10,
          "auto_retrain": "conditionally",
          "auto_redeploy": "never",
          "last_training_record": 0
        }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Exemple de sortie :

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass",
    "metrics": [
      {
        "name": "accuracy",
        "threshold": 0.8
      }
    ]
   },
   "feedback_data_reference":{
      "connection":{
         "db":"BLUDB",
         "host":"awh-yp-small02.services.dal.bluemix.net",
         "username":"***",
         "password":"***"
      },
      "source":{
         "tablename":"DRUG_FEEDBACK_DATA",
         "type":"dashdb"
      }
   },
   "min_feedback_data_size":10,
   "spark_service":{
      "credentials": {
         "tenant_id":"***",
         "cluster_master_url":"https://spark.bluemix.net",
         "tenant_id_full":"***",
         "tenant_secret":"***",
         "instance_id":"***",
         "plan":"ibm.SparkService.PayGoPersonal"
      },
         "version":"2.0"
   },
   "training_data_reference": {
   "connection":{
     "db": "BLUDB",
     "host": "dashdb-entry-yp-dal09-08.services.dal.bluemix.net",
     "password": "***",
     "username": "***"
   },
   "source":{
     "tablename": "DRUG_TRAIN_DATA_UPDATED",
     "type": "dashdb"
    }
   }
}
```
{: codeblock}

**Remarque** : Dans cet exemple, nous utilisons les valeurs par défaut des paramètres `auto_retrain` et `auto_redeploy`. Pour obtenir des informations supplémentaires sur ces paramètres, consultez la [documentation de l'API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration) relative au paramètre `learning_configuration`.

## Exécution de l'itération du système d'apprentissage continu

Pour démarrer l'itération du système d'apprentissage, utilisez la méthode d'API REST suivante. Le modèle publié est évalué pendant le processus d'itération. Si l'évaluation de la précision est inférieure à la valeur de seuil spécifiée, le réapprentissage du modèle se déclenche. Les jeux de données d'apprentissage et de commentaires sont tous deux utilisés pour le réapprentissage et l'évaluation.

Exemple de requête :

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Exemple de sortie :

```
The request has been fulfilled and resulted in a new resource being created.
```
{: codeblock}

Pour vérifier le statut de l'itération, émettez la requête suivante :

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Exemple de sortie :

```
{
  "count": 1,
  "resources": [
    {
      "entity":{
        "status": {
          "state": "INITIALIZED"
        },
        "model_version": {
          "url": "https://ibm-watson-ml-svt.stage1.mybluemix.net/v2/artifacts/models/71dc688a-ebda-4174-9574-e8805059e08f/versions/de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d",
          "created_at": "2017-10-30T15:45:12.926Z",
          "guid": "de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d"
        },
      "spark_service":{
          "credentials": {
            "tenant_id_full": "***",
            "tenant_secret": "***",
            "tenant_id": "***",
            "instance_id": "***",
            "plan": "ibm.SparkService.PayGoPersonal",
            "cluster_master_url": "https://spark.bluemix.net"
          },
         "version":"2.0"
        },
        "published_model": {
          "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f",
          "guid": "71dc688a-ebda-4174-9574-e8805059e08f"
        }
      },
      "metadata": {
        "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f/learning_iterations/a308838b-445f-45b8-9fbf-1c3dd1b392c1",
        "created_at": "2017-10-30T15:54:30.657Z",
        "guid": "a308838b-445f-45b8-9fbf-1c3dd1b392c1"
      }
    }
  ]
}
```
{: codeblock}

Vous pouvez également obtenir la liste des métriques d'évaluation et des valeurs en émettant la requête suivante :

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

Exemple de sortie :

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-07T10:11:02.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
      {
      "phase": "monitoring",
      "values": [
        {
          "name": "accuracy",
          "value": 0.75,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    },    
    {
      "phase": "training",
      "values": [
        {
          "name": "accuracy",
          "value": 0.88281694,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:22.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    }
  ]
}
```
{: codeblock}

## Magasin de données de commentaires

Vous pouvez faire appel à un [noeud final de commentaires](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) pour envoyer de nouveaux enregistrements (enregistrements non utilisés dans le processus d'apprentissage) vers le magasin de commentaires qui est défini dans la configuration d'apprentissage. Le magasin de données de commentaires est utilisé pour surveiller et ré-entrainer votre modèle, si nécessaire. {{site.data.keyword.dashdbshort}} est pris en chage en tant que magasin de données de commentaires. Si la table de commentaires n'existe pas, le service la crée. Si elle existe, le schéma est vérifié pour voir s'il correspond à celui de la table d'apprentissage et une colonne supplémentaire nommée `_training` est ajoutée à la table. La colonne supplémentaire est utilisée pour signaler les enregistrements qui sont utilisés dans le cadre du processus de réapprentissage.

**Remarque :** La table de commentaires est gérée, modifiée et utilisée par le service {{site.data.keyword.pm_short}}.

Vous pouvez envoyer un nouvel enregistrement de commentaires au magasin de données de commentaires en soumettant la requête suivante :

Exemple de requête :

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields": [
      "AGE",
      "SEX",
      "BP",
      "CHOLESTEROL",
      "NA",
      "K",
      "DRUG"
   ],
   "values":[
      [
         
         16,
         "M",
         "HIGH",
         "NORMAL",
         0.58301000000000003,
         0.033884999999999998,
         "drugY"
      ]
   ]
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/feedback
```
{: codeblock}

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
