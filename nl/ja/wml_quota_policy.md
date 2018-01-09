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

# 割り当て量ポリシー

{{site.data.keyword.pm_full}} エンジンは、リソースの割り当ておよび使用量が適切になるように、インスタンスごとに割り当て量を設定します。これらのポリシーは、アカウント・プロファイル、サービス使用量履歴、およびリソースの可用性に応じて変わります。割り当て量ポリシーは、価格設定プランでは定義されていない制限を記述するものであり、**通知なしで変更されることがあります**。現在アクティブなサービス割り当て量制限は以下のとおりです。
{: shortdesc}

## リソース使用量

リソースは以下の最大値に制限されています。

-  **公開されたモデルの最大数**: 200 (ライト・プラン) 1000 (標準プランおよびプロフェッショナル・プラン)
-  **Spark ML モデルの最大サイズ**: 200 MB
-  **デプロイされたモデルの最大数**: 1000 (標準プラン & プロフェッショナル・プラン)

**注:** ライト・プランの制限を理解するには、[{{site.data.keyword.Bluemix_notm}} カタログ](https://console.bluemix.net/catalog/)の[ライト・プラン](https://console.bluemix.net/catalog/services/machine-learning)の説明を参照してください。

## サービス要求の制限

特定の API に対して 1 分当たりに行うことのできる要求の数には制限があります。より大きな限度を必要とするアプリケーションを使用する場合、[{{site.data.keyword.Bluemix_notm}} サポートに連絡](https://support.ng.bluemix.net/)して割り当て量を増やす必要があります。

## デプロイメント要求

デプロイメント API 要求には、以下の制限が適用されます。

-  **期間**: 60 秒
-  **デフォルト制限**: 10

## オンライン予測要求

[オンライン予測](pm_service_api_spark_building.html)要求には、以下の制限が適用されます。

-  **期間**: 60 秒
-  **デフォルト制限**: 500

## リソース管理要求

次のリスト中の要求を結合した合計に対して、以下の制限が適用されます。

[トークン](https://watson-ml-api.mybluemix.net/#/Token): post 要求、get 要求、delete 要求、put 要求、patch 要求

[デプロイメント](https://watson-ml-api.mybluemix.net/#/Deployments): post 要求、get 要求、delete 要求、put 要求、patch 要求

-  **期間**: 60 秒
-  **デフォルト制限**: 100

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM® Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
