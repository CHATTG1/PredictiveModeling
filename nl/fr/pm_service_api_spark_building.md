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

# Construction d'applications d'analyse prédictive

Le service {{site.data.keyword.pm_full}} permet de déployer une application Node.js.
{: shortdesc}

**Nom du scénario **:  Pronostic de ligne de produits.

**Description du scénario **: notre client gère l'une des chaînes de magasins les plus connues en
Europe. Il souhaite que nous identifions les intérêts de ses clients en ce qui concerne leurs lignes de produits, par exemple
Accessoires personnels, Matériel de camping, Vêtements de plein air.
Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à préparer et à déployer l'application Node.js, laquelle recommande des lignes de produits sportifs en émettant des requêtes de score au modèle déployé.

1. Connectez-vous à {{site.data.keyword.Bluemix_short}} et créez une instance d'{{site.data.keyword.pm_full}}.
2. Ouvrez le tableau de bord {{site.data.keyword.pm_full}}.
3. Accédez à l'onglet **Exemples**.
4. Dans la section **Exemples de modèles**, recherchez la vignette **Pronostic de ligne de produits** et cliquez sur l'icône **Ajouter le modèle** (+). L'exemple de modèle Pronostic de ligne de produits s'affiche dans la liste des modèles disponibles sous l'onglet **Modèles**.

   **Remarque** : Si vous désirez utiliser vos propres projet et modèles Data Science Experience au lieu des exemples, vous devez conserver un modèle personnalisé dans le référentiel {{site.data.keyword.pm_short}}. Vous pouvez effectuer cette opération à l'aide de l'API
REST ou de bibliothèques client. Pour plus d'informations, voir Développement et persistance du modèle personnalisé.

5. Dans le menu **Action**, cliquez sur **Créer un déploiement** pour déployer le modèle Pronostic de ligne de produits en tant que déploiement en ligne.
6. Indiquez un nom pour le déploiement (par exemple, Pronostic de ligne de produits) et cliquez sur
Sauvegarder. Le déploiement Pronostics de ligne de produits devrait à présent être affiché dans la liste Déploiements.
7. Dans le menu Action, sélectionnez Afficher les détails pour votre déploiement.
   Le noeud final d'évaluation présenté dans le panneau d'informations détaillées sera utilisé pour exécuter des requêtes d'évaluation.
8. Pour exécuter des requêtes d'évaluation via l'API REST, un noeud final d'évaluation et un jeton d'autorisation sont requis. Pour plus d'informations, voir Déploiement de modèles en ligne.
9. Vous pouvez à présent vous faire la main avec l'exemple d'application Node.js, qui se trouve à l'emplacement suivant :
   https://github.com/pmservice/product-line-prediction.

   Pour déployer automatiquement le code de l'exemple d'application dans votre espace {{site.data.keyword.Bluemix_short}}, accédez à l'onglet Exemples et, dans la section Modèles d'applications, sélectionnez l'application Pronostic de ligne de produits, puis déployez-la en cliquant sur l'icône (+). Authentifiez-vous sur le formulaire DeployToBluemix si vous y êtes invité.

   L'exemple d'application devrait maintenant être affiché sur le Tableau de bord {{site.data.keyword.Bluemix_short}} dans la section Toutes les applications.

10. Cliquez sur l'application pour afficher ses détails. Vous pouvez modifier ici des éléments comme le nombre d'instances ou l'allocation de mémoire.
11. Liez votre application à l'instance {{site.data.keyword.pm_short}}. Dans l'onglet **Connexions**, cliquez sur **Connecter un existant**.
    Après reconstitution, votre application est connectée à l'instance de service.
12. Exécutez l'application en utilisant l'adresse de routage.
13. Dans l'application, sélectionnez votre déploiement Pronostic de ligne de produits. Vous pouvez maintenant vous exercer avec des exemples d'enregistrement et des résultats de pronostic.
    
## Informations supplémentaires

Prêt à commencer ? Pour créer une instance de service ou lier une application, voir [Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou [Utilisation du service avec des modèles IBM® SPSS®](using_pm_service.html).

Pour plus d'informations sur l'API, voir [API de service pour les modèles Spark et Python](pm_service_api_spark.html) ou [API de service pour les modèles IBM® SPSS®](pm_service_api_spss.html).

Pour plus d'informations sur IBM® SPSS® Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site [https://datascience.ibm.com](https://datascience.ibm.com).
