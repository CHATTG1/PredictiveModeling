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

# 클라이언트 라이브러리

## 공통 API 클라이언트 라이브러리 <span class='tag--beta'>베타</span>

IBM Watson Machine Learning 서비스를 Python 프로그래밍 언어와 함께 사용하려는 경우에는 PyPI를 사용하여 `watson-machine-learning-client` 클라이언트 라이브러리를 설치할 수 있습니다. 

이를 수행하는 방법에 대한 세부사항을 보려는 경우에는 다음 기술을 보여주는 [Spark MLlib 노트북](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) 또는 [scikit-learn 노트북](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a) 샘플 노트북을 참조할 수 있습니다. 

* 모델 지속성
* 온라인 배치
* 공통 API 클라이언트를 사용한 스코어링

공통 API 클라이언트 라이브러리에 대한 문서는 [여기](http://wml-api-pyclient.mybluemix.net/)에 있습니다. 

### 제한사항

* 공통 API 클라이언트는 **베타** 단계입니다. 
* 공통 API 클라이언트를 사용하기 위해서는 먼저 XGboost, scikit-learn 또는 pyspark 패키지 중 하나 이상을 설치해야 합니다. 
* Python 3만 지원됩니다. 

## 저장소 API 클라이언트 라이브러리

Machine Learning 서비스의 저장소 REST API에 대한 클라이언트 라이브러리 랩퍼는 Python 및 Scala 프로그래밍 언어로 개발되었습니다. 

`repository` 클라이언트 라이브러리를 사용하여 Watson Machine Learning 저장소에 모델 지속성을 구현하는 데 대한 자세한 정보는 [샘플 노트북](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)을 참조하십시오. 

저장소 라이브러리에 대한 자세한 정보는 다음 주제를 참조하십시오. 

* [Scala 저장소 모듈 문서](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python 저장소 모듈 문서](https://watson-ml-staging-libs.mybluemix.net/repository-python/)
### 제한사항

저장소 라이브러리는 [IBM Data Science Experience](https://datascience.ibm.com)를 사용하는 경우에만 사용 가능합니다. 
