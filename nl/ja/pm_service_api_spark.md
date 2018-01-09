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

# REST API

{{site.data.keyword.pm_short}} サービスは、任意のプログラミング言語から呼び出すことができ、アプリケーション内での Data Science Experience 分析の統合を可能にする、REST API セットです。{{site.data.keyword.Bluemix_short}} アプリケーションを {{site.data.keyword.pm_short}} サービス・インスタンスにバインドし、お使いのアプリケーションがユーザーに高い価値を提供するために必要な予測分析を生成します。[管理ダッシュボード](pm_service_ui_spark.html)でモデルを管理し、そのダッシュボードを使用して、アプリケーションと統合されたオンライン・デプロイメント、バッチ・デプロイメント、またはストリーミング・デプロイメントを作成します。
{: shortdesc}

強力な [REST API](https://watson-ml-api.mybluemix.net/) を使用して、サービス・インスタンスにデプロイされた Spark モデル、Python モデル、および IBM® SPSS® モデルに対してアプリケーションを開発します。

*  特定の予測モデル用のメタデータの取得
*  モデルのデプロイおよびデプロイされたモデルの管理
    *  オンライン・デプロイメント (スコアリング)
    *  バッチ・デプロイメント (IBM Cloud オブジェクト・ストレージおよび {{site.data.keyword.dashdbshort}} をサポート)
    *  ストリーム・デプロイメント (IBM Cloud Message Hub をサポート)
*  特定のデプロイメント用のメタデータの取得
*  継続学習システムの使用によるデプロイされたモデルのモニターとリトレーニング
*  デプロイされたモデルにスコアリング要求を出すことによる予測分析の生成

詳しくは、以下の REST API 使用例を参照してください。

*  [オンライン・モデルのデプロイ](pm_service_api_spark_online.html)
*  [オンライン・モデルのスコアリング](pm_service_api_develop_score.html)
*  [バッチ・モデルのデプロイ](pm_service_api_spark_batch.html)
*  [ストリーム・モデルのデプロイ](pm_service_api_spark_streaming.html)
*  [継続学習システム](pm_service_api_spark_learning_system.html)

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
