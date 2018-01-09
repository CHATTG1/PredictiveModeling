---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# REST API（新）<span class='tag--beta'>Beta</span>

为了提供单用户体验，{{site.data.keyword.pm_full}} 服务提供了兼容其他支持框架的 [REST API](http://watson-ml-api.mybluemix.net/)。可以对 SPSS® 模型和其他框架（例如，scikit-learn、XGboost 或 Spark MLlib）使用相同的 API 请求。
{: shortdesc}

**注**：目前此功能为 beta 版本。如果您想要参与，请将您自己加入等待列表！
有关更多信息，请参阅：[https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist)。

通过使用 {{site.data.keyword.pm_full}} 服务 REST API，可以创建联机部署，然后通过对已部署的模型发出评分请求来生成预测性分析。要熟悉 REST API，请使用以下客户满意度场景以及使用 `bash` 命令运行此场景的[样本 Jupyter 配置页](https://dataplatform.ibm.com/analytics/notebooks/3c2ef65c-7d0e-4ff7-ab11-578ef2a46d66/view?access_token=21517d9a2f59b1ff5e386982b3fa03d7b41cff53a22bd30b5dc7785535b0b80d)。

**场景名称**：客户满意度预测

**场景描述**：一家通信公司希望了解哪些客户有流失风险。
展示的模型预测客户流失率。为此，数据研究员开发了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到**客户满意度预测（SPSS 模型）**磁贴，并单击**添加模型**图标 (+)。

样本“客户满意度预测”模型将显示在**模型**选项卡上的可用模型列表中。

## 生成访问令牌

使用 IBM Watson Machine Learning 服务实例的“服务凭证”选项卡上提供的用户名和密码来生成访问令牌。

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

使用以下 API 调用可获取实例详细信息，其中包含以下值：

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

提供 **published_models** `url` 以运行以下 API 调用来获取模型详细信息：

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
"guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "created_at":"2017-11-06T11:34:17.991Z",
            "modified_at":"2017-11-06T11:34:18.227Z"
         },
   "entity":{
      "runtime_environment":"spss-modeler-17.0",
            "learning_configuration_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_configuration",
            "author":{
               "name":"IBM",
            "email":""
         },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts customer satisfaction in telco churn model",
            "label_col":"Churn",
            "learning_iterations_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_iterations",
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
            "feedback_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/feedback",
            "latest_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments"
            },
            "evaluation_metrics_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/evaluation_metrics",
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

请记下在下一步中创建联机部署所需的 **deployments** `url`。

## 创建联机部署

使用以下 API 调用来创建预测模型的联机部署。

请求示例：

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Customer Satisfaction Prediction", "description": "Customer Satisfaction Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


输出示例：

```
{
   "metadata":{ 
"guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
      "created_at":"2017-11-06T12:03:05.080Z",
      "modified_at":"2017-11-06T12:04:10.766Z"
   },
   "entity":{
      "runtime_environment":"spss-modeler-17.0",
      "name":"Customer Satisfaction Deployment",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
      "description":"My 1st Deployment",
      "published_model":{
         "author": {
"name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
         "description":"Predicts customer satisfaction in telco churn model",
         "created_at":"2017-11-06T12:01:04.394Z"
      },
      "model_type":"spss-modeler-model-17.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
         "created_at":"2017-11-06T11:34:18.227Z"
      }
   }
}
```
{: codeblock}

## 获取部署详细信息

可以检查状态、评分端点地址 (`scoring_url`) 以及与部署的模型相关的参数。

请求示例：

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

输出示例：

```
{
   "count":1,
   "resources":[
      {
         "metadata":{ 
"guid":"1466867a-99aa-4708-a22c-f4db06caacf8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8",
            "created_at":"2017-11-06T12:03:05.080Z",
            "modified_at":"2017-11-06T12:04:10.766Z"
         },
   "entity":{
      "runtime_environment":"spss-modeler-17.0",
            "name":"Customer Satisfaction Deployment",
            "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online",
            "description":"My 1st Deployment",
            "published_model":{
               "author": {
"name":"IBM",
            "email":""
         },
               "name":"Customer Satisfaction Prediction",
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
               "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
               "description":"Predicts customer satisfaction in telco churn model",
               "created_at":"2017-11-06T12:01:04.394Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "status":"ACTIVE",
            "type":"online",
            "deployed_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            }
         }
      }
   ]
}
```
{: codeblock}

## 发出评分请求

因为评分端点 (`scoring_url`) 已创建，所以现在可以通过发出评分请求来生成预测了。在以下场景中，客户记录会传递到预测模型，并会返回满意度预测。

样本记录头：

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges
```
{: codeblock}


样本客户记录：

```
"3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768
```
{: codeblock}


请求示例：

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"values":[["3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

输出示例：

```
{
   "fields": [
"customerID",
      "Churn",
      "Predicted Churn",
      "Probability of Churn"
   ],
   "values":[
      [
         "3638-WEABW",
         "No",
         "No",
         0.0526309571556145
      ]
   ]
}

```
{: codeblock}

从结果中，可以看出此客户很满意。

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM® Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
