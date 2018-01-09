---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Estruturas suportadas

O serviço IBM Watson Machine Learning suporta diferentes estruturas de aprendizado profundo e uma faixa de versões para seu projeto de aprendizado profundo.
{: shortdesc}

## Listando estruturas de aprendizado profundo suportadas

Para obter uma lista atualizada de estruturas, que inclui nomes e versões, use a
[interface da linha de comandos](ml_dlaas_environment.html) para
executar o comando a seguir:

```
bx ml list frameworks
```
{: codeblock}

Amostra da saída:

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

**Nota**: Se a estrutura em si for programada na linguagem Python, a versão deverá incluir a extensão `-py2` ou `-py3` para
indicar se python2 ou python3 deve ser usado.

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
