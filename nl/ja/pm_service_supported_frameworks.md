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

# サポートされるフレームワーク

{{site.data.keyword.pm_full}} サービスでは、モデルのライフサイクル管理の一環としてのモデル開発および検証のために、以下のフレームワークがサポートされています。
{: shortdesc}

## Spark MLlib

{{site.data.keyword.pm_full}} サービスは、オンライン・デプロイメント、バッチ (ベータ) デプロイメント、ストリーム (ベータ) デプロイメント、およびスコアリング・サービス向けに、Spark MLlib をサポートします。 特定のバージョンおよびランタイム環境のみがサポートされています。

### 機能

* デプロイメントのタイプ:
  * オンライン
  * Object Storage、DB2 Warehouse on Cloud、および DB2 on Cloud のあるバッチ
  * Message Hub のあるストリーム
*  継続学習システム

### サポートされるバージョン

*  Spark 2.0

### 制約事項

  *  分類モデルおよび回帰モデルのみがサポートされます。
  *  カスタム変換関数、ユーザー定義関数、およびクラスへの参照を含んでいるモデルはサポートされません。
  * バッチ・デプロイメントおよびストリーム・デプロイメントは、IBM Cloud Apache Spark サービスを使用します。登録されているサービス・プランに基づいて、並行 spark-submit ジョブの数には次の制限があることに注意してください ([spark-submit 資料を使用](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3))。
    * ライト: 一度に 1 つのみの spark-submit ジョブ
    * 予約済み企業: 一度に 150 の spark-submit ジョブ

## Scikit-learn

{{site.data.keyword.pm_full}} オンライン・デプロイメントおよびスコアリング・サービス向けに、scikit-learn のサポートがあります。特定のバージョンおよびランタイム環境のみがサポートされています。

### 機能

* デプロイメントのタイプ:
  * オンライン

### サポートされるバージョン

- Python 3.5 ランタイム用 Anaconda 4.2.x
- Scikit-Learn 0.17

### Python ランタイム

WML Online Deployment and Scoring サービスを使用してデプロイされる必要のあるモデル用の Python ランタイムは、Python 3.5 です。

モデルが Python 2.7 ランタイムを使用して作成された場合には、Python2.7 と Python3.5 の非互換性が原因でデプロイメントが失敗することがあります。

### サポートされる Python パッケージ

WML Online Deployment and Scoring サービスによってサポートされる Python パッケージのリストは、
[こちら](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35)にあります。

### 制約事項

以下の制限の一部またはすべてを含んでいるいくつかの制約があります。

* 分類モデルおよび回帰モデルのみがサポートされます。
* モデルは、Python 3.5 ランタイム用 Anaconda 4.2.x によってサポートされるパッケージ (およびパッケージ・バージョン) への参照のみを含むことができます。
* デプロイメント要求中に返される "schema" エンドポイントは実装されません。
* "predict()" API の入力タイプとして Pandas データ・フレームを必要とするモデルはサポートされません。
* カスタム変換関数、ユーザー定義関数、およびクラスへの参照を含んでいるモデルはサポートされません。

## XGBoost

{{site.data.keyword.pm_full}} Online Deployment and Scoring サービス向けに、XGBoost のサポートがあります。特定のバージョンおよびランタイム環境のみがサポートされています。

### 機能

* デプロイメントのタイプ:
  * オンライン

### サポートされるバージョン

- XGBoost 0.6
- Python 3.5 ランタイム用 Anaconda 4.2.x
- Scikit-Learn 0.17

### Python ランタイム

WML Online Deployment and Scoring サービスを使用してデプロイされる必要のあるモデル用の Python ランタイムは、Python 3.5 です。

モデルが Python 2.7 ランタイムを使用して作成された場合には、Python2.7 と Python3.5 の非互換性が原因でデプロイメントが失敗することがあります。

### サポートされる Python パッケージ

WML Online Deployment and Scoring サービスによってサポートされる Python パッケージのリストは、
[こちら](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35)にあります。

### 制約事項

以下の制限の一部またはすべてを含んでいるいくつかの制約があります。

* モデルは、Python 3.5 ランタイム用 Anaconda 4.2.x によってサポートされるパッケージ (およびパッケージ・バージョン) への参照のみを含むことができます。
* デプロイメント要求中に返される "schema" エンドポイントは実装されません。
* このステージでは、スコアリング要求の応答として予測の確率は返されません。
* カスタム変換関数、ユーザー定義関数、およびクラスへの参照を含んでいるモデルはサポートされません。

## SPSS Modeler

{{site.data.keyword.pm_full}} Online、Batch Deployment and Scoring サービス向けに、IBM® SPSS® Modeler ストリームのサポートがあります。特定のバージョンおよびランタイム環境のみがサポートされています。

### 機能

* デプロイメントのタイプ:
  * オンライン
  * バッチ
* リトレーニング
* ストリームの実行

### サポートされるバージョン

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### 制約事項

IBM® SPSS® Modeler ストリームには以下の制約事項があります。

*  IBM® Data Science Experience Flow Editor を使用して作成されたストリームはサポートされません。
*  スコアリング枝をリアルタイム・スコアリングで使用するために準備する場合、スコアリング要求で取得される入力データによって、スコアリング枝に入るように設計されたソース・ノードが置き換えられる必要があります。また、その結果の予測分析出力は、応答フローに戻る必要があります (事実上、スコアリング枝設計の端末ノードが置き換えられます)。
*  スコアリング枝は、{{site.data.keyword.Bluemix_short}} でのリアルタイム実行のために準備されるため、外部サービスへの接続を要求できません。例えば、IBM Analytical Decision Management スコアリング枝設計には、IBM® SPSS® Collaboration and Deployment Services リポジトリーに保管されたルールやモデルへの参照を含めることはできません。
*  スコアリング枝を {{site.data.keyword.Bluemix_short}} でリアルタイム・スコアリングのために実行する場合、外部サービスを要求できません。例えば、リアルタイムで IBM® SPSS®
Analytic Server および Apache Hadoop データ・ストアを必要とするモデル・アルゴリズムのデプロイとスコアリングを行うことはできません。
*  {{site.data.keyword.pm_short}} では、Modeler の組み込みの Python スクリプトがサポートされます。ストリームを {{site.data.keyword.pm_short}} での実行前に処理するために使用する方法が原因で、いくつかの制限があります。通常、ユーザーがストリームの実行を制御することを選択した場合、枝の端末ノードが参照されます。{{site.data.keyword.pm_short}} では、ストリームの処理時に、オーバーライドされる JSON のノードを識別してから、ストリームの実行前に置換を行います。これによって、参照される入力ノードとエクスポート・ノードが存在しなくなるためにストリームはスクリプトで失敗します。解決方法は、別のノードの ID を使用して、実行中に枝を一意的に識別することです。これで、組み込みの Python スクリプトで定義されたようにストリームが実行されるようになります。

## 詳細はこちら

IBM® SPSS® Analytic Server の学習された予測モデルの現行サポートに関する詳細については、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/) の [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) セクションを参照してください。
