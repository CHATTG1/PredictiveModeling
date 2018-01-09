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

# Introduction

<!-- ![deep learning process flow](images/ml_dlaas_api_calls.png) -->

En tant qu'analyste scientifique, vous devez former des centaines de modèles afin d'identifier la bonne combinaison de données et d'hyperparamètres permettant d'optimiser les performances de vos réseaux de neurones. Vous devez réaliser encore plus d'expérimentations…encore plus rapidement. Vous devez former des réseaux plus profonds et explorer des espaces d'hyperparamètres encore plus vastes. {{site.data.keyword.pm_full}} accélère ce cycle d'expérimentation en simplifiant le processus d'apprentissage des modèles en parallèle sur un cluster de calcul par GPU élastique.
{: shortdesc}

Pour commencer :
1. [Configurez votre environnement pour {{site.data.keyword.pm_full}}](ml_getting_access.html)
2. [Installez l'interface de ligne de commande WML](ml_dlaas_environment.html)
3. Découvrez comment configurer votre session d'apprentissage
4. Téléchargez les données d'apprentissage vers le cloud
5. Démarrez l'apprentissage
6. Surveillez et évaluez

## Configuration de chaque session d'apprentissage

{{site.data.keyword.pm_full}} vous permet de rapidement conduire des expérimentations d'apprentissage en profondeur en soumettant des centaines de sessions d'apprentissage qui peuvent être mises en file d'attente à des fins de formation. Chaque session d'apprentissage est composée des éléments suivants : 

* Votre modèle de réseau de neurones défini à l'aide d'une [infrastructure d'apprentissage en profondeur prise en charge](ml_dlaas_supported_framework.html) 
* La configuration indiquant les modalités d'exécution de l'apprentissage, notamment le nombre de processeurs graphiques (GPU) et l'emplacement du [stockage d'objets contenant votre jeu de données](ml_dlaas_object_store.html)

<p align="center"><img src="images/experiment_to_training_runs_text.svg" alt="relation des expérimentations liées aux sessions d'apprentissage"></p>

Consultez la section [Exemples de session d'apprentissage](ml_dlaas_working_with_sample_models.html) incluant des données hébergées sur un stockage de données IBM. Examinez ces exemples pour comprendre la manière dont les fichiers manifest.yml sont configurés, puis consultez la section permettant de [découvrir comment définir vos propres sessions d'apprentissage](ml_dlaas_working_with_new_models.html).  

## Téléchargement des données d'apprentissage vers le cloud

Pour pouvoir commencer à former vos réseaux de neurones, vous devez au préalable avoir déplacé vos données dans IBM Cloud. Pour cela, [téléchargez vos données d'apprentissage vers une instance de service de stockage d'objets](ml_dlaas_object_store.html). Une fois l'apprentissage terminé, la sortie de la session d'apprentissage est consignée dans votre stockage d'objets pour vous permettre de faire glisser les fichiers vers votre bureau.

## Démarrage de l'apprentissage

Une fois que vous avez créé vos définitions d'apprentissage, utilisez l'[interface de ligne de commande](ml_dlaas_environment.html) pour soumettre vos tâches dans {{site.data.keyword.pm_full}}. {{site.data.keyword.pm_full}} enregistre chacune de vos sessions d'apprentissage sous forme de package, puis les affecte à un conteneur Kubernetes avec les ressources et l'infrastructure d'apprentissage en profondeur appropriées. Les sessions sont exécutées en parallèle en fonction du nombre de ressources GPU disponibles pour votre niveau de compte. Les comptes gratuits disposent d'un seul GPU, ce qui signifie que toutes les sessions supplémentaires sont mises en file d'attente.

<p align="center"><img src="images/ml_dlaas_markitecture.svg" alt="Flux de processus d'apprentissage en profondeur"></p>

Comme indiqué dans le diagramme précédent, 4 sessions d'apprentissage sont allouées à 4 conteneurs. Chacun de ces conteneurs héberge l'infrastructure d'apprentissage en profondeur requise par la session d'apprentissage et a accès à un seul GPU K40 (dans cette instance). Toutes les ressources sont attribuées de manière élastique afin que ne vous soit facturée que la durée qui s'écoule entre l'affectation d'un GPU à votre session d'apprentissage et la fin de l'apprentissage avec le transfert de la sortie des données vers votre instance Object Storage.

## Etapes suivantes

Commencez à utiliser ces [exemples de sessions d'apprentissage](ml_dlaas_working_with_sample_models.html) ou créez vos propres [sessions d'apprentissage](ml_dlaas_working_with_new_models.html).
