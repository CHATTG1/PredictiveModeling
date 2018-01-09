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

모델을 배치하고 배치된 모델에 대해 스코어링을 요청하여 예측 분석을 생성하려면 {{site.data.keyword.pm_full}} 서비스를 사용하십시오. 다음 시나리오는 이를 수행하는 방법에 대한 예를 제공합니다.
{: shortdesc}

**시나리오 이름**: 제품 라인 예측

**시나리오 설명**: 아웃도어 장비를 판매하는 회사가 자사 제품 라인에서 고객 관심사항을 예측하는 모델을 구축하고자 합니다. 
데이터 과학자는 예측 모델을 준비하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 고객 관심사항의 예측을 생성하는 것입니다. 

## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 **샘플** 탭으로 이동하십시오. 
2. **샘플 모델** 섹션에서 **제품 라인 예측** 타일을 찾아 **모델 추가** 아이콘(+)을 클릭하십시오. 

샘플 **제품 라인 예측** 모델이 **모델** 탭의 사용 가능한 모델 목록에 표시됩니다. 


## 온라인 배치 작성

1. {{site.data.keyword.pm_full}} 대시보드의 **모델** 탭으로 이동하십시오. 
2. **조치** 메뉴에서 **배치 작성**을 클릭하십시오.
3. **배치 작성** 양식에서 **이름**, **설명** 및 **온라인 유형** 필드를 완성하십시오. 
4. **저장**을 클릭하십시오. 

온라인 배치가 **배치** 탭의 사용 가능한 배치 목록에 표시됩니다. 

## 배치 세부사항 확보

배치 모델과 관련된 상태, 스코어링 엔드포인트 주소(`Scoring Endpoint`) 및
매개변수를 확인할 수 있습니다. 

1. {{site.data.keyword.pm_full}} 대시보드의 **배치** 탭으로 이동하십시오. 
2. **조치** 메뉴에서 **세부사항 보기**를 클릭하십시오.

스코어링을 요청하는 데 필요한 `스코어링 엔드포인트` 값을 기록해 두십시오. 


## 스코어 요청 작성

스코어링 엔드포인트를 작성한 후에는 스코어링을 요청하여 예측을 생성할 수 있습니다. 다음 시나리오에서는 고객 레코드가 예측 모델에 전달되고 스포츠 제품 예측이 리턴됩니다. 

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
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

출력
예제: 

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

예를 들어, 55세인 경영자는 등산 장비에 관심을 갖고 있으며
23세인 학생은 개인 액세서리에 관심을 갖고 있음을 알 수 있습니다. 

## 자세히 보기

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM® Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
