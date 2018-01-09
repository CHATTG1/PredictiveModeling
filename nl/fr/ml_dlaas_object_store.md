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

# Configuration d'un conteneur d'objets pour l'apprentissage en profondeur

Pour utiliser le service {{site.data.keyword.pm_full}} et les expérimentations d'apprentissage en profondeur qui font partie de ce service, vous devez disposer d'un accès à une instance Cloud Object Storage (control.softlayer.com) ou Object Storage OpenStack Swift (console.bluemix.net). Définissez des compartiments Cloud Object Storage pour fournir les données d'apprentissage et stocker le modèle formé et les journaux. Vous pouvez utiliser pour cela des instances Cloud Object Storage identiques ou distinctes.
{: shortdesc}

## Création d'une instance Cloud Object Storage

1. Accédez à la page [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) et sélectionnez l'une des options suivantes :

   - Stockage non chiffré gratuit (Lite) : [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - Service chiffré payant : [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. Configurez votre instance Object Storage et cliquez sur **Créer**.
3. Une fois le service créé, dans le menu latéral, cliquez sur **Données d'identification pour le service** afin d'obtenir les données d'identification, telles que l'URL d'API, le nom d'utilisateur et le mot de passe permettant d'accéder à l'instance Object Storage.

## Accès à l'instance Cloud Object Storage (IaaS)

Le service Cloud Object Storage (IaaS) fournit des API compatibles S3 et est accessible depuis n'importe quelle application client prise en charge.

## Interface de ligne de commande Amazon Web Services (AWS)

1. Installez l'[interface de ligne de commande AWS](https://aws.amazon.com/cli/) à l'aide de `pip install awscli`
2. Configurez les données d'identification pour Cloud Object Storage (IaaS) dans votre fichier de données d'identification AWS (sous Linux, situé dans ~/.aws/credentials).

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

L'interface de ligne de commande AWS permet de répertorier et de mettre à jour les compartiments et les fichiers, en spécifiant le profil ci-dessus et le noeud final Cloud Object Storage avec vos données d'identification. Par exemple, pour dresser la liste des compartiments présents dans l'instance, exécutez la commande suivante :

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Accès à l'instance Object Storage OpenStack Swift for Bluemix

L'interface de ligne de commande swift ou les bibliothèques client (par exemple, swift python) permettent d'accéder à ce conteneur d'objets. Pour plus d'informations, consultez [la documentation](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## Formatage des données

Plusieurs infrastructures exigent que les jeux de données d'apprentissage et d'évaluation utilisent différents formats. Par exemple, Caffe requiert des jeux de données en LevelDB ou LMDB tandis que Torch requiert que les jeux de données utilisent le format propriétaire de Torch. Nous partons du principe que les données sont déjà au format approprié pour l'infrastructure spécifique. Les informations relatives à la conversion des données brutes au format spécifique de l'infrastructure dépassent le cadre du présent document. Consultez la documentation relative à l'infrastructure d'apprentissage en profondeur que vous souhaitez utiliser pour obtenir des informations complémentaires.
