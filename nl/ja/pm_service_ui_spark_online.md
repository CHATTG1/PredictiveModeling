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

# オンライン・モデルのデプロイ

モデルをデプロイし、デプロイされたモデルに対してスコアリング要求を行うことによって予測分析を生成するには、{{site.data.keyword.pm_full}} サービスを使用します。次のシナリオで、これを行う方法の例を示します。
{: shortdesc}

**シナリオ名**: 製品ライン予測。

**シナリオの説明**: アウトドア用の備品を販売する企業が、自社の製品ラインに対する顧客の関心を予測するモデルを作成したいと考えています。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコアリング要求を行うことにより、顧客の興味の予測を生成することです。

## サンプル・モデルの使用

1. {{site.data.keyword.pm_full}} ダッシュボードの**「サンプル」**タブに移動します。
2. **「サンプル・モデル (Sample Models)」**セクションで**「Product Line Prediction」**タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。

**「モデル (Models)」**タブの使用可能なモデルのリストに、サンプルの**「Product Line Prediction」**モデルが表示されます。


## オンライン・デプロイメントの作成

1. {{site.data.keyword.pm_full}} ダッシュボードの**「モデル」**タブに移動します。
2. **「アクション」**メニューから、**「デプロイメントの作成」**をクリックします。
3. **「デプロイメントの作成 (Create Deployment)」**フォームの**「名前 (Name)」**、**「説明 (Description)」**、および**「オンライン・タイプ (Online Type)」**フィールドに入力します。
4. **「保存 (Save)」**をクリックします。

**「デプロイメント」**タブの使用可能なデプロイメントのリストに、オンライン・デプロイメントが表示されます。

## デプロイメントの詳細の取得

デプロイされたモデルに関連する状況、スコアリング・エンドポイント・アドレス (`Scoring Endpoint`)、およびパラメーターを確認できます。

1. {{site.data.keyword.pm_full}} ダッシュボードの**「デプロイメント」**タブに移動します。
2. **「アクション」**メニューで**「詳細の表示」**をクリックします。

`Scoring Endpoint` 値はスコアリング要求を行うために必要なのでメモしておいてください。


## スコアリング要求の実行

スコアリング・エンドポイントを作成した後、スコアリング要求を行うことによって予測を生成できます。次のシナリオでは、顧客レコードが予測モデルに渡され、スポーツ製品予測が返されます。

サンプル・レコード・ヘッダー:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION```
{: codeblock}

サンプル・レコード:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

要求の例:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

出力例:

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

例えば、55 歳の会社役員は「登山用備品 (Mountaineering Equipment)」に関心があり、一方、23 歳の学生は「個人用アクセサリー (Personal Accessories)」に関心があることが分かります。

## 詳細はこちら

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM® Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
