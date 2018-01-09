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

# 환경 설정

{{site.data.keyword.pm_full}}을 사용하려면 적절한 기계 학습 환경을 작성하고 해당 환경의 신임 정보를 검색할 수 있어야 합니다. {{site.data.keyword.Bluemix}} 인스턴스는 [Cloud Foundry 명령행 인터페이스](https://github.com/cloudfoundry/cli#getting-started)를 사용하거나 {{site.data.keyword.Bluemix}} 대시보드의 그래픽 사용자 인터페이스를 통해 작성할 수 있습니다.
{: shortdesc}

## IBM Cloud 대시보드 사용

{{site.data.keyword.pm_full}}을 사용하려면 {{site.data.keyword.Bluemix_notm}} 대시보드에서 [해당 인스턴스를 작성](https://console.bluemix.net/catalog/services/machine-learning)해야 합니다. 

시나리오에 따라 다음 리소스 또한 필요할 수 있습니다. 

- Object Storage(일괄처리 배치)
- Db2 Warehouse on Cloud(일괄처리 배치)
- Apache Spark(일괄처리, 스트림 배치 및 연속 학습 시스템)
- Message Hub(스트림 배치)

{{site.data.keyword.Bluemix_notm}} 인스턴스를 작성하고 신임 정보를 보기 위해 대시보드에서 작업하는 방법을 보려면 다음 비디오를 시청하고 그 뒤에 표기된 단계를 참조하십시오. 

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>그림 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="이 페이지에 임베드된 비디오에 액세스할 수 없는 경우에는 유튜브 웹 사이트에서 이 비디오에 액세스할 수 있습니다(새 탭 또는 창에서 열림). ">    <img src="images/video.png" alt="비디오 아이콘"></a>IBM Watson Machine Learning: 시작하기</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>이 비디오는 기계 학습 환경을 설정하는 방법에 대한 개요를 제공합니다. </span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## {{site.data.keyword.Bluemix_notm}} 인스턴스 작성

{{site.data.keyword.pm_full}} 인스턴스를 작성하려면 다음 단계를 수행해야 합니다. 

1. {{site.data.keyword.pm_full}} [서비스 페이지](https://console.bluemix.net/catalog/services/machine-learning)를 여십시오. 
2. 서비스 인스턴스를 작성하기 위해 가입하거나 로그인하십시오. 
3. 인스턴스를 나타내는 이름을 입력하고, 영역을 선택하고, 데이터 플랜을 선택하십시오. 
4. **인스턴스 작성**을 클릭하십시오. 

인스턴스가 열립니다. 

## 신임 정보 검색

{{site.data.keyword.Bluemix_notm}}(이전 이름은 Bluemix) 인스턴스를 사용하려면 서비스 인스턴스 신임 정보가 필요합니다. 신임 정보는 [CF CLI](using_pm_service.html) 또는 {{site.data.keyword.Bluemix_notm}} 대시보드를 통해 작성하고 액세스할 수 있습니다. {{site.data.keyword.pm_short}} 서비스 인스턴스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인드하고 나면 {{site.data.keyword.pm_short}} 신임 정보가 `VCAP_SERVICES` 환경 변수에 추가됩니다. 자세한 정보는 [서비스를 애플리케이션과 함께 사용](using_pm_service.html)을 참조하십시오. 

{{site.data.keyword.pm_full}} 인스턴스 신임 정보를 검색하려면 다음 단계를 수행해야 합니다. 

1. [{{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)에 로그인하십시오. 
2. **서비스** 섹션에서 검색할 신임 정보를 포함하는 서비스의 이름을 클릭하십시오. 
3. 탐색 분할창에서 **서비스 신임 정보**를 클릭하십시오. 
4. 신임 정보 목록에서 **신임 정보 보기**를 클릭하십시오. 
5. 이 서비스에 대한 신임 정보가 존재하지 않는 경우에는 **신임 정보 작성**을 클릭하여 이를 작성하십시오. 
6. 신임 정보를 복사하려면 복사 아이콘을 클릭하십시오. 

애플리케이션에서 {{site.data.keyword.pm_short}} 서비스를 사용하려면 서비스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인드하고 애플리케이션을 실행하는 데 필요한 신임 정보를 `VCAP_SERVICES` 환경 변수에 삽입해야 합니다. 자세한 정보는 [Cloud Foundry 명령행 인터페이스](#cloud-foundry-command-line-interface)를 참조하십시오. 

## Cloud Foundry CLI(명령행 인터페이스) 사용

### 서비스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인드하는 단계

Node.js [샘플 코드](https://github.com/pmservice/product-line-prediction/blob/master/README.md)를 다운로드하여
Machine Learning 서비스를 시도할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 애플리케이션을 작성하고 {{site.data.keyword.pm_short}} 서비스를 바인드하려면 다음 단계를 완료하십시오. 이 예제에서는 인기 있는 런타임인 Node.js를 사용합니다. iOS, Ruby, Perl 또는 Java 등의 다른 런타임도 사용할 수 있습니다. 

Cloud Foundry 도구(cf) 대신 {{site.data.keyword.Bluemix_notm}} 그래픽 인터페이스를 사용하여 1 - 3단계를 수행할 수도 있습니다. 

1. 서비스 인스턴스를 작성하려면 `cf create-service` 명령을 사용하십시오. 

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   예를 들어, 다음 명령은 사용자의 {{site.data.keyword.Bluemix_notm}} 영역에 `my_wml_lite`라는 이름의 Lite 플랜을 사용하는 하나의 {{site.data.keyword.pm_short}} 서비스 인스턴스를 작성합니다. 

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. 서비스 신임 정보를 작성하려면 `cf create-service-key` 명령을
사용하십시오. 

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   예를 들어, 다음 명령은 {{site.data.keyword.pm_short}} 서비스 신임 정보를 작성합니다. 

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. `cf bind-service` 명령을 사용하여 서비스 인스턴스 `my_wml_lite`를 애플리케이션에 바인드하십시오. 

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   예를 들어, 다음 명령은 {{site.data.keyword.pm_short}} 서비스 인스턴스 `my_wml_lite`를 {{site.data.keyword.Bluemix_notm}} 애플리케이션 `my_app1`에 바인드합니다. 

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### `VCAP_SERVICES` 변수를 통한 신임 정보 액세스

{{site.data.keyword.pm_short}} 서비스 인스턴스를 {{site.data.keyword.Bluemix}} 애플리케이션에 바인드하고 나면 {{site.data.keyword.pm_short}} 신임 정보가 `VCAP_SERVICES` 환경 변수에 추가됩니다. 

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   `VCAP_SERVICES` 환경 변수에는 다음 정보가 포함되어 있습니다.


<dl>
<dt>plan</dt>
<dd>서비스 프로비저닝에 사용되는 {{site.data.keyword.pm_short}} 플랜입니다. </dd>
<dt>url</dt><dd>{{site.data.keyword.pm_short}} 서비스 인스턴스의 주소입니다.
<dt>access_key</dt><dd>IBM® SPSS® Modeler 스트림 전용입니다. </dd>
<dt>instance_id</dt><dd>{{site.data.keyword.pm_short}} 인스턴스 고유 ID입니다. </dd>
<dt>username</dt><dd>이 서비스 인스턴스에 대한 모든 요청에서 전달하기 위한 액세스 토큰을 생성하는 데 필요한 기본 권한의 일부인 사용자 이름입니다. </dd>
<dt>비밀번호</dt><dd>이 서비스 인스턴스에 대한 모든 요청에서 전달하기 위한 액세스 토큰을 생성하는 데 필요한 기본 권한의 일부인 비밀번호입니다. </dd>
</dl>

예를 들면, 다음 `curl` 명령을 사용하여 액세스 토큰을 생성할 수 있습니다. 

요청 예제: 

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Output example:

       {"token":"**********"}
```
{: codeblock}

   다음 Node.js 코드는 `VCAP_SERVICES` 환경 변수에서 `username` 및 `password` 값을 얻는 방법에 대한 예입니다. 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
