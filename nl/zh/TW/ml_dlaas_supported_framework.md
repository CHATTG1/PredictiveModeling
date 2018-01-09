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

# 支援的架構

IBM Watson Machine Learning 服務支援不同的深度學習架構以及某範圍的深度學習專案版本。
{: shortdesc}

## 列出支援的深度學習架構

若要取得包括名稱及版本的最新架構清單，請使用[指令行介面](ml_dlaas_environment.html)來執行下列指令：

```
bx ml list frameworks
```
{: codeblock}

輸出範例：

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

**附註**：如果架構本身是以 Python 語言進行程式設計，則版本必須包括 `-py2` 或 `-py3` 副檔名，以指出應該使用 python2 還是 python3。

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
