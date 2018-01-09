---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 開始使用
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} 是一種 IBM Cloud 服務，可讓使用者執行兩項機器學習基礎作業：訓練及評分。
{: shortdesc}

- **訓練**是調整演算法的程序，以讓它可以從資料集學習。此作業的輸出稱為模型。模型會封裝學習到的數學表示式係數。
- **評分**是使用訓練模型來預測結果的作業。評分作業的輸出是另一個包含預測值的資料集。

{{site.data.keyword.pm_full}} 的設計旨在處理兩位主要人員的需求：

- 資料科學家：建立機器學習管線，以運用資料轉換及機器學習演算法。它們通常會使用記事本或外部工具來訓練並評估其模型。資料科學家經常會與「資料工程師」分工合作，以探索及瞭解資料。
- 開發人員：建置透過機器學習模型使用預測輸出的智慧型應用程式。

雖然訓練是機器學習程序中的重要步驟，但 {{site.data.keyword.pm_full}} 可讓您簡化模型的運作，方法是部署模型，並透過一段時間以及所有其反覆運算來取得實際商業價值。

## 必要條件

若要從 {{site.data.keyword.Bluemix_short}} 型錄使用 {{site.data.keyword.pm_full}}，您必須[在這裡建立服務實例](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/)。此設定可讓您執行下列作業：

## 步驟

1. [設定 Machine Learning 環境。](ml_getting_access.html)
1. [建立並儲存模型](pm_custom_models.html)。
2. [部署模型](pm_service_api_spark_online.html)。
3. 在應用程式使用已部署的模型 `scoring endpoint`，以[取得預測](pm_service_api_spark_building.html)。

## 搭配使用 Machine Learning 與 Data Science Experience

{{site.data.keyword.pm_full}} 已與 IBM Data Science Experience 整合。您可以在 Data Science Experience 記事本中使用 Machine Learning API 用戶端程式庫；您必須具有 Machine Learning 實例，才能使用「模型建置器」及「流程編輯器」。

## 搭配使用 Machine Learning 與 SPSS Modeler

{{site.data.keyword.pm_full}} 已與 IBM® SPSS® Modeler 整合。您可以使用 Machine Learning API 來運用進階數學演算法。


## 搭配使用 Machine Learning 與您的環境

{{site.data.keyword.pm_full}} 可以用來作為鏈結本端環境與雲端的混合式解決方案。您可以使用 Machine Learning API 來發佈模型、部署及評分。如需相關資訊，請參閱 [DSX：混合式模式](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b)。

## 關於

{{site.data.keyword.pm_full}} 服務是一組 REST API，可以從任何程式設計語言呼叫。

{{site.data.keyword.pm_full}} 服務的焦點是部署，但您可以使用 IBM® SPSS® Modeler 或 IBM® Data Science Experience 來編寫及使用模型和管線。使用 Spark MLlib 及 Python scikit-learn 的 SPSS® Modeler 及 Data Science Experience 提供各種建模方法，這些方法取自機器學習、人工智慧及統計資料。

## 相關鏈結

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[使用服務搭配 Spark
及 Python 模型](using_pm_service_dsx.html)或[使用服務搭配 SPSS 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [SPSS 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
