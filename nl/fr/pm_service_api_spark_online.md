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

# Déploiement de modèles en ligne

Grâce au service {{site.data.keyword.pm_full}}, vous pouvez déployer un modèle et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.
{: shortdesc}

**Nom du scénario **:  Pronostic de ligne de produits.

**Description du scénario** : Une entreprise qui commercialise des équipements de plein air désire construire un modèle capable de prédire l'intérêt des clients pour une ligne de produits spécifique. Un spécialiste des données a développé un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des pronostics de l'intérêt des clients en lançant des requêtes de score à partir du modèle déployé.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet Exemples du tableau de bord {{site.data.keyword.pm_full}}.

2. Dans la section Exemples de modèles, recherchez la vignette Pronostics de ligne de produits et cliquez sur l'icône Ajouter le modèle (+).

L'exemple de modèle Pronostics de ligne de produits s'affiche dans la liste des modèles disponibles sous l'onglet Modèles.

## Génération du jeton d'accès

Générez un jeton d'accès à l'aide des éléments `user` et `password` disponibles sur l'onglet **Données d'identification pour le service** de l'instance de service {{site.data.keyword.pm_full}}.

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

Utilisez l'appel API suivant pour obtenir des détails sur votre instance, notamment :

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
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

Exemple de sortie :

```
{
   "count":1,
   "resources":[
      {
         "metadata":{
            "guid":"7715dfcc-3005-4bc2-8bee-58ebdc9a43f3",
            "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publishepublished_model_id}",
            "created_at":"2017-07-10T12:45:56.623Z",
            "modified_at":"2017-07-10T12:45:56.710Z"
         },
   "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
            "email":""
            },
            "name":"Product Line Prediction",
            "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
            "label_col":"PRODUCT_LINE",
            "training_data_schema":{ },
            "latest_version":{ },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publipublished_model_id}/deployments"
            },
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"GENDER",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MARITAL_STATUS",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PROFESSION",
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


Notez la valeur **deployments** `url` nécessaire à la création du déploiement en ligne ci-dessous.


## Création du déploiement en ligne

Utilisez l'appel d'API suivant pour générer un déploiement en ligne de votre modèle prédictif.

Exemple de requête :

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


Exemple de sortie :

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"en cours d'initialisation",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Obtention des détails du déploiement

Vous pouvez vérifier le statut, l'adresse de noeud final d'évaluation (`scoring_url`) et les paramètres associés au modèle déployé.

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemple de sortie :

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"Actif",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Soumission de requêtes de score

Comme votre noeud final d'évaluation a été créé (`scoring_url`), vous pouvez maintenant générer des prévisions en effectuant des requêtes de score. Dans ce scénario, des enregistrements client sont transmis au modèle prédictif et un pronostic sur les articles de sport est renvoyé.

Exemple d'en-tête d'enregistrement :

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Exemple d'enregistrement :

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Exemple de requête :

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Exemple de sortie :

```
{
   "fields": ["GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "values":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Accessoires personnels"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Equipements d'alpinisme montagne"
      ]
   ]
}
```
{: codeblock}

Nous pouvons constater, par exemple, qu'un cadre âgé de 55 ans est intéressé par les produits de la catégorie Equipements d'alpinisme, tandis qu'un étudiant âgé de 23 ans s'intéresse à la catégorie Accessoires personnels.

**Remarque **: pour les modèles scikit-learn et XGBoost, seule la zone `values` est obligatoire dans le contenu d'évaluation.

Exemple de requête :

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"values": [[0.0,1.0],[4.0,15.0]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
