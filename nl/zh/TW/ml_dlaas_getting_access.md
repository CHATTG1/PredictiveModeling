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

# 設定機器學習環境並擷取認證

若要使用 IBM Watson Machine Learning，您必須能夠建立適當的機器學習環境，並擷取該環境特有的認證。

## 設定環境

若要使用 IBM Watson Machine Learning，您必須在 IBM Cloud 儀表板中設定下列項目的實例：

- Watson Machine Learning
- Object Storage
- Apache Spark

除了必要實例之外，部分記事本及指導教學還會使用下列實例。若要使用指導教學，您也必須設定這些服務。

- Python Flask
- 自然語言理解
- 深度學習實驗

### 建立 IBM Cloud 實例

觀看此視訊，瞭解如何建立 IBM Cloud 實例並檢視認證。然後，在視訊之後，遵循步驟來設定自己的環境。如需檢視認證的步驟，請參閱<a href="#retrieving-your-credentials">擷取認證</a>小節。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>圖 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="如果您無法存取此頁面所內嵌的視訊，可以從 YouTube 網站存取視訊。（在新分頁或視窗中開啟）">    <img src="images/video.png" alt="「視訊」圖示"></a>IBM Watson Machine Learning：開始使用</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>此視訊概述如何設定機器學習環境。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

若要建立 Watson Data Platform 實例，您必須執行下列步驟：

1. [登入 IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 從導覽畫面中，按一下**資料 & 分析**。

   您會看到置中於資料服務的畫面。您可以定期回到這裡，從一個易於使用的頁面使用您的資料及分析服務。

3. 按一下**分析**標籤，然後按一下**實例**。
4. 按一下**新建實例**。
5. 配置實例及資料儲存空間。

   輸入實例的敘述性名稱，選擇空間，然後選取資料方案（在此頁面上尋找方案比較及定價詳細資料）。IBM Cloud 會自動將您的實例名稱移入「物件儲存空間」名稱。您可能需要 Spark 專案的資料儲存空間，因此，也請在這裡選擇空間及方案。

6. 按一下**建立實例**。

即會開啟您的實例。

### 在 Data Science Experience 中將現有 Bluemix 實例新增至專案

觀看此視訊，瞭解如何建立專案並設定它來使用 Watson Machine Learning。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>圖 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="如果您無法存取此頁面所內嵌的視訊，可以從 YouTube 網站存取視訊。（在新分頁或視窗中開啟）">    <img src="images/video.png" alt="「視訊」圖示"></a>IBM Watson Machine Learning：建立 Watson Machine Learning 的專案</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>此視訊概述如何設定機器學習環境。</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

如果您已有實例，但尚未將它們鏈結至 Data Science Experience 中的專案，則必須執行下列步驟：

1. [登入 IBM Data Science Experience](https://datascience.ibm.com)。
2. 按一下**專案** > **檢視所有專案**。
3. 按一下**設定**標籤。
4. 若要新增服務，請在**關聯的服務**畫面中按一下**新增關聯的服務**，選取服務，完成配置資訊，然後按一下**儲存**。
5. 若要新增存取記號，請執行下列步驟：
   6. 在**存取記號**畫面中，按一下**建立新的記號**。
   7. 在**名稱**方框中，鍵入名稱。
   8. 在**存取專案的角色**方框中，選取角色：**檢視者**或**編輯者**。
   9. 按一下**新增**。

6. 若要連接至 GitHub 儲存庫，請在**儲存庫 URL** 中鍵入您的儲存庫位置。如果您尚未設定進行 GitHub 存取的個人存取記號，則現在必須按一下**設定**來建立它。完成時，請按一下**新增**。


## 擷取認證

若要在記事本及應用程式中使用 Bluemix 實例、服務及模型，您必須能夠將認證、記號及評分端點插入至用於處理的程式碼。

1. [登入 Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 捲動至**服務**區段，然後按一下您要擷取其認證的服務名稱。
3. 在導覽窗格中，按一下**服務認證**。
4. 在認證清單中，按一下**檢視認證**。
5. 如果此服務沒有任何認證，您可以按一下**建立認證**來建立它們。
6. 若要複製認證，請按一下複製圖示。

根據服務，您可能會有不同的欄位（例如 `username` 及 `password`）。您必須複製所顯示的值，以在程式碼中使用它們。

## 進一步瞭解

[快速深度學習指導教學](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

