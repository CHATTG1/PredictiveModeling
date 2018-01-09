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

# Déploiement ou actualisation d'un modèle prédictif

Pour déployer ou actualiser un modèle prédictif à l'aide du service, utilisez un appel API afin de télécharger le fichier qui contient la branche d'évaluation ayant été déployée par le biais d'IBM® SPSS® Modeler. Cette API est mise à votre disposition afin que vous puissiez évaluer par score les données de vos applications. Chaque fichier de modèle dispose d'un ID de contexte, lequel constitue un alias pratique pour référencer le modèle déployé dans les appels de service ultérieurs. Si un modèle est associé à un ID de contexte, il est remplacé par cet appel PUT en actualisant l'analyse prédictive utilisée par vos
applications.
{: shortdesc}

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application liée}
```
{: codeblock}

Exemple de requête :

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

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
