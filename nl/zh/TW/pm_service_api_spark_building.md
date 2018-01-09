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

# 建置預測分析應用程式

您可以使用 {{site.data.keyword.pm_full}} 服務來部署 Node.js 應用程式。
{: shortdesc}

**情境名稱**：產品線預測。

**情境說明**：我們的客戶經營歐洲最著名的連鎖店之一。他們想要我們依據其產品線（例如個人配件、露營設備和戶外防護器材），找出其客戶的興趣所在。資料科學家開發出一套預測模型，並將其與您（開發人員）分享。您的工作是要針對所部署的模型提出評分要求，以準備及部署可建議運動產品線的 Node.js 應用程式。

1. 登入 {{site.data.keyword.Bluemix_short}} 並建立 {{site.data.keyword.pm_full}} 的實例。
2. 啟動 {{site.data.keyword.pm_full}} 儀表板。
3. 移至**範例**標籤。
4. 在**範例模型**區段中，尋找**產品線預測**磚，然後按一下**新增模型**圖示 (+)。範例「產品線預測」模型即會出現在**模型**標籤的可用模型清單中。

   **附註**：如果您要使用自己的 Data Science Experience 專案及模型，而不使用範例，則必須在 {{site.data.keyword.pm_short}} 儲存庫中持續保存自訂模型。您可以使用 REST API 或用戶端程式庫來執行此作業。如需詳細資料，請參閱「自訂模型的開發和持續性」。

5. 從**動作**功能表中，按一下**建立部署**，以將「產品線預測」模型部署為線上部署。
6. 指定部署名稱（例如，產品線預測），然後按一下儲存。您現在應該會在「部署」清單中看到產品線預測部署。
7. 從「動作」功能表中，針對您的部署選取檢視詳細資料。顯示在詳細資料窗格中的「評分端點」，將會用來執行評分要求。
8. 若要透過 REST API 來執行評分要求，需要有評分端點和授權記號。如需相關資訊，請參閱「部署線上模型」。
9. 使用位於下列位置的 Node.js 應用程式範例來進行實驗：
   https://github.com/pmservice/product-line-prediction.

   若要將範例應用程式碼自動部署至 {{site.data.keyword.Bluemix_short}} 空間，請移至「範例」標籤，並且在「範例應用程式」區段中選取「產品線預測」應用程式，然後按一下 (+) 圖示進行部署。若出現提示，在 DeployToBluemix 表單上進行鑑別。

   您現在應該會看到範例應用程式出現在「{{site.data.keyword.Bluemix_short}} 儀表板」的「所有應用程式」區段中。

10. 按一下應用程式，以檢視其詳細資料。您可以在這裡修改應用程式詳細資料（例如實例數、記憶體配置）。
11. 將應用程式與 {{site.data.keyword.pm_short}} 實例連結。在**連線**標籤上，按一下**連接現有的**。重新編譯打包之後，您的應用程式會連接至服務實例。
12. 使用路徑位址來執行應用程式。
13. 在應用程式中選取您的「產品線預測」部署。您現在已準備好可以使用範例記錄及預測結果。
    
## 進一步瞭解

準備好要開始了嗎？若要建立服務的實例或是連結應用程式，請參閱[搭配使用服務與 Spark 及 Python 模型](using_pm_service_dsx.html)或[搭配使用服務與 IBM® SPSS® 模型](using_pm_service.html)。

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
