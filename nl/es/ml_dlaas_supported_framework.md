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

# Infraestructuras soportadas

El servicio de IBM Watson Machine Learning da soporte a distintas infraestructuras de aprendizaje profundo y a un rango de versiones para su proyecto de aprendizaje profundo.
{: shortdesc}

## Listado de las infraestructuras de aprendizaje profundo soportadas

Para obtener una lista actualizada de infraestructuras, que incluya los nombres y las versiones, utilice la [interfaz de línea de mandatos](ml_dlaas_environment.html) para ejecutar el mandato siguiente:

```
bx ml list frameworks
```
{: codeblock}

Salida de ejemplo:

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

**Nota**: Si la propia infraestructura está programada en el lenguaje Python, la versión debe incluir la extensión `-py2` o `-py3` para indicar si se debe utilizar python2 o python3.

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
