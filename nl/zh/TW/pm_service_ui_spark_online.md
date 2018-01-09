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

# 部署線上模型

若要部署模型，並針對已部署模型提出評分要求來產生預測分析，請使用 {{site.data.keyword.pm_full}} 服務。下列情境提供如何執行此作業的範例。
{: shortdesc}

**情境名稱**：產品線預測。

**情境說明**：銷售戶外設備的公司想要建置模型，預測客戶對他們產品線的興趣。資料科學家準備了一套預測模型，並將其與您（開發人員）分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測客戶興趣預測。

## 使用範例模型

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**範例**標籤。
2. 在**範例模型**區段中，尋找**產品線預測**磚，然後按一下**新增模型**圖示 (+)。

範例**產品線預測**模型即會出現在**模型**標籤的可用模型清單中。


## 建立線上部署

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**模型**標籤。
2. 從**動作**功能表選取**建立部署**。
3. 在**建立部署**表單中，完成**名稱**、**說明**及**線上類型**欄位。
4. 按一下**儲存**。

線上部署即會出現在**部署**標籤的可用部署清單中。

## 取得部署詳細資料

您可以檢查與所部署模型相關的狀態、評分端點位址 (`Scoring Endpoint`) 和參數。

1. 移至「{{site.data.keyword.pm_full}} 儀表板」的**部署**標籤。
2. 從**動作**功能表選取**檢視詳細資料**。

請記下 `Scoring Endpoint` 值，在提出評分要求時需要該值。


## 提出評分要求

建立評分端點之後，您可以提出評分要求來產生預測。在下列情境中，客戶記錄會傳遞至預測模型，並且會傳回運動產品預測。

記錄標頭範例：

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

記錄範例：

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

要求範例：

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

輸出範例：

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

例如，我們可以看到 55 歲的主管對於 Mountaineering Equipment 有興趣，而 23 歲的學生則對 Personal Accessories 有興趣。

## 進一步瞭解

如需 API 的相關資訊，請參閱 [Spark 及 Python 模型的服務 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服務 API](pm_service_api_spss.html)。

如需 IBM® SPSS® Modeler 及其提供之建模演算法的相關資訊，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

如需 IBM® Data Science Experience 及其提供之建模演算法的相關資訊，請參閱 [https://datascience.ibm.com](https://datascience.ibm.com)。
