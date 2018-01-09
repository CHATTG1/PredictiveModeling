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

# 継続学習システム

{{site.data.keyword.pm_full}} 継続学習システムは、予測品質を確保するため、モデル・パフォーマンスの自動モニタリング、リトレーニング、および再デプロイメントを提供します。
{: shortdesc}

**シナリオ名**: 最適な心臓治療薬品の選択。

**シナリオの説明**: 心臓用の医薬品を製造するバイオ医薬品会社が、心臓治療のために最適な薬品を選択するモデルをデプロイしたいと考えています。薬品の有効性に関する新しいエビデンスが出現した際には、モデル更新が検討される必要があります。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、learning configuration を設定し、learning iteration を実行して、モデルの評価、リトレーニング、および再デプロイを行うことです。

## 前提条件

この例を使用して作業するには、以下のサービスが必要です。

* フィードバック・データを保管するための [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) サービス

   まだ Apache Spark インスタンスがない場合は、[このリンク](https://console.bluemix.net/catalog/services/apache-spark)を使用して作成してください。

## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル」**セクションで**「Heart Drug Selection」**タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。

**「モデル (Models)」**タブの使用可能なモデルのリストに、サンプルの「Heart Drug Selection」モデルが表示されます。


## 公開されたモデルの継続学習システムの設定

1.  {{site.data.keyword.pm_full}} ダッシュボードの**「モデル」**タブに移動します。
2.  **「アクション」**メニューで**「詳細の表示」**をクリックします。
3.  **「リトレーニング詳細」**までスクロールダウンし、**「編集」**を押します。
4.  以下を入力する必要があります。

    **フィードバック接続**: モデルの評価およびリトレーニングのための入力 (フィードバック・データ) として使用される、{{site.data.keyword.dashdbshort}} の詳細。

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
        "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    以下の説明に従って、{{site.data.keyword.dashdbshort}} に `DRUG_FEEDBACK_DATA` 表を作成してください。
    
    - [{{site.data.keyword.dashdbshort}} サービス](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)のインスタンスを作成します (入門プランが提供されています)。
    - **{{site.data.keyword.dashdbshort_notm}}** に **DRUG_FEEDBACK_DATA** 表を作成します。
      + GitHub リポジトリーから [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) ファイルをダウンロードします。
      + **「コンソールを開く」**をクリックし、**「{{site.data.keyword.dashdbshort_notm}}」**で開始します。
      + **「データのロード」**をクリックし、**「デスクトップ」**ロード・タイプを選択します。
      + 前にダウンロードしたファイルをロード域まで**ドラッグ**し、**「次へ」**をクリックします。
      + データをインポートする**スキーマ**を選択し、**「新規表」**をクリックします。
      + 新規表の名前を入力し、**「次へ」**をクリックします。
      + **「フィールド分離文字」**としてセミコロン (;) を使用します。
      + **「次へ」**をクリックして、アップロードしたデータで表を作成します。

     **注:** [REST API エンドポイント](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)を使用して、新規フィードバック・レコードをフィードバック・データベースに追加できます。

     **最小データ・サイズ**: モデルの評価 (学習システムのイテレーションの一部) を開始するための、フィードバック・データ・セット内のレコードの最小数。

     ```
    20
    ```
     {: codeblock}

     **Spark 接続**: Spark サービス資格情報は、{{site.data.keyword.Bluemix_notm}} Spark サービス・ダッシュボードの**「サービス資格情報」**タブにあります。

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

    **しきい値**: リトレーニング・プロセスをトリガーするしきい値。メトリック値 (例えば、モデル評価中に計算される正確度の値) がしきい値より小さい場合、リトレーニングがトリガーされます。

     ```
    0.8
    ```

     **リトレーニングされたモデルの自動デプロイ**: デプロイされているモデルよりもリトレーニングされたモデルのほうがメトリック値が優れている場合、リトレーニングされたモデルをデプロイします。

5.  **「セットアップ」**をクリックします。

## 継続学習システムのイテレーションの実行

公開されたモデルは、反復して評価されます。メトリック値 (例えば、モデル評価中に計算される正確度の値) がしきい値より小さい場合、リトレーニングがトリガーされます。両方のデータ・セット (トレーニングとフィードバック) が、リトレーニングおよび評価のこの反復プロセスに使用されます。

1. 新規イテレーションを開始するには、モデルの**「詳細」**フォームから、**「モニター」**タブで**「評価」**をクリックします。
3. イテレーション結果を確認するには、「Evaluation Events」表または「グラフ」ビューのいずれかに移動します。 

   フィードバック・データに基づいて計算されたモデル品質についての情報を表示できます (モニタリング)。 また、作成され、評価された、新しいモデル・バージョンである、リトレーニングされたモデル品質についての情報も表示できます。

4. 自動的にトレーニングされたモデルとそれらの品質のリストを表示するには、**「詳細を表示」** > **「概要」**をクリックし、**「バージョン履歴 (Version History)」**セクションに移動します。

## フィードバック・データ・ストア

[フィードバック・エンドポイント](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)を使用して、machine learning configuration に定義されているフィードバック・ストアに新規レコードを送信できます。現在、フィードバック・データ・ストアとして {{site.data.keyword.dashdbshort}} がサポートされています。フィードバック表が存在しない場合、当サービスによって作成されます。表が存在する場合、スキーマが検証されてトレーニング表のスキーマと一致しているか確認され、`_training` という名前の列が表に付加されます。この `_training` 列は、リトレーニング・プロセスでコンシュームされたレコードをマークするために使用されます。

フィードバック・データ・ストアの詳しい説明および例については、[API 資料](pm_service_api_spark_learning_system.html)を参照してください。

**注:** フィードバック表は、{{site.data.keyword.pm_full}} サービスによって、管理、変更、および使用されます。

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
