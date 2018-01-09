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

# 支援的架構

{{site.data.keyword.pm_full}} 服務支援下列架構，以在模型生命週期管理過程中進行模型開發及驗證。
{: shortdesc}

## Spark MLlib

{{site.data.keyword.pm_full}} 服務支援「線上、批次（測試版）、串流（測試版）部署及評分」服務的 Spark MLlib。只支援特定版本和運行環境。

### 功能

* 部署類型：
  * 線上
  * 含 Object Storage、Db2 Warehouse on Cloud 及 Db2 on Cloud 的批次
  * 含 Message Hub 的串流
*  持續學習系統

### 支援的版本

*  Spark 2.0

### 限制

  *  只支援分類及迴歸模型。
  *  不支援包含對自訂轉換器、使用者定義函數及類別之參照的模型。
  * 「批次」及「串流」部署使用 IBM Cloud Apache Spark 服務。根據已訂閱的服務方案，請注意下列平行 spark-submit 工作數目限制（[使用 spark-submit 文件](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)）。
    * 精簡：一次只有一個 spark-submit 工作
    * 保留的企業：一次 150 個 spark-submit 工作

## Scikit-learn

「{{site.data.keyword.pm_full}} 線上部署及評分」服務具有 scikit-learn 支援。只支援特定版本和運行環境。

### 功能

* 部署類型：
  * 線上

### 支援的版本

- Anaconda 4.2.x for Python 3.5 運行環境
- Scikit-Learn 0.17

### Python 運行環境

針對需要使用「WML 線上部署及評分」服務部署的模型，Python 運行環境為 Python 3.5。

在某些情況下，使用 Python 2.7 運行環境建立模型時，部署可能會因為 Python 2.7 與 Python 3.5 之間的不相容性而失敗。

### 支援的 Python 套件

您可以在[這裡](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35)找到「WML 線上部署及評分」服務所支援的 Python 套件清單。

### 限制

有一些限制，其中可能包含下列部分或所有限制：

* 只支援分類及迴歸模型。
* 對於 Python 3.5 運行環境，模型只能包含對 Anaconda 4.2.x 所支援套件（及套件版本）的參照。
* 未實作在部署要求期間傳回的 "schema" 端點。
* 不支援對於 "predict()" API 需要 Pandas Dataframe 作為輸入類型的模型。
* 不支援包含對自訂轉換器、使用者定義函數及類別之參照的模型。

## XGBoost

「{{site.data.keyword.pm_full}} 線上部署及評分」服務具有 XGBoost 支援。只支援特定版本和運行環境。

### 功能

* 部署類型：
  * 線上

### 支援的版本

- XGBoost 0.6
- Anaconda 4.2.x for Python 3.5 運行環境
- Scikit-Learn 0.17

### Python 運行環境

針對需要使用 WML 線上部署及評分服務部署的模型，Python 運行環境為 Python 3.5。

在某些情況下，使用 Python 2.7 運行環境建立模型時，部署可能會因為 Python 2.7 與 Python 3.5 之間的不相容性而失敗。

### 支援的 Python 套件

您可以在[這裡](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35)找到「WML 線上部署及評分」服務所支援的 Python 套件清單。

### 限制

有一些限制，其中包括下列部分或所有限制：

* 對於 Python 3.5 運行環境，模型只能包含對 Anaconda 4.2.x 所支援套件（及套件版本）的參照。
* 未實作在部署要求期間傳回的 "schema" 端點。
* 現階段預測的機率不會以評分要求的回應傳回。
* 不支援包含對自訂轉換器、使用者定義函數及類別之參照的模型。

## SPSS Modeler

「{{site.data.keyword.pm_full}} 線上、批次部署及評分」服務具有 IBM® SPSS® Modeler 串流支援。只支援特定版本和運行環境。

### 功能

* 部署類型：
  * 線上
  * 批次
* 重新訓練
* 執行串流

### 支援的版本

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### 限制

IBM® SPSS® Modeler 串流的限制如下：

*  不支援使用「IBM® Data Science Experience 流程編輯器」所建立的串流。
*  準備好評分分支以用於即時評分時，評分要求上進入的輸入資料必須取代設計到評分分支中的來源節點，而且產生的預測分析輸出必須流回回應流程（實際上會取代評分分支設計中的終端機節點）。
*  因為評分分支是為了在 {{site.data.keyword.Bluemix_short}} 中進行即時執行而準備，所以不需要與外部服務的連線。例如，IBM Analytical Decision Management 評分分支設計無法包含對 IBM® SPSS® Collaboration and Deployment Services 儲存庫中所儲存規則或模型的參照。
*  為了在 {{site.data.keyword.Bluemix_short}} 中進行即時評分而執行評分分支時，不需要外部服務。例如，您無法針對需要 IBM® SPSS® Analytic Server 及 Apache Hadoop 資料儲存庫的模型演算法，即時進行部署及評分。
*  {{site.data.keyword.pm_short}} 支援 Modeler 內嵌式 Python Scripting。由於在 {{site.data.keyword.pm_short}} 中執行之前用於處理串流的方法之故，會有一些限制。一般來說，如果使用者選擇控制串流的執行，則會參照分支的終端機節點。對於 {{site.data.keyword.pm_short}}，我們在處理串流時，會識別 JSON 中將置換的節點，然後在串流執行之前執行取代。這會導致串流在 Script 中失敗，因為所參照的輸入及匯出節點已不存在。解決方案是在執行期間使用另一個節點的 ID 來唯一識別分支。這可確保串流會如內嵌式 Python Script 中所定義的方式執行。

## 進一步瞭解

如需 IBM® SPSS® Analytic Server 訓練之預測模型最新支援的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/) 的 [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) 小節。
