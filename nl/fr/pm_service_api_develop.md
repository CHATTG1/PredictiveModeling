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

# Développement d'applications d'optimisation des modèles SPSS déployés

Vous pouvez développer des applications {{site.data.keyword.pm_full}} à l'aide des modèles IBM® SPSS®.  
{: shortdesc}

*  [Evaluation par score avec un modèle prédictif déployé](#scoring-with-a-deployed-predictive-model)

*  [Extraction des métadonnées d'un modèle prédictif déployé](#retrieving-metadata-for-a-deployed-predictive-model)

*  [Extraction du récapitulatif WADL (Web Application Description Language)
de ce service](#retrieving-the-web-application-description-language-wadl-summary-of-this-service)

## Evaluation par score avec un modèle prédictif déployé

Utilisez l'appel API suivant pour publier les données d'entrée à utiliser par le modèle déployé pour générer et renvoyer les analyses prédictives dans les résultats du score.

```
POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key pour cette application liée}
```
{: codeblock}

Exemple de requête :

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: identificateur du modèle déployé à utiliser pour traiter cette requête de score
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav",
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"],
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Exemple de réponse réussie à la requête précédente :

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: résultat du score, tableau json, par exemple.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"],
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Réponse en cas d'échec de la requête de score :

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

## Extraction des métadonnées d'un modèle prédictif déployé

Utilisez l'appel API suivant pour extraire les métadonnées de la branche d'évaluation d'un flux IBM® SPSS® Modeler déployé. Vous ne devez pas indiquer de corps de demande avec cette méthode.

```
GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score
```
{: codeblock}

Exemple de requête :

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: identificateur du modèle déployé à utiliser pour traiter cette requête d'extraction des métadonnées
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Exemple de réponse réussie à la requête précédente :

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: métadonnées (formatées pour lisibilité dans ce document) sur la branche d'évaluation, par exemple.
    [
      {
        flag: true
        message: 
          "Model Score Input Metadata: 
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata> 
          Model Score Output Metadata:
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id32CPAJBGJFG" name="Flat File" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
            <field measurementLevel="FLAG" storageType="STRING" name="$C-beer_beans_pizza"/>
            <field measurementLevel="CONTINUOUS" storageType="DOUBLE" name="$CC-beer_beans_pizza"/>
          </table>
          </metadata>"
      }
    ]
```
{: codeblock}

Réponse en cas d'échec de la demande d'évaluation :

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

## Extraction du récapitulatif WADL (web application description language) de ce service

Utilisez l'appel API suivant pour extraire le récapitulatif WADL de ce service.

```
OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl
```
{: codeblock}

Exemple de requête :

```
    Content-Type: */*
```
{: codeblock}

Réponse en cas de succès de la requête WADL :

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score utilisant le modèle avec l'ID de contexte indiqué" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <requête>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

Réponse en cas d'échec de la requête WADL :

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
