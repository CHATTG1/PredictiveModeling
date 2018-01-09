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

# REST API

{{site.data.keyword.pm_short}} 서비스는 모든 프로그래밍 언어에서 호출할 수 있으며 사용자의 애플리케이션에서 Data Science Experience 분석과의 통합을 허용하는 REST API 세트입니다. 
애플리케이션의 사용자에게 더 높은 가치를 제공하기 위해 {{site.data.keyword.Bluemix_short}} 애플리케이션을 {{site.data.keyword.pm_short}} 서비스 인스턴스에 바인드하고
애플리케이션에서 필요로 하는 예측 분석을 생성하십시오. [관리 대시보드](pm_service_ui_spark.html)에서
모델을 관리하고, 대시보드를 사용하여 애플리케이션과 통합된 온라인, 일괄처리 또는
스트리밍 배치를 작성합니다.
{: shortdesc}

강력한 [REST API](https://watson-ml-api.mybluemix.net/)를 통해 서비스 인스턴스에 배치된 Spark, Python 및 IBM® SPSS® 모델에 대한 애플리케이션을 개발하십시오. 

*  지정된 예측 모델에 대한 메타데이터 검색
*  모델 배치 및 배치된 모델 관리
    *  온라인 배치(스코어링)
    *  일괄처리 배치(IBM Cloud Object Storage 및 {{site.data.keyword.dashdbshort}} 지원)
    *  스트림 배치(IBM Cloud Message Hub 지원)
*  지정된 배치에 대한 메타데이터 검색
*  연속 학습 시스템을 사용하여 배치된 모델을 모니터하고 재훈련
*  배치된 모델에 대해 스코어링 요청을 작성하여 예측 분석 생성

자세한 정보는 다음 REST API 사용 예를 참조하십시오. 

*  [온라인 모델 배치](pm_service_api_spark_online.html)
*  [온라인 모델 스코어링](pm_service_api_develop_score.html)
*  [일괄처리 모델 배치](pm_service_api_spark_batch.html)
*  [스트림 모델 배치](pm_service_api_spark_streaming.html)
*  [연속 학습 시스템](pm_service_api_spark_learning_system.html)

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
