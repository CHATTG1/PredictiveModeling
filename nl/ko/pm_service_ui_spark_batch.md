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

# 일괄처리 모델 배치

{{site.data.keyword.pm_full}} 서비스를 사용하여, 모델을 배치하고 배치된 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
{: shortdesc}


**시나리오 이름**: 고객 만족도 예측

**시나리오 설명**: 통신사에서는 어떤 고객이 떠날 위험성이 있는지 알려고 합니다. 제시된 모델은 고객 이탈을 예측합니다. 데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

## 전제조건

이 예에 대해 작업하려면 다음 서비스가 필요합니다. 

* 모델에 대한 입력 및 모델 출력에 대한 스토리지로 사용되는 [Object Storage](https://console.bluemix.net/catalog/services/object-storage) 인스턴스 세부사항. [여기](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv)에서 샘플 입력 데이터 .csv 파일을 다운로드하십시오. 입력 파일을 Object Storage 인스턴스에 추가해야 합니다. 
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 서비스 인스턴스 신임 정보. [이 링크](https://console.bluemix.net/catalog/services/apache-spark)를 사용하여 작성할 수 있습니다. 


## 샘플 모델 사용

1.  {{site.data.keyword.pm_full}} 대시보드의 샘플 탭으로 이동하십시오. 
2.  샘플 모델 섹션에서 고객 만족도 예측 타일을 찾아 모델 추가 아이콘(+)을 클릭하십시오. 

샘플 고객 만족도 예측 모델이 모델 탭의 사용 가능한 모델 목록에 표시됩니다. 

## Object Storage를 사용하여 일괄처리 배치 작성

1.  {{site.data.keyword.pm_full}} 대시보드의 모델 탭으로 이동하십시오. 
2.  **조치** 메뉴에서 **배치 작성**을 클릭하십시오.
3.  배치 작성 양식에 이름, 설명 및 일괄처리 유형을 제공하십시오. 
4.  다음 항목을 제공해야 합니다. 

    **입력 연결**: 모델에 대한 입력(스코어할 고객 데이터) 및 모델 출력(이 경우 자동으로 작성되는 results.csv)에 대한 스토리지로 사용되는 Object Storage 세부사항. 

    ```
       {
          "source":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **출력 연결**

    ```
       {
          "target":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Spark 연결**: Spark 서비스 신임 정보는 {{site.data.keyword.Bluemix_short}} Spark 서비스 대시보드의 서비스 신임 정보 탭에 있습니다. 

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

5.  **저장**을 클릭하십시오.

예측 결과가 IBM Object Storage의 .csv 파일에 저장됩니다. 다음은 샘플 행입니다.

입력 파일 미리보기:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

출력 파일 미리보기:

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


## 배치 세부사항 가져오기

배치된 모델과 관련된 상태 및 매개변수를 확인할 수 있습니다.

1. {{site.data.keyword.pm_full}} 대시보드의 배치 탭으로 이동하십시오.
2. **조치** 메뉴에서 **세부사항 보기**를 클릭하십시오.

## 일괄처리 배치 삭제

배치가 더 이상 필요하지 않은 경우 다음 샘플과 같은 쿼리를 사용하여
배치를 삭제할 수 있습니다.

1. {{site.data.keyword.pm_full}} 대시보드의 배치 탭으로 이동하십시오.
2. **조치** 메뉴에서 **삭제**를 클릭하십시오.

## 자세히 알아보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는 [IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오.
API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오.
