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

# 사용자 정의 모델의 개발 및 지속성

{{site.data.keyword.pm_full}} 서비스를 사용하여, 모델을 배치하고 배치된 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
{: shortdesc}

## 사용자 정의 모델에 대해 작업

### 시나리오 이름: 제품 라인 예측

### 시나리오 설명:

고객이 유럽에서 가장 유명한 체인점 중 하나를 운영하고 있습니다. 개인 용품, 캠핑 장비 및 아웃도어 보호 장비와 같은 제품 라인에 관해서 고객의 관심 분야를 확인할 수 있도록 사용자에게 요청합니다.
데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

### 사용자 정의 모델의 개발 및 지속성

#### Data Science Experience 사용

[Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience)를 사용하여 사용자 정의 모델을 작성할 수 있습니다. 가입한 후에는 로그인하여 다음 단계를 완료해야 합니다. 

1. 조직과 영역을 작성하십시오. 처음 로그인할 때는 이 정보를 제공해야 합니다. 기본값을 수락하려면 **계속**을 클릭하십시오. 
2. 조직이 작성되고 나면, **프로젝트**로 이동하여
**새 프로젝트**를 클릭하십시오. 
3. 프로젝트에 이름 및 설명을 지정한 후 **작성**을 클릭하십시오. 지정한 프로젝트 이름이 대상 컨테이너 이름으로 사용됩니다. 
4. 프로젝트가 작성되고 나면 다음 태스크 중 하나를 수행할 수 있습니다. 
   
   *  **노트북을 추가**하고 고유 모델 개발을 시작하거나 다음 샘플 노트북 중 하나를 업로드합니다. 
        *  [Python으로 Spark MLlib 모델 개발](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Scala로 Spark MLlib 모델 개발](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Python으로 scikit-learn 모델 개발](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * 모델 마법사를 사용하여 **모델을 추가**하고 고유 모델 개발을 시작합니다. 


#### 로컬 환경 사용

원하는 환경을 사용하여 모델을 개발하고, PyPI에서 사용 가능한 Watson Machine Learning [공통 API 클라이언트 라이브러리]()를 사용하여 이를 공개하고, 배치하고 스코어할 수도 있습니다.
클라이언트 라이브러리에 대한 자세한 정보는 샘플 [노트북](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125) 및 [문서](pm_service_client_library.html)를 참조하십시오. 

**참고:** 공통 API 클라이언트 라이브러리는 **베타** 단계입니다. 

### 사용자 정의 모델의 배치 및 스코어링

API를 사용하여 온라인, 일괄처리 및 스트리밍 모델을 배치하고 스코어할 수 있습니다. 

*  [온라인 모델 배치](pm_service_api_spark_online.html)
*  [일괄처리 모델 배치](pm_service_api_spark_batch.html)
*  [스트리밍 모델 배치](pm_service_api_spark_streaming.html)

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM® Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
