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

# ストリーミング・モデルのデプロイ

{{site.data.keyword.pm_full}} サービスを使用して、モデルをデプロイし、デプロイされたストリーミング・モデルに対してスコアリング要求を行うことによって予測分析を生成することができます。
{: shortdesc}

**シナリオ名**: 感情分析。

**シナリオの説明**: あるマーケティング代理店が、特定のトピックに関する感情を把握したいと考えています。その代理店は、発言を肯定または否定に分類するモデルの開発を依頼したいと考えています。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、予測分析を生成することです。

詳細については、この[文書](https://github.com/pmservice/tweet-sentiment-prediction)を参照してください。

## 前提条件

この例を使用して作業するには、以下のリソースが必要です。

* [Message Hub](https://console.bluemix.net/catalog/services/message-hub) トピックの詳細。モデルの入力 (ツイート・テキスト)、およびモデルの出力 (予測結果) のストレージとして使用されます。2 つのトピック (ツイート・テキストの入った入力トピックと、出力トピック) が作成されることを確認してください。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) サービス・インスタンス資格情報。[このリンク](https://console.bluemix.net/catalog/services/apache-spark)を使用して作成できます。


## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル (Sample Models)」**セクションで**「Sentiment Prediction」**タイルを見つけて、「モデルの追加」アイコン (+) をクリックします。

「モデル (Models)」タブの使用可能なモデルのリストに、サンプルの「Sentiment Prediction」モデルが表示されます。


## IBM Message Hub を使用するストリーミング・デプロイメントの作成

1.  {{site.data.keyword.pm_full}} ダッシュボードの**「モデル」**タブに移動します。
2.  **「アクション」**メニューから、**「デプロイメントの作成」**をクリックします。
3.  **「デプロイメントの作成 (Create Deployment)」**フォームの**「名前 (Name)」**、**「説明 (Description)」**、および**「ストリーム・タイプ (Stream Type)」**フィールドに入力します。
4.  以下を入力する必要があります。

    **入力接続**: モデルの入力 (ツイート)、およびモデルの出力 (予測結果) のストレージとして使用される、IBM Message Hub トピックの詳細。

    ```
  {
     "connection":{
         "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
         ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
   "source":{
      "topic":"sinput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **出力接続**

    ```
 {
    "connection":{
         "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
         ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
      "target":{
         "topic":"soutput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **Spark 接続**: Spark サービス資格情報は、{{site.data.keyword.Bluemix_notm}} Spark サービス・ダッシュボードの「サービス資格情報」タブにあります。

     ```
{
     "credentials":{
            "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "cluster_master_url": "https://spark.bluemix.net",
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
      "plan": "ibm.SparkService.PayGoPersonal"
},
         "version":"2.0"
      }
     ```
     {: codeblock}

5. 「保存」をクリックします。

予測結果は、定義された MessageHub トピック (出力接続) に送信されます。

## デプロイメント詳細の取得

デプロイされたモデルに関連する状況およびパラメーターを確認できます。

1. {{site.data.keyword.pm_full}} ダッシュボードの「デプロイメント」タブに移動します。

2. 「アクション」メニューから「詳細の表示」をクリックします。

## ストリーミング・デプロイメントの削除

以下の例のような照会を実行して、不要になったデプロイメントを削除できます。

1. {{site.data.keyword.pm_full}} ダッシュボードの「デプロイメント」タブに移動します。

2. 「アクション」メニューから「削除」をクリックします。

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html) または [IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html) を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API](pm_service_api_spss.html) を参照してください。
IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。
IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
