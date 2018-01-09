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

# 予測分析アプリケーションの作成

{{site.data.keyword.pm_full}} サービスを使用して、Node.js アプリケーションをデプロイすることができます。
{: shortdesc}

**シナリオ名**: 製品ライン予測。

**シナリオの説明**: クライアントは、ヨーロッパで最も有名なチェーン・ストアの 1 つを経営しています。そのクライアントは、個人用装飾品、キャンプ用品、およびアウトドア用保護用品などの製品ラインの観点で顧客の興味を把握してほしいと考えています。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、デプロイ済みモデルに対してスコアリング要求を行うことにより、スポーツ製品ラインを推奨する Node.js アプリケーションを準備してデプロイすることです。

1. {{site.data.keyword.Bluemix_short}} にログオンし、{{site.data.keyword.pm_full}} のインスタンスを作成します。
2. {{site.data.keyword.pm_full}} ダッシュボードを起動します。
3. **「サンプル」**タブに移動します。
4. **「サンプル・モデル (Sample Models)」**セクションで**「Product Line Prediction」**タイルを見つけて、**「モデルの追加」**アイコン (+) をクリックします。**「モデル (Models)」**タブの使用可能なモデルのリストに、サンプルの「Product Line Prediction」モデルが表示されます。

   **注**: サンプルの代わりに独自の Data Science Experience プロジェクトおよびモデルを使用する場合は、{{site.data.keyword.pm_short}} リポジトリー内でカスタム・モデルを永続化する必要があります。これを行うには、REST API またはクライアント・ライブラリーを使用します。詳しくは、『カスタム・モデルの開発および永続性』を参照してください。

5. **「アクション」**メニューから、**「デプロイメントの作成 (Create Deployment)」**をクリックして、Product Line Prediction モデルをオンライン・デプロイメントとしてデプロイします。
6. デプロイメント名 (例えば「Product line prediction」など) を指定し、「保存」をクリックします。「デプロイメント」リストに「Product line prediction」デプロイメントが表示されるようになります。
7. 「アクション」メニューから、該当するデプロイメント用の「詳細を表示」を選択します。詳細ペインに表示される「スコアリング・エンドポイント (Scoring Endpoint)」は、スコアリング要求を実行するために使用されます。
8. REST API を通じてスコアリング要求を実行するには、スコアリング・エンドポイントおよび許可トークンが必要です。詳しくは、『オンライン・モデルのデプロイ』を参照してください。
9. 次の場所に置かれているサンプル Node.js アプリケーションを試してみることができます。
   https://github.com/pmservice/product-line-prediction.

   サンプル・アプリケーション・コードを自動的に {{site.data.keyword.Bluemix_short}} スペースにデプロイするには、「サンプル」タブに移動し、
「サンプル・アプリケーション」セクションで「Product Line Prediction」アプリケーションを選択し、(+) アイコンをクリックしてそれをデプロイします。プロンプトが出されたら、DeployToBluemix フォームで認証を行います。

   これで、「{{site.data.keyword.Bluemix_short}} ダッシュボード」の「すべてのアプリ」セクション内に、サンプル・アプリケーションが表示されまるようになります。

10. アプリケーションをクリックすると、その詳細が表示されます。ここで、インスタンス数やメモリー割り振りなどのアプリケーション詳細を変更できます。
11. アプリケーションを {{site.data.keyword.pm_short}} インスタンスとバインドします。
**「接続」**タブで、**「既存に接続」**をクリックします。再ステージング後に、アプリケーションはサービス・インスタンスに接続されます。
12. 経路アドレスを使用してアプリケーションを実行します。
13. アプリケーションで、Product Line Prediction デプロイメントを選択します。これで、サンプル・レコードおよび予測結果を使用する準備ができました。
    
## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
