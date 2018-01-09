---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 概説
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} は、機械学習の 2 つの基本的な工程であるトレーニングとスコアリングをユーザーが実行することを可能にする IBM Cloud サービスです。
{: shortdesc}

- **トレーニング**は、アルゴリズムを洗練させてデータ・セットから学習できるようにするプロセスです。この工程の出力をモデルと呼びます。モデルには学習した数式の係数が包含されます。
- **スコアリング**は、トレーニングされたモデルを使用して成果を予測する工程です。スコアリング工程の出力は、予測された値が含まれる別のデータ・セットです。

{{site.data.keyword.pm_full}} は、主として以下の 2 種類の人物のニーズに対応するよう設計されています。

- データ・サイエンティスト: データ変換および機械学習アルゴリズムを利用する機械学習パイプラインを作成します。一般的に、データ・サイエンティストはノートブックまたは外部ツールを使用してモデルのトレーニングと評価を行います。データ・サイエンティストは、データ・エンジニアと共同でデータの探索と理解のための作業を行うことがよくあります。
- 開発者: 機会学習モデルによる予測出力を使用するインテリジェント・アプリケーションを作成します。

トレーニングは機械学習プロセスにおける重要なステップですが、{{site.data.keyword.pm_full}} を使用すると、モデルをデプロイし、長期間にわたって、モデルのすべてのイテレーションを通して、モデルから実際のビジネスの価値を引き出すことによって、モデルの機能を整備することができます。

## 前提条件

{{site.data.keyword.pm_full}} を使用するには、{{site.data.keyword.Bluemix_short}} カタログから、[サービス・インスタンス](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/)を作成する必要があります。このセットアップにより、以下のタスクを実行できるようになります。

## 手順

1. [Machine Learning 環境をセットアップします](ml_getting_access.html)。
1. [モデルを作成して保管します](pm_custom_models.html)。
2. [モデルをデプロイします](pm_service_api_spark_online.html)。
3. デプロイされたモデル `scoring endpoint` をアプリケーションで使用して、[予測を取得します](pm_service_api_spark_building.html)。

## Machine Learning と Data Science Experience の併用

{{site.data.keyword.pm_full}} は IBM Data Science Experience と統合されています。Data Science Experience ノートブックで Machine Learning API クライアント・ライブラリーを使用できますが、モデル・ビルダーおよびフロー・エディターを使用するには、Machine Learning インスタンスがある必要があります。

## Machine Learning と SPSS Modeler の併用

{{site.data.keyword.pm_full}} は IBM® SPSS® Modeler と統合されています。Machine Learning API を使用して、高機能の数学的アルゴリズムを利用できます。


## ご使用の環境での Machine Learning の使用

{{site.data.keyword.pm_full}} を、ローカル環境をクラウドとリンクする混成ソリューションとして使用できます。Machine Learning API を使用して、モデルの公開、デプロイ、およびスコアリングを実行できます。詳しくは、[DSX: Hybrid Mode](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b) を参照してください。

## 製品情報

{{site.data.keyword.pm_full}} サービスは、任意のプログラミング言語から呼び出すことのできる REST API セットです。


{{site.data.keyword.pm_full}} サービスはデプロイメントに重点を置いていますが、IBM® SPSS® Modeler または IBM® Data Science Experience を使用して、モデルおよびパイプラインを作成して操作することができます。Spark MLlib および Python scikit-learn を使用する SPSS® Modeler と Data Science Experience はどちらも、機械学習、人工知能、統計学から得られたさまざまなモデリング手法を提供します。

## 関連リンク

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[SPSS モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [SPSS モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
