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

# 온라인 모델 배치

{{site.data.keyword.pm_full}} 서비스를 사용하여, 모델을 배치하고 배치된 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
{: shortdesc}

**시나리오 이름**: 제품 라인 예측

**시나리오 설명**: 아웃도어 장비를 판매하는 회사가 특정 제품 라인에 대한 고객 관심도를 예측하는 모델을 빌드하려 합니다. 
데이터 과학자는 예측 모델을 준비하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 고객 관심사항의 예측을 생성하는 것입니다. 

## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 샘플 탭으로 이동하십시오. 

2. 샘플 모델 섹션에서 제품 라인 예측 타일을 찾고
모델 추가 아이콘(+)을 클릭하십시오. 

샘플 제품 라인 예측 모델이 모델 탭의 사용 가능한 모델 목록에 표시됩니다. 

## 액세스 토큰 생성

{{site.data.keyword.pm_full}} 서비스 인스턴스의 **서비스 신임 정보** 탭에서 사용 가능한 `user` 및 `password`를 사용하여 액세스 토큰을 생성하십시오. 

요청 예제: 

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

출력 예제: 

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

출력 예제: 

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


**published_models** `url`이 있으므로 다음 API 호출을 사용하여 모델의 세부사항을 가져오십시오.

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

출력 예제: 

```
{
   "count":1,
   "resources":[
      {
         "metadata":{
      "guid":"7715dfcc-3005-4bc2-8bee-58ebdc9a43f3",
            "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publishepublished_model_id}",
            "created_at":"2017-07-10T12:45:56.623Z",
            "modified_at":"2017-07-10T12:45:56.710Z"
         },
   "entity":{
      "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
            "email":""
         },
            "name":"Product Line Prediction",
            "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
            "label_col":"PRODUCT_LINE",
            "training_data_schema":{ },
            "latest_version":{ },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publipublished_model_id}/deployments"
            },
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
        "metadata":{
      },
                     "type":"string",
                     "name":"GENDER",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"MARITAL_STATUS",
                     "nullable":true
                  },
                  {
                     "metadata":{
      },
                     "type":"string",
                     "name":"PROFESSION",
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


다음 온라인 배치를 작성하는 데 필요한 **deployments** `url` 값을 기록하십시오. 


## 온라인 배치 작성

예측 모델의 온라인 배치를 작성하려면 다음 API 호출을 사용하십시오. 

요청 예제: 

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


출력 예제: 

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{  
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
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

출력 예제: 

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"ACTIVE",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## 스코어 요청 작성

스코어링 엔드포인트(`scoring_url`)가 작성되었으므로 이제 스코어 요청을 작성하여 예측을
생성할 수 있습니다. 이 시나리오에서는 고객 레코드가 예측 모델에 전달되며 스포츠 제품 예측이 리턴됩니다. 

샘플 레코드 헤더:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

샘플 레코드:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

요청 예제: 

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

출력 예제: 

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
   "values":[
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

예를 들어, 55세인 경영자는 등산 장비에 관심을 갖고 있으며
23세인 학생은 개인 액세서리에 관심을 갖고 있음을 알 수 있습니다. 

**참고**: scikit-learn 및 XGBoost 모델의 경우에는 스코어링 페이로드에서 `values` 필드만 필요합니다. 

요청 예제: 

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"values": [[0.0,1.0],[4.0,15.0]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
