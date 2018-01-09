---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API REST (nouvelle version) <span class='tag--beta'>Bêta</span>

Afin de garantir une expérience utilisateur unique, le service {{site.data.keyword.pm_full}} fournit une [API REST](http://watson-ml-api.mybluemix.net/) compatible avec l'ensemble des infrastructures prises en charge. Vous pouvez utiliser les mêmes requêtes API pour les modèles SPSS® et les autres infrastructures, par exemple scikit-learn, XGboost ou Spark MLlib.
{: shortdesc}

**Remarque **: cette fonctionnalité est actuellement offerte en version bêta. Si vous désirez participer, inscrivez-vous sur la liste d'attente. Pour plus d'informations,  voir : [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

A l'aide de l'API REST du service {{site.data.keyword.pm_full}}, vous pouvez créer un déploiement en ligne et générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé. Pour vous familiariser avec l'API REST, utilisez le scénario de satisfaction des clients, ainsi que [l'exemple de notebook Jupyter](https://dataplatform.ibm.com/analytics/notebooks/3c2ef65c-7d0e-4ff7-ab11-578ef2a46d66/view?access_token=21517d9a2f59b1ff5e386982b3fa03d7b41cff53a22bd30b5dc7785535b0b80d) basé sur les commandes `bash` pour exécuter le scénario.

**Nom du scénario** : Pronostic de satisfaction des clients

**Description du scénario **: Une entreprise de télécommunications désire savoir quels clients risquent de les abandonner. Le modèle présenté pronostique une attrition de la clientèle. Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet **Exemples** du tableau de bord {{site.data.keyword.pm_full}}.
2. Dans la section **Exemples de modèles**, recherchez la vignette **Pronostic de satisfaction des clients (SPSS MODEL)** et cliquez sur l'icône **Ajouter le modèle** (+).

Le modèle Pronostic de satisfaction des clients s'affiche dans la liste des modèles disponibles sous l'onglet **Modèles**.

## Génération du jeton d'accès

Générez un jeton d'accès à l'aide du nom d'utilisateur et du mot de passe disponibles dans l'onglet Données d'identification pour le service de l'instance de service IBM Watson Machine Learning.

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
* informations sur l'utilisation

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
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

Fournissez la valeur **published_models** `url` pour exécuter l'appel API suivant et obtenir des détails sur le modèle :

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
            "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "created_at":"2017-11-06T11:34:17.991Z",
            "modified_at":"2017-11-06T11:34:18.227Z"
         },
   "entity":{
            "runtime_environment":"spss-modeler-17.0",
            "learning_configuration_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_configuration",
            "author":{
               "name":"IBM",
            "email":""
            },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts customer satisfaction in telco churn model",
            "label_col":"Churn",
            "learning_iterations_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_iterations",
            "training_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Churn",
                     "nullable":true
                  }
               ]
            },
            "feedback_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/feedback",
            "latest_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments"
            },
            "evaluation_metrics_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/evaluation_metrics",
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TotalCharges",
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

Notez la valeur **deployments** `url` requise pour créer le déploiement en ligne à l'étape suivante.

## Création du déploiement en ligne

Utilisez l'appel d'API suivant pour générer un déploiement en ligne de votre modèle prédictif.

Exemple de requête :

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Customer Satisfaction Prediction", "description": "Customer Satisfaction Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


Exemple de sortie :

```
{
   "metadata":{
      "guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
      "created_at":"2017-11-06T12:03:05.080Z",
      "modified_at":"2017-11-06T12:04:10.766Z"
   },
   "entity":{
      "runtime_environment":"spss-modeler-17.0",
      "name":"Customer Satisfaction Deployment",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
      "description":"My 1st Deployment",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "description":"Predicts customer satisfaction in telco churn model",
         "created_at":"2017-11-06T12:01:04.394Z"
      },
      "model_type":"spss-modeler-model-17.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "created_at":"2017-11-06T11:34:18.227Z"
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
   "count":1,
   "resources":[
      {
         "metadata":{
            "guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
            "created_at":"2017-11-06T12:03:05.080Z",
            "modified_at":"2017-11-06T12:04:10.766Z"
         },
   "entity":{
            "runtime_environment":"spss-modeler-17.0",
            "name":"Customer Satisfaction Deployment",
            "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
            "description":"My 1st Deployment",
            "published_model":{
               "author": {
                  "name":"IBM",
            "email":""
               },
               "name":"Customer Satisfaction Prediction",
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
               "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
               "description":"Predicts customer satisfaction in telco churn model",
               "created_at":"2017-11-06T12:01:04.394Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "status":"ACTIVE",
            "type":"online",
            "deployed_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            }
         }
      }
   ]
}
```
{: codeblock}

## Soumission de requêtes de score

Votre noeud final d'évaluation ayant été créé (`scoring_url`), vous pouvez maintenant générer des prévisions en effectuant des requêtes de score. Dans le scénario suivant, des enregistrements client sont transmis au modèle prédictif et le pronostic de satisfaction est renvoyé.

Exemple d'en-tête d'enregistrement :

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges
```
{: codeblock}


Exemple d'enregistrement client :

```
"3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768
```
{: codeblock}


Exemple de requête :

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"values":[["3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Exemple de sortie :

```
{
   "fields": [
      "customerID",
      "Churn",
      "Predicted Churn",
      "Probability of Churn"
   ],
   "values":[
      [
         
         "3638-WEABW",
         "No",
         "No",
         0.0526309571556145
      ]
   ]
}

```
{: codeblock}

D'après les résultats, ce client semble satisfait.

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM® Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
