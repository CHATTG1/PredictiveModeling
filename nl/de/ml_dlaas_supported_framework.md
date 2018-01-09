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

# Unterstützte Frameworks

Der IBM Watson Machine Learning-Service unterstützt unterschiedliche Deep-Learning-Frameworks sowie einen Bereich von Versionen für Ihr Deep-Learning-Projekt.
{: shortdesc}

## Unterstützte Deep-Learning-Frameworks auflisten

Führen Sie über die [Befehlszeilenschnittstelle](ml_dlaas_environment.html) den folgenden Befehl aus, um eine aktuelle Liste mit Frameworks abzurufen, die Namen und Versionen enthält:

```
bx ml list frameworks
```
{: codeblock}

Beispielausgabe:

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

**Hinweis**: Wenn das Framework selbst in der Python-Sprache programmiert wurde, muss die Version die Erweiterung `-py2` oder `-py3` enthalten, um anzugeben, ob Python 2 oder Python 3 verwendet werden muss.

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
