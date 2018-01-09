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

# Configuration de votre environnement d'apprentissage automatique et extraction de vos données d'identification

Pour utiliser IBM Watson Machine Learning, vous devez être capable de créer l'environnement d'apprentissage automatique approprié et d'extraire les données d'identification spécifiques à cet environnement.

## Configuration de votre environnement

Pour utiliser IBM Watson Machine Learning, configurez des instances des éléments suivants dans votre tableau de bord IBM Cloud :

- Watson Machine Learning
- Object Storage
- Apache Spark

Outre les instances requises, certains notebooks et tutoriels utilisent également les instances ci-dessous. Par conséquent, pour pouvoir utiliser ces tutoriels, les services suivants doivent également être configurés :

- Python Flask
- Natural Language Understanding
- Expérimentations d'apprentissage en profondeur

### Création d'instances IBM Cloud

Visionnez cette vidéo pour savoir comment créer des instances IBM Cloud et afficher leurs données d'identification. Effectuez ensuite la procédure qui suit la vidéo pour configurer votre propre environnement. Voir la section <a href="#retrieving-your-credentials">Extraction de vos données d'identification</a> pour savoir comment les extraire.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figure 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Si vous ne parvenez pas à visionner la vidéo intégrée dans cette page, consultez-là depuis le site Web YouTube. (Affichage dans un nouvel onglet ou dans une nouvelle fenêtre)">    <img src="images/video.png" alt="Icône Vidéo"></a>IBM Watson Machine Learning - Initiation</span></figcaption>
<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Cette vidéo fournit un aperçu de la manière de configurer votre environnement d'apprentissage automatique.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Pour créer des instances Watson Data Platform, procédez comme suit :

1. [Connectez-vous à IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Dans le panneau de navigation, cliquez sur **Data & Analytics**.

   L'écran répertoriant les services de données s'affiche. Vous pouvez utiliser cet écran chaque fois que vous avez besoin d'utiliser vos données et vos services d'analyse à partir d'une seule page facile à utiliser.

3. Cliquez sur l'onglet **Analyse**, puis cliquez sur **Instances**.
4. Cliquez sur **Créer une instance**.
5. Configurez les instances et le stockage de données.

   Entrez un nom descriptif pour votre instance, choisissez un espace et sélectionnez votre plan de données (vous trouverez un comparatif des plans accompagné de leur tarification plus loin sur cette page). IBM Cloud renseigne automatiquement le nom du stockage d'objets avec celui de votre instance. Sachant que vous aurez sans doute besoin de stocker les données de votre projet Spark, choisissez également un espace et un plan ici.

6. Cliquez sur **Create Instance**.

Votre instance s'ouvre.

### Ajout d'instances Bluemix existantes à un projet dans Data Science Experience

Visionnez cette vidéo pour savoir comment créer un projet et le configurer de manière à utiliser Watson Machine Learning.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Figure 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="Si vous ne parvenez pas à visionner la vidéo intégrée dans cette page, consultez-là depuis le site Web YouTube. (Affichage dans un nouvel onglet ou dans une nouvelle fenêtre)">    <img src="images/video.png" alt="Icône Vidéo"></a>IBM Watson Machine Learning : Création d'un projet pour Watson Machine Learning</span></figcaption>
<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>Cette vidéo fournit un aperçu de la manière de configurer votre environnement d'apprentissage automatique.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Si vous possédez déjà des instances, mais qu'elles ne sont pas liées à un projet dans Data Science Experience, effectuez les étapes suivantes :

1. [Connectez-vous à IBM Data Science Experience](https://datascience.ibm.com).
2. Cliquez sur **Projects** > **View All Projects**.
3. Cliquez sur l'onglet **Settings**.
4. Pour ajouter un service, dans le panneau **Associated Services**, cliquez sur **add associated service**, sélectionnez un service, fournissez les informations de configuration et cliquez sur **Save**.
5. Pour ajouter un jeton d'accès, procédez comme suit :
   6. Dans le panneau **Access Tokens**, cliquez sur **create new token**.
   7. Dans la zone **Name**, entrez un nom.
   8. Dans la zone **Access Role For Project**, sélectionnez le rôle **Viewer** ou **Editor**.
   9. Cliquez sur **Add**.

6. Pour vous connecter à un référentiel GitHub, indiquez l'emplacement du référentiel dans la zone **Repository URL**. Si aucun jeton d'accès personnel n'est déjà configuré pour pouvoir accéder à GitHub, créez-en un maintenant en cliquant sur **Settings**. Lorsque vous avez terminé, cliquez sur **Add**.


## Récupération de vos données d'identification

Pour utiliser vos instances, services et modèles Bluemix dans des notebooks et des applications, vous devez être capable d'insérer vos données d'identification, vos jetons et vos noeuds finaux d'évaluation dans le code utilisé pour le traitement.

1. [Connectez-vous à Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Dans la section **Services**, cliquez sur le nom de service dont vous voulez extraire les données d'identification.
3. Dans le panneau de navigation, cliquez sur **Données d'identification pour le service**.
4. Dans la liste qui s'affiche, cliquez sur **Afficher les données d'identification**.
5. Si aucune donnée d'identification n'existe pour ce service, vous devez en créer en cliquant sur l'option **Create credentials**.
6. Pour copier les données d'identification, utilisez l'icône de copie.

En fonction du service, différentes zones peuvent s'afficher, par exemple `username` et `password`. Copiez les valeurs qui apparaissent pour les utiliser dans votre code.

## Informations supplémentaires

[A quick deep learning tutorial](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

