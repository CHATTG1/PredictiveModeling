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

# 할당량 정책

{{site.data.keyword.pm_full}} 엔진에서는 적절한 리소스 할당 및 사용을 위해 인스턴스별 할당량을 적용합니다. 이러한 정책은 계정 프로파일, 히스토리 서비스 사용 및 리소스 가용성에 따라 달라집니다. 할당량 정책은 가격 책정 플랜을 통해 정의되지 않은 한도를 설명하며 **사전 통보 없이 변경될 수 있습니다**. 현재 적용되는 서비스 할당량 한도는 다음과 같습니다.
{: shortdesc}

## 리소스 사용

리소스는 다음 최대값으로 제한됩니다. 

-  **공개된 최대 모델 수**: 200(Lite 플랜), 1000(Standard & Professional 플랜)
-  **최대 Spark ML 모델 크기**: 200MB
-  **배치된 최대 모델 수**: 1000(Standard & Professional 플랜)

**참고**: Lite 플랜의 한도를 알아보려면 [Bluemix 카탈로그](https://console.bluemix.net/catalog/)에서 [Lite 플랜](https://console.bluemix.net/catalog/services/machine-learning)에 대한 설명을 참조하십시오. 

## 서비스 요청 한도

특정 API에 대한 분당 요청 수는 제한되어 있습니다. 애플리케이션에서 더 높은 한도를 필요로 하는 경우에는 [IBM Bluemix 지원 센터에 문의](https://support.ng.bluemix.net/)하여 할당량을 늘려야 합니다. 

## 배치 요청

배치 API 요청에는 다음 한도가 적용됩니다.

-  **기간**: 60초
-  **기본 한도**: 10

## 온라인 예측 요청

[온라인 예측](pm_service_api_spark_building.html) 요청에는 다음 한도가 적용됩니다.

-  **기간**: 60초
-  **기본 한도**: 500

## 리소스 관리 요청

다음 한도는 이 목록에 결합된 총 요청에만 적용됩니다.

[토큰](https://watson-ml-api.mybluemix.net/#/Token): 요청 게시, 가져오기, 삭제, 넣기, 패치

[배치](https://watson-ml-api.mybluemix.net/#/Deployments): 요청 게시, 가져오기, 삭제, 넣기, 패치

-  **기간**: 60초
-  **기본 한도**: 100

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM® Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
