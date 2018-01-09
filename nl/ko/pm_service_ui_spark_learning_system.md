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

{{site.data.keyword.pm_full}} 연속 학습 시스템은 예측 품질을 보장하기 위해 모델 성능, 재훈련 및 재배치에 대한 자동화된 모니터링을 제공합니다.
{: shortdesc}

**시나리오 이름**: 심장병 치료용으로 선택할 수 있는 최고의 약물.

**시나리오 설명**: 심장병 치료 약물을 생산하는 생물 의학 회사에서 최고의 심장병 치료 약물을 선택하는 모델을 배치하고자 합니다. 약물 효과에 관한 새 증거가 발견되면 모델 업데이트를 고려해야 합니다. 데이터 과학자는 예측 모델을 준비하고 사용자(개발자)와 이를 공유합니다. 사용자의 태스크는 모델을 배치하고, 학습 구성을 설정하고, 학습 반복을 실행하여 모델을 평가하고, 재훈련하고 재배치하는 것입니다. 

## 전제조건

이 예에 대해 작업하려면 다음 서비스가 필요합니다. 

* 피드백 데이터 저장을 위한 [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 서비스

   아직 Apache Spark 인스턴스가 없는 경우에는 [이 링크](https://console.bluemix.net/catalog/services/apache-spark)를 사용하여 작성하십시오. 

## 샘플 모델 사용

1. {{site.data.keyword.pm_full}} 대시보드의 **샘플** 탭으로 이동하십시오. 
2. **샘플 모델** 섹션에서 **심장병 약물 선택** 타일을 찾아 **모델 추가** 아이콘(+)을 클릭하십시오. 

샘플 심장병 약물 선택 모델이 **모델** 탭의 사용 가능한 모델 목록에 표시됩니다. 


## 공개된 모델의 연속 학습 시스템 설정

1.  {{site.data.keyword.pm_full}} 대시보드의 **모델** 탭으로 이동하십시오. 
2.  **조치** 메뉴에서 **세부사항 보기**를 클릭하십시오.
3.  **재훈련 세부사항**으로 화면이동하여 **편집**을 누르십시오.
4.  다음 항목을 제공해야 합니다. 

    **피드백 연결**: 모델 평가 및 재훈련을 위한 입력(피드백 데이터)으로 사용되는 {{site.data.keyword.dashdbshort}} 세부사항입니다. 

    ```
    {
        "connection": {
      "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
      "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    다음 지시사항을 사용하여 {{site.data.keyword.dashdbshort}}에서 `DRUG_FEEDBACK_DATA` 테이블을 준비하십시오. 
    
    - [{{site.data.keyword.dashdbshort}} 서비스](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) 인스턴스(기본 플랜이 제공됨)를 작성하십시오. 
    - **{{site.data.keyword.dashdbshort_notm}}**에서 **DRUG_FEEDBACK_DATA** 테이블을 작성하십시오. 
      + GitHub 저장소에서 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 파일을 다운로드하십시오. 
      + **콘솔 열기**를 클릭하여 **{{site.data.keyword.dashdbshort_notm}}**를 시작하십시오. 
      + **데이터 로드**를 클릭하고 **데스크탑** 로드 유형을 선택하십시오. 
      + 이전에 다운로드한 파일을 로드 영역으로 **끌어** 온 후 **다음**을 클릭하십시오. 
      + **스키마**를 선택하여 데이터를 가져오고 **새 테이블**을 클릭하십시오.
      + 새 테이블의 이름을 입력하고 **다음**을 클릭하십시오. 
      + 세미콜론(;)을 **필드 구분 기호**로 사용하십시오. 
      + **다음**을 클릭하여 업로드된 데이터로 테이블을 작성하십시오. 

     **참고**: [REST API 엔드포인트](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)를 사용하여 피드백 데이터베이스에 새 피드백 레코드를 추가할 수 있습니다. 

     **최소 데이터 크기**: 모델 평가를 시작할 피드백 데이터 세트의 최소 레코드 수입니다(학습 시스템 반복의 일부). 

     ```
    20
    ```
     {: codeblock}

     **Spark 연결**: Spark 서비스 신임 정보는 {{site.data.keyword.Bluemix_notm}} Spark 서비스 대시보드의 **서비스 신임 정보** 탭에 있습니다. 

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

    **Thresholds**: 재훈련 프로세스를 트리거하는 임계값입니다. 모델 평가 중에 계산되는 정확도 값과 같은 메트릭 값이 임계값 미만인 경우 재훈련이 트리거됩니다.

     ```
    0.8
    ```

     **재훈련된 모델을 자동으로 배치**: 재훈련된 모델의 메트릭 값이 배치된 것보다 좋은 경우 이를 배치합니다.

5.  **설정**을 클릭하십시오.

## 연속 학습 시스템 반복 실행

공개된 모델은 반복적으로 평가됩니다. 모델 평가 중에 계산되는 정확도 값과 같은 메트릭 값이 임계값 미만인 경우 재훈련이 트리거됩니다. 재훈련 및 평가의 이 반복 프로세스에는 두 가지 데이터 세트(훈련 및 피드백)가 모두 사용됩니다. 

1. 새 반복을 시작하려면 모델 **세부사항** 양식의 **모니터** 탭에서 **평가**를 클릭하십시오.
3. 반복 결과를 확인하려면 평가 이벤트 테이블 또는 차트 보기로 이동하십시오.  

   사용자는 피드백 데이터(모니터링)를 기반으로 계산된 모델 품질에 대한 정보를 볼 수 있습니다. 작성되고 평가된 새 모델 버전인 재훈련된 모델 품질에 대한 정보 또한 볼 수 있습니다. 

4. 자동으로 훈련된 모델 및 해당 품질의 목록을 보려면 **세부사항 보기** > **개요**를 클릭하고 **버전 히스토리** 섹션으로 이동하십시오. 

## 피드백 데이터 저장소

[피드백 엔드포인트](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)를 사용하여 기계 학습 구성에서 정의한 피드백 저장소에 새 레코드를 전송할 수 있습니다. {{site.data.keyword.dashdbshort}}는 현재 피드백 데이터 저장소로 지원됩니다. 피드백 테이블이 없는 경우에는 서비스가 이를 작성합니다. 이 테이블이 있는 경우에는 스키마가 훈련 테이블의 스키마와 일치하는지 확인되며 이 테이블에 `_training`이라는 열이 추가됩니다. `_training` 열은 재훈련 프로세스 중에 이용되는 레코드를 표시하는 데 사용됩니다. 

피드백 데이터 저장소에 대한 자세한 정보 및 예는 [API 문서](pm_service_api_spark_learning_system.html)를 참조하십시오. 

**참고:** 피드백 테이블은 {{site.data.keyword.pm_full}} 서비스가 관리하고, 수정하고 사용합니다. 

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
