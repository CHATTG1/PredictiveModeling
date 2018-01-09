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

Le service IBM Watson Machine Learning prend en charge différentes infrastructures d'apprentissage en profondeur, ainsi que plusieurs versions d'apprentissage en profondeur pour votre projet.
{: shortdesc}

## Liste des infrastructures d'apprentissage en profondeur prises en charge

Pour obtenir une liste à jour des infrastructures, avec les noms et les versions, utilisez l'[interface de ligne de commande](ml_dlaas_environment.html) pour exécuter la commande suivante :

```
bx ml list frameworks
```
{: codeblock}

Exemple de sortie :

```
Fetching the list of frameworks ...
SI No   Name           Version
1       scikit-learn   0.17
2       tensorflow     1.2-py2
3       tensorflow     1.2-py3
4       tensorflow     1.3-py2
5       tensorflow     1.3-py3
6       caffe          1.0-py2
7       torch          luajit
8       torch          lua52
9       mxnet          0.10
10     theano         0.10
11     caffe2         0.8
OK
List frameworks successful
```

**Remarque** : Si l'infrastructure est elle-même programmée en langage Python, la version doit inclure l'extension `-py2` ou `-py3` pour indiquer si python2 ou python3 doit être utilisé.

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
