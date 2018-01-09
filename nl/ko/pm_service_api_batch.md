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

# IBM SPSS Modeler 모델에 대한 Machine Learning 서비스 일괄처리 작업 API

{{site.data.keyword.pm_full}} 서비스용 일괄처리 작업 API는 모델 훈련, 모델 평가 및 일괄처리 스코어링과 관련된 장기 실행 태스크를 지원합니다.
이 API는 두 가지 자산 유형(일괄처리 작업에서 사용되는 IBM® SPSS® Modeler 스트림 파일 및 제출된 작업 정의)을 관리합니다.
{: shortdesc}

사용자가 업로드하는 각 SPSS® Modeler 스트림 파일에는 제출된 다양한 유형의 여러 작업이 있을 수 있습니다. 특정 작업에 Modeler 스트림 파일 컨텐츠를 업데이트할 가능성이 있는 경우, 수정된 파일은 작업 결과 첨부 파일에 포함됩니다.
모델 평가 작업 유형에서, 생성된 모든 평가 파일은 작업 결과 첨부 파일에 있습니다. 

**참고**: 업로드 기능의 데이터와 같이, 대시보드에 표시되는 데이터는 실시간 예측에만 관련됩니다. 

## 작업 삭제

작업은 삭제할 수 있습니다. 특정 작업이 실행 중인 경우, 해당 작업을 삭제하면 이 작업이 먼저 취소됩니다. `job ID`와 함께 `DELETE` 명령을 사용하십시오. 여러 ID를 전달하여 한 번에 두 개 이상의 작업을 취소할 수 있습니다.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

리턴 결과는 요청의 ID로 참조된 작업이 삭제된 개수를 표시합니다. 이 개수가 요청에 전달한 목록과 일치하지 않는 경우에는 개별 작업의 상태를 검사해야 합니다. 

## 작업 상태 확인

`GET` 명령을 사용하여 언제든지 `job ID`의 상태를 가져올 수 있습니다.


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

리턴된 JSON 파일은 작업 상태를 표시하며, 해당 작업이 완료된 경우에는 모든 생성된 파일 컨텐츠를 얻는 데 사용할 수 있는 데이터 URL도 표시합니다. 

## 작업 다시 제출

작업을 다시 제출하려면 `job ID`와 함께 `PUT` 명령을 사용하십시오. 작업을 다시 제출하려면 해당 작업이 실행 중이 아니어야 합니다. 

참조한 ID가 없는 경우에는 다음 호출이 새 작업을 작성합니다. 리턴 상태는 201 또는 200(둘 중 발생한 쪽을 표시함)입니다. 사용자는 이 실행에서 사용되는 작업 정의 JSON 파일을 전달해야 합니다. 

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

`content_type` v값을 "application/json"으로 설정하여 작업을 다시 제출하십시오. 요청의 본문에 신규 또는 업데이트된 작업 정의 JSON 파일을 포함시켜야 합니다. 

## 업로드한 Modeler 스트림 파일에 대해 작업 제출

작업을 실행 큐에 두려면 다음과 같이 `PUT` 명령을 사용하십시오.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

`content_type` v값을 "application/json"으로 설정하여 작업을 제출하십시오. 요청의 본문에 신규 또는 업데이트된 작업 정의 JSON 파일을 포함시켜야 합니다. 

이 요청의 결과는 즉시 리턴되며, 작업 정의가 실행 큐에 삽입되면 성공을 표시합니다. 
작업 정의에 구성된 대로 Modeler 스트림을 성공적으로 실행하는 기능의 테스트는 없습니다. 작업은 클라우드의
{{site.data.keyword.pm_short}} 작업 서버 중 하나에 의해 실행되며, 사용자는 해당 상태를 모니터할 수 있습니다.
모델 훈련을 수행 중이며 자동으로 새로 고치기를 수행하도록 표시하는 경우 이 작업은 작업 실행이 성공하면 원래 Modeler 스트림 파일을 대체합니다. 

## 작업에서 사용할 스트림 파일 업로드

**참고**: {{site.data.keyword.pm_short}} 대시보드는 실시간 스코어링 전용입니다. 
작업 실행(일괄처리 스코어링)에는 이를 사용할 수 없습니다. 

작업에서 Modeler 스트림 파일에 액세스할 수 있게 하려면 다음과 같이 `PUT` 명령을 사용하십시오.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

content_type "multipart/form-data"(요청에서 파일을 전달함). 

사용된 고유 이름(`PUT` 호출의 `file ID`)은
작업 정의의 모델 참조는 물론 파일 API에 대한 `DELETE` 호출에서
참조됩니다. 작업에서 사용된 파일의 네임스페이스는
완벽하게 제어할 수 있습니다. `file ID`가
동일한 파일의 `PUT` 명령은 현재 사본을
암시적으로 대체합니다.

작업에 업로드된 모든 파일 목록을 생성하려면 `GET` 명령을 사용하십시오. 단, `file ID` 매개변수는 생략하십시오. 특정 파일을 검색하려면 `file ID` 매개변수와 함께 `GET` 명령을 사용하십시오. `DELETE` 명령을 실행하여 업로드된 파일을 삭제할 수도 있습니다. 그러면 파일을 참조하는 보류 중인 작업 실행 시 오류가 발생합니다.

요청 예제: 

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

## 작업 유형

**Training**: 이 작업 유형은 모든 "모델 빌더" 터미널 노드가
Modeler 스트림에서 실행되어야 함을 표시합니다. 
성공적인 작업 완료 이후, 새 훈련 모델 너깃의 업데이트된 Modeler 스트림은 검색 가능한 작업 결과에 있습니다. 
Modeler 스트림 파일에 모델 빌드 노드에서 평가 및 스코어링 분기의 훈련 모델 너깃으로
링크가 있는 경우, 이에 따라 해당 노드의 새로 고치기가 발생합니다. 

**Evaluation**: 이 작업 유형은 다시 호출자에게
전달될 수 있는 정적 보고서 파일 컨텐츠를 생성하는 모든 "문서 빌더"
터미널 노드의 실행을 트리거합니다(주로 Modeler 클라이언트의 그래프 및
출력 탭에서).
스코어링 분기는 이 작업 유형의 일부로서 간주되지 않습니다. 

**Auto-Refresh**: `TRAINING` 작업 유형의 한 버전입니다.
여기서는 작업이 성공적으로 완료되면 일괄처리 파일 목록의
원래 Modeler 스트림 파일이 업데이트됩니다.
실시간 스코어링을 위해 배치된 Modeler 스트림의 새로 고치기 이벤트와 관련된 평가 및
명시적 의사결정을 담당하며, 이 때는 자동으로 새로 고치기에서 커버되지 않습니다. 

**Batch Score**: Use as Scoring Branch 옵션이
적용된 터미널 노드의 실행이며, 이 Modeler 스트림 디자인의
스코어링 분기임을 표시합니다. 작업 정의는 소스 세부사항은 물론 내보내기를 지정해야 합니다. 

**Run Stream**: 실행은 스트림 특성의 실행 탭에서 Run this script 옵션을 선택한 상태로 Modeler의 초록색 "실행" 아이콘을 클릭하는 것과 유사합니다. 
사용법에서는 모델 훈련 또는 기타 작업 유형의 스크립트된 실행에 대한 필요성을 커버합니다. 
스크립트의 모든 동적 제어는 작업정의에서 매개변수값을 전달하여 스트림 매개변수로 처리해야 합니다. 

## 작업 상태

**Pending**: 작업 정의가 제출되었지만 작업 서버가 아직 실행을
요청하지 않았습니다. 

**Running**: 작업 서버가 작업 정의를 요청했으며
이를 실행 중입니다. 

**Canceling**: 작업을 취소 중입니다. 

**Canceled**: 작업이 취소되었습니다. 

**Failed**: 작업에 실패했습니다. 실패의 원인에 대한 세부사항은 작업 상태의 GET에서 리턴됩니다. 

**Success**: 작업 실행에 성공했습니다. 이 이벤트에 대한 통신 결과는 작업 상태의 GET의 JSON에서 리턴됩니다. 

## 작업 API 세부사항

`POST /v1/jobs/{id}`

설명: 실행할 작업 정의를 제출합니다. `job ID`가 이미 존재하면 오류가 발생합니다. 

컨텐츠 유형:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

사용자 지정 `job ID`. {{site.data.keyword.pm_short}} 서비스 인스턴스에 고유해야 합니다. 

```
@PathParam("id")
```
{: codeblock}

JSON 작업 정의(작업 정의 JSON 참조):

```
@BodyParam
```
{: codeblock}

응답:

성공. 실행할 작업이 제출되며, 제출된 작업을 설명하는 JSON을 리턴합니다. 

```
@ApiResponse(code = 200)
```
{: codeblock}

작업이 이미 존재함 오류. 이 ID의 작업이 이미 제출되었습니다. 리턴된 메시지에서 이 오류를 설명합니다. 

```
@ApiResponse(code = 409)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

설명: 작업을 작성하거나 업데이트합니다. 
이 ID의 작업이 존재하지 않으면 이를 작성합니다. 그렇지 않으면, 이를 업데이트합니다(실제로 이는 실행할 작업을 다시 제출함). 

컨텐츠 유형:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

사용자 지정 `job ID`:

```
@PathParam("id")
```
{: codeblock}

JSON 작업 정의(작업 정의 JSON 참조):

```
@BodyParam
```
{: codeblock}

응답:

성공. 실행할 작업이 업데이트되고 제출됩니다. 제출된 작업을 설명하는 JSON을 리턴합니다. 

```
@ApiResponse(code = 200)
```
{: codeblock}

성공. 실행할 작업이 작성되고 제출됩니다. 제출된 작업을 설명하는 JSON을 리턴합니다. 

```
@ApiResponse(code = 201)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON입니다. 

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

설명: 이 Machine Learning 서비스 인스턴스에서 정의된 모든 작업의 목록을 리턴합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

응답:

성공. 모든 작업 정의의 JSON 배열을 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

설명: 특정 작업 정의의 리턴을 요청합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

제출된 `job ID`:

```
@PathParam("id")
```
{: codeblock}

응답:

성공. 참조된 작업을 설명하는 JSON을 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

작업을 찾을 수 없음 오류. 리턴된 메시지의 세부사항:

```
@ApiResponse(code = 404)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

설명: 특정 작업의 상태를 검색합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

제출된 `job ID`:

```
@PathParam("id")
```
{: codeblock}

응답:

성공. 작업 상태를 설명하는 JSON을 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

작업을 찾을 수 없음 오류. 리턴된 메시지의 세부사항:

```
@ApiResponse(code = 404)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

설명: 하나 이상의 작업을 삭제합니다. 작업이 현재 실행 중이면 이를 취소합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

제출된 `job ID` 또는 ID 값이 쉼표(,)로 분리된 `job ID` 목록

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

응답:

성공. 삭제된 작업의 수를 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

## 작업 정의 JSON

작업 정의 JSON에는 다음의 일반 섹션이 포함되어 있습니다. 

작업 유형 및 예측 모델 참조

**참고**: 업로드 기능의 데이터를 포함하여 대시보드에 표시되는 데이터는 실시간 예측에만 관련됩니다.

### 조치 유형

**TRAINING** – 모델 빌더 노드 훈련 또는 새로 고치기 모델 너깃을 실행합니다. 업데이트된 Modeler 스트림은 작업 결과에서 검색됩니다. 

**EVALUATION** – 훈련 모델을 평가 중인 문서 빌더 및 분석 노드를 실행합니다. 

**AUTO_REFRESH** – TRAINING을 수행하고 일괄처리 파일 업로드 영역의
   파일 컨텐츠를 업데이트합니다. 

**BATCH_SCORE** – 스코어링 분기를 실행하고 작업 정의에서 지시한 대로 결과 데이터를 내보냅니다. 

**RUN_STREAM** – 스트림 특성에서 표시된 대로 Modeler 스트림을 실행합니다. 주로 스크립트된 실행이 필요할 때 사용됩니다. 

모델: 일괄처리 작업 참조에 대한 파일 업로드 조치에 지정된 ID입니다. 자세한 정보는 작업에서 사용할 스트림 파일 업로드를 참조하십시오. 참고로, `/pm/v1/file`이 사용됩니다. 

```
"action": "TRAINING",
"model": {
     "id": "str2"
     "name":"stream1.str"
},
```
{: codeblock}

ID는 `PUT` API에서 사용된 `file ID`와
동일해야 합니다. name은 필수는 아니지만, 모델 훈련 및 자동으로 새로 고치기의 경우
작업 결과는 여기서 정의된 이름을 사용하여 저장됩니다. name이 정의되지 않은 경우,
{{site.data.keyword.pm_short}} 서비스는 사전 정의된 이름 지정 규칙에 따라 결과를 생성합니다. 

### 작업 설정

이 작업을 실행하는 데 필요한 모든 설정입니다. 

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### 데이터베이스 연결 정의

데이터베이스 유형: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL, ProgressOpenEdge, Salesforce,  SQLServer, Sybase, SybaseIQ, Teradata.

DB 서비스 인스턴스에 지정된 연결이며, 다수의 DB 유형은 '옵션'에서 전달되는 특정 설정을 요구합니다. 

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### 소스 노드 설정

소스 노드 이름으로 식별된 대로 Modeler 스트림에서 제공된 분기를 소싱하는 데 사용되는 DB 연결 및 테이블을 참조합니다. 

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

table은 데이터베이스 테이블 이름입니다. 이 테이블은 스트림 소스 노드를 대체하는 데 사용됩니다. node는 스트림의 데이터 소스 이름입니다. dbRef는 dbDefinitions 오브젝트에 의해 정의된 DB 연결을 참조하며,
table은 Modeler 스트림에서 제공된 분기의 입력으로 사용되는 데이터베이스 테이블입니다. node는 제공된 해당 매개변수로 생성된 DB 소스 노드로 대체될 Modeler 스트림의 원래 소스 노드를 식별합니다. 

### 내보내기 노드 설정

터미널 노드 이름으로 식별된 대로 Modeler 스트림에서 제공된 분기의 데이터를 지속하는 데 사용되는 DB 연결 및 테이블을 참조합니다. 

지속적 데이터가 insertMode 속성을 통해 통신할 때 사용되는 제어 방법:

**Append** – 테이블이 존재해야 하며 삽입에 대해 호환 가능해야 함

**Create** – 테이블이 작성됨(이미 존재하면 오류가 리턴됨)

**Drop** – 참조된 테이블이 존재하면 이를 삭제하고 재작성함

**Refresh** – 새 행을 삽입하기 전에 테이블의 기존 행이 삭제됨
   
table은 작업 결과가 쓰여지는 데이터베이스 테이블 이름입니다. node는 스트림의 터미널 노드 이름입니다. 소스 노드 설정과 유사하게 node는
제공된 해당 매개변수로 생성된 DB 내보내기 노드로 대체될 Modeler 스트림의 원래 출력 노드를 식별합니다. 

**참고**: 내보내기 노드 설정 및 소스 노드 설정은
성공적인 작업 실행에 매우 중요합니다. 

### 매개변수값 대체

작업 실행 전에 스트림 레벨 매개변수의 기본값을 대체하기 위해 작업 정의에서 사용할 이름/값 쌍을 지정할 수 있습니다. 

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

평가 작업 유형을 실행할 때 리턴된 평가 문서의 원하는 보고서 형식

평가 작업 실행에서 생성된 모든 문서는 하나의 .zip 파일로 번들화됩니다. 표시된 reportFormat을 지원할 수 있는 문서 빌더 노드만 실행되며 해당 컨텐츠는 작업이 성공적으로 실행된 이후에 리턴됩니다. 

지원되는 형식은 HTML, JPG, PNG, RTF, SAV, TAB 및 XML입니다.

```
"reportFormat": "HTML"
```
{: codeblock}

### 전체 JSON 예제

```
{ 
     "action": "TRAINING",
     "model": {
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
               "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## 일괄처리 작업 API 세부사항

다음 섹션은 일괄처리 작업 IBM® SPSS® Modeler 파일 관리 API 세부사항을 제공합니다. 

`PUT /v1/file/{id}`

설명: 일괄처리 작업에서 사용할 IBM® SPSS® Modeler 스트림 파일을 업로드합니다. 

**참고**: 지정된 ID로 저장된 파일이 이미 있으면
이 파일을 대체합니다. 

### 컨텐츠 유형

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### 매개변수

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

사용자 지정 `file ID`. 서비스 인스턴스 저장소에서 고유해야 합니다.

```
@PathParam("id")
```
{: codeblock}

### 응답

성공. 서비스 인스턴스의 저장소로부터 파일을 다운로드하는 데 사용될 수 있는 URL을 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

설명: 서비스 인스턴스 저장소에서 지정된 ID 아래 저장된 파일을 삭제합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

업로드 시에 파일에 대한 사용자 지정 ID:

```
@PathParam("id")
```
{: codeblock}

응답:

성공. 지정된 파일이 삭제됨:

```
@ApiResponse(code = 200)
```
{: codeblock}

오류. 이 ID로 저장된 파일이 서비스 인스턴스 저장소에 존재하지 않습니다.

```
@ApiResponse(code = 404)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

설명: 일괄처리 작업 처리를 위해 서비스 인스턴스 저장소에서 관리 중인 모든 파일의 ID 목록을 리턴합니다. 

컨텐츠 유형:

```
@Produces({“application/json”})
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

응답:

성공. `file ID` 값의 JSON 배열을 리턴합니다.

```
@ApiResponse(code = 200)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

설명: 지정된 ID로 처리 중인 일괄처리 작업에서 사용하기 위해 저장된 IBM® SPSS® Modeler 스트림 파일을 검색합니다. 

컨텐츠 유형:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

매개변수:

프로비저닝 또는 바인드에서 신임 정보로서 리턴된 액세스 키:

```
@QueryParam("accesskey")
```
{: codeblock}

업로드 시에 파일에 대한 사용자 지정 ID:

```
@PathParam("id")
```
{: codeblock}

응답:

성공. IBM® SPSS® Modeler 파일을 리턴합니다. 

```
@ApiResponse(code = 200)
```
{: codeblock}

오류. 이 ID로 저장된 파일이 서비스 인스턴스 저장소에 존재하지 않습니다.

```
@ApiResponse(code = 404)
```
{: codeblock}

기타 오류. 리턴된 예외의 JSON:

```
@ApiResponse(code = 500)
```
{: codeblock}

## 자세히 보기

노트북의 일괄처리 작업 선정에 대한 예는 [From SPSS stream to batch scoring with Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37)을 참조하십시오. 

작업 정의 JSON에 대한 자세한 정보는 [작업 정의 JSON](#job-definition-json)의 내용을 참조하십시오. 
