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

# Téléchargement d'une copie d'un fichier de modèle déployé spécifique

Le service {{site.data.keyword.pm_full}} permet de télécharger une copie d'un fichier de modèle spécifique déployé via un appel API.
{: shortdesc}

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application
liée}
```
{: codeblock}

Exemple de requête :

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

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
