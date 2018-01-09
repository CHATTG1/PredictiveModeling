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

# 支持的框架

IBM Watson Machine Learning 服务支持不同的深度学习框架和各种版本的深度学习项目。
{: shortdesc}

## 列出支持的深度学习框架

要获取最新的框架列表（包括名称和版本），请使用[命令行界面](ml_dlaas_environment.html)来运行以下命令：

```
bx ml list frameworks
```
{: codeblock}

样本输出：

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

**注**：如果框架本身是用 Python 语言编程的，那么版本必须包含 `-py2` 或 `-py3` 扩展名，以指示应该使用 python2 还是 python3。

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
