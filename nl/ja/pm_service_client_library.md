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

# クライアント・ライブラリー

## 共通 API クライアント・ライブラリー <span class='tag--beta'>ベータ</span>

IBM Watson Machine Learning サービスを Python プログラミング言語で使用するために、PyPI を使用して `watson-machine-learning-client` クライアント・ライブラリーをインストールできます。

これを行う方法について詳しくは、[Spark MLlib ノートブック](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550)または [scikit-learn ノートブック](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a)を参照してください。これらのサンプル・ノートブックでは以下の技法が説明されています。

* モデルの永続性
* オンライン・デプロイメント
* 共通 API クライアントを使用したスコアリング

共通 API クライアント・ライブラリーについての資料は[ここ](http://wml-api-pyclient.mybluemix.net/)にあります。

### 制約事項

* 共通 API クライアントは**ベータ版**です。
* 共通 API クライアントを使用するには、その前に、XGboost、scikit-learn、または pyspark のうち 1 つ以上のパッケージをインストールする必要があります。
* Python 3 のみがサポートされています。

## リポジトリー API クライアント・ライブラリー

Machine Learning サービスのリポジトリー REST API 用のクライアント・ライブラリー・ラッパーは Python プログラミング言語と Scala プログラミング言語で開発されています。

モデル永続性を Watson Machine Learning リポジトリーに提示するための `repository` クライアント・ライブラリーの使用について詳しくは、[サンプル・ノートブック](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)を参照してください。

リポジトリー・ライブラリーについて詳しくは、以下のトピックを参照してください。

* [Scala リポジトリー・モジュール資料](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python リポジトリー・モジュール資料](https://watson-ml-staging-libs.mybluemix.net/repository-python/)
### 制約事項

リポジトリー・ライブラリーは、[IBM Data Science Experience](https://datascience.ibm.com) を使用する場合のみ使用可能です。
