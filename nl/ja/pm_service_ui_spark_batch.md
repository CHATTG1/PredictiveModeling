---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# バッチ・モデルのデプロイ (<span class='tag--beta'>ベータ</span>)

**注**: この機能は、現在はベータ版であり、Spark MLlib でのみ使用可能です。参加をご希望の場合は、ご自身を待機リストに追加してください。
詳しくは、[https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist) を参照してください。

**シナリオ名**: 顧客満足度予測。

**シナリオの説明**: ある通信会社が、解約する恐れがあるのはどの顧客であるかを知りたいと考えています。提供されたモデルは、顧客のチャーン (契約/解約を繰り返す顧客の流動現象) を予測します。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコア要求を行うことにより、予測分析を生成することです。

## サンプル・モデルの使用

1.  IBM® Watson™ Machine Learning ダッシュボードの「サンプル」タブに移動します。

2.  「サンプル・モデル (Sample Models)」セクションで、「Customer Satisfaction Prediction」タイルを見つけて、「モデルの追加 (Add model)」ボタン (+) をクリックします。

これで、「モデル (Models)」タブの使用可能なモデルのリストに、サンプルの「Customer Satisfaction Prediction」モデルが表示されます。

## オブジェクト・ストレージを使用するバッチ・デプロイメントの作成

1.  IBM® Watson™ Machine Learning ダッシュボードの「モデル」タブに移動します。

2.  **「アクション」**メニューから、**「デプロイメントの作成」**をクリックします。

3.  「デプロイメントの作成」フォームで、名前、説明、およびバッチ・タイプを指定します。

4.  以下を入力する必要があります。

    **入力接続**: モデルの入力 (スコアリングする顧客データ)、およびモデルの出力 (この場合は results.csv であり、自動的に作成される) のストレージとして使用される、オブジェクト・ストレージの詳細。

    ```
       {
          "source":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"TelcoCustomerData.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **出力接続**

    ```
       {
          "target":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"result.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Spark 接続**: Spark サービス資格情報は、Bluemix Spark サービス・ダッシュボードの「サービス資格情報」タブにあります。

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

5.  「保存」をクリックします。

予測結果は、IBM オブジェクト・ストレージ内の .csv ファイルに保存されます。サンプル行を以下に示します。

入力ファイルのプレビュー:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

出力ファイルのプレビュー:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## デプロイメント詳細の取得

デプロイされたモデルに関連する状況およびパラメーターを確認できます。

1. IBM® Watson™ Machine Learning ダッシュボードの「デプロイメント」タブに移動します。

2. 「アクション」メニューから「詳細の表示」を選択します。


## バッチ・デプロイメントの削除

以下の例のような照会を使用して、不要になったデプロイメントを削除できます。

1. IBM® Watson™ Machine Learning ダッシュボードの「デプロイメント」タブに移動します。

2. 「アクション」メニューから「削除」を選択します。
