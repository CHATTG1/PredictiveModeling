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

# Utilisation du service

Grâce aux méthodes de modélisation de la palette de modélisation IBM® SPSS® Modeler, vous pouvez extraire de vos données de nouvelles informations et développer ainsi des modèles prédictifs. Chaque méthode possède ses propres avantages et est donc plus adaptée à certains types de problèmes spécifiques.
{: .shortdesc}

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Une fois que vous avez implémenté les exigences en matière d'entrée et de sortie de votre application {{site.data.keyword.Bluemix_notm}} et de conception de la branche d'évaluation IBM® SPSS® Modeler, votre analyste de données peut modifier n'importe quel aspect interne de cette branche. Il peut même modifier le ou les algorithmes de modélisation utilisés dans une opération d'actualisation afin que vous puissiez ensuite affiner vos analyses prédictives sans devoir réécrire vos applications.


## Etapes de liaison du service avec l'application {{site.data.keyword.Bluemix_notm}}

Exécutez les étapes ci-après pour créer votre application {{site.data.keyword.Bluemix_notm}} et la lier au service {{site.data.keyword.pm_short}}.

1. Téléchargez l'exemple de code d'application Node.js depuis le [référentiel GitHub](https://github.com/pmservice/customer-satisfaction-prediction).

2. Créez une instance de service à l'aide de la commande `cf create-service`.

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Par exemple :

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   Cette commande crée une instance de service {{site.data.keyword.pm_short}} avec un plan Lite nommé my_pm_lite dans votre espace {{site.data.keyword.Bluemix_notm}}.

3. Utilisez la commande `cf create-service-key` pour créer des données d'identification pour le service :

   ```
   cf create-service-key "{nom_instance_service}" {nom_clé_vcap}
   ```
   {: codeblock}

   Par exemple :

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Cette commande crée les données d'identification pour le service {{site.data.keyword.pm_short}}.

4. Utilisez la commande `cf bind-service` pour lier l'instance de service `my_pm_lite` à votre application.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   Par exemple :

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   Cette commande lie l'instance de service {{site.data.keyword.pm_short}} `my_pm_lite` à l'application {{site.data.keyword.Bluemix_notm}} my_app1.

5. Données d'identification {{site.data.keyword.pm_short}} :

   Une fois l'instance de service {{site.data.keyword.pm_short}} liée à votre application {{site.data.keyword.Bluemix_notm}}, les données d'identification {{site.data.keyword.pm_short}} sont ajoutées à la variable d'environnement `VCAP_SERVICES` :

```
    {   
        "pm-20": {
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   La variable d'environnement `VCAP_SERVICES` contient les informations suivantes :

   <dl>

   <dt>plan</dt>
   <dd>Plan {{site.data.keyword.pm_short}} utilisé dans la mise à disposition du service.</dd>

   <dt>url</dt>
   <dd>Adresse de l'instance de service {{site.data.keyword.pm_short}}.</dd>

   <dt>access_key</dt>
   <dd>Paramètre de requête accessKey via lequel sont transmises toutes les requêtes à cette instance de service.</dd>

   </dl>

Par exemple :             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Exemple de code Node.js illustrant comment obtenir la valeur de accessKey depuis la variable d'environnement `VCAP_SERVICES` :

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
