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

# Extraction d'une liste de tous les modèles actuellement déployés

Récupérez la liste de tous les modèles actuellement déployés sur cette instance de service {{site.data.keyword.pm_full}}.
{: shortdesc}

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key pour cette application liée}
```
{: codeblock}

Exemple de requête :

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

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
