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

# Infrastructures prises en charge

Le service {{site.data.keyword.pm_full}} prend en charge les infrastructures suivantes pour le développement et la validation de modèles dans le cadre de la gestion du cycle de vie des modèles.
{: shortdesc}

## Spark MLlib

Le service {{site.data.keyword.pm_full}} prend en charge Spark MLlib pour les services de déploiement et d'évaluation en ligne, par lots (bêta) et en flux. Seules certaines versions et seuls
certains environnements d'exécution sont pris en charge.

### Fonctionnalités

* Types de déploiement :
  * En ligne
  * Par lots avec Object Storage, DB2 Warehouse on Cloud et DB2 on Cloud
  * En flux avec Message Hub
*  Système d'apprentissage continu

### Versions prises en charge

*  Spark 2.0

### Restrictions

  *  Seuls les modèles de classification et de régression sont pris en charge.
  *  Les modèles qui contiennent des références à des transformateurs personnalisés, à des classes et à des fonctions définies par l'utilisateur ne sont pas pris en charge.
  * Les déploiements par lots et en flux utilisent le service IBM Cloud Apache Spark. En fonction du plan de service souscrit, notez les restrictions suivantes quant au nombre de travaux spark-submit parallèles ([Using spark-submit documentation](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite : Un seul travail spark-submit à la fois
    * Reserved Enterprise : 150 travaux spark-submit à la fois

## Scikit-learn

Une prise en charge de scikit-learn existe pour le service de déploiement et d'évaluation en ligne d'{{site.data.keyword.pm_full}}. Seules certaines versions et seuls certains
environnements d'exécution sont pris en charge.

### Fonctionnalités

* Types de déploiement :
  * En ligne

### Versions prises en charge

- Anaconda 4.2.x pour environnement d'exécution Python 3.5
- Scikit-Learn 0.17

### Environnement d'exécution Python

L'environnement d'exécution Python pour les modèles qui ont besoin d'être déployés à l'aide du service Déploiement et évaluation en ligne WML est Python 3.5.

Dans certains cas, où les modèles ont été créés à l'aide de l'environnement d'exécution Python 2.7, le déploiement peut échouer en raison d'une incompatibilité entre Python2.7 et Python3.5.

### Package Python pris en charge

Pour obtenir une liste des packages Python pris en charge par le service Déploiement et évaluation en ligne WML, cliquez [ici](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restrictions

Des restrictions existent qui peuvent inclure toutes ou partie des limitations suivantes :

* Seuls les modèles de classification et de régression sont pris en charge.
* Les modèles peuvent contenir des références aux packages (et versions de package) pris en charge par Anaconda 4.2.x pour environnement d'exploitation Python 3.5.
* Le noeud final "schema" renvoyé durant la demande de déploiement n'est pas implémenté.
* Les modèles nécessitant Pandas Dataframe comme type d'entrée pour l'API "predict()" ne sont pas pris en charge.
* Les modèles qui contiennent des références à des transformateurs personnalisés, à des classes et à des fonctions définies par l'utilisateur ne sont pas pris en charge.

## XGBoost

Une prise en charge de XGBoost existe pour le service Déploiement et évaluation en ligne d'{{site.data.keyword.pm_full}}. Seules certaines versions et seuls certains
environnements d'exécution sont pris en charge.

### Fonctionnalités

* Types de déploiement :
  * En ligne

### Versions prises en charge

- XGBoost 0.6
- Anaconda 4.2.x pour environnement d'exécution Python 3.5
- Scikit-Learn 0.17

### Environnement d'exécution Python

L'environnement d'exécution Python pour les modèles qui ont besoin d'être déployés à l'aide du service Déploiement en ligne et évaluation de WML est Python 3.5.

Dans certains cas, où les modèles ont été créés à l'aide de l'environnement d'exécution Python 2.7, le déploiement peut échouer en raison d'une incompatibilité entre Python2.7 et Python3.5.

### Package Python pris en charge

Pour obtenir une liste des packages Python pris en charge par le service Déploiement et évaluation en ligne WML, cliquez [ici](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restrictions

Des restrictions existent, qui peuvent inclure toutes ou partie des limitations suivantes :

* Les modèles peuvent contenir des références aux packages (et versions de package) pris en charge par Anaconda 4.2.x pour environnement d'exploitation Python 3.5.
* Le noeud final "schema" renvoyé durant la demande de déploiement n'est pas implémenté.
* La probabilité de la prévision n'est pas renvoyée comme réponse à la demande d'évaluation à ce stade.
* Les modèles qui contiennent des références à des transformateurs personnalisés, à des classes et à des fonctions définies par l'utilisateur ne sont pas pris en charge.

## SPSS Modeler

Une prise en charge des flux IBM® SPSS® Modeler existe pour le service Déploiement et évaluation par lots et en ligne d'{{site.data.keyword.pm_full}}. Seules certaines versions et seuls certains
environnements d'exécution sont pris en charge.

### Fonctionnalités

* Types de déploiement :
  * En ligne
  * Par lots
* Réapprentissage
* Exécution en flux

### Versions prises en charge

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Restrictions

Les restrictions suivantes s'appliquent pour les flux IBM® SPSS® Modeler :

*  Les flux créés à l'aide de l'éditeur de flux IBM® Data Science Experience ne sont pas pris en charge.
*  Lorsqu'une branche d'évaluation est préparée pour être utilisée dans une évaluation en temps réel, les données d'entrée dans la demande d'évaluation doivent remplacer le noeud source conçu dans la branche d'évaluation et la sortie de l'analyse prédictive doit être renvoyée dans le flux de réponse (afin de remplacer le noeud terminal dans la conception de branche d'évaluation).
*  La branche d'évaluation étant préparée pour une exécution en temps réel dans {{site.data.keyword.Bluemix_short}}, elle ne peut pas exiger de connexion avec un service externe. Par exemple, une conception de branche d'évaluation IBM Analytical Decision Management ne peut pas contenir de références à des règles ou des modèles stockés dans un référentiel IBM® SPSS® Collaboration et Deployment Services.
*  L'exécution d'une branche d'évaluation pour l'évaluation en temps réel dans {{site.data.keyword.Bluemix_short}} ne peut pas exiger de connexion avec un service externe. Par exemple, vous ne pouvez pas effectuer de déploiement ni d'évaluation par rapport à des algorithmes de modèles qui nécessitent un serveur IBM® SPSS® Analytic Server et un magasin de données Apache Hadoop en temps réel.
*  {{site.data.keyword.pm_short}} prend en charge les scripts Python imbriqués dans Modeler. Il existe quelques restrictions liées à la méthode utilisée pour le traitement des flux avant leur exécution dans {{site.data.keyword.pm_short}}. En règle générale, si un utilisateur choisit de contrôler l'exécution du flux, il fait référence au noeud terminal de la branche. Pour {{site.data.keyword.pm_short}}, lorsque nous traitons le flux, nous identifions les noeuds provenant du JSON qui seront remplacés, puis nous les remplaçons avant l'exécution du flux. Cela provoque l'échec du flux dans le script car l'entrée référencée et les noeuds d'exportation n'existent plus. La solution consiste à utiliser l'ID d'un autre noeud afin d'identifier de manière unique la branche lors de l'exécution. Cela garantit que le flux s'exécute conformément à sa définition dans le script Python imbriqué.

## Informations supplémentaires

Pour obtenir plus d'informations sur la prise en charge actuelle des modèles prédictifs formés par IBM® SPSS® Analytic Server, consultez la rubrique [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) sur le site [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
