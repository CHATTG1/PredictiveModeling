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

# 스트리밍 모델 배치

{{site.data.keyword.pm_full}} 서비스를 사용하여, 모델을 배치하고 배치된 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
{: shortdesc}

**시나리오 이름**: 감성 분석

**시나리오 설명**: 마케팅 에이전시에서 특정 주제에 대한 감성에 대해 알고자 합니다.
에이전시는 문장을 POSITIVE 또는 NEGATIVE로 분류하는 모델을 개발하도록 요청합니다. 데이터 과학자는 예측 모델을 준비하고 사용자(개발자)와 이를 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

자세한 정보는 이 [문서](https://github.com/pmservice/tweet-sentiment-prediction)를 참조하십시오.

## 전제조건

이 예에 대해 작업하려면 다음 리소스가 필요합니다. 

* 모델에 대한 입력(트윗 텍스트) 및 모델 출력(예측 결과)에 대한 스토리지로 사용되는 [Message Hub](https://console.bluemix.net/catalog/services/message-hub) 주제 세부사항. 트윗 텍스트를 사용한 입력, 그리고 출력의 두 가지 주제가 작성되도록 하십시오. 
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 서비스 인스턴스 신임 정보. [이 링크](https://console.bluemix.net/catalog/services/apache-spark)를 사용하여 작성하십시오. 


## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 **샘플** 탭으로 이동하십시오. 
2. **샘플 모델** 섹션에서 **감성 예측** 타일을 찾아 모델 추가 아이콘(+)을 클릭하십시오. 

감성 예측 샘플 모델이 모델 탭의 사용 가능한 모델 목록에 표시됩니다. 


## IBM Message Hub를 사용하여 스트리밍 배치 작성

1.  {{site.data.keyword.pm_full}} 대시보드의 **모델** 탭으로 이동하십시오. 
2.  **조치** 메뉴에서 **배치 작성**을 클릭하십시오.
3.  **배치 작성** 양식에서 **이름**, **설명** 및 **스트림 유형** 필드를 완성하십시오. 
4.  다음 항목을 제공해야 합니다. 

    **입력 연결**: 모델에 대한 입력(트윗) 및 모델 출력(예측 결과)에 대한 스토리지로 사용되는 IBM Message Hub 주제 세부사항입니다. 

    ```
  {
     "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
       "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
       "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
       "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
       "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
       "user":"Dv5kEVNNsbuJ9RFE",
       "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
   "source":{
      "topic":"sinput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **출력 연결**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
       "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
       "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
       "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
       "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
       "user":"Dv5kEVNNsbuJ9RFE",
       "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
    "target":{
       "topic":"soutput",
       "type":"kafka"
    }
 }
    ```
    {: codeblock}

    **Spark 연결**: Spark 서비스 신임 정보는 {{site.data.keyword.Bluemix_notm}} Spark 서비스 대시보드의 서비스 신임 정보 탭에 있습니다. 

     ```
{
     "credentials":{
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
       "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
       "cluster_master_url": "https://spark.bluemix.net",
       "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
       "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
       "plan": "ibm.SparkService.PayGoPersonal"
     },
     "version":"2.0"
}
     ```
     {: codeblock}

5. **저장**을 클릭하십시오.

정의된 MessageHub 주제에 예측 결과가 전송됩니다(출력 연결).

## 배치 세부사항 가져오기

배치 모델과 관련된 상태 및 매개변수를 확인할 수 있습니다.

1. {{site.data.keyword.pm_full}} 대시보드의 **배치** 탭으로 이동하십시오.
2. **조치** 메뉴에서 **세부사항 보기**를 클릭하십시오.

## 스트리밍 배치 삭제

배치가 더 이상 필요하지 않은 경우 다음 샘플과 같은 조회를 사용하여 이를 삭제할 수 있습니다.

1. {{site.data.keyword.pm_full}} 대시보드의 **배치** 탭으로 이동하십시오.
2. **조치** 메뉴에서 **삭제**를 클릭하십시오.

## 자세히 알아보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는 [IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오.
API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오.
