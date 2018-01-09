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

# サポートされるフレームワーク

IBM Watson Machine Learning サービスは、お客様の深層学習プロジェクト用に、各種の深層学習フレームワークと広範なバージョンをサポートします。
{: shortdesc}

## サポートされる深層学習フレームワークのリスト表示

フレームワークの名前とバージョンが含まれた最新のフレームワーク・リストを取得するには、[コマンド・ライン・インターフェース](ml_dlaas_environment.html)を使用して次のコマンドを実行します。

```
bx ml list frameworks
```
{: codeblock}

サンプル出力:

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

**注:** フレームワーク自体が Python 言語でプログラムされている場合、python2 と python3 のどちらが使用される必要があるのかを示すため、バージョンには `-py2` または `-py3` 拡張子が含まれている必要があります。

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
