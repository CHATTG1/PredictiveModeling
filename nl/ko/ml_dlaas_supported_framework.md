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

# 지원되는 프레임워크

IBM Watson Machine Learning 서비스는 다양한 딥 러닝 프레임워크와 여러 사용자 딥 러닝 프로젝트 버전을 지원합니다.
{: shortdesc}

## 지원되는 딥 러닝 프레임워크 목록

이름 및 버전을 포함하는 최신 프레임워크 목록을 얻으려면 [명령행 인터페이스](ml_dlaas_environment.html)를 사용하여 다음 명령을 실행하십시오. 

```
bx ml list frameworks
```
{: codeblock}

샘플 출력:

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

**참고**: 프레임워크 자체가 Python 언어로 프로그래밍된 경우에는 사용해야 하는 항목(python2 또는 python3)을 표시하기 위해 버전이 `-py2` 또는 `-py3` 확장자를 포함해야 합니다. 

<!-- Models trained using the following frameworks can be additionally be deployed (deployment support for other frameworks will be added):-->

<!-- * Tensorflow (with Keras 2) versions **1.2-py3**
-->
