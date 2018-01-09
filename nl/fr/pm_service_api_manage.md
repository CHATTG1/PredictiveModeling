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

# Gestion des modèles SPSS déployés

L'API de service {{site.data.keyword.pm_full}} permet de télécharger un fichier qui contient la branche d'évaluation IBM® SPSS® Modeler à déployer. Une fois téléchargé, ce fichier est mis à votre disposition pour que vous puissiez évaluer les données de vos applications.
{: shortdesc}

Vous pouvez en particulier effectuer les tâches suivantes :

*  [Déploiement ou actualisation d'un modèle prédictif](#deploying-or-refreshing-a-predictive-model)
*  [Extraction d'une liste de tous les modèles actuellement déployés](#retrieving-a-list-of-all-currently-deployed-models)
*  [Téléchargement d'une copie d'un fichier de modèle déployé spécifique](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [Suppression d'un modèle prédictif déployé](#deleting-a-deployed-predictive-model)

## Déploiement ou actualisation d'un modèle prédictif

Chaque fichier de modèle dispose d'un ID de contexte, lequel constitue un alias pratique pour référencer le modèle déployé dans les appels de service ultérieurs. Si un modèle est associé à un ID de contexte, il est remplacé par l'appel `PUT` suivant afin d'actualiser l'analyse prédictive utilisée par vos applications.

Exemple de requête :

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application liée}
```
{: codeblock}

Paramètres de la requête :

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file : fichier modèle à télécharger
        Path parameters:
            contextId : identificateur unique à affecter à votre modèle ou référence au modèle déployé à actualiser
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse si le déploiement aboutit :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"informations détaillées"
         }
```
{: codeblock}

Réponse en cas d'échec du déploiement :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"motif"
        }
```
{: codeblock}

## Extraction d'une liste de tous les modèles actuellement déployés

Utilisez l'appel API suivant pour extraire le récapitulatif de tous les modèles actuellement déployés sur cette instance de service.

Exemple de requête :

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key pour cette application liée}
```
{: codeblock}

Paramètres de la requête :

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de la demande de récapitulatif du modèle déployé :

```
    Content-Type: application/json
    Status code: 200
    body : tableau vide si aucun modèle n'a été déployé ou récapitulatif des modèles déployés...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Réponse en cas d'échec de la demande de récapitulatif du modèle déployé :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"motif"
         }
```
{: codeblock}

## Téléchargement d'une copie d'un fichier de modèle déployé spécifique

Utilisez l'appel API suivant pour télécharger une copie d'un fichier de modèle spécifique déployé.

Exemple de requête :

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application
liée}
```
{: codeblock}

Paramètres de la requête :

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId : identificateur du modèle prédictif déployé que vous désirez télécharger
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de la requête de téléchargement :

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=nom_fichier_modèle.str"
```
{: codeblock}

Réponse en cas d'échec de la requête de téléchargement :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":String // motif de l'échec
        }
```
{: codeblock}

## Suppression d'un modèle prédictif déployé

Utilisez l'appel API suivant pour supprimer le modèle prédictif de l'instance de service Machine Learning. Après cet appel, le modèle prédictif ne peut plus être téléchargé et les données ne peuvent plus être évaluées dans vos applications.

Exemple de requête :

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key pour cette application
liée}
```
{: codeblock}

Paramètres de la requête :

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId : identificateur du modèle déployé dont vous désirez annuler le déploiement et supprimer
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de l'annulation du déploiement :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"informations détaillées"
         }
```
{: codeblock}

Réponse en cas d'échec de l'annulation du déploiement :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"motif"
        }
```
{: codeblock}

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
