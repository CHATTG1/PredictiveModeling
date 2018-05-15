---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 시작하기 튜토리얼
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}}은 [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com)와 통합되어 있습니다.
{: shortdesc}

[{{site.data.keyword.DSX_full}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics)에서 기계 학습 및 인공 지능에 대해 학습하십시오.

{{site.data.keyword.pm_full}}을 통해 사용자는 기계 학습의 두 가지 기본적인 오퍼레이션(훈련 및 스코어링)을 수행할 수 있습니다.

- **훈련**은 알고리즘이 데이터 세트로부터 학습할 수 있도록 알고리즘을 개선하는 프로세스입니다. 이 오퍼레이션의 출력을 모델이라고 합니다. 모델은 수학 표현식의 학습된 계수를 포함합니다.
- **스코어링**은 훈련된 모델을 사용하여 결과를 예측하는 오퍼레이션입니다. 스코어링 오퍼레이션의 출력은 예측된 값을 포함하는 또 다른 데이터 세트입니다.

{{site.data.keyword.pm_full}}은 두 가지 주요 인물의 요구사항을 해결하기 위해 디자인되었습니다.

- 데이터 과학자: 데이터 변환 및 기계 학습 알고리즘을 이용하는 기계 학습 파이프라인을 작성합니다. 이들은 주로 노트북 또는 외부 도구를 사용하여 모델을 훈련시키고 평가합니다. 데이터 과학자는 데이터를 탐색하고 이해하기 위해 보통 데이터 엔지니어와 협업합니다.
- 개발자: 기계 학습 모델의 예측 결과물을 사용하는 지능형 애플리케이션을 빌드합니다.

기계 학습 프로세스에서 가장 중요한 단계는 훈련이지만, {{site.data.keyword.pm_full}}은 사용자가 모델을 배치하고 시간 경과에 따라 모델 및 모델의 모든 반복으로부터 실제 비즈니스 값을 얻어 모델의 기능을 능률화할 수 있게 해 줍니다.

## 정보

{{site.data.keyword.pm_full}} 서비스는 프로그래밍 언어에서
호출할 수 있는 REST API 세트입니다.

{{site.data.keyword.pm_full}} 서비스의 핵심은 배치에 있지만, IBM® SPSS® Modeler 또는 {{site.data.keyword.DSX_full}}를 사용하여 모델 및 파이프라인을 작성하고 이에 대해 작업할 수 있습니다. SPSS® Modeler 및 {{site.data.keyword.DSX_full}}(Spark MLlib 및 Python scikit-learn 사용)는 모두 기계 학습, 인공 지능 및 통계학에서 가져온 다양한 모델링 방법을 제공합니다.

## 관련 링크

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](https://datascience.ibm.com/docs/content/analyze-data/pm_service_api_spark.html) 또는 [SPSS 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오.

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오.

