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

# 部署联机模型

要部署模型并通过对已部署的模型发出评分请求来生成预测性分析，请使用 {{site.data.keyword.pm_full}} 服务。以下场景提供了如何执行此操作的示例。
{: shortdesc}

**场景名称**：产品系列预测。

**场景描述**：一家卖户外设备的公司想要构建模型，以预测客户对其产品系列的兴趣。
为此，数据研究员准备了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成客户兴趣预测。

## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到**产品系列预测**磁贴，并单击**添加模型**图标 (+)。

样本**产品系列预测**模型将显示在**模型**选项卡上的可用模型列表中。


## 创建联机部署

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**模型**选项卡。
2. 从**操作**菜单中，单击**创建部署**。
3. 在**创建部署**表单中，填写**名称**、**描述**和**联机类型**字段。
4. 单击**保存**。

联机部署将显示在**部署**选项卡上的可用部署列表中。

## 获取部署详细信息

可以检查状态、评分端点地址 (`Scoring Endpoint`) 以及与部署的模型相关的参数。

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**部署**选项卡。
2. 从**操作**菜单中，单击**查看详细信息**。

请记下发出评分请求所需要的`评分端点`值。


## 发出评分请求

创建评分端点后，可以通过发出评分请求来生成预测。在以下场景中，客户记录会传递到预测模型，并返回运动产品预测。

样本记录头：

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

样本记录：

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

请求示例：

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

输出示例：

```
{
   "fields": [
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

例如，您可以看到 55 岁的管理人员对 Mountaineering Equipment 感兴趣，而 23 岁的学生则对 Personal Accessories 感兴趣。

## 了解更多信息

有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM® Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
