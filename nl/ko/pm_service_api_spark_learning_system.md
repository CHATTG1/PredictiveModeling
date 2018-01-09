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

# 연속 학습 시스템

{{site.data.keyword.pm_full}} 서비스는 연속 학습 시스템을 포함합니다. 연속 학습 시스템은 예측 품질을 보장하기 위해 모델 성능, 재훈련 및 재배치에 대한 자동화된 모니터링을 제공합니다.
{: shortdesc}

**시나리오 이름**: 심장병 치료용으로 선택할 수 있는 최고의 약물.

**시나리오 설명**: 심장병 치료 약물을 생산하는 생물 의학 회사에서 최고의 심장병 치료 약물을 선택하는 모델을 배치하고자 합니다. 약물 효과에 관한 새 증거가 발견되면 모델 업데이트를 고려해야 합니다. 
데이터 과학자는 예측 모델을 준비하고 이를 사용자(개발자)와 공유합니다. 사용자의 태스크는 모델을 배치하고, 학습 구성을 설정하고, 학습 반복을 실행하여 모델을 평가하고, 재훈련하고, 재배치하는 것입니다. 

**참고**: 샘플 모델을 작성하고, 학습 시스템을 구성하고 학습 반복을 실행하는 [샘플 Python 노트북](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325)을 사용해 볼 수도 있습니다. 

## 전제조건

샘플에 대해 작업하려면 다음 서비스가 있어야 합니다. 

* 피드백 데이터 저장을 위한 [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 서비스 인스턴스 신임 정보. [이 링크](https://console.bluemix.net/catalog/services/apache-spark)를 사용하여 작성할 수 있습니다. 

## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 **샘플** 탭으로 이동하십시오. 
2. **샘플 모델** 섹션에서 심장병 약물 선택 타일을 찾아 **모델 추가** 아이콘(+)을 클릭하십시오.

샘플 **심장병 약물 선택** 모델이 **모델** 탭의 사용 가능한 모델 목록에 표시됩니다. 

## 액세스 토큰 생성

{{site.data.keyword.pm_full}} 서비스 인스턴스의 서비스 신임 정보 탭에서 사용 가능한 사용자 및 비밀번호를 사용하여 액세스 토큰을 생성하십시오. 

요청 예제: 

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
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

다음 API 호출을 사용하여 다음과 같은 인스턴스 세부 사항을 가져오십시오.

* published_models `url` 주소
* deployments `url` 주소
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

**published_models** `url` 주소를 얻은 후에는 다음 API 호출을 사용하여 모델의 세부사항을 가져오십시오. 

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
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


## 공개된 모델의 연속 학습 시스템 설정

모델의 연속 학습 시스템을 구성하려면 다음 태스크를 수행해야 합니다. 

1.  [권한 부여 헤더를 준비하십시오](#prepare-the-authorization-header). 
2.  [피드백 데이터 세트](#prepare-the-feedback-data-set)를 준비하십시오. 
3.  [구성 페이로드를 준비하십시오](#prepare-the-configuration-payload). 

권한 부여 헤더, 피드백 데이터 세트, 구성 페이로드의 준비를 완료한 후에는 연속 학습 시스템을 반복할 수 있습니다. 자세한 정보는 [연속 학습 시스템 반복 실행](#run-continuous-learning-system-iteration)을 참조하십시오. 

### 권한 부여 헤더 준비

{{site.data.keyword.pm_full}} 토큰과 Spark 인스턴스 신임 정보를 결합하는 권한 부여 헤더를 준비하려면 다음 세부사항을 제공하십시오. 

*  이전 단계에서 작성된 액세스 토큰
*  {{site.data.keyword.Bluemix_notm}} Spark 서비스 대시보드의 서비스 신임 정보 탭에서 찾을 수 있는 Spark 서비스 신임 정보. 배치를 요청하기 전에 Spark 신임 정보를 Base64로 인코딩해야 합니다. 이 정보는 X-Spark-Service-Instance 필드의 요청 헤더로 전달됩니다.

   사용 중인 운영 체제에 따라서 base64 인코딩을 수행한 다음 환경 변수에 지정하려면 다음 터미널 명령 중 하나를 실행해야 합니다. 

   **macOS** 운영 체제에서는 다음 명령을 사용하십시오. 

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   **Microsoft Windows** 또는 **Linux** 운영 체제에서는 Base64 인코딩을 수행하려면 `--wrap=0` 매개변수를 `base64` 명령과 함께 사용해야 합니다. 

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### 피드백 데이터 세트 준비

학습 시스템은 훈련 데이터(모델 훈련에 사용되는 데이터) 외에 피드백 데이터와의 연결 또한 필요로 합니다. 피드백 데이터 저장소는 필요한 경우 모델을 모니터하고 재훈련하는 데 사용됩니다. {{site.data.keyword.dashdbshort}}는 피드백 데이터 저장소로 지원됩니다. 피드백 테이블은 {{site.data.keyword.pm_short}} 서비스가 관리하고, 수정하고 사용합니다.
**{{site.data.keyword.dashdbshort}}**에서 **DRUG_FEEDBACK_DATA** 테이블을 준비하려면 다음 단계를 완료해야 합니다. 

1. [{{site.data.keyword.dashdbshort}} 서비스](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) 인스턴스(기본 플랜이 제공됨)를 작성하십시오. 
2. **{{site.data.keyword.dashdbshort}}**에서 `DRUG_FEEDBACK_DATA` 테이블을 작성하십시오. 
   1. GitHub 저장소에서 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 파일을 다운로드하십시오. 
   2. **{{site.data.keyword.dashdbshort_notm}}**를 시작하려면 **콘솔 열기** 아이콘을 클릭하십시오. 
   3. **데이터 로드** 및 **데스크탑** 로드 유형을 선택하십시오.
   4. 이전에 다운로드한 파일을 **끌어 온** 후 **다음**을 누르십시오. 
   5. 데이터를 가져오려면 **스키마**를 클릭한 후 **새 테이블**을 클릭하십시오. 
   6. **새 테이블** 필드에서 이름을 입력하고 **다음**을 클릭하십시오.
   7. **필드 구분 기호**로 세미콜론(;)을 사용하십시오.
   8. 업로드된 데이터로 테이블을 작성하려면 **다음**을 클릭하십시오.

**참고**: 이 [REST API 엔드포인트](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)를 사용하여 피드백 데이터베이스에 새 피드백 레코드를 추가할 수 있습니다. 자세한 정보는 [피드백 데이터 저장소 섹션](#feedback-data-store)을 참조하십시오. 

### 구성 페이로드 준비

학습 구성을 지정하려면 다음 엔드포인트를 사용해야 합니다. 

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

페이로드를 완료하려면 다음 매개변수의 값을 정의해야 합니다. 

<dl><dt>feedback_data_reference</dt>
<dd>피드백 데이터 저장소의 연결 및 소스</dd>
<dt>min_feedback_data_size</dt>
<dd>연속 학습 시스템 반복을 시작하기 위해 필요한 피드백 데이터 세트 내의 최소 레코드 수</dd>
<dt>auto_retrain</dt>
<dd>재훈련 프로세스가 트리거되는 시점을 [never, always, conditionally]로 지정합니다. [conditionally]로 지정되는 경우에는 모델 품질이 지정된 임계값 미만이면 재훈련 프로세스가 트리거됩니다.
</dd>
<dt>auto_redeploy</dt>
<dd>재훈련된 모델의 배치 여부를 [never, always, conditionally]로 지정합니다. [conditionally]로 지정되는 경우에는 새로 훈련된 모델 품질이 현재 배치된 모델의 품질보다 우수하면 모델 재배치가 트리거됩니다. </dd></dl>

요청 예제: 

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
           "connection": {
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

출력
예제: 

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
      "credentials":{
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
   "connection": {
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

**참고**: 이 예에서는 `auto_retrain` 및 `auto_redeploy` 매개변수에 대해 기본값을 사용합니다. 이러한 매개변수에 대한 자세한 정보는 `learning_configuration` 매개변수에 대한 [REST API 문서](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration)를 참조하십시오. 

## 연속 학습 시스템 반복 실행

학습 시스템의 반복을 시작하려면 다음 REST API 메소드를 사용하십시오. 반복되는 동안 공개된 모델이 평가됩니다. 평가된 정확도가 지정된 임계값 미만이면 모델 재훈련이 트리거됩니다. 훈련 및 피드백 데이터 세트 모두 재훈련 및 평가에 사용됩니다. 

요청 예제: 

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

출력
예제: 

```
요청이 이행되었으므로 새로운 리소스가 작성됩니다.
```
{: codeblock}

반복 상태를 확인하려면 다음 요청을 실행하십시오.

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

출력
예제: 

```
{
  "count": 1,
  "resources": [
    {
      "entity":{ 
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

다음 요청을 실행하여 평가 메트릭 및 값 목록도 가져올 수 있습니다.

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

출력
예제: 

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

## 피드백 데이터 저장소

[피드백 엔드포인트](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)를 사용하여 학습 구성에 정의된 피드백 저장소에 새 레코드(훈련 프로세스에서 사용되지 않은 레코드)를 전송할 수 있습니다. 피드백 데이터 저장소는 필요한 경우 모델을 모니터하고 재훈련하는 데 사용됩니다. {{site.data.keyword.dashdbshort}}는 피드백 데이터 저장소로 지원됩니다. 피드백 테이블이 없는 경우에는 서비스가 이를 작성합니다. 이 테이블이 있는 경우에는 스키마가 훈련 테이블의 스키마와 일치하는지 확인되며 이 테이블에 `_training`이라는 열이 추가됩니다. 이 추가 열은 재훈련 프로세스에서 이용되는 레코드를 표시하는 데 사용됩니다. 

**참고:** 피드백 테이블은 {{site.data.keyword.pm_short}} 서비스가 관리하고, 수정하고 사용합니다. 

다음 요청을 발행하여 피드백 데이터 저장소에 새 피드백 레코드를 전송할 수 있습니다. 

요청 예제: 

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

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
