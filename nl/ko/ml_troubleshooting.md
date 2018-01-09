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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 문제점 해결

다음은 {{site.data.keyword.pm_full}} 사용에 관한 일반적인 문제점 해결 질문에 대한 응답입니다.
{: shortdesc}

## Machine Learning에 대해 도움 및 지원 받기
{: #gettinghelp}

{{site.data.keyword.pm_short}}을 사용하는 중에 문제점 또는 질문이 있는 경우에는 정보를 검색하거나 포럼을 통해 질문하여 도움을 받을 수 있습니다. 또한 지원 티켓을 열 수도 있습니다.

포럼을 통해 질문하는 경우 {{site.data.keyword.pm_short}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오. 

{{site.data.keyword.pm_short}}에 대한 기술적 질문이 있는 경우에는 <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 질문을 게시하고 질문의 태그를 "ibm-bluemix" 및 "machine-learning"으로 지정하십시오. 

서비스 및 시작하기 지시사항에 대한 질문은 <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW 응답 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a> 포럼을 사용하십시오. "machine-learning" 및 "bluemix" 태그를 포함하십시오.
포럼 사용에 대한 자세한 정보는 [도움말 가져오기](https://console.bluemix.net/docs/support/index.html#getting-help)를 참조하십시오.

IBM 지원 티켓을 여는 데 대한 정보 또는 지원 레벨과 티켓 심각도에 대한 정보는 [지원 센터에 문의](https://console.bluemix.net/docs/support/index.html#contacting-support)를 참조하십시오

## 인증 토큰이 제공되지 않았습니다.
{: #ts_missing_authorization_token}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
`Authorization` 헤더에 인증 토큰이 제공되지 않았습니다.
{: tsCauses}
 
`Authorization` 헤더에 인증 토큰을 전달하십시오.
{: tsResolve}
       
## 올바르지 않은 인증 토큰입니다.
{: #ts_invalid_authorization_token}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
제공된 인증 토큰을 디코딩하거나 구문 분석할 수 없습니다.
{: tsCauses}
 
`Authorization` 헤더에 올바른 인증 토큰을 전달하십시오.
{: tsResolve}
       
## 요청에서 사용된 인증 토큰과 instance_id가 같지 않습니다.
{: #ts_not_matching_authorization_token}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
사용된 인증 토큰은 해당 토큰이 사용된 서비스 인스턴스용으로 생성되지 않았습니다.
{: tsCauses}
 
사용 중인 서비스 인스턴스에 해당하는 `Authorization` 헤더의 인증 토큰을 전달하십시오.
{: tsResolve}
       
## 인증 토큰이 만료됩니다.
{: #ts_expired_authorization_token}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
인증 토큰이 만료됩니다.
{: tsCauses}
 
`Authorization` 헤더에 만료되지 않은 인증 토큰을 전달하십시오.
{: tsResolve}
       
## 인증에 필요한 공개 키를 사용할 수 없습니다.
{: #ts_missing_public_key}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
내부 서비스 문제입니다.
{: tsCauses}
 
문제는 지원 팀에서 수정해야 합니다.
{: tsResolve}
       
## {{timeout}} 후에 오퍼레이션의 제한시간이 초과됨
{: #ts_operation_timeout}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
요청한 오퍼레이션을 수행하는 중에 제한시간이 초과되었습니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오.
{: tsResolve}
       
## {{status}}인 {{type}} 유형의 예외가 처리되지 않음
{: #ts_unhandled_exception_with_status}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
내부 서비스 문제입니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오.
이 문제가 여러 번 발생하면 지원 팀에서 수정해야 합니다.
{: tsResolve}
       
## {{response}}인 {{type}} 유형의 예외가 처리되지 않음
{: #ts_unhandled_exception_with_response}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
내부 서비스 문제입니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오.
이 문제가 여러 번 발생하면 지원 팀에서 수정해야 합니다.
{: tsResolve}
       
## {{json}}인 {{type}} 유형의 예외가 처리되지 않음
{: #ts_unhandled_exception_with_json}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
내부 서비스 문제입니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오.
이 문제가 여러 번 발생하면 지원 팀에서 수정해야 합니다.
{: tsResolve}
       
## {{message}}인 {{type}} 유형의 예외가 처리되지 않음
{: #ts_unhandled_exception_with_message}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
내부 서비스 문제입니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오. 이 문제가 여러 번 발생하면 지원 팀에서 수정해야 합니다.
{: tsResolve}
       
## 요청한 오브젝트를 찾을 수 없습니다.
{: #ts_not_found}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
요청 리소스를 찾을 수 없습니다.
{: tsCauses}
 
기존 리소스를 참조 중인지 확인하십시오.
{: tsResolve}
       
## 기본 데이터베이스에서 너무 많은 요청을 보고했습니다.
{: #ts_too_many_cloudant_requests}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
사용자가 지정된 시간 동안 너무 많은 요청을 전송했습니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오.
{: tsResolve}
       
 ## 평가 정의가 artifactModelVersion과 deployment에 모두 정의되지 않았습니다. 둘 중 적어도 하나에 \" +\n      \"가 지정되어야 합니다.
{: #ts_missing_evaluation_definition}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
학습 구성에 필요한 모든 정보가 포함되어 있지 않습니다.
{: tsCauses}
 
`learning configuration`에 `definition`을 제공하십시오.
{: tsResolve}
       
## 평가를 하려면 모델에 지정된 학습 구성이 필요합니다.
{: #ts_missing_learning_configuration}
 
`learning iteration`을 작성할 수 없습니다.
{: tsSymptoms}
 
모델에 정의된 `learning configuration`이 없습니다.
{: tsCauses}
 
`learning configuration`을 작성하고 `learning iteration`을 다시 작성해 보십시오.
{: tsResolve}
       
## 평가하려면 `X-Spark-Service-Instance` 헤더에 spark 인스턴스가 제공되어야 함
{: #ts_missing_spark_definition_for_evaluation}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
`learning configuration`에 필요한 모든 정보가 포함되어 있지 않습니다.
{: tsCauses}
 
학습 구성 또는 `X-Spark-Service-Instance`에 `spark_service`를 제공하십시오.
{: tsResolve}
       
## 모델에 버전이 포함되어 있지 않습니다.
{: #ts_missing_latest_model_version}
 
배치를 작성하거나 학습 구성을 설정할 수 없습니다.
{: tsSymptoms}
 
모델 지속성과 관련된 불일치 사항이 있습니다.
{: tsCauses}
 
모델을 다시 지속해 보고 조치를 다시 수행해 보십시오.
{: tsResolve}
       
## 패치 오퍼레이션을 통해서는 기존 학습 구성만 수정할 수 있습니다.
{: #ts_patch_non_existing_learning_configuration}
 
학습 구성을 패치하기 위해 REST API 메소드를 호출할 수 없습니다.
{: tsSymptoms}
 
이 모델에 설정된 `learning configuration`이 없거나 모델이 없습니다.
{: tsCauses}
 
모델이 있으며 이미 학습 구성이 설정되어 있는지 확인하십시오.
{: tsResolve}
       
## 패치 오퍼레이션에는 정확히 하나의 대체 오퍼레이션만 필요합니다.
{: #ts_patch_multiple_ops}
 
배치를 패치할 수 없습니다.
{: tsSymptoms}
 
패치 페이로드에 두 개 이상의 오퍼레이션이 포함되어 있거나 패치 오퍼레이션이 `replace`와 다릅니다.
{: tsCauses}
 
`replace` 오퍼레이션인 패치 페이로드에서는 한 개의 오퍼레이션만 사용하십시오.
{: tsResolve}
       
## 지정된 페이로드에서 필수 필드 FIELD이(가) 누락되었거나 필드의 값이 손상되었습니다. 
{: #ts_invalid_request_payload}
 
기본 데이터 세트에 액세스하는 데 관련된 조치를 처리할 수 없습니다.
{: tsSymptoms}
 
데이터 세트에 대한 액세스 권한이 제대로 정의되지 않았습니다.
{: tsCauses}
 
데이터 세트의 액세스 정의를 정정하십시오
{: tsResolve}
       
## 제공된 평가 메소드 METHOD이(가) 지원되지 않습니다. 지원되는 값: VALUE. 
{: #ts_evaluation_method_not_supported}
 
학습 구성을 작성할 수 없습니다.
{: tsSymptoms}
 
잘못된 평가 메소드를 사용하여 학습 구성을 작성했습니다.
{: tsCauses}
 
지원되는 평가 메소드인 `regression`, `binary`, `multiclass` 중 하나를 사용하십시오.
{: tsResolve}
       
## 모델당 활성 평가는 하나뿐이어야 합니다. 기존 활성 평가 때문에 요청을 완료할 수 없습니다. {{url}}
{: #ts_active_evaluation_conflict}
 
또 다른 학습 반복을 작성할 수 없습니다.
{: tsSymptoms}
 
모델에 대해 실행 중인 평가는 하나뿐이어야 합니다.
{: tsCauses}
 
이미 실행 중인 평가를 확인하거나 해당 평가가 종료될 때까지 기다린 다음 새 평가를 시작하십시오.
{: tsResolve}
       
## {{type}} 배치 유형이 지원되지 않습니다.
{: #ts_not_supported_deployment_type}
 
배치를 작성할 수 없습니다.
{: tsSymptoms}
 
지원되지 않은 배치 유형이 사용되었습니다.
{: tsCauses}
 
지원되는 배치 유형을 사용해야 합니다.
{: tsResolve}
       
## 잘못된 입력: ({{message}})
{: #ts_deserialization_error}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
json을 구문 분석하는 데 문제가 있습니다.
{: tsCauses}
 
요청에서 올바른 json이 전달되었는지 확인하십시오.
{: tsResolve}
       
## 부족한 데이터 - 메트릭 {{name}}을(를) 계산할 수 없음
{: #ts_missing_metric}
 
학습 반복에 실패함
{: tsSymptoms}
 
부족한 피드백 데이터 때문에 임계값이 정의된 메트릭의 값을 계산할 수 없음
{: tsCauses}
 
`learning configuration`에서 데이터 소스 `feedback_data_ref`의 데이터를 검토 및 개선
{: tsResolve}
       
## {{type}} 유형의 경우 `X-Spark-Service-Instance` 헤더에 spark 인스턴스가 제공되어야 함
{: #ts_missing_prediction_spark_definition}
 
배치를 작성할 수 없음
{: tsSymptoms}
 
`batch` 및 `streaming` 배치에서는 spark 인스턴스가 제공되어야 함
{: tsCauses}
 
`X-Spark-Service-Instance` 헤더에 spark 인스턴스를 제공하십시오.
{: tsResolve}
       
## {{action}} 조치가 실패하고 {{message}} 메시지가 표시됨
{: #ts_http_client_error}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
기본 서비스를 호출하는 중에 문제가 발생했습니다.
{: tsCauses}
 
문제 해결 방법에 대한 제안사항이 있으면 따르십시오. 메시지에 제안사항이 없거나 제안사항을 사용해도 문제가 해결되지 않으면 지원 팀에 문의하십시오.
{: tsResolve}
       
## `{{path}}` 경로는 허용되지 않습니다. 패치 스트림에 허용된 경로는 `/status`뿐입니다.
{: #ts_wrong_stream_patch_path}
 
스트림 배치를 패치할 수 없습니다.
{: tsSymptoms}
 
`stream` 배치를 패치하는 데 잘못된 경로가 사용되었습니다.
{: tsCauses}
 
지원되는 패치 옵션, 즉 `/status`를 사용하여 `stream` 배치를 패치하십시오(스트림 처리를 시작/중지할 수 있음).
{: tsResolve}
       
## `{{$type}}` 유형의 인스턴스에는 패치 오퍼레이션이 허용되지 않음
{: #ts_patch_not_supported}
 
배치를 패치할 수 없습니다.
{: tsSymptoms}
 
잘못된 배치 유형이 패치됩니다.
{: tsCauses}
 
`stream` 배치 유형을 패치하십시오.
{: tsResolve}
       
## feedback_data_ref의 데이터 연결 `{{data}}`이(가) 올바르지 않습니다.
{: #ts_invalid_feedback_data_connection}
 
모델의 `learning configuration`을 작성할 수 없습니다.
{: tsSymptoms}
 
feedback_data_ref를 정의할 때 지원되지 않는 데이터 소스가 사용되었습니다.
{: tsCauses}
 
`dashdb`인 지원되는 데이터 소스 유형만 사용하십시오.
{: tsResolve}
       
## {{path}} 경로는 허용되지 않습니다. 패치 모델에 허용된 경로는 `/deployed_version/url` 또는 `/deployed_version/href`(V2의 경우)뿐입니다.
{: #ts_patch_model_path_not_allowed}
 
모델을 패치하는 옵션이 없습니다.
{: tsSymptoms}
 
모델을 패치하는 중에 잘못된 경로가 사용되었습니다.
{: tsCauses}
 
배치된 모델의 버전을 업데이트하도록 허용하는 지원 경로로 모델을 패치하십시오.
{: tsResolve}
       
## 구문 분석 실패: {{msg}}
{: #ts_parsing_error}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
요청된 페이로드를 정상적으로 구문 분석할 수 없습니다.
{: tsCauses}
 
요청 페이로드가 올바르며 제대로 구문 분석될 수 있는지 확인하십시오.
{: tsResolve}
       
## 선택한 모델의 런타임 환경인 {{env}}은(는) `learning configuration`에 지원되지 않습니다. 지원되는 환경은 [{{supported_envs}}]입니다.
{: #ts_runtime_env_not_supported}
 
`learning configuration`을 작성할 옵션이 없음
{: tsSymptoms}
 
`learning_configuration`에서 작성하려는 모델은 지원되지 않습니다.
{: tsCauses}
 
지원되는 런타임이 있는 모델의 `learning configuration`을 작성하십시오.
{: tsResolve}
       
## 현재 플랜 \'{{plan}}\'에서는 {{limit}} 배치만 허용함
{: #ts_deployments_plan_limit_reached}
 
배치를 작성할 수 없습니다.
{: tsSymptoms}
 
현재 플랜에 대한 배치 수 제한에 도달했습니다.
{: tsCauses}
 
이 제한이 없는 플랜으로 업그레이드하십시오.
{: tsResolve}
       
## 데이터베이스 연결 정의가 올바르지 않음({{code}})
{: #ts_sql_error}
 
`learning configuration` 기능을 이용할 수 없습니다.
{: tsSymptoms}
 
데이터베이스 연결 정의가 올바르지 않습니다.
{: tsCauses}
 
기본 데이터베이스에서 리턴한 `code`를 통해 설명된 문제를 수정하십시오.
{: tsResolve}
       
## 기본 {{system}}에 연결하는 중에 문제 발생
{: #ts_stream_tcp_error}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
기본 시스템에 연결하는 중에 문제가 발생했습니다. 일시적인 네트워크 문제일 수 있습니다.
{: tsCauses}
 
원하는 오퍼레이션을 다시 호출해 보십시오. 이 문제가 여러 번 발생하면 지원 팀에 문의하십시오.
{: tsResolve}
       
## X-Spark-Service-Instance 헤더의 압축을 푸는 중에 다음 오류가 발생했습니다. ({{message}})
{: #ts_spark_header_deserialization_error}
 
Spark 신임 정보가 필요한 REST API를 호출할 수 없습니다.
{: tsSymptoms}
 
base-64 디코딩 또는 Spark 신임 정보를 구문 분석하는 데 문제가 있습니다.
{: tsCauses}
 
올바른 Spark 신임 정보가 base-64로 올바르게 인코딩되었는지 확인하십시오. 자세한 정보는 문서를 참조하십시오.
{: tsResolve}
       
## 이 기능은 베타가 아닌 사용자는 사용할 수 없습니다.
{: #ts_not_beta_user}
 
원하는 REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
호출된 REST API는 현재 베타 버전입니다.
{: tsCauses}
 
참여하는 데 관심이 있으면 자신을 대기 목록에 추가하십시오. 세부사항은 문서에 있습니다.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
REST API를 정상적으로 호출할 수 없습니다.
{: tsSymptoms}
 
기본 서비스를 호출하는 중에 문제가 발생했습니다.
{: tsCauses}
 
문제 해결 방법에 대한 제안사항이 있으면 따르십시오. 메시지에 제안사항이 없거나 제안사항을 사용해도 문제가 해결되지 않으면 지원 팀에 문의하십시오.
{: tsResolve}
       
## 비율 제한이 초과되었습니다.
{: #ts_rate_limit_exceeded}
 
비율 제한이 초과되었습니다.
{: tsSymptoms}
 
현재 플랜의 비율 한계가 초과되었습니다.
{: tsCauses}
 
이 문제점을 해결하려면 비율 한계가 더 큰 다른 플랜을 얻으십시오.
{: tsResolve}
       
## 올바르지 않은 조회 매개변수 `{{paramName}}` 값: {{value}}
{: #ts_invalid_query_parameter_value}
 
조회 매개변수의 잘못된 값을 전달했으므로 유효성 검증 오류가 발생했습니다.
{: tsSymptoms}
 
조회 결과를 가져오는 중에 오류가 발생했습니다.
{: tsCauses}
 
조회 매개변수 값을 정정하십시오. 세부사항은 문서에 있습니다.
{: tsResolve}
       
## 올바르지 않은 토큰 유형: {{type}}
{: #ts_invalid_token_type}
 
토큰 유형과 관련하여 오류가 발생했습니다.
{: tsSymptoms}
 
권한에 오류가 있습니다.
{: tsCauses}
 
토큰은 `Bearer` 접두부로 시작되어야 함
{: tsResolve}
       
## 올바르지 않은 토큰 형식입니다. Bearer 토큰 형식을 사용해야 합니다.
{: #ts_invalid_token_format}
 
토큰 형식과 관련하여 오류가 발생했습니다.
{: tsSymptoms}
 
권한에 오류가 있습니다.
{: tsCauses}
 
토큰은 bearer 토큰이어야 하며 `Bearer` 접두부로 시작해야 함
{: tsResolve}

## 입력 JSON 파일이 누락되었거나 올바르지 않음: 400
{: #os_invalid_input}

온라인으로 스코어링하려고 하면 다음 메시지가 표시됩니다. **입력 JSON 파일이 누락되었거나 올바르지 않음**.
{: tsSymptoms}

이 메시지는 스코어링 입력 페이로드가 모델을 스코어링하는 데 필요한 예상 입력 유형과 일치하지 않을 때 표시됩니다. 특히 다음과 같은 이유가 적용될 수 있습니다.

- 입력 페이로드가 비어 있습니다.
- 입력 페이로드 스키마가 올바르지 않습니다.
- 입력 데이터 유형이 예상 데이터 유형과 일치하지 않습니다.
{: tsCauses}

입력 페이로드를 정정하십시오. 페이로드에 정확한 구문, 올바른 스키마 및 적절한 데이터 유형이 있는지 확인하십시오. 정정한 다음 온라인으로 다시 스코어링해 보십시오. 구문 문제의 경우 `jsonlint` 명령을 사용하여 JSON 파일을 확인하십시오.
{: tsResolve}

## 인증 토큰이 만료됨: 401
{: #os_expired_authorization_token}

온라인으로 스코어링하려고 하면 다음 메시지가 표시됩니다. **권한 부여 실패**.
{: tsSymptoms}

스코어링에 사용된 토큰이 만료되면 이 메시지가 표시됩니다.
{: tsCauses}

이 {{site.data.keyword.pm_full}} 인스턴스의 토큰을 다시 생성한 후 재시도하십시오. 여전히 이 문제가 발생하면 IBM 지원 센터에 문의하십시오.
{: tsResolve}

## 알 수 없는 배치 ID: 404
{: #os_unkown_depid}

온라인으로 스코어링하려고 하면 다음 메시지가 표시됩니다. **알 수 없는 배치 ID입니다**. {: osSymptoms}

스코어링에 사용된 배치 ID가 없으면 이 메시지가 표시됩니다.
{: tsCauses}

올바른 배치 ID를 제공하도록 하십시오. 그렇지 않으면 배치 ID를 사용하여 모델을 배치하고 다시 스코어링을 시도하십시오.
{: tsResolve}

## 내부 서버 오류: 500
{: #os_internal_error}

온라인으로 스코어링하려고 하면 다음 메시지가 표시됩니다. **내부 서버 오류**.{: osSymptoms}

온라인 스코어링이 종속된 다운스트림 데이터 플로우에 실패하면 이 메시지가 표시됩니다.
{: tsCauses}

일정 기간 동안 기다린 다음 다시 온라인으로 스코어링해 보십시오. 또 다시 실패하면 IBM 지원 센터에 문의하십시오.
{: tsResolve}
              
