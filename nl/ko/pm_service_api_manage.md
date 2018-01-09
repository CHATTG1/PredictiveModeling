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

# 배치된 SPSS 모델 관리

{{site.data.keyword.pm_full}} 서비스 API를 사용하여 배치할 IBM® SPSS® Modeler 스코어링 분기를 포함하는 파일을 업로드할 수 있습니다. 이 파일이 업로드되면 애플리케이션의 데이터를 스코어하는 데 이 파일을 사용할 수 있게 됩니다.
{: shortdesc}

구체적으로는 다음 태스크를 수행할 수 있습니다. 

*  [예측 모델 배치 또는 새로 고치기](#deploying-or-refreshing-a-predictive-model)
*  [현재 배치된 모든 모델의 목록 검색](#retrieving-a-list-of-all-currently-deployed-models)
*  [배치된 특정 모델 파일의 사본 다운로드](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [배치된 예측 모델 삭제](#deleting-a-deployed-predictive-model)

## 예측 모델 배치 또는 새로 고치기

각 모델 파일은
후속 서비스 호출에서 배치된 모델을 참조하는 데 사용하기 위한 편리한 별명으로 컨텍스트 ID가
지정됩니다. 컨텍스트 ID에 대한 모델이 존재하는 경우, 이는 애플리케이션에서 사용 중인 예측 분석을
새로 고치기 위한 수단으로서 다음 `PUT` 호출로 대체됩니다. 

요청 예제: 

```
PUT http://{PA Bluemix load balancer URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound application}
```
{: codeblock}

요청 매개변수:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

배치에 성공한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

배치에 실패한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## 현재 배치된 모든 모델의 목록 검색

현재 이 서비스 인스턴스에 배치된 모든 모델의 요약을 검색하려면 다음 API 호출을 사용하십시오. 

요청 예제: 

```
GET http://{PA Bluemix load balancer URL}/pm/v1/model?accesskey={access_key for this bound application}
```
{: codeblock}

요청 매개변수:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

배치된 모델 요약에 대한 요청이 성공한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

배치된 모델 요약에 대한 요청이 실패한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
         }
```
{: codeblock}

## 배치된 특정 모델 파일의 사본 다운로드

배치된 특정 모델 파일의 사본을 다운로드하려면 다음 API 호출을 사용하십시오. 

요청 예제: 

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

요청 매개변수:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

다운로드 요청이 성공한 경우의 응답:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

다운로드 요청이 실패한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":String // failure reason
        }
```
{: codeblock}

## 배치된 예측 모델 삭제

Machine Learning 서비스 인스턴스에서 예측 모델을 삭제하려면 다음 API 호출을 사용하십시오. 이 호출을 실행하고 나면 해당 예측 모델이 더 이상 애플리케이션의 데이터를 다운로드하거나 스코어할 수 없게 됩니다.


요청 예제: 

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}
```
{: codeblock}

요청 매개변수:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

배치 취소에 성공한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

배치 취소에 실패한 경우의 응답:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
        }
```
{: codeblock}

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
