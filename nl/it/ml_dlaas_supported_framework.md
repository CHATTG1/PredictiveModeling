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

# Framework supportati

Il servizio IBM Watson Machine Learning supporta diversi framework di apprendimento approfondito e un'ampia gamma di versioni per il tuo progetto di apprendimento approfondito.
{: shortdesc}

## Elenco dei framework di apprendimento approfondito supportati

Per ottenere un elenco aggiornato di framework, che include i nomi e le versioni, utilizza l'[interfaccia riga di comando](ml_dlaas_environment.html) per eseguire il seguente comando:

```
bx ml list frameworks
```
{: codeblock}

Output di
esempio:

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

**Nota**: se il framework stesso è programmato nel linguaggio Python, la versione deve includere l'estensione `-py2` o `-py3` per indicare se deve essere utilizzato python2 o python3.

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
