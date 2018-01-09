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

# カスタム・モデルの開発および永続性

{{site.data.keyword.pm_full}} サービスを使用して、モデルをデプロイし、デプロイされたモデルに対してスコアリング要求を行うことによって予測分析を生成することができます。
{: shortdesc}

## カスタム・モデルの処理

### シナリオ名: 製品ライン予測。

### シナリオの説明:

クライアントは、ヨーロッパで最も有名なチェーン・ストアの 1 つを経営しています。そのクライアントは、個人用装飾品、キャンプ用品、およびアウトドア用保護用品などの製品ラインの観点で顧客の興味を把握してほしいと考えています。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、予測分析を生成することです。

### カスタム・モデルの開発および永続性

#### Data Science Experience の使用

[Data Science Experience](https://console.bluemix.net/catalog/services/data-science-experience) を使用して、カスタム・モデルを作成します。登録後、サインインして以下の手順を実行する必要があります。

1. 組織およびスペースを作成します。初回のサインイン時に、この情報を指定する必要があります。**「続行」**をクリックして、デフォルト値を受け入れます。
2. 組織が作成された後、**「プロジェクト」**に移動し、**「新規プロジェクト」**をクリックします。
3. プロジェクトの名前および説明を入力し、**「作成」**をクリックします。指定したプロジェクト名は、ターゲット・コンテナー名として使用されます。
4. プロジェクトが作成された後、以下のいずれかのタスクを実行できます。
   
   *  **ノートブックの追加**を行い、独自のモデルの開発を開始するか、以下のサンプル・ノートブックのいずれかをアップロードします。
        *  [Python を使用した Spark MLlib モデルの開発](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Scala を使用した Spark MLlib モデルの開発](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Python を使用した scikit-learn モデルの開発](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **モデルの追加**を行い、モデル・ウィザードを使用して独自のモデルの開発を開始します


#### ローカル環境の使用

任意に選択した環境を使用してモデルを開発し、後で、pypi で使用可能な Watson Machine Learning [共通 API クライアント・ライブラリー]() を使用して、公開、デプロイ、およびスコアリングを行うことができます。
クライアント・ライブラリーについて詳しくは、サンプル・[ノートブック](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125)および[資料](pm_service_client_library.html)を参照してください。

**注:** 共通 API クライアント・ライブラリーは**ベータ版**です。

### カスタム・モデルのデプロイメントおよびスコアリング

API を使用して、オンライン・モデル、バッチ・モデル、およびストリーミング・モデルをデプロイし、スコアリングすることができます。

*  [オンライン・モデルのデプロイ](pm_service_api_spark_online.html)
*  [バッチ・モデルのデプロイ](pm_service_api_spark_batch.html)
*  [ストリーミング・モデルのデプロイ](pm_service_api_spark_streaming.html)

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM® Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
