---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 入門指導教學
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} 已與 [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com) 整合。
{: shortdesc}

瞭解 [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics) 中的機器學習與人工智慧。

{{site.data.keyword.pm_full}} 可讓使用者執行兩項機器學習基礎作業：訓練及評分。

- **訓練**是調整演算法的程序，以讓它可以從資料集學習。此作業的輸出稱為模型。模型會封裝學習到的數學表示式係數。
- **評分**是使用訓練模型來預測結果的作業。評分作業的輸出是另一個包含預測值的資料集。

{{site.data.keyword.pm_full}} 的設計旨在處理兩種主要人物的需求：

- 資料科學家：建立機器學習管線，以運用資料轉換及機器學習演算法。他們通常會使用記事本或外部工具來訓練並評估其模型。資料科學家經常會與「資料工程師」分工合作，以探索及瞭解資料。
- 開發人員：建置智慧型應用程式，以透過機器學習模型使用預測輸出。

雖然訓練是機器學習程序中的重要步驟，但 {{site.data.keyword.pm_full}} 可讓您簡化模型的運作，方法是部署模型，並透過一段時間以及所有其反覆運算來取得實際商業價值。

## 關於

{{site.data.keyword.pm_full}} 服務是一組 REST API，可以從任何程式設計語言呼叫。

{{site.data.keyword.pm_full}} 服務的焦點是部署，但您可以使用 IBM® SPSS® Modeler 或 {{site.data.keyword.DSX_full}} 來編寫及使用模型和管線。使用 Spark MLlib 及 Python scikit-learn 的 SPSS® Modeler 及 {{site.data.keyword.DSX_full}} 都能提供各種建模方法，這些方法取自機器學習、人工智慧及統計資料。

## 相關鏈結

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_spark.html) 或 [SPSS 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

