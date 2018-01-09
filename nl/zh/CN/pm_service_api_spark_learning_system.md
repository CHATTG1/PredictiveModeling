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

# 持续学习系统

{{site.data.keyword.pm_full}} 服务包含持续学习系统。持续学习系统可自动监视模型性能、执行重新培训和重新部署，以确保预测质量。
{: shortdesc}

**场景名称**：选择治疗心脏疾病的最佳药物。

**场景描述**：一家生产心脏疾病药物的生物医学公司希望部署一个模型，用于选择治疗心脏疾病的最佳药物。在药物有效性方面出现新的证据时，应该考虑更新模型。为此，数据研究员准备了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，设置学习配置和执行学习迭代来评估、重新培训并重新部署模型。

**注**：您还可以利用[样本 Python 配置页](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325)，此配置页用于创建样本模型，配置学习系统并运行学习迭代。

## 先决条件

要使用样本，必须具有以下服务：

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)，用于存储反馈数据
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服务实例凭证。可以使用[此链接](https://console.bluemix.net/catalog/services/apache-spark)来创建凭证。

## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到“心脏疾病药物选择”磁贴，并单击**添加模型**图标 (+)。

样本**心脏疾病药物选择**模型将显示在**模型**选项卡上的可用模型列表中。

## 生成访问令牌

生成使用 {{site.data.keyword.pm_full}} 服务实例的“服务凭证”选项卡上所提供用户和密码的访问令牌。

请求示例：

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

输出示例：

```
{"token":"**********"}
```
{: codeblock}

使用以下终端命令将令牌值分配给环境变量令牌：

```
token="<token_value>"
```
{: codeblock}

## 处理已发布的模型

使用以下 API 调用来获取实例详细信息，例如：

* published models `url` 地址
* deployments `url` 地址
* usage information

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

输出示例：

```
{
   "metadata":{ 
"guid":"360c510b-012c-4793-ae3f-063410081c3e",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e",
      "created_at":"2017-08-04T09:15:48.344Z",
      "modified_at":"2017-08-22T08:34:47.759Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models"
      },
      "usage":{
         "expiration_date":"2017-09-01T00:00:00.000Z",
         "computation_time":{
            "limit":18000,
            "current":4
         },
         "model_count":{
            "limit":200,
            "current":2
         },
         "prediction_count":{
            "limit":5000,
            "current":16
         },
         "deployment_count":{
            "limit":5,
            "current":1
         }
      },
      "plan_id":"0f2a3c2c-456b-40f3-9b19-726d2740b11c",
      "status":"Active",
      "organization_guid":"b0e61605-a82e-4f03-9e9f-2767973c084d",
      "region":"us-south",
      "account":{
         "id":"f52968f3dbbe7b0b53e15743d45e5e90",
         "name":"Umit Cakmak's Account",
         "type":"TRIAL"
      },
      "owner":{
         "ibm_id":"31000292EV",
         "email":"umit.cakmak@pl.ibm.com",
         "user_id":"43e0ee0e-6bfb-48fc-bcd8-d61e40d19253",
         "country_code":"POL",
         "beta_user":true
      },
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/deployments"
      },
      "space_guid":"4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "plan":"standard"
   }
}
```
{: codeblock}

获得 **published_models** `url` 地址后，请使用以下 API 调用来获取模型的详细信息：

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

输出示例：

```
{  
   "count":1,
   "resources":[  
      {  
         "metadata":{  
      "guid":"361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "created_at":"2017-08-22T08:34:47.597Z",
            "modified_at":"2017-08-22T08:34:47.691Z"
         },
   "entity":{  
      "runtime_environment":"spark-2.0",
            "author":{  
               "name":"IBM",
            "email":""
         },
            "name":"Best Heart Drug Selection",
            "label_col":"DRUG",
            "training_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"DRUG"
                     },
                     "type":"string",
                     "name":"DRUG",
                     "nullable":true
                  }
               ],
               "type":"struct"
            },
            "latest_version":{  
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "guid":"8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "created_at":"2017-08-22T08:34:47.691Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{  
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/deployments"
            },
            "input_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  }
               ],
               "type":"struct"
            }
         }
      }
}
```
{: codeblock}


## 为已发布的模型设置持续学习系统

要为模型配置持续学习系统，必须执行以下任务：

1.  [准备 Authorization 头](#prepare-the-authorization-header)。
2.  [准备反馈数据集](#prepare-the-feedback-data-set)。
3.  [准备配置有效内容](#prepare-the-configuration-payload)。

完成 Authorization 头、反馈数据集和配置有效内容的准备之后，可以开始对持续学习系统进行迭代。有关更多信息，请参阅[运行持续学习系统迭代](#run-continuous-learning-system-iteration)。

### 准备 Authorization 头

要准备用于将 {{site.data.keyword.pm_full}} 令牌与 Spark 实例凭证组合在一起的 Authorization 头，请提供以下详细信息：

*  在上一步中创建的访问令牌
*  Spark 服务凭证，可在 {{site.data.keyword.Bluemix_notm}} Spark 服务仪表板的“服务凭证”选项卡上找到。在发出部署请求之前，必须将 Spark 凭证编码为 Base64。它们将在请求头中的 X-Spark-Service-Instance 字段中传递。

   根据所使用的操作系统，必须发出以下某个终端命令来执行基本 64 位编码，并将其分配给环境变量。

   在 **macOS** 操作系统上，请使用以下命令：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   在 **Microsoft Windows** 或 **Linux** 操作系统上，必须使用 `--wrap=0` 参数搭配 `base64` 命令来执行 base64 编码：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### 准备反馈数据集

学习系统需要连接到培训数据（模型培训中使用的数据）以及反馈数据。反馈数据存储器用于在需要时监视和重新培训模型。
支持 {{site.data.keyword.dashdbshort}} 作为反馈数据存储器。反馈表由 {{site.data.keyword.pm_short}} 服务管理、修改和使用。要在 **{{site.data.keyword.dashdbshort}}** 中准备 **DRUG_FEEDBACK_DATA** 表，必须完成以下步骤：

1. 创建 [{{site.data.keyword.dashdbshort}} 服务](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)实例（提供了入门套餐）。
2. 在 **{{site.data.keyword.dashdbshort}}** 中创建 `DRUG_FEEDBACK_DATA` 表。
   1. 从 GitHub 存储库，下载 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 文件。
   2. 要开始使用 **{{site.data.keyword.dashdbshort_notm}}**，请单击**打开控制台**图标。
   3. 选择**装入数据**和**桌面**装入类型。
   4. **拖动**先前下载的文件，然后按**下一步**。
   5. 要导入数据，请单击**模式**，然后单击**新建表**。
   6. 在**新表**字段中，输入名称，然后单击**下一步**。
   7. 对于**字段分隔符**，请使用分号 (;)。
   8. 单击**下一步**，以使用上传的数据来创建表。

**注**：您可以使用此 [REST API 端点](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)向反馈数据库添加新的反馈记录。有关更多信息，请参阅[反馈数据存储器](#feedback-data-store)部分。

### 准备配置有效内容

要指定学习配置，必须使用以下端点：

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

要最终完成有效内容，必须定义以下参数的值：

<dl><dt>feedback_data_reference</dt>
<dd>反馈数据存储器的连接和源</dd>
<dt>min_feedback_data_size</dt>
<dd>反馈数据集内用于开始持续学习系统迭代的最小记录数</dd>
<dt>auto_retrain</dt>
<dd>[never、always 或 conditionally] 指定何时触发重新培训过程。如果设置为 [conditionally]，那么会在模型质量低于指定阈值时，触发重新培训过程。</dd>
<dt>auto_redeploy</dt>
<dd>[never、always 或 conditionally] 指定应该何时部署重新培训的模型。如果设置为 [conditionally]，那么会在经过培训的新模型质量高于当前已部署模型的质量时，触发模型重新部署。</dd></dl>

请求示例：

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
          "definition": {
            "method": "multiclass",
           "metrics": [
             {
               "name": "accuracy",
               "threshold": 0.8
             }
           ]
         },
         "feedback_data_reference": {
           "connection":{
"db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
           },
    "source": {
      "tablename": "DRUG_FEEDBACK_DATA",
            "type": "dashdb"
           }
          },
          "min_feedback_data_size": 10,
          "auto_retrain": "conditionally",
          "auto_redeploy": "never",
          "last_training_record": 0
        }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

输出示例：

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass",
    "metrics": [
      {
        "name": "accuracy",
        "threshold": 0.8
      }
    ]
   },
   "feedback_data_reference":{
      "connection":{
"db":"BLUDB",
         "host":"awh-yp-small02.services.dal.bluemix.net",
         "username":"***",
         "password":"***"
      },
      "source":{
         "tablename":"DRUG_FEEDBACK_DATA",
         "type":"dashdb"
      }
   },
   "min_feedback_data_size":10,
   "spark_service":{
      "credentials": {
"tenant_id":"***",
         "cluster_master_url":"https://spark.bluemix.net",
         "tenant_id_full":"***",
         "tenant_secret":"***",
         "instance_id":"***",
         "plan":"ibm.SparkService.PayGoPersonal"
      },
         "version":"2.0"
      },
   "training_data_reference": {
   "connection":{
"db": "BLUDB",
     "host": "dashdb-entry-yp-dal09-08.services.dal.bluemix.net",
     "password": "***",
     "username": "***"
   },
    "source": {
      "tablename": "DRUG_TRAIN_DATA_UPDATED",
     "type": "dashdb"
    }
   }
}
```
{: codeblock}

**注**：示例使用的是 `auto_retrain` 和 `auto_redeploy` 参数的缺省值。有关这些参数的更多信息，请参阅 [REST API 文档](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration)中的 `learning_configuration` 参数。

## 运行持续学习系统迭代

要开始迭代学习系统，请使用以下 REST API 方法。在迭代期间，将对已发布的模型进行评估。如果评估出的准确性低于指定的阈值，那么会触发模型重新培训。培训和反馈数据集都会用于重新培训和评估。

请求示例：

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

输出示例：

```
该请求已执行，并导致创建了新资源。
```
{: codeblock}

要检查迭代的状态，请发出以下请求：

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

输出示例：

```
{
  "count": 1,
  "resources": [
    {
      "entity": {
        "status": {
          "state": "INITIALIZED"
        },
        "model_version": {
          "url": "https://ibm-watson-ml-svt.stage1.mybluemix.net/v2/artifacts/models/71dc688a-ebda-4174-9574-e8805059e08f/versions/de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d",
          "created_at": "2017-10-30T15:45:12.926Z",
          "guid": "de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d"
        },
      "spark_service":{
         "credentials": {
"tenant_id_full": "***",
            "tenant_secret": "***",
            "tenant_id": "***",
            "instance_id": "***",
            "plan": "ibm.SparkService.PayGoPersonal",
            "cluster_master_url": "https://spark.bluemix.net"
          },
         "version":"2.0"
      },
        "published_model": {
          "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f",
          "guid": "71dc688a-ebda-4174-9574-e8805059e08f"
        }
      },
      "metadata": {
        "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f/learning_iterations/a308838b-445f-45b8-9fbf-1c3dd1b392c1",
        "created_at": "2017-10-30T15:54:30.657Z",
        "guid": "a308838b-445f-45b8-9fbf-1c3dd1b392c1"
      }
    }
  ]
}
```
{: codeblock}

您还可以通过发出以下请求来获取评估度量及其值的列表：

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

输出示例：

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-07T10:11:02.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
      {
      "phase": "monitoring",
      "values": [
        {
          "name": "accuracy",
          "value": 0.75,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    },    
    {
      "phase": "training",
      "values": [
        {
          "name": "accuracy",
          "value": 0.88281694,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:22.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    }
  ]
}
```
{: codeblock}

## 反馈数据存储器

可以使用[反馈端点](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)将新记录（培训过程中未使用的记录）发送到学习配置中定义的反馈存储器。反馈数据存储器用于在需要时监视和重新培训模型。支持 {{site.data.keyword.dashdbshort}} 作为反馈数据存储器。如果反馈表不存在，那么服务会创建反馈表。如果该表存在，那么会验证其模式是否与培训表的模式相匹配，并且会向该表额外附加一个名为 `_training` 的列。该附加列用于标记在重新培训过程中使用的记录。

**注：**反馈表由 {{site.data.keyword.pm_short}} 服务管理、修改和使用。

可以通过发出以下请求，将新的反馈记录发送到反馈数据存储器：

请求示例：

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields": [
"AGE",
      "SEX",
      "BP",
      "CHOLESTEROL",
      "NA",
      "K",
      "DRUG"
   ],
   "values":[
      [
         16,
         "M",
         "HIGH",
         "NORMAL",
         0.58301000000000003,
         0.033884999999999998,
         "drugY"
      ]
   ]
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/feedback
```
{: codeblock}

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
