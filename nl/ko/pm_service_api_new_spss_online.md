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

# REST API(신규) <span class='tag--beta'>베타</span>

단일 사용자 경험을 제공하기 위해, {{site.data.keyword.pm_full}} 서비스는 기타 지원되는 프레임워크를 준수하는 [REST API](http://watson-ml-api.mybluemix.net/)를 제공합니다. 사용자는 SPSS® 모델, 또는 scikit-learn, XGboost 또는 Spark MLlib와 같은 기타 프레임워크에 대해 동일한 API 요청을 사용할 수 있습니다.
{: shortdesc}

**참고**: 이 기능은 베타 단계입니다. 참여하는 데 관심이 있으면 자신을 대기 목록에 추가하십시오!
자세한 정보는 [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist)를 참조하십시오.

{{site.data.keyword.pm_full}} 서비스 REST API를 사용하여, 온라인 배치를 작성하고 배치된 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
이 REST API에 대해 자세히 알아보려면 다음 고객 만족도 시나리오와, 시나리오를 실행하는 데 `bash` 명령을 사용하는 [샘플 Jupyter 노트북](https://dataplatform.ibm.com/analytics/notebooks/3c2ef65c-7d0e-4ff7-ab11-578ef2a46d66/view?access_token=21517d9a2f59b1ff5e386982b3fa03d7b41cff53a22bd30b5dc7785535b0b80d)을 사용하십시오. 

**시나리오 이름**: 고객 만족도 예측

**시나리오 설명**: 통신사에서는 어떤 고객이 떠날 위험성이 있는지 알려고 합니다. 제시된 모델은 고객 이탈을 예측합니다. 데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 **샘플** 탭으로 이동하십시오. 
2. **샘플 모델** 섹션에서 **고객 만족도 예측(SPSS 모델)** 타일을 찾아 **모델 추가** 아이콘(+)을 클릭하십시오. 

샘플 고객 만족도 예측 모델이 **모델** 탭의 사용 가능한 모델 목록에 표시됩니다. 

## 액세스 토큰 생성

IBM Watson Machine Learning 서비스 인스턴스의 서비스 신임 정보 탭에서 사용 가능한 사용자 이름 및 비밀번호를 사용하여 액세스 토큰을 생성하십시오. 

요청 예제: 

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

출력
예제: 

```
{"token":"**********"}
```
{: codeblock}

환경 변수 토큰에 토큰 값을 지정하려면 다음 터미널
명령을 사용하십시오.

```
token="<token_value>"
```
{: codeblock}

## 공개된 모델에 대해 작업

다음 API 호출을 사용하여 다음 값을 포함하는 인스턴스 세부 사항을 가져오십시오. 

* published_models `url` 값
* deployments `url` 값
* 사용 정보

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

출력
예제: 

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

모델 세부사항을 가져오는 다음 API 호출을 실행할 수 있도록 **published_models** `url`을 제공하십시오. 

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

출력
예제: 

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
               "type": "struct",
    "fields": [
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
               "type": "struct",
    "fields": [
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

다음 단계에서 온라인 배치를 작성하는 데 필요한 **deployments** `url`을 기록해 두십시오. 

## 온라인 배치 작성

예측 모델의 온라인 배치를 작성하려면 다음 API 호출을 사용하십시오. 

요청 예제: 

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Customer Satisfaction Prediction", "description": "Customer Satisfaction Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


출력
예제: 

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
         "author":{
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

## 배치 세부사항 확보

배치 모델과 관련된 상태, 스코어링 엔드포인트 주소(`scoring_url`) 및
매개변수를 확인할 수 있습니다. 

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력
예제: 

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
               "author":{
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

## 스코어 요청 작성

스코어링 엔드포인트(`scoring_url`)가 작성되었으므로 이제 스코어링을 요청하여 예측을 생성할 수 있습니다. 다음 시나리오에서는 고객 레코드가 예측 모델에 전달되고 만족도 예측이 리턴됩니다. 

샘플 레코드 헤더:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges
```
{: codeblock}


샘플 고객 레코드:

```
"3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768
```
{: codeblock}


요청 예제: 

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"values":[["3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

출력
예제: 

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

결과에서, 이 고객이 만족하고 있음을 볼 수 있습니다. 

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM® Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
