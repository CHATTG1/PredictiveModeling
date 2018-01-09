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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Résolution des incidents

Vous trouverez ici les réponses aux questions fréquentes liées au traitement des incidents concernant l'utilisation d'{{site.data.keyword.pm_full}}.
{: shortdesc}

## Aide et support pour Machine Learning
{: #gettinghelp}

Si vous avez des problèmes ou de questions quand vous utilisez {{site.data.keyword.pm_short}}, vous pouvez obtenir de l'aide en recherchant des informations précises ou en posant des questions via un forum. Vous pouvez également ouvrir un ticket de demande de service.

Si vous utilisez les forums pour poser une question, libellez votre question de sorte qu'elle soit vue par les équipes de développement {{site.data.keyword.pm_short}}.

Si vous avez des questions d'ordre technique sur {{site.data.keyword.pm_short}}, publiez votre question sur <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> et balisez votre question avec "ibm-bluemix" et "machine-learning".

Pour toute question relative au service et aux instructions de mise en route, utilisez le
forum <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Incluez
les balises "machine-learning" et "bluemix".
Consultez [Obtenir de l'aide](https://console.bluemix.net/docs/support/index.html#getting-help) pour obtenir plus de détails sur l'utilisation des forums.

Pour plus d'informations sur l'ouverture d'un ticket de demande de service IBM, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir la rubrique
[Contacter le service de support](https://console.bluemix.net/docs/support/index.html#contacting-support).

## Le jeton d'autorisation n'a pas été fourni.
{: #ts_missing_authorization_token}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le jeton d'autorisation n'a pas été fourni dans l'en-tête `Authorization`.
{: tsCauses}
 
Transférez le jeton d'autorisation dans l'en-tête `Authorization`.
{: tsResolve}
       
## Jeton d'autorisation non valide.
{: #ts_invalid_authorization_token}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le jeton d'autorisation fourni ne peut pas être décodé ou analysé.
{: tsCauses}
 
Transférez un jeton d'autorisation correct dans l'en-tête `Authorization`.
{: tsResolve}
       
## Le jeton d'autorisation ne correspond pas à l'ID d'instance (instance_id) utilisé dans la requête.
{: #ts_not_matching_authorization_token}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le jeton d'autorisation qui a été utilisé n'est pas généré pour l'instance de service dans laquelle il a été utilisé.
{: tsCauses}
 
Transférez le jeton d'autorisation dans l'en-tête `Authorization` qui correspond à l'instance de service utilisée.
{: tsResolve}
       
## Le jeton d'autorisation est arrivé à expiration.
{: #ts_expired_authorization_token}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le jeton d'autorisation est arrivé à expiration.
{: tsCauses}
 
Transférez un jeton d'autorisation n'ayant pas expiré dans l'en-tête `Authorization`.
{: tsResolve}
       
## La clé publique requise pour l'authentification n'est pas disponible.
{: #ts_missing_public_key}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Il s'agit d'un problème de service interne.
{: tsCauses}
 
Ce problème doit être résolu par l'équipe de support technique.
{: tsResolve}
       
## L'opération a dépassé le délai d'attente après {{timeout}}
{: #ts_operation_timeout}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le délai d'attente a été dépassé pendant l'exécution de l'opération demandée.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée.
{: tsResolve}
       
## Exception non gérée de type {{type}} avec {{status}}
{: #ts_unhandled_exception_with_status}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Il s'agit d'un problème de service interne.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée. Si le problème persiste, il doit être résolu par l'équipe de support technique.
{: tsResolve}
       
## Exception non gérée de type {{type}} avec {{response}}
{: #ts_unhandled_exception_with_response}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Il s'agit d'un problème de service interne.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée. Si le problème persiste, il doit être résolu par l'équipe de support technique.
{: tsResolve}
       
## Exception non gérée de type {{type}} avec {{json}}
{: #ts_unhandled_exception_with_json}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Il s'agit d'un problème de service interne.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée. Si le problème persiste, il doit être résolu par l'équipe de support technique.
{: tsResolve}
       
## Exception non gérée de type {{type}} avec {{message}}
{: #ts_unhandled_exception_with_message}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Il s'agit d'un problème de service interne.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée. Si le problème persiste, il doit être résolu par l'équipe de support technique.
{: tsResolve}
       
## L'objet demandé est introuvable.
{: #ts_not_found}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
La ressource demandée est introuvable.
{: tsCauses}
 
Vérifiez que vous faites référence à une ressource existante.
{: tsResolve}
       
## La base de données sous-jacente a signalé un nombre de demandes trop élevé.
{: #ts_too_many_cloudant_requests}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
L'utilisateur a envoyé trop de demandes dans un délai donné.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée.
{: tsResolve}
       
 ## La définition de l'évaluation n'est définie ni dans artifactModelVersion ni dans le déploiement. Elle doit être spécifiée \" +\n      \"au moins dans l'un de ces emplacements.
{: #ts_missing_evaluation_definition}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Learning Configuration ne contient pas toutes les informations requises.
{: tsCauses}
 
Spécifiez `definition` dans `learning configuration`.
{: tsResolve}
       
## L'évaluation nécessite la configuration d'apprentissage spécifiée pour le modèle.
{: #ts_missing_learning_configuration}
 
Impossible de créer `learning iteration`.
{: tsSymptoms}
 
`learning configuration` n'est pas définie pour le modèle.
{: tsCauses}
 
Créez `learning configuration` et essayez à nouveau de créer `learning iteration`.
{: tsResolve}
       
## L'évaluation nécessite qu'une instance Spark soit fournie dans l'en-tête `X-Spark-Service-Instance`
{: #ts_missing_spark_definition_for_evaluation}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
`learning configuration` ne contient pas toutes les informations requises
{: tsCauses}
 
Fournissez `spark_service` dans Learning Configuration ou dans l'en-tête `X-Spark-Service-Instance`
{: tsResolve}
       
## Le modèle ne contient aucune version.
{: #ts_missing_latest_model_version}
 
Impossible de créer le déploiement ou de définir la configuration d'apprentissage.
{: tsSymptoms}
 
Une incohérence liée à la persistance du modèle existe.
{: tsCauses}
 
Essayez de conserver à nouveau le modèle et retentez l'opération.
{: tsResolve}
       
## L'opération de correction peut uniquement modifier la configuration d'apprentissage existante.
{: #ts_patch_non_existing_learning_configuration}
 
Il n'est pas possible d'appeler la méthode de correction de l'API REST pour corriger la configuration d'apprentissage.
{: tsSymptoms}
 
`learning configuration` n'est pas défini pour ce modèle ou le modèle n'existe pas.
{: tsCauses}
 
Assurez-vous que le modèle existe et que la configuration d'apprentissage est déjà définie.
{: tsResolve}
       
## L'opération de correction attend exactement une opération de remplacement.
{: #ts_patch_multiple_ops}
 
Le déploiement ne peut pas être corrigé.
{: tsSymptoms}
 
Le contenu de correction contient plus d'une opération ou l'opération de correction est différente de `replace`.
{: tsCauses}
 
Utilisez une seule opération dans le contenu de correction qui est une opération `replace`
{: tsResolve}
       
## Les zones obligatoires suivantes sont absentes du contenu donné : FIELD ou les valeurs des zones sont corrompues.
{: #ts_invalid_request_payload}
 
L'opération en relation avec l'accès au jeu de données sous-jacentes ne peut pas être traitée.
{: tsSymptoms}
 
L'accès au jeu de données n'est pas correctement défini.
{: tsCauses}
 
Corrigez la définition d'accès pour le jeu de données.
{: tsResolve}
       
## Méthode d'évaluation fournie : METHOD n'est pas prise en charge. Valeurs prises en charge : VALUE.
{: #ts_evaluation_method_not_supported}
 
Impossible de créer une configuration d'apprentissage.
{: tsSymptoms}
 
La méthode d'évaluation utilisée pour créer la configuration d'apprentissage est incorrecte.
{: tsCauses}
 
Utilisez une méthode d'évaluation prise en charge parmi les suivantes : `regression`, `binary`, `multiclass`.
{: tsResolve}
       
## Une seule évaluation active peut exister par modèle. La demande n'a pas pu être exécutée car une évaluation active existe déjà : {{url}}
{: #ts_active_evaluation_conflict}
 
Impossible de créer une autre itération d'apprentissage.
{: tsSymptoms}
 
Une seule évaluation peut être en cours d'exécution pour le modèle.
{: tsCauses}
 
Consultez l'évaluation déjà en cours ou attendez qu'elle se termine et démarrez une nouvelle évaluation.
{: tsResolve}
       
## Le type de déploiement {{type}} n'est pas pris en charge.
{: #ts_not_supported_deployment_type}
 
Impossible de créer le déploiement.
{: tsSymptoms}
 
Un type de déploiement non pris en charge a été utilisé.
{: tsCauses}
 
Vous devez utiliser un type de déploiement pris en charge.
{: tsResolve}
       
## Entrée incorrecte : ({{message}})
{: #ts_deserialization_error}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Un incident s'est produit lors de l'analyse json.
{: tsCauses}
 
Vérifiez que le json correct est transmise dans la requête.
{: tsResolve}
       
## Données insuffisantes - la métrique {{name}} n'a pas pu être calculée
{: #ts_missing_metric}
 
L'itération d'apprentissage a échoué.
{: tsSymptoms}
 
La valeur de la métrique avec le seuil défini n'a pas pu être calculée car les données de commentaire sont insuffisantes
{: tsCauses}
 
Vérifiez et améliorez les données dans la source de données `feedback_data_ref` dans `learning configuration`
{: tsResolve}
       
## Pour le type {{type}}, une instance Spark doit être fournie dans l'en-tête `X-Spark-Service-Instance`
{: #ts_missing_prediction_spark_definition}
 
Le déploiement ne peut pas être créé.
{: tsSymptoms}
 
Les déploiements `batch` et `streaming` nécessitent que l'instance Spark soit fournie.
{: tsCauses}
 
Fournissez une instance Spark dans l'en-tête `X-Spark-Service-Instance`.
{: tsResolve}
       
## L'action {{action}} a échoué avec le message {{message}}.
{: #ts_http_client_error}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Une erreur s'est produite lors de l'appel du service sous-jacent.
{: tsCauses}
 
Si une suggestion est fournie pour résoudre le problème, suivez-la. Contactez l'équipe de support si aucune suggestion n'est fournie dans le message ou si la suggestion ne résout pas
le problème.
{: tsResolve}
       
## Le chemin d'accès `{{path}}` n'est pas autorisé. Le seul chemin d'accès autorisé pour le flux de correction est `/status`
{: #ts_wrong_stream_patch_path}
 
Impossible de corriger le déploiement de flux.
{: tsSymptoms}
 
Le chemin d'accès utilisé pour corriger le déploiement `stream` est incorrect.
{: tsCauses}
 
Corrigez le déploiement `stream` avec l'option d'accès prise en charge : `/status` (autorise le démarrage/l'arrêt du traitement de flux).
{: tsResolve}
       
## L'opération de correction n'est pas autorisée pour l'instance de type `{{$type}}`
{: #ts_patch_not_supported}
 
Impossible de corriger le déploiement.
{: tsSymptoms}
 
Le type de déploiement en cours de correction est incorrect.
{: tsCauses}
 
Corrigez le type de déploiement `stream`.
{: tsResolve}
       
## La connexion de données `{{data}}` n'est pas valide pour feedback_data_ref
{: #ts_invalid_feedback_data_connection}
 
Impossible de créer `learning configuration` pour le modèle.
{: tsSymptoms}
 
Une source de données non prise en charge a été utilisée lors de la définition de feedback_data_ref.
{: tsCauses}
 
Utilisez uniquement le type de données pris en charge, à savoir `dashdb`
{: tsResolve}
       
## Le chemin d'accès {{path}} n'est pas autorisé. Le seul chemin d'accès autorisé pour le modèle de flux est `/deployed_version/url` ou `/deployed_version/href` pour V2
{: #ts_patch_model_path_not_allowed}
 
Aucune option n'existe pour la correction du modèle.
{: tsSymptoms}
 
Le chemin d'accès utilisé lors de la correction du modèle est incorrect.
{: tsCauses}
 
Corrigez le modèle avec le chemin d'accès pris en charge, qui permet de mettre à jour la version du modèle déployé.
{: tsResolve}
       
## Echec d'analyse : {{msg}}
{: #ts_parsing_error}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Le contenu demandé n'a pas pu être analysé correctement.
{: tsCauses}
 
Vérifiez que le contenu de votre requête est correct et peut être analysé correctement.
{: tsResolve}
       
## L'environnement d'exécution du modèle sélectionné, {{env}}, n'est pas pris en charge pour `learning configuration`. Environnements pris en charge :
[{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
Aucune option n'existe pour la création de `learning configuration`
{: tsSymptoms}
 
Le modèle pour lequel la création de `learning_configuration` a été tentée, n'est pas pris en charge.
{: tsCauses}
 
Créez `learning configuration` pour un modèle ayant un environnement d'exécution pris en charge.
{: tsResolve}
       
## Le plan en cours \'{{plan}}\' permet uniquement {{limit}} déploiements
{: #ts_deployments_plan_limit_reached}
 
Impossible de créer le déploiement.
{: tsSymptoms}
 
Le nombre de déploiements limite a été atteint pour le plan en cours.
{: tsCauses}
 
Effectuez une mise à niveau à un plan qui ne possède pas cette limitation.
{: tsResolve}
       
## La définition de la connexion à la base de données n'est pas valide ({{code}})
{: #ts_sql_error}
 
En cas d'impossibilité, utilisez la fonction `learning configuration`.
{: tsSymptoms}
 
La définition de connexion à la base de données n'est pas valide.
{: tsCauses}
 
Essayez de résoudre le problème décrit par le `code` renvoyé par la base de données sous-jacente.
{: tsResolve}
       
## Des problèmes se sont produits lors de la connexion au {{system}} sous-jacent
{: #ts_stream_tcp_error}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Un incident s'est produit lors de la connexion à un système sous-jacent. Il peut s'agir d'un problème de réseau temporaire.
{: tsCauses}
 
Essayez d'appeler à nouveau l'opération souhaitée. Si le problème persiste, contactez l'équipe de support technique.
{: tsResolve}
       
## Une erreur s'est produite lors de l'extraction de l'en-tête X-Spark-Service-Instance : ({{message}})
{: #ts_spark_header_deserialization_error}
 
Impossible d'appeler l'API REST qui nécessite les données d'identification Spark
{: tsSymptoms}
 
Un problème lié au décodage en base-64 ou à l'analyse des données d'identification Spark est survenu.
{: tsCauses}
 
Vérifiez que les données d'identification Spark correctes ont été correctement codées en base-64. Pour plus d'informations, consultez la documentation.
{: tsResolve}
       
## Cette fonctionnalité est interdite pour les utilisateurs non bêta.
{: #ts_not_beta_user}
 
L'API REST souhaité ne peut pas être appelé correctement.
{: tsSymptoms}
 
L'API REST appelé se trouve actuellement à l'état bêta.
{: tsCauses}
 
Si vous souhaitez participer, inscrivez-vous sur la liste d'attente. Vous trouverez les informations correspondantes dans la documentation.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
L'appel de l'API REST échoue.
{: tsSymptoms}
 
Une erreur s'est produite lors de l'appel du service sous-jacent.
{: tsCauses}
 
Si une suggestion est fournie pour résoudre le problème, suivez-la. Contactez l'équipe de support si aucune suggestion n'est fournie dans le message ou si la suggestion ne résout pas le
problème.
{: tsResolve}
       
## La limite de débit a été dépassée.
{: #ts_rate_limit_exceeded}
 
La limite de débit a été dépassée.
{: tsSymptoms}
 
La limite de débit du plan en cours a été dépassée.
{: tsCauses}
 
Pour résoudre ce problème, faites l'acquisition d'un nouveau plan avec une limite de débit supérieure
{: tsResolve}
       
## Valeur de paramètre de requête `{{paramName}}` non valide : {{value}}
{: #ts_invalid_query_parameter_value}
 
Erreur de validation en raison de la transmission d'une valeur incorrecte pour le paramètre de requête.
{: tsSymptoms}
 
Erreur lors de l'obtention d'un résultat pour la requête.
{: tsCauses}
 
Corrigez la valeur de paramètre de requête. Vous trouverez les informations correspondantes dans la documentation.
{: tsResolve}
       
## Type de jeton non valide : {{type}}
{: #ts_invalid_token_type}
 
Erreur relative au type de jeton.
{: tsSymptoms}
 
Erreur d'autorisation.
{: tsCauses}
 
Le jeton doit être démarré avec le préfixe `Bearer`
{: tsResolve}
       
## Format de jeton non valide. Le format de jeton bearer doit être utilisé.
{: #ts_invalid_token_format}
 
Erreur relative au format du jeton.
{: tsSymptoms}
 
Erreur d'autorisation.
{: tsCauses}
 
Le jeton doit être un jeton bearer et doit commencer avec le préfixe `Bearer`
{: tsResolve}

## Le fichier JSON d'entrée est manquant ou non valide : 400
{: #os_invalid_input}

Le message suivant s'affiche lorsque vous tentez une évaluation en ligne : **Le fichier JSON d'entrée est manquant ou non valide**.
{: tsSymptoms}

Ce message s'affiche lorsque le contenu d'entrée d'évaluation ne correspond pas au type d'entrée attendu requis pour l'évaluation du modèle. De manière plus spécifique, les raisons suivantes
peuvent être en cause :

- Le contenu d'entrée est vide.
- Le schéma du contenu d'entrée n'est pas valide.
- Les types de données d'entrée ne correspondent pas aux types de données attendus.
{: tsCauses}

Corrigez le contenu d'entrée. Assurez-vous que le contenu a la syntaxe correcte, un schéma valide et des types de données adéquats. Après avoir effectué vos corrections, tentez à
nouveau l'évaluation en ligne. En cas de problème de syntaxe, vérifiez le fichier JSON à l'aide de la commande `jsonlint`.
{: tsResolve}

## Le jeton d'autorisation a expiré : 401
{: #os_expired_authorization_token}

Le message suivant s'affiche lorsque vous tentez d'effectuer une évaluation en ligne : **Echec d'autorisation**.
{: tsSymptoms}

Ce message s'affiche lorsque le jeton utilisé pour l'évaluation a expiré.
{: tsCauses}

Régénérez le jeton pour cette instance {{site.data.keyword.pm_full}}, puis essayez à nouveau. Si le problème persiste, contactez le support IBM.
{: tsResolve}

## Identification de déploiement inconnue : 404
{: #os_unkown_depid}

Le message suivant s'affiche lorsque vous tentez une évaluation en ligne : **Identification de déploiement inconnue**. {: osSymptoms}

Ce message s'affiche lorsque l'ID de déploiement utilisé pour l'évaluation n'existe pas.
{: tsCauses}

Assurez-vous de fournir l'ID de déploiement correct. Dans le cas contraire, déployez le modèle avec l'ID de déploiement puis essayez de l'évaluer à nouveau.
{: tsResolve}

## Erreur de serveur interne : 500
{: #os_internal_error}

Le message suivant s'affiche lorsque vous tentez une évaluation en ligne : **Erreur de serveur interne**  {: osSymptoms}

Ce message s'affiche si le flux de données en aval dont dépend l'évaluation en ligne est défaillant.
{: tsCauses}

Attendez un certain temps puis renouvelez votre tentative d'évaluation en ligne. En cas de nouvel échec, contactez le support IBM.
{: tsResolve}
              
