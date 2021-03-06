---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# チュートリアルの概説
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} は [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com) と統合されています。
{: shortdesc}

[{{site.data.keyword.DSX_full}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics) で機械学習および人工知能について学習します。

{{site.data.keyword.pm_full}} は、機械学習の 2 つの基本的な工程であるトレーニングとスコアリングをユーザーが実行することを可能にします。

- **トレーニング**は、アルゴリズムを洗練させてデータ・セットから学習できるようにするプロセスです。 この工程の出力をモデルと呼びます。 モデルには学習した数式の係数が包含されます。
- **スコアリング**は、トレーニングされたモデルを使用して成果を予測する工程です。 スコアリング工程の出力は、予測された値が含まれる別のデータ・セットです。

{{site.data.keyword.pm_full}} は、主として以下の 2 種類の人物のニーズに対応するよう設計されています。

- データ・サイエンティスト: データ変換および機械学習アルゴリズムを利用する機械学習パイプラインを作成します。 一般的に、データ・サイエンティストはノートブックまたは外部ツールを使用してモデルのトレーニングと評価を行います。 データ・サイエンティストは、データ・エンジニアと共同でデータの探索と理解のための作業を行うことがよくあります。
- 開発者: 機会学習モデルによる予測出力を使用するインテリジェント・アプリケーションを作成します。

トレーニングは機械学習プロセスにおける重要なステップですが、{{site.data.keyword.pm_full}} を使用すると、モデルをデプロイし、長期間にわたって、モデルのすべてのイテレーションを通して、モデルから実際のビジネスの価値を引き出すことによって、モデルの機能を整備することができます。

## 製品情報

{{site.data.keyword.pm_full}} サービスは、任意のプログラミング言語から呼び出すことのできる REST API セットです。

{{site.data.keyword.pm_full}} サービスはデプロイメントに重点を置いていますが、IBM® SPSS® Modeler または {{site.data.keyword.DSX_full}} を使用して、モデルおよびパイプラインを作成して操作することができます。Spark MLlib および Python scikit-learn を使用する SPSS® Modeler と {{site.data.keyword.DSX_full}} はどちらも、機械学習、人工知能、統計学から得られたさまざまなモデリング手法を提供します。

## 関連リンク

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_spark.html) または [SPSS モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

