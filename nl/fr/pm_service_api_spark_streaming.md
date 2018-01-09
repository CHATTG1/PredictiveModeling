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

Grâce au service {{site.data.keyword.pm_full}}, vous pouvez déployer un modèle et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.
{: shortdesc}

**Nom du scénario **: analyse d'opinions.

**Description du scénario **: une agence de marketing désire connaître les opinions sur un sujet spécifique. Elle désire que nous développions un modèle classant une déclaration comme POSITIVE ou NEGATIVE. Un spécialiste des données prépare un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

**Remarque :** vous pouvez également vous familiariser avec [un exemple de notebook Python](https://apsportal.ibm.com/analytics/notebooks/913a7daa-cf39-414d-9017-3a7840a53c59/view?access_token=f1ebc10873a226f248f744b26ee7f71d53c81d5752b9d940e23a33518a3e115d) qui crée un exemple de modèle et classe les tweets.

Consultez ce document pour plus d'informations.


## Conditions requises
Pour pouvoir utiliser cet exemple, vous aurez besoin :
* Des informations de la rubrique [Message Hub](https://console.bluemix.net/catalog/services/message-hub) qui seront utilisées comme entrées (tweet) pour le modèle et lieu de stockage pour la sortie du modèle (résultats de la prévision). Assurez-vous que deux rubriques soient créées : une rubrique d'entrée avec des tweets et une rubrique de sortie.
* Des données d'identification de l'instance de service [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilisez ce [lien](https://console.bluemix.net/catalog/services/apache-spark) pour en créer une.



## Utilisation de l'exemple de modèle

1. Accédez à l'onglet Exemples du tableau de bord {{site.data.keyword.pm_full}}.

2. Dans la section Exemples de modèles, recherchez la vignette Pronostics des opinions et cliquez sur l'icône Ajouter le modèle (+).

Vous pouvez observer à présent le modèle Pronostics des opinions dans la liste des modèles disponibles sous l'onglet Modèles.

## Génération d'un jeton d'accès

Générez un jeton d'accès à partir de l'ID et du mot de passe utilisateur affichés dans l'onglet Données d'identification pour le service de l'instance de service IBM Watson Machine Learning.

Exemple de requête :

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
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
* `url` des modèles publiés
* `url` des déploiements
* informations sur l'utilisation

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"87452a37-6a8f-4d59-bf88-59c66b5463e4",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
      "created_at":"2017-06-23T08:31:52.026Z",
      "modified_at":"2017-06-23T08:31:52.026Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
      },
      "usage":{ },
      "plan_id":"5325f63a-683a-47f0-a04e-97e371385588",
      "account_id":"b56398ea52f470c3173f4cf3bef5cc7e",
      "status":"Active",
      "organization_guid":"3e658178-a60c-48b8-8be9-bf58cc821656",
      "region":"us-south",
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}}/deployments"
      },
      "space_guid":"c3ea6205-b895-48ad-bb55-6786bc712c24",
      "plan":"lite"
   }
}
```
{: codeblock}


Avec l'`url` **published_models**, utilisez l'appel API suivant pour obtenir les détails du modèle :

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

Exemple de sortie :

```
{
   "count":1,
   "resources":[
      {
         "metadata":{
            "guid":"f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5",
            "created_at":"2017-07-11T10:51:10.900Z",
            "modified_at":"2017-07-11T10:51:11.012Z"
         },
   "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
            "email":""
            },
            "name":"Sentiment Prediction",
            "description":"Predicts comment sentiment about particular topic for marketing company.",
            "label_col":"sentiment",
            "training_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"id",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"text",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"label",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5/versions/0bb2bfd2-418d-4458-892b-1d9c02e56686",
               "guid":"0bb2bfd2-418d-4458-892b-1d9c02e56686",
               "created_at":"2017-07-11T10:51:11.012Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5/deployments"
            },
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"id",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"text",
                     "nullable":true
                  }
               ]
            }
         }
      }
   ]
}
```
{: codeblock}

Notez la valeur `url` de la section **deployments** sur laquelle créer le déploiement par lots.

## Création d'un déploiement en flux à l'aide d'IBM Message Hub

Pour utiliser un appel d'API REST pour créer un déploiement en flux de votre modèle prédictif, fournissez les informations suivantes :

*  Le jeton d'accès que vous avez créé à l'étape précédente

*  Les données d'identification du service Spark, que vous pouvez trouver dans l'onglet Données d'identification du service du tableau de bord du service {{site.data.keyword.Bluemix_notm}} Spark. Avant de soumettre la demande de déploiement, les données d'identification Spark doivent être décodées en base64 et transmises dans l'en-tête d'une requête `curl` sous la forme X-Spark-Service-Instance.

   Selon le système d'exploitation que vous utilisez, vous devez exécuter l'une des commandes de terminal suivantes pour effectuer un décodage en base64 et l'affecter à la variable d'environnement.

   Sur le système d'exploitation **macOS**, utilisez la commande suivante :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Sur les systèmes d'exploitation **Microsoft Windows** ou **Linux**, utilisez le paramètre `--wrap=0` avec la commande `base64` pour effectuer un décodage en base64 :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

*  Des informations de la rubrique IBM Message Hub, qui seront utilisées comme entrées (tweets) pour le modèle et lieu de stockage pour la sortie du modèle (résultats de la prévision)

*  La valeur `url` de la section **deployments** 

Exemple de requête :

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $token" \
-H "X-Spark-Service-Instance: $spark_credentials" \
-d '{
   "name":"Sentiment Prediction ",
   "type":"stream",
   "description": "Streaming Deployment",
   "input":{
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
   },
   "output":{
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
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601/deployments/0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "created_at":"2017-06-28T12:39:01.474Z",
      "modified_at":"2017-06-28T12:39:04.274Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Sentiment Prediction ",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/streaming/deployments/553f0ed7-b3fc-4659-ae45-c42c66cb5e04",
      "description":"Streaming Deployment",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Sentiment Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "guid":"ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "description":"Predicts comment sentiment about particular topic for marketing company.",
         "created_at":"2017-06-28T11:57:47.636Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "target":{
            "topic":"streamingo",
            "type":"kafka"
         }
      },
      "type":"stream",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/ceed18b3-ada2-42db-bf00-429f4e2c7601/versions/848e632f-1ffc-4241-992b-6301686eadbd",
         "guid":"848e632f-1ffc-4241-992b-6301686eadbd",
         "created_at":"2017-06-28T11:57:10.997Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "source":{
            "topic":"streamingi",
            "type":"kafka"
         }
      }
   }
}
```
{: codeblock}

**Remarque **: vous pouvez également utiliser le tableau de bord pour créer un déploiement en flux.


## Obtention des détails du déploiement

Vous pouvez examiner les détails de votre déploiement en flux via une requête GET à l'aide de l'`url` **metadata** renvoyée comme résultat de
votre déploiement. Examinez les
informations suivantes.

Exemple de requête :

```
curl -i \
-X GET \
-H "Authorization: Bearer $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601/deployments/0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "created_at":"2017-06-28T12:39:01.474Z",
      "modified_at":"2017-06-28T12:39:04.274Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Sentiment Prediction ",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/streaming/deployments/553f0ed7-b3fc-4659-ae45-c42c66cb5e04",
      "description":"Streaming Deployment",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Sentiment Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "guid":"ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "description":"Predicts comment sentiment about particular topic for marketing company.",
         "created_at":"2017-06-28T11:57:47.636Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"RUNNING",
      "output":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "target":{
            "topic":"streamingo",
            "type":"kafka"
         }
      },
      "type":"stream",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/ceed18b3-ada2-42db-bf00-429f4e2c7601/versions/848e632f-1ffc-4241-992b-6301686eadbd",
         "guid":"848e632f-1ffc-4241-992b-6301686eadbd",
         "created_at":"2017-06-28T11:57:10.997Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "source":{
            "topic":"streamingi",
            "type":"kafka"
         }
      }
   }
}
```
{: codeblock}

## Arrêt d'un déploiement en flux

A nouveau, vous pouvez utiliser la zone Location (emplacement) dans les résultats d'un déploiement pour arrêter facilement un déploiement en flux en procédant comme suit.

Exemple de requête :

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $token" \
-d '[{"op": "replace","path": "/status","value": "STOPPED"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemple de sortie :

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: close
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:34:37 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 2590068775
```
{: codeblock}

## Lancement du déploiement en flux

Vous pouvez lancer facilement un déploiement en flux en procédant comme suit.

Exemple de requête :

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $token" \
-d '[{"op": "replace","path": "/status","value": "RUNNING"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemple de sortie :

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: close
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:35:39 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 4242073343
```
{: codeblock}

## Suppression du déploiement en flux

Si vous n'en avez plus besoin, vous pouvez supprimer un déploiement en flux.

Exemple de requête :

```
curl -i \
-X DELETE \
-H "Authorization: Bearer $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemple de sortie :

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:36:38 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 2025130991
```
{: codeblock}

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
