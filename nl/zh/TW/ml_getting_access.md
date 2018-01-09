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

# 設定環境

若要使用 {{site.data.keyword.pm_full}}，您必須能夠建立適當的機器學習環境，並擷取該環境特有的認證。您可以使用 [Cloud Foundry 指令行介面](https://github.com/cloudfoundry/cli#getting-started)或透過「{{site.data.keyword.Bluemix}} 儀表板」中的圖形使用者介面，來建立 {{site.data.keyword.Bluemix}} 實例。
{: shortdesc}

## 使用 IBM Cloud 儀表板

若要使用 {{site.data.keyword.pm_full}}，您必須在「{{site.data.keyword.Bluemix_notm}} 儀表板」中為它[建立實例](https://console.bluemix.net/catalog/services/machine-learning)。

根據情境，您可能還需要下列資源：

- Object Storage（批次部署）
- Db2 Warehouse on Cloud（批次部署）
- Apache Spark（批次、串流部署及持續學習系統）
- Message Hub（串流部署）

若要瞭解如何使用「儀表板」來建立 {{site.data.keyword.Bluemix_notm}} 實例以及檢視認證，請觀看下列視訊，其補充遵循視訊的書面步驟。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>圖 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="如果您無法存取此頁面所內嵌的視訊，可以從 YouTube 網站存取視訊。（在新分頁或視窗中開啟）">    <img src="images/video.png" alt="「視訊」圖示"></a>IBM Watson Machine Learning：開始使用</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>此視訊概述如何設定機器學習環境。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## 建立 {{site.data.keyword.Bluemix_notm}} 實例

若要建立 {{site.data.keyword.pm_full}} 實例，您必須執行下列步驟：

1. 開啟 {{site.data.keyword.pm_full}} [服務頁面](https://console.bluemix.net/catalog/services/machine-learning)。
2. 註冊或登入，以建立服務實例。
3. 輸入實例的敘述性名稱，選擇空間，然後選取資料方案。
4. 按一下**建立實例**。

即會開啟您的實例。

## 擷取認證

若要使用 {{site.data.keyword.Bluemix_notm}}（先前稱為 Bluemix）實例，您需要服務實例認證。您可以透過 [CF CLI](using_pm_service.html) 或「{{site.data.keyword.Bluemix_notm}} 儀表板」，來建立及存取您的認證。將 {{site.data.keyword.pm_short}} 服務實例連結至 {{site.data.keyword.Bluemix_notm}} 應用程式之後，{{site.data.keyword.pm_short}} 認證即會新增至 `VCAP_SERVICES` 環境變數。如需相關資訊，請參閱[搭配使用服務與應用程式](using_pm_service.html)。

若要擷取 {{site.data.keyword.pm_full}} 實例認證，您必須執行下列步驟：

1. [登入 {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. 從**服務**區段中，按一下您要擷取其認證的服務名稱。
3. 在導覽窗格中，按一下**服務認證**。
4. 在認證清單中，按一下**檢視認證**。
5. 如果此服務沒有任何認證，請按一下**建立認證**來建立它們。
6. 若要複製認證，請按一下複製圖示。

若要在應用程式中使用 {{site.data.keyword.pm_short}} 服務，您必須將服務連結至 {{site.data.keyword.Bluemix_notm}} 應用程式，而執行應用程式的必要認證即會插入至 `VCAP_SERVICES` 環境變數。如需相關資訊，請參閱 [Cloud Foundry 指令行介面](#cloud-foundry-command-line-interface)。

## 使用 Cloud Foundry CLI（指令行介面）

### 連結服務與 {{site.data.keyword.Bluemix_notm}} 應用程式的步驟

您可以下載 Node.js [範例程式碼](https://github.com/pmservice/product-line-prediction/blob/master/README.md)，以嘗試 Machine Learning 服務。請完成下列步驟來建立 {{site.data.keyword.Bluemix_notm}} 應用程式並連結 {{site.data.keyword.pm_short}} 服務。這些範例使用 Node.js，因為它是熱門的運行環境。可以使用其他運行環境，例如 iOS、Ruby、Perl 或 Java。

您也可以使用「{{site.data.keyword.Bluemix_notm}} 圖形介面」執行步驟 1-3，而不使用 Cloud Foundry 工具 (cf)。

1. 使用 `cf create-service` 指令來建立服務實例：

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例如，下列指令會在 {{site.data.keyword.Bluemix_notm}} 空間中建立一個 {{site.data.keyword.pm_short}} 服務實例，其中包含名為 `my_wml_lite` 的精簡方案：

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. 使用 `cf create-service-key` 指令來建立服務認證：

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   例如，下列指令會建立 {{site.data.keyword.pm_short}} 服務認證：

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. 使用 `cf bind-service` 指令，將服務實例 `my_wml_lite` 連結至應用程式。

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   例如，下列指令會將 {{site.data.keyword.pm_short}} 服務實例 `my_wml_lite` 連結至 {{site.data.keyword.Bluemix_notm}} 應用程式 `my_app1`：

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### 透過 `VCAP_SERVICES` 變數存取認證

將 {{site.data.keyword.pm_short}} 服務實例連結至 {{site.data.keyword.Bluemix}} 應用程式之後，{{site.data.keyword.pm_short}} 認證即會新增至 `VCAP_SERVICES` 環境變數：

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   `VCAP_SERVICES` 環境變數包括下列資訊：

<dl>
<dt>plan</dt>
<dd>使用於服務佈建的 {{site.data.keyword.pm_short}} 方案。</dd>
<dt>url</dt><dd>{{site.data.keyword.pm_short}} 服務實例的位址。
<dt>access_key</dt><dd>僅限 IBM® SPSS® Modeler 串流。</dd>
<dt>instance_id</dt><dd>{{site.data.keyword.pm_short}} 實例唯一 ID。</dd>
<dt>username</dt><dd>使用者名稱，產生存取記號所需的基本授權的一部分，它能將所有要求傳入這個服務實例。</dd>
<dt>password</dt><dd>密碼，產生存取記號所需的基本授權的一部分，它能將所有要求傳入這個服務實例。</dd>
</dl>

例如，您可以使用下列 `curl` 指令來產生存取記號：

要求範例：

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       輸出範例：

       {"token":"**********"}
```
{: codeblock}

   下列 Node.js 程式碼是如何從 `VCAP_SERVICES` 環境變數取得 `username` 及 `password` 值的範例：

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
