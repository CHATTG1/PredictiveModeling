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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 疑難排解

以下是關於使用 {{site.data.keyword.pm_full}} 的常見疑難排解問題的回答。
{: shortdesc}

## 取得 Machine Learning 的協助及支援
{: #gettinghelp}

如果您在使用 {{site.data.keyword.pm_short}} 時有問題或疑問，可以搜尋資訊或透過討論區提問來取得協助。您也可以開立支援問題單。

使用討論區提問時，請標記您的問題，以讓 {{site.data.keyword.pm_short}} 開發團隊能看到它。

如果您有 {{site.data.keyword.pm_short}} 的相關技術問題，請將問題張貼在 <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，並使用 "ibm-bluemix" 和 "machine-learning" 來標記您的問題。

若為服務及開始使用指示的相關問題，請使用 <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 討論區。請包含 "machine-learning" 和 "bluemix" 標籤。
如需使用討論區的詳細資料，請參閱[取得協助](https://console.bluemix.net/docs/support/index.html#getting-help)。

如需開啟 IBM 支援問題單的相關資訊，或支援層次與問題單嚴重性的相關資訊，請參閱[與支援中心聯絡](https://console.bluemix.net/docs/support/index.html#contacting-support)。

## 未提供授權記號。
{: #ts_missing_authorization_token}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
`Authorization` 標頭中未提供授權記號。
{: tsCauses}
 
請在 `Authorization` 標頭中傳遞授權記號。
{: tsResolve}
       
## 無效的授權記號。
{: #ts_invalid_authorization_token}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
無法解碼或剖析已提供的授權記號。
{: tsCauses}
 
請在 `Authorization` 標頭中傳遞正確的授權記號。
{: tsResolve}
       
## 授權記號和要求中使用的 instance_id 不同。
{: #ts_not_matching_authorization_token}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
已使用的授權記號不是針對它使用對象的服務實例所產生。
{: tsCauses}
 
請在 `Authorization` 標頭中傳遞對應於使用中服務實例的授權記號。
{: tsResolve}
       
## 授權記號已過期。
{: #ts_expired_authorization_token}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
授權記號已過期。
{: tsCauses}
 
請在 `Authorization` 標頭中傳遞未過期的授權記號。
{: tsResolve}
       
## 無法使用鑑別所需的公開金鑰。
{: #ts_missing_public_key}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
這是內部服務問題。
{: tsCauses}
 
支援團隊需要修正問題。
{: tsResolve}
       
## 作業在 {{timeout}} 之後逾時
{: #ts_operation_timeout}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
執行所要求的作業期間發生逾時。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。
{: tsResolve}
       
## 無法處理的異常狀況，類型為 {{type}}，並具有 {{status}}
{: #ts_unhandled_exception_with_status}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
這是內部服務問題。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。如果多次出現，則支援團隊需要加以修正。
{: tsResolve}
       
## 無法處理的異常狀況，類型為 {{type}}，並具有 {{response}}
{: #ts_unhandled_exception_with_response}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
這是內部服務問題。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。如果多次出現，則支援團隊需要加以修正。
{: tsResolve}
       
## 無法處理的異常狀況，類型為 {{type}}，並具有 {{json}}
{: #ts_unhandled_exception_with_json}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
這是內部服務問題。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。如果多次出現，則支援團隊需要加以修正。
{: tsResolve}
       
## 無法處理的異常狀況，類型為 {{type}}，並具有 {{message}}
{: #ts_unhandled_exception_with_message}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
這是內部服務問題。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。如果多次出現，則支援團隊需要加以修正。
{: tsResolve}
       
## 找不到所要求的物件。
{: #ts_not_found}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
找不到要求資源。
{: tsCauses}
 
請確定您參照現有的資源。
{: tsResolve}
       
## 基礎資料庫報告太多要求。
{: #ts_too_many_cloudant_requests}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
使用者在給定的時間量內，傳送太多要求。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。
{: tsResolve}
       
 ## 評估的定義未定義在 artifactModelVersion 也不在部署中。需要在至少其中一個地方指定。
{: #ts_missing_evaluation_definition}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
學習配置未包含所有必要資訊
{: tsCauses}
 
請在 `learning configuration` 中提供 `definition`
{: tsResolve}
       
## 評估需要為模型指定學習配置。
{: #ts_missing_learning_configuration}
 
無法建立 `learning iteration`。
{: tsSymptoms}
 
未針對模型定義任何 `learning configuration`。
{: tsCauses}
 
請建立 `learning configuration`，並嘗試重新建立 `learning iteration`。
{: tsResolve}
       
## 評估需要在 `X-Spark-Service-Instance` 標頭中提供 Spark 實例
{: #ts_missing_spark_definition_for_evaluation}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
`learning configuration` 中沒有所有必要資訊
{: tsCauses}
 
請在學習配置或 `X-Spark-Service-Instance` 標頭中提供 `spark_service`
{: tsResolve}
       
## 模型不包含任何版本。
{: #ts_missing_latest_model_version}
 
無法建立部署也無法設定學習配置。
{: tsSymptoms}
 
有與模型持續性相關的不一致。
{: tsCauses}
 
請嘗試重新持續保存模型，並嘗試重新執行動作。
{: tsResolve}
       
## 修補作業只能修改現有的學習配置。
{: #ts_patch_non_existing_learning_configuration}
 
無法呼叫修補 REST API 方法，以修補學習配置。
{: tsSymptoms}
 
此模型未設定 `learning configuration`，或是模型不存在。
{: tsCauses}
 
請確定模型存在，且已設定學習配置。
{: tsResolve}
       
## 修補作業預期正好一個 replace 作業。
{: #ts_patch_multiple_ops}
 
無法修補部署。
{: tsSymptoms}
 
修補有效負載包含多個作業，或修補作業不是 `replace`。
{: tsCauses}
 
請在修補有效負載中使用只有一個的作業，且該作業是 `replace`
{: tsResolve}
       
## 給定的有效負載遺漏必要欄位：FIELD 或欄位的值已毀損。
{: #ts_invalid_request_payload}
 
無法處理與基礎資料集存取相關的動作。
{: tsSymptoms}
 
未適當地定義對資料集的存取。
{: tsCauses}
 
請更正資料集的存取定義。
{: tsResolve}
       
## 不支援提供的評估方法：METHOD。支援的值：VALUE。
{: #ts_evaluation_method_not_supported}
 
無法建立學習配置。
{: tsSymptoms}
 
使用了錯誤的評估方法來建立學習配置。
{: tsCauses}
 
請使用支援的評估方法，即下列其中之一：`regression`、`binary`、`multiclass`。
{: tsResolve}
       
## 每個模型只能有一個作用中的評估。因為現有的作用中評估，所以無法完成要求：{{url}}
{: #ts_active_evaluation_conflict}
 
無法建立另一個學習反覆運算
{: tsSymptoms}
 
模型只能有一個執行中的評估。
{: tsCauses}
 
請參閱已在執行的評估，或等到它結束然後開始新的評估。
{: tsResolve}
       
## 不支援部署類型 {{type}}。
{: #ts_not_supported_deployment_type}
 
無法建立部署。
{: tsSymptoms}
 
使用了不受支援的部署類型。
{: tsCauses}
 
應該使用支援的部署類型。
{: tsResolve}
       
## 輸入不正確：({{message}})
{: #ts_deserialization_error}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
剖析 json 時發生問題。
{: tsCauses}
 
請確定在要求中傳遞正確的 json。
{: tsResolve}
       
## 資料不足 - 無法計算度量值 {{name}}
{: #ts_missing_metric}
 
學習反覆運算失敗
{: tsSymptoms}
 
無法計算已定義臨界值的度量值，因為回饋資料不足
{: tsCauses}
 
請在 `learning configuration` 中檢閱並改善資料來源 `feedback_data_ref` 中的資料
{: tsResolve}
       
## 對於類型 {{type}}，必須在 `X-Spark-Service-Instance` 標頭中提供 Spark 實例
{: #ts_missing_prediction_spark_definition}
 
無法建立部署
{: tsSymptoms}
 
`batch` 和 `streaming` 部署需要提供 Spark 實例
{: tsCauses}
 
請在 `X-Spark-Service-Instance` 標頭中提供 Spark 實例
{: tsResolve}
       
## 動作 {{action}} 已失敗，訊息為：{{message}}
{: #ts_http_client_error}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
呼叫基礎服務時發生問題。
{: tsCauses}
 
如果有如何修正問題的建議，請遵循該建議。如果訊息中沒有建議，或是建議無法解決問題，請與支援團隊聯絡。
{: tsResolve}
       
## 不容許路徑 `{{path}}`。修補串流的唯一接受路徑是 `/status`
{: #ts_wrong_stream_patch_path}
 
無法修補 stream 部署。
{: tsSymptoms}
 
使用了錯誤的路徑來修補 `stream` 部署。
{: tsCauses}
 
請使用支援的路徑選項 `/status` 來修補 `stream` 部署（如果容許啟動/停止串流處理）。
{: tsResolve}
       
## 類型 `{{$type}}` 的實例不接受修補作業
{: #ts_patch_not_supported}
 
無法修補部署。
{: tsSymptoms}
 
正在修補錯誤的部署類型。
{: tsCauses}
 
請修補 `stream` 部署類型。
{: tsResolve}
       
## 資料連線 `{{data}}` 對 feedback_data_ref 而言無效
{: #ts_invalid_feedback_data_connection}
 
無法建立模型的 `learning configuration`。
{: tsSymptoms}
 
定義 feedback_data_ref 時使用了不支援的資料來源。
{: tsCauses}
 
請僅使用支援的資料來源類型 `dashdb`
{: tsResolve}
       
## 不容許路徑 {{path}}。修補模型的唯一接受路徑是 `/deployed_version/url` 或適用於 V2 的 `/deployed_version/href`
{: #ts_patch_model_path_not_allowed}
 
沒有修補模型的選項。
{: tsSymptoms}
 
模型的修補期間使用了錯誤的路徑。
{: tsCauses}
 
請使用支援的路徑來修補模型，支援的路徑容許更新已部署模型的版本。
{: tsResolve}
       
## 剖析失敗：{{msg}}
{: #ts_parsing_error}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
無法順利剖析所要求的有效負載。
{: tsCauses}
 
請確定您的要求有效負載正確無誤，而且可以正確剖析。
{: tsResolve}
       
## `learning configuration` 不支援所選模型的運行環境 {{env}}。支援的環境：[{{supported_envs}}]。
{: #ts_runtime_env_not_supported}
 
沒有建立 `learning configuration` 的選項。
{: tsSymptoms}
 
嘗試為其建立 `learning_configuration` 的模型不受支援。
{: tsCauses}
 
請為具有受支援運行環境的模型建立 `learning configuration`。
{: tsResolve}
       
## 現行方案 \'{{plan}}\' 只容許 {{limit}} 部署
{: #ts_deployments_plan_limit_reached}
 
無法建立部署。
{: tsSymptoms}
 
已達到現行方案的部署數目限制。
{: tsCauses}
 
請升級為沒有這類限制的方案。
{: tsResolve}
       
## 資料庫連線定義無效 ({{code}})
{: #ts_sql_error}
 
無法利用 `learning configuration` 功能。
{: tsSymptoms}
 
資料庫連線定義無效。
{: tsCauses}
 
請嘗試修正基礎資料庫所傳回 `code` 說明的問題。
{: tsResolve}
       
## 連接基礎 {{system}} 時發生問題
{: #ts_stream_tcp_error}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
連接基礎系統期間發生問題。可能是暫時的網路問題。
{: tsCauses}
 
請嘗試重新呼叫所需的作業。如果多次出現，請與支援團隊聯絡。
{: tsResolve}
       
## 擷取 X-Spark-Service-Instance 實例時發生錯誤：({{message}})
{: #ts_spark_header_deserialization_error}
 
無法呼叫需要 Spark 認證的 REST API
{: tsSymptoms}
 
base-64 解碼或剖析 Spark 認證時發生問題。
{: tsCauses}
 
請確定正確的 Spark 認證已正確地進行 base-64 編碼。如需相關資訊，請參閱文件。
{: tsResolve}
       
## 對於非測試版使用者已禁止此功能。
{: #ts_not_beta_user}
 
無法順利呼叫想要的 REST API。
{: tsSymptoms}
 
已呼叫的 REST API 目前為測試版。
{: tsCauses}
 
如果您有興趣參與，請將自己新增到等待清單。詳細資料可以在文件中找到。
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
無法順利呼叫 REST API。
{: tsSymptoms}
 
呼叫基礎服務時發生問題。
{: tsCauses}
 
如果有如何修正問題的建議，請遵循該建議。如果訊息中沒有建議，或是建議無法解決問題，請與支援團隊聯絡。
{: tsResolve}
       
## 已超出速率限制。
{: #ts_rate_limit_exceeded}
 
已超出速率限制。
{: tsSymptoms}
 
已超出現行方案的速率限制。
{: tsCauses}
 
若要解決此問題，請取得速率限制更高的另一個方案。
{: tsResolve}
       
## 查詢參數 `{{paramName}}` 值無效：{{value}}
{: #ts_invalid_query_parameter_value}
 
傳遞了不正確的值給查詢參數，因此發生驗證錯誤。
{: tsSymptoms}
 
取得查詢的結果時發生錯誤。
{: tsCauses}
 
請更正查詢參數值。詳細資料可以在文件中找到。
{: tsResolve}
       
## 記號類型無效：{{type}}
{: #ts_invalid_token_type}
 
關於記號類型的錯誤。
{: tsSymptoms}
 
授權時發生錯誤。
{: tsCauses}
 
記號應該以 `Bearer` 字首為開頭
{: tsResolve}
       
## 記號格式無效。應該使用 Bearer 記號格式。
{: #ts_invalid_token_format}
 
關於記號格式的錯誤。
{: tsSymptoms}
 
授權時發生錯誤。
{: tsCauses}
 
記號應該是 bearer 記號，且開頭應該以 `Bearer` 字首為開頭
{: tsResolve}

## 輸入 JSON 檔遺漏或無效：400
{: #os_invalid_input}

當您嘗試線上評分時會顯示下列訊息：**輸入 JSON 檔遺漏或無效**。
{: tsSymptoms}

當評分輸入有效負載不符合模型評分所需的預期輸入類型時，會顯示此訊息。具體而言，可能的原因如下：

- 輸入有效負載是空的。
- 輸入有效負載綱目無效。
- 輸入資料類型不符合預期的資料類型。
{: tsCauses}

請更正輸入有效負載。請確定有效負載語法正確、綱目有效，且資料類型適當。更正之後，請重新嘗試在線上評分。若為語法問題，請使用 `jsonlint` 指令驗證 JSON 檔案。
{: tsResolve}

## 授權記號已過期：401
{: #os_expired_authorization_token}

當您嘗試線上評分時會顯示下列訊息：**授權失敗**。
{: tsSymptoms}

當用於評分的記號已過期時會顯示此訊息。
{: tsCauses}

請針對此 {{site.data.keyword.pm_full}} 實例重新產生記號，然後重試一次。如果仍有這個問題，請與 IBM 支援中心聯絡。
{: tsResolve}

## 不明的部署識別：404
{: #os_unkown_depid}

當您嘗試線上評分時會顯示下列訊息：**不明的部署識別**。 {: osSymptoms}

當用於評分的部署 ID 不存在時會顯示此訊息。
{: tsCauses}

請確定您提供正確的部署 ID。若否，請使用部署 ID 來部署模型，然後重新嘗試評分。
{: tsResolve}

## 內部伺服器錯誤：500
{: #os_internal_error}

當您嘗試線上評分時會顯示下列訊息：**內部伺服器錯誤**。 {: osSymptoms}

如果線上評分所依賴的下游資料流程失敗，則會顯示此訊息。
{: tsCauses}

請等待一段時間之後，重新嘗試在線上評分。如果再次失敗，請與 IBM 支援中心聯絡。
{: tsResolve}
              
