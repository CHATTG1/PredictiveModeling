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

# 서비스 사용

IBM® SPSS® Modeler 모델링 팔레트의 모델링 방법을 사용하여, 데이터에서 새 정보를 이끌어내고 예측 모델을 개발할 수 있습니다.
각 방법에는 특유의 장점이 있으며 이들은 각각 특정 유형의 기계 학습 문제점에 가장 적합합니다.
{: .shortdesc}

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 세부사항은 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

데이터 분석가는 {{site.data.keyword.Bluemix_notm}} 애플리케이션 및 IBM® SPSS® Modeler 스코어링 분기 디자인의 입력 및
출력 요구사항이 구현된 후 스코어링 분기의 내부 요소를 변경할 수 있습니다. 데이터 분석가는 새로 고치기 오퍼레이션에서 사용되는 모델 알고리즘을
변경할 수도 있으며 애플리케이션을 다시 작성하지 않고 예측 분석을 정밀 조정할 수 있습니다.



## 서비스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인드하는 단계

{{site.data.keyword.Bluemix_notm}} 애플리케이션을 작성하고 이를 {{site.data.keyword.pm_short}} 서비스에 바인드하려면 다음 단계를 완료하십시오. 

1. [GitHub 저장소](https://github.com/pmservice/customer-satisfaction-prediction)에서 Node.js 샘플 애플리케이션 코드를 다운로드하십시오. 

2. 서비스 인스턴스를 작성하려면 `cf create-service` 명령을 사용하십시오. 

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   예: 

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   이 명령은 사용자의 {{site.data.keyword.Bluemix_notm}} 영역에 my_pm_lite라는 이름의 Lite 플랜을 사용하는 하나의 {{site.data.keyword.pm_short}} 서비스 인스턴스를 작성합니다. 

3. 서비스 신임 정보를 작성하려면 `cf create-service-key` 명령을
사용하십시오. 

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   예: 

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   이 명령은 {{site.data.keyword.pm_short}} 서비스 신임 정보를 작성합니다. 

4. `cf bind-service` 명령을 사용하여 서비스 인스턴스 `my_pm_lite`를 애플리케이션에 바인드하십시오. 

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   예: 

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   이 명령은 {{site.data.keyword.pm_short}} 서비스 인스턴스 `my_pm_lite`를 {{site.data.keyword.Bluemix_notm}} 애플리케이션 my_app1에 바인드합니다. 

5. {{site.data.keyword.pm_short}} 신임 정보:

   {{site.data.keyword.pm_short}} 서비스 인스턴스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인드하고 나면 {{site.data.keyword.pm_short}} 신임 정보가 `VCAP_SERVICES` 환경 변수에 추가됩니다. 

```
    {   
        "pm-20": {
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   `VCAP_SERVICES` 환경 변수에는 다음 정보가 포함되어 있습니다.


   <dl>

   <dt>plan</dt>
   <dd>서비스 프로비저닝에 사용되는 {{site.data.keyword.pm_short}} 플랜입니다.
</dd>

   <dt>url</dt>
   <dd>{{site.data.keyword.pm_short}} 서비스 인스턴스의 주소입니다. </dd>

   <dt>access_key</dt>
   <dd>모든 요청에서 이 서비스 인스턴스로 전달하는 조회 매개변수 accessKey입니다.
</dd>

   </dl>

예:              

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   `VCAP_SERVICES` 환경 변수에서 accessKey를 얻는 방법을 보여주는 예제 Node.js 코드입니다. 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
