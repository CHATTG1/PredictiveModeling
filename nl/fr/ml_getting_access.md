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

# Configuration de votre environnement

Pour pouvoir utiliser {{site.data.keyword.pm_full}}, vous devez être capable de créer le bon environnement d'apprentissage automatique et d'extraire les données d'identification spécifiques à votre environnement. Vous pouvez créer l'instance {{site.data.keyword.Bluemix}} en utilisant soit l'[interface de ligne de commande Cloud Foundry](https://github.com/cloudfoundry/cli#getting-started), soit l'interface graphique du tableau de bord {{site.data.keyword.Bluemix}}.
{: shortdesc}

## Utilisation du tableau de bord IBM Cloud

Pour utiliser {{site.data.keyword.pm_full}}, vous devez [créer une instance](https://console.bluemix.net/catalog/services/machine-learning) correspondante dans votre tableau de bord {{site.data.keyword.Bluemix_notm}}.

En fonction de votre scénario, vous pourrez également avoir besoin des ressources suivantes :

- Object Storage (déploiement par lots)
- Db2 Warehouse on Cloud (déploiement par lots)
- Apache Spark (déploiement par lots, en flux, et système d'apprentissage continu)
- Message Hub (déploiement en flux)

Pour savoir comment utiliser le tableau de bord pour créer des instances {{site.data.keyword.Bluemix_notm}} et afficher les données d'identification, visionnez la vidéo suivante, qui est fournie en complément des étapes ci-dessous.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figure 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Si vous ne parvenez pas à visionner la vidéo intégrée dans cette page, consultez-là depuis le site Web YouTube. (Affichage dans un nouvel onglet ou dans une nouvelle fenêtre)">    <img src="images/video.png" alt="Icône Vidéo"></a>IBM Watson Machine Learning - Initiation</span></figcaption>
<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Cette vidéo fournit un aperçu de la manière de configurer votre environnement d'apprentissage automatique.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## Création d'instances {{site.data.keyword.Bluemix_notm}}

Pour créer une instance {{site.data.keyword.pm_full}}, procédez comme suit :

1. Ouvrez la [page de service](https://console.bluemix.net/catalog/services/machine-learning) {{site.data.keyword.pm_full}} .
2. Inscrivez-vous ou connectez-vous pour pouvoir créer l'instance de service.
3. Entrez un nom descriptif pour votre instance, choisissez un espace et sélectionnez votre plan de données.
4. Cliquez sur **Create Instance**.

Votre instance s'ouvre.

## Récupération de vos données d'identification

Pour utiliser votre instance {{site.data.keyword.Bluemix_notm}} (anciennement appelée Bluemix), vous devez disposer de données d'identification pour l'instance de service. Vous pouvez créer vos données d'identification et y accéder en utilisant soit l'[interface de ligne de commande CF](using_pm_service.html), soit le tableau de bord {{site.data.keyword.Bluemix_notm}}. Une fois l'instance de service {{site.data.keyword.pm_short}} liée à votre application {{site.data.keyword.Bluemix_notm}}, les données d'identification {{site.data.keyword.pm_short}} sont ajoutées à la variable d'environnement `VCAP_SERVICES`. Pour plus d'informations, voir l'[utilisation du service avec l'application](using_pm_service.html).

Pour récupérer les données d'identification de l'instance {{site.data.keyword.pm_full}}, effectuez les étapes suivantes :

1. [Connectez-vous à {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Dans la section **Services**, cliquez sur le nom de service dont vous voulez extraire les données d'identification.
3. Dans le panneau de navigation, cliquez sur **Données d'identification pour le service**.
4. Dans la liste qui s'affiche, cliquez sur **Afficher les données d'identification**.
5. Si aucune donnée d'identification n'existe pour ce service, vous devez en créer à l'aide de l'option de **création de données d'identification**.
6. Pour copier les données d'identification, utilisez l'icône de copie.

Pour pouvoir utiliser le service {{site.data.keyword.pm_short}} dans votre application, vous devez le lier à l'application {{site.data.keyword.Bluemix_notm}} et les données d'identification requises pour exécuter l'application sont insérées dans la variable d'environnement `VCAP_SERVICES`. Pour plus d'informations, voir [Interface de ligne de commande Cloud Foundry](#cloud-foundry-command-line-interface).

## Utilisation de l'interface de ligne de commande Cloud Foundry 

### Etapes de liaison du service avec l'application {{site.data.keyword.Bluemix_notm}}

Vous pouvez télécharger l'[exemple de code](https://github.com/pmservice/product-line-prediction/blob/master/README.md) Node.js pour essayer le service Machine Learning. Exécutez les étapes ci-après pour créer votre application {{site.data.keyword.Bluemix_notm}} et la lier au service {{site.data.keyword.pm_short}}. Ces
exemples utilisent Node.js car il s'agit d'un environnement d'exécution populaire. D'autres langages, tels que iOS, Ruby, Perl ou Java, sont également disponibles.

Vous pouvez également réaliser les étapes 1 à 3 via l'interface graphique de {{site.data.keyword.Bluemix_notm}} au lien de l'outil Cloud Foundry (cf).

1. Créez une instance de service à l'aide de la commande `cf create-service`.

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Par exemple, la commande suivante crée une instance de service {{site.data.keyword.pm_short}} avec un plan Lite nommé `my_wml_lite` dans votre espace {{site.data.keyword.Bluemix_notm}} :

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Utilisez la commande `cf create-service-key` pour créer des données d'identification pour le service :

   ```
   cf create-service-key "{nom_instance_service}" {nom_clé_vcap}
   ```
   {: codeblock}

   Par exemple, la commande suivante crée les données d'identification de {{site.data.keyword.pm_short}} :

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. Utilisez la commande `cf bind-service` pour lier l'instance de service `my_wml_lite` à votre application.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   Par exemple, la commande suivante lie l'instance de service {{site.data.keyword.pm_short}} `my_wml_lite` à l'application {{site.data.keyword.Bluemix_notm}} `my_app1` :

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Accès aux données d'identification via la variable `VCAP_SERVICES`

Une fois l'instance de service {{site.data.keyword.pm_short}} liée à votre application {{site.data.keyword.Bluemix}}, les données d'identification {{site.data.keyword.pm_short}} sont ajoutées à la variable d'environnement `VCAP_SERVICES` :

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   La variable d'environnement `VCAP_SERVICES` contient les informations suivantes :

<dl>
<dt>plan</dt>
<dd>Le plan {{site.data.keyword.pm_short}} utilisé dans la mise à disposition du service.</dd>
<dt>url</dt><dd>L'adresse de l'instance de service {{site.data.keyword.pm_short}}.
<dt>access_key</dt><dd>Pour les flux IBM® SPSS® Modeler uniquement.</dd>
<dt>instance_id</dt><dd>L'identificateur unique de l'instance {{site.data.keyword.pm_short}}.</dd>
<dt>username</dt><dd>Le nom d'utilisateur, qui fait partie de l'autorisation de base requise pour générer un jeton d'accès qui sera transmis dans toutes les requêtes à cette instance de service.</dd>
<dt>password</dt><dd>Le mot de passe, qui fait partie de l'autorisation de base requise pour générer un jeton d'accès qui sera transmis dans toutes les requêtes à cette instance de service. </dd>
</dl>

Par exemple, vous pouvez générer un jeton d'accès à l'aide de la commande `curl` suivante :

Exemple de requête :

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Exemple de sortie :

       {"token":"**********"}
```
{: codeblock}

   Le code Node.js suivant illustre comment obtenir les valeurs `username` et `password` depuis la variable d'environnement `VCAP_SERVICES` :

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
