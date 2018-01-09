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

# 部署批处理模型

通过使用 {{site.data.keyword.pm_full}} 服务，可以部署模型，然后通过对已部署的模型发出评分请求来生成预测性分析。
{: shortdesc}


**场景名称**：客户满意度预测。

**场景描述**：一家通信公司希望了解哪些客户有流失风险。
展示的模型预测客户流失率。为此，数据研究员开发了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

**注**：您还可以利用样本 Python [配置页](https://apsportal.ibm.com/analytics/notebooks/5e4963d9-faea-455d-a7db-ff6302d1d8f5/view?access_token=5d23d36be72dea35ebbde9b4b5f4a16d0053ee898f1ab2ab73cf1301ce9322be)，此配置页用于创建样本模型和预测客户流失率。

## 先决条件

要使用此示例，必须具有以下资源：

* [Object Storage](https://console.bluemix.net/catalog/services/object-storage) 实例详细信息，将用作模型的输入（要评分的客户数据）以及模型输出的存储。可以从[此处](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv)下载样本输入数据 .csv 文件。您应该将输入文件添加到 Object Storage 实例。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服务实例凭证。可以使用[此链接](https://console.bluemix.net/catalog/services/apache-spark)来创建凭证。



## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到**客户满意度预测**磁贴，并单击**添加模型**图标 (+)。

样本“客户满意度预测”模型将显示在**模型**选项卡上的可用模型列表中。

## 生成访问令牌

使用 {{site.data.keyword.pm_full}} 服务实例的“服务凭证”选项卡上提供的用户和密码来生成访问令牌。

请求示例：

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
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

使用以下 API 调用可获取实例详细信息，其中包含以下各项：

* published models `url` 值
* deployments `url` 值
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
"guid":"87452a37-6a8f-4d59-bf88-59c66b5463e4",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
      "created_at":"2017-06-23T08:31:52.026Z",
      "modified_at":"2017-06-23T08:31:52.026Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
      },
      "usage":{ },
      "plan_id":"5325f63a-683a-47f0-a04e-97e371385588",
      "account_id":"b56398ea52f470c3173f4cf3bef5cc7e",
      "status":"Active",
      "organization_guid":"3e658178-a60c-48b8-8be9-bf58cc821656",
      "region":"us-south",
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}}/deployments"
      },
      "space_guid":"c3ea6205-b895-48ad-bb55-6786bc712c24",
      "plan":"lite"
   }
}
```
{: codeblock}

通过提供 **published_models** `url` 值，可以使用以下 API 调用来获取模型详细信息：

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

输出示例：

```
{
   "count":1,
   "resources":[
      {
         "metadata":{

                     "guid":"dc46315a-c30e-46a3-8e30-33518e6f7976",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976",
            "created_at":"2017-03-21T13:49:38.711Z",
            "modified_at":"2017-03-21T13:49:38.802Z"
         },
   "entity":{
      "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
            "email":""
         },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts Telco customer churn.",
            "label_col":"Churn",
            "training_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Churn",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/dc46315a-c30e-46a3-8e30-33518e6f7976/versions/658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "guid":"658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "created_at":"2017-03-21T13:49:38.802Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976/deployments"
            },
            "input_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{ 
},
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  }
               ]
            }
         }
      }
   ]
}
```
{: codeblock}

请记下在下一步中创建批量部署所需的 **deployments** `url` 值。

## 使用 Object Storage 创建批量部署

要使用 REST API 调用来创建预测模型的批量部署，请提供以下详细信息：

*  在上一步中创建的访问令牌
*  Spark 服务凭证，可在 {{site.data.keyword.Bluemix_notm}} Spark 服务仪表板的“服务凭证”选项卡上找到。在发出部署请求之前，必须将 Spark 凭证解码为 base64，并在 `curL` 请求的头中作为 X-Spark-Service-Instance 传递。

   根据所使用的操作系统，必须发出以下其中一个终端命令执行 base64 解码，并将其分配给环境变量。

   在 **macOS** 操作系统上，请使用以下命令：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   在 **Microsoft Windows** 或 **Linux** 操作系统上，必须使用 `--wrap=0` 参数搭配 `base64` 命令来执行 base64 解码：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

*  Object Storage 详细信息，将用作模型输入（要评分的客户数据），以及模型输出的存储（在本例中为 results.csv，此文件会自动创建）。
*  要创建部署，请使用上一部分中的 **deployments** `url` 值。


请求示例：

```
curl -v -XPOST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"Customer Satisfaction Prediction",
      "type": "batch",
      "description": "Batch Deployment",
       "input":{
          "source":{
"container":"batchjob",
             "filename":"TelcoCustomerData.csv",
             "fileformat":"csv",
             "inferschema":"1",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       },
       "output":{
          "target":{
"container":"batchjob",
             "filename":"result.csv",
             "fileformat":"csv",
             "writemode":"write",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b283cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       }
    }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

输出示例：

```
{
   "metadata":{ 
"guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author": {
"name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "target":{
"fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
"tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
"fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

**注**：您还可以使用“仪表板”来创建批量部署。


## 获取部署详细信息

可以使用 **metadata** `url` 值来检查状态以及与部署模型相关的参数。请求示例：

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

输出示例：

```
{
   "metadata":{ 
"guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author": {
"name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"COMPLETED",
      "output":{
         "target":{
"fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
"tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
"fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

预测结果会保存到 IBM Object Storage 中的 .csv 文件。请参阅以下样本行以了解输出示例。

输入文件预览：

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

输出文件预览：

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}

## 删除批量部署

要删除部署，请使用以下查询：

请求示例：

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

输出示例：

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
