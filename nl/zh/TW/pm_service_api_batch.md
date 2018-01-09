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

# IBM SPSS Modeler 模型的 Machine Learning 服務批次工作 API

{{site.data.keyword.pm_full}} 服務的批次工作 API 支援與模型訓練、模型評估及批次評分相關的長時間執行作業。此 API 管理兩種資產類型：批次工作中所使用的 IBM® SPSS® Modeler 串流檔，以及所提交的工作定義。
{: shortdesc}

針對您上傳的每一個 SPSS® Modeler 串流檔，可能會提交許多類型的許多工作。如果工作可能會更新 Modeler 串流檔內容，則修改過的檔案會內含在工作結果附件中。在模型評估工作類型中，所有產生的評估檔案都位於工作結果附件中。

**附註**：儀表板中出現的資料只會與即時預測相關（例如來自上傳特性的資料）。

## 刪除工作

您可以刪除工作。如果工作正在執行，則刪除工作時會先取消工作。請使用 `DELETE` 指令搭配 `job ID`。您可以傳遞多個 ID，一次取消多個工作。

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

這項傳回指出已刪除要求中 ID 所參照的工作數目。如果此數目不符合要求中所傳遞的清單，則您必須檢查個別工作的狀態。

## 檢查工作的狀態

您可以隨時使用 `GET` 指令，取得 `job ID` 的狀態：


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

傳回的 JSON 檔案會指出工作狀態，因此，如果工作順利完成，則是可用來取得所有已產生檔案內容的資料 URL。

## 重新提交工作

若要重新提交工作，請使用 `PUT` 指令搭配 `job ID`。若要重新提交工作，則工作不得為執行中。

如果您參照的 ID 不存在，則下列呼叫會建立新工作。傳回狀態為 201 及 200（指出發生的狀況）。您必須傳遞要在此執行作業中使用的工作定義 JSON 檔案。

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

將 `content_type` 值設為 "application/json"，以重新提交工作。您必須在要求主體中包括全新或更新過的工作定義 JSON 檔案。

## 針對已上傳的 Modeler 串流檔提交工作

若要將工作放入執行佇列中，請使用 `PUT` 指令：

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

將 `content_type` 值設為 "application/json"，以提交工作。您必須在要求主體中包括全新或更新過的工作定義 JSON 檔案。

此要求會立即傳回，並指出將工作定義放在執行佇列中時即成功。不會測試是否能夠順利執行工作定義中所配置的 Modeler 串流。雲端中的其中一個 {{site.data.keyword.pm_short}} 工作伺服器將會執行工作，而且您可以監視其狀態。如果您要執行模型訓練，並指出您要自動重新整理，則工作會在順利執行工作時取代原始 Modeler 串流檔。

## 上傳要在工作中使用的串流檔

**附註**：{{site.data.keyword.pm_short}} 儀表板僅適用於即時評分。您不可以將它用於執行工作（批次評分）。

若要讓 Modeler 串流檔可供工作存取，請使用 `PUT` 指令：

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

具有在要求中傳遞檔案的 content_type "multipart/form-data"。

所使用的唯一名稱（`PUT` 呼叫中的 `file ID`）將在檔案 API 的 `DELETE` 呼叫中進行參照，而且是您工作定義中的模型參照。請注意，工作中所使用檔案的名稱空間完全由您所控制。以相同的 `file ID` 對檔案執行
`PUT` 指令，會隱含地取代現行副本。

若要為針對您的工作上傳的所有檔案產生清單，請使用 `GET` 指令，但省略 `file ID` 參數。若要擷取特定的檔案，請使用 `GET` 指令搭配 `file ID` 參數。您也可以發出 `DELETE` 指令來刪除已上傳的檔案。這會在參照該檔案的任何擱置中工作執行造成錯誤。

要求範例：

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

當部署成功時回應：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

當部署失敗時回應：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## 工作類型

**訓練**：此工作類型指出應該執行 Modeler 串流中的所有「模型建置器」終端機節點。工作順利完成之後，具有新訓練模型區塊的已更新 Modeler 串流會在可擷取的工作結果中。如果 Modeler 串流檔的鏈結是從模型建置節點到評估及評分分支中的訓練模型區塊，則這會重新整理這些節點。

**評估**：此工作類型會觸發所有「文件建置器」終端機節點的執行（主要是從 Modeler 用戶端中的圖形及輸出標籤），以產生可傳回給呼叫者的靜態報告檔案內容。評分分支不是視為此工作類型的一部分。

**自動重新整理**：`TRAINING` 工作類型的版本，在其中，工作順利完成時，將會更新批次檔清單中的原始 Modeler 串流檔。會採用有關針對即時評分所部署的 Modeler 串流之重新整理事件的評估及明確決策，而且目前未涵蓋在自動重新整理中。

**批次評分**：執行已套用「作為評分分支使用」選項的終端機節點，指出這是此 Modeler 串流設計中的評分分支。工作定義必須指定匯出及來源詳細資料。

**執行串流**：執行方式與已在串流內容之「執行」標籤上選取「執行此 Script」選項的情況下，在 Modeler 中按一下綠色「執行」圖示類似。用法會涵蓋模型訓練或其他工作類型之 Script 執行的需求。串流參數必須處理 Script 的所有動態控制，而且參數值是在工作定義中進行傳遞。

## 工作狀態

**擱置**：已提交工作定義，但工作伺服器尚未宣告要執行它。

**執行中**：工作定義已由工作伺服器所宣告，並且正在執行中。

**取消中**：正在取消工作。

**已取消**：已取消工作。

**失敗**：工作失敗。在取得 (GET) 工作狀態時，會傳回失敗原因的詳細資料。

**成功**：已順利執行工作。在取得 (GET) 工作狀態的 JSON 中，會傳回針對此事件所傳達的任何結果。

## 工作 API 詳細資料

`POST /v1/jobs/{id}`

說明：提交工作定義來執行。如果 `job ID` 已存在，則會導致錯誤。

內容類型：

```
@Consumes({"application/json"})
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

使用者指定的 `job ID`。對於 {{site.data.keyword.pm_short}} 服務實例必須是唯一的：

```
@PathParam("id")
```
{: codeblock}

JSON 工作定義（請參閱工作定義 JSON）：

```
@BodyParam
```
{: codeblock}

回應：

成功。已提交工作來執行，並傳回說明已提交工作的 JSON。

```
@ApiResponse(code = 200)
```
{: codeblock}

工作已存在錯誤。已提交具有此 ID 的工作。傳回的訊息會說明此錯誤。

```
@ApiResponse(code = 409)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

說明：建立或更新工作。如果具有此 ID 的工作不存在，請建立它；否則，請更新它（實際上會重新提交它來執行）。

內容類型：

```
@Consumes({"application/json"})
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

使用者指定的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

JSON 工作定義（請參閱工作定義 JSON）：

```
@BodyParam
```
{: codeblock}

回應：

成功。已更新工作，並已提交來執行。傳回說明已提交工作的 JSON。

```
@ApiResponse(code = 200)
```
{: codeblock}

成功。已建立工作，並已提交來執行。傳回說明已提交工作的 JSON。

```
@ApiResponse(code = 201)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON。

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

說明：傳回此 Machine Learning 服務實例上的所有已定義工作清單。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

回應：

成功。傳回所有工作定義的 JSON 陣列：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

說明：要求傳回特定工作定義。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

已提交的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

回應：

成功。傳回說明所參照工作的 JSON：

```
@ApiResponse(code = 200)
```
{: codeblock}

找不到工作錯誤。所傳回訊息中的詳細資料：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

說明：擷取特定工作的狀態。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

已提交的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

回應：

成功。傳回說明工作狀態的 JSON：

```
@ApiResponse(code = 200)
```
{: codeblock}

找不到工作錯誤。所傳回訊息中的詳細資料：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

說明：刪除一個以上的工作。如果工作目前正在執行中，將會取消工作。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

已提交的 `job ID`，或以逗點 (,) 分割 ID 值的 `job ID` 清單

```
@PathParam("id" or "id,id,id")
```
{: codeblock}

回應：

成功。傳回已刪除的工作數目：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

## 工作定義 JSON

工作定義 JSON 包含下列一般區段：

工作類型及預測模型參照

**附註**：儀表板中顯示的資料只與即時預測相關，包括來自上傳特性的資料。

### 動作類型

**TRAINING** - 執行模型建置器節點訓練或重新整理模型區塊。在工作結果中，會擷取更新過的 Modeler 串流。

**EVALUATION** - 執行評估訓練模型的文件建置器及分析節點。

**AUTO_REFRESH** - 執行 TRAINING，並更新批次檔上傳空間中的檔案內容。

**BATCH_SCORE** - 執行評分分支，並匯出工作定義所指示之產生的資料。

**RUN_STREAM** - 執行串流內容中所指出的 Modeler 串流；最常用於需要 Script 執行時。

模型：批次工作參照之檔案上傳動作中所指定的 ID。如需相關資訊，請參閱「上傳要在工作中使用的串流檔」。請注意，會使用 `/pm/v1/file`。

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

請注意，ID 應該與 `PUT` API 中所使用的 `file ID` 相同。不需要 name，但針對模型訓練及自動重新整理，將會使用這裡定義的名稱來儲存工作結果。如果未定義 name，{{site.data.keyword.pm_short}} 服務將會根據預先定義的命名規則來產生結果。

### 工作設定

執行此工作所需的所有設定。

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### 資料庫連線功能定義

資料庫類型：ApacheHive、DashDB、DB2、Greenplum、Impala、Informix、MySQL、Oracle、PostgreSQL、ProgressOpenEdge、Salesforce、SQLServer、Sybase、SybaseIQ、Teradata。

DB 服務實例中所指定的連線功能，許多 DB 類型都需要將特定設定傳入 'Options' 中

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### 來源節點設定

參照 DB 連線功能及表格（該表格用來作為來源節點名稱所識別之 Modeler 串流中給定分支的來源）。

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

table 是資料庫表格名稱。此表格用於置換串流來源節點。node 是串流的資料來源名稱。dbRef 參照 dbDefinitions 物件所定義的 DB 連線功能，而 table 是作為 Modeler 串流中給定分支輸入使用的資料庫表格。node 會將 Modeler 串流中的原始來源節點識別成要取代為使用這些已提供參數所建構的 DB 來源節點。

### 匯出節點設定

參照 DB 連線功能及表格（該表格用來持續保存終端機節點名稱所識別之 Modeler 串流中給定分支的資料）。

透過 insertMode 屬性傳達持續保存資料時所使用的控制方法：

**Append** - 表格必須存在並且相容才能插入

**Create** - 建立表格（如果已存在，則會傳回錯誤）

**Drop** - 如果參照的表格已存在，則會予以捨棄並重建

**Refresh** - 會先刪除表格中的現有列，再插入新列
   
table 是要將工作結果寫入其中的資料庫表格名稱。node 是串流的終端機節點名稱。node 與「來源節點設定」類似，會將 Modeler 串流中的原始「輸出」節點識別成要取代為使用這些已提供參數所建構的 DB 匯出節點。

**附註**：「匯出節點設定」及「來源節點設定」對於讓工作順利執行而言十分重要。

### 參數值置換

若要在執行工作之前置換串流層次參數的預設值，您可以指定要在工作定義中使用的名稱/值配對。

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

執行評估工作類型時傳回之評估文件所需的報告格式

從「評估工作執行」產生的所有文件都會組合成一個 .zip 檔案。只會執行可支援所指出 reportFormat 的文件建置器節點，並在順利執行工作之後傳回其內容。

支援下列格式：HTML、JPG、PNG、RTF、SAV、TAB 及 XML。

```
"reportFormat": "HTML"
```
{: codeblock}

### 完整 JSON 範例

```
{ 
     "action": "TRAINING",
     "model": {
          "id": "str2"
          "name": "stream1.str"
     },
     "dbDefinitions":{
          "db":{
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
               "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
          "value": "v1"
     },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## 批次工作 API 詳細資料

下列各節提供批次工作 IBM® SPSS® Modeler 檔案管理 API 詳細資料。

`PUT /v1/file/{id}`

說明：上傳 IBM® SPSS® Modeler 串流檔以用於批次工作中。

**附註**：如果檔案已儲存在指定的 ID 下，則會改寫它。

### 內容類型

```
@Consumes({ "multipart/form-data"  })
@Produces({"application/json"})
```
{: codeblock}

### 參數

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

使用者指定的 `file ID`。在服務實例儲存庫中必須是唯一的：

```
@PathParam("id")
```
{: codeblock}

### 回應

成功。傳回可用來從服務實例儲存庫下載檔案的 URL：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

說明：從服務實例儲存庫中，刪除指定 ID 下所儲存的檔案。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

上傳時檔案的使用者指定 ID：

```
@PathParam("id")
```
{: codeblock}

回應：

成功。已刪除指定的檔案：

```
@ApiResponse(code = 200)
```
{: codeblock}

錯誤。在服務實例儲存庫中，此 ID 下所儲存的檔案不存在：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

說明：傳回服務實例儲存庫中管理之所有檔案的 ID 清單，來進行批次工作處理。

內容類型：

```
@Produces({"application/json"})
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

回應：

成功。傳回 `file ID` 值的 JSON 陣列：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

說明：擷取在所指定 ID 下儲存用於批次工作處理的 IBM® SPSS® Modeler 串流檔。

內容類型：

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

參數：

傳回為佈建或連結中認證的存取金鑰：

```
@QueryParam("accesskey")
```
{: codeblock}

上傳時檔案的使用者指定 ID：

```
@PathParam("id")
```
{: codeblock}

回應：

成功。傳回 IBM® SPSS® Modeler 檔案：

```
@ApiResponse(code = 200)
```
{: codeblock}

錯誤。在服務實例儲存庫中，此 ID 下所儲存的檔案不存在：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他錯誤。傳回異常狀況的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

## 進一步瞭解

如需記事本中批次工作採用的範例，請參閱 [From SPSS stream to batch scoring with Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37)。

如需工作定義 JSON 的相關資訊，請參閱[工作定義 JSON](#job-definition-json)。
