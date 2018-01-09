---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 시작하기
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}}은 사용자가 기계 학습의 두 가지 기본적인 오퍼레이션(훈련 및 스코어링)을 수행할 수 있게 해 주는 IBM Cloud 서비스입니다.
{: shortdesc}

- **훈련**은 알고리즘이 데이터 세트로부터 학습할 수 있도록 알고리즘을 개선하는 프로세스입니다. 이 오퍼레이션의 출력을 모델이라고 합니다. 모델은 수학 표현식의 학습된 계수를 포함합니다. 
- **스코어링**은 훈련된 모델을 사용하여 결과를 예측하는 오퍼레이션입니다. 스코어링 오퍼레이션의 출력은 예측된 값을 포함하는 또 다른 데이터 세트입니다. 

{{site.data.keyword.pm_full}}은 두 주요 인물의 요구사항을 해결하기 위해 디자인되었습니다. 

- 데이터 과학자: 데이터 변환 및 기계 학습 알고리즘을 이용하는 기계 학습 파이프라인을 작성합니다. 이들은 주로 노트북 또는 외부 도구를 사용하여 모델을 훈련시키고 평가합니다. 데이터 과학자는 데이터를 탐색하고 이해하기 위해 보통 데이터 엔지니어와 협업합니다. 
- 개발자: 기계 학습 모델의 예측 결과물을 사용하는 지능형 애플리케이션을 빌드합니다. 

기계 학습 프로세스에서 가장 중요한 단계는 훈련이지만, {{site.data.keyword.pm_full}}은 사용자가 모델을 배치하고 시간 경과에 따라 모델 및 모델의 모든 반복으로부터 실제 비즈니스 값을 얻어 모델의 기능을 능률화할 수 있게 해 줍니다. 

## 전제조건

{{site.data.keyword.pm_full}}을 사용하려면 {{site.data.keyword.Bluemix_short}} 카탈로그에서 여기에 [서비스 인스턴스](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/)를 작성해야 합니다. 이 설정은 사용자가 다음 태스크를 수행할 수 있게 해 줍니다. 

## 단계

1. [Machine Learning 환경 설정](ml_getting_access.html)
1. [모델 작성 및 저장](pm_custom_models.html)
2. [모델 배치](pm_service_api_spark_online.html)
3. 애플리케이션에 배치된 모델 `scoring endpoint`를 사용하여 [예측 가져오기](pm_service_api_spark_building.html)

## Machine Learning을 Data Science Experience와 함께 사용

{{site.data.keyword.pm_full}}은 IBM Data Science Experience와 통합되어 있습니다. 사용자는 Data Science Experience 노트북에서 Machine Learning API 클라이언트 라이브러리를 사용할 수 있습니다. 모델 빌더 및 플로우 편집기를 사용하려면 Machine Learning 인스턴스가 있어야 합니다. 

## Machine Learning을 SPSS Modeler와 함께 사용

{{site.data.keyword.pm_full}}은 IBM® SPSS® Modeler와 통합되어 있습니다. 사용자는 Machine Learning API를 사용하여 고급 수학 알고리즘을 이용할 수 있습니다. 


## Machine Learning을 자신의 환경과 함께 사용

{{site.data.keyword.pm_full}}은 로컬 환경을 클라우드와 연결하는 하이브리드 솔루션으로 사용할 수 있습니다. 사용자는 Machine Learning API를 사용하여 모델을 공개하고, 배치하고 스코어할 수 있습니다. 자세한 정보는 [DSX: 하이브리드 모드](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b)를 참조하십시오. 

## 정보

{{site.data.keyword.pm_full}} 서비스는 프로그래밍 언어에서
호출할 수 있는 REST API 세트입니다. 

{{site.data.keyword.pm_full}} 서비스는 배치를 중점적으로 다루지만, 사용자는 IBM® SPSS® Modeler 또는 IBM® Data Science Experience를 사용하여 모델 및 파이프라인을 작성하고 이에 대해 작업할 수도 있습니다. SPSS® Modeler 및 Data Science Experience(Spark MLlib 및 Python scikit-learn 사용)는 모두 기계 학습, 인공 지능 및 통계학에서 가져온 다양한 모델링 방법을 제공합니다. 

## 관련 링크

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을
바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[SPSS 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오.

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [SPSS 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
