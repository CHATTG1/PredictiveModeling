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

# 用戶端程式庫

## 一般 API 用戶端程式庫<span class='tag--beta'>測試版</span>

若要搭配使用 IBM Watson Machine Learning 服務與 Python 程式設計語言，您可以使用 PyPI 來安裝 `watson-machine-learning-client` 用戶端程式庫。

如需如何執行此作業的詳細資料，您可以查看用於示範下列技術的 [Spark MLlib 記事本](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550)或 [scikit-learn 記事本](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a)範例記事本：

* 模型持續性
* 線上部署
* 使用一般 API 用戶端進行評分

[這裡](http://wml-api-pyclient.mybluemix.net/)提供一般 API 用戶端程式庫的文件。

### 限制

* 一般 API 用戶端為**測試版**。
* 必須至少安裝下列其中一個套件，才能使用一般 API 用戶端：XGboost、scikit-learn 或 pyspark。
* 僅支援 Python 3。

## 儲存庫 API 用戶端程式庫

Machine Learning 服務的儲存庫 REST API 用戶端程式庫封套是使用 Python 及 Scala 程式設計語言所開發的。

如需使用 `repository` 用戶端程式庫向 Watson Machine Learning 儲存庫呈現模型持續性的相關資訊，請參閱[範例記事本](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)。

如需儲存庫程式庫的相關資訊，請參閱下列主題：

* [Scala 儲存庫模組文件](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python 儲存庫模組文件](https://watson-ml-staging-libs.mybluemix.net/repository-python/)
### 限制

只有在使用 [IBM Data Science Experience](https://datascience.ibm.com) 時，才能使用儲存庫程式庫。
