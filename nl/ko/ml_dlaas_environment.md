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

# 명령행 인터페이스(CLI) 설치

{{site.data.keyword.pm_full}}을 사용하여 딥 러닝 실험을 수행하려면 먼저 [환경을 설정](ml_getting_access.html)한 후 훈련 실행 작성 및 모니터링을 지원하는 명령행 인터페이스(CLI)를 설치하십시오.
{: shortdesc}

## 명령행 인터페이스 설치

1.  [명령행 인터페이스](http://clis.ng.bluemix.net/ui/home.html)를 설치하십시오. 지시사항의 설명과 같이, 이를 위해서는 먼저 [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html)를 설치해야 합니다. 
2.  이 저장소의 [ML CLI 릴리스](https://github.ibm.com/NGP-TWC/wml-cli/releases) 페이지에서 특정 버전의 CLI 플러그인을 다운로드하십시오. 자신의 플랫폼에 맞는 버전을 다운로드해야 합니다. 
3. CLI 플러그인을 설치하십시오(예를 들면, OSX에서는 ml_cli_plugin_osx를 다운로드함). 

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   샘플 출력:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  CLI 플러그인이 설치되었는지 확인하십시오. 

   ```
   bx ml help
   ```
   {: codeblock}

샘플 출력:

```
NAME:
   bx ml - Manage machine learning lifecycle on Bluemix
USAGE:
   bx ml command [arguments...] [command options]
COMMANDS:
   show      Get detailed information about models/deployments/trained-models
   train     Start training a model
   delete    Delete a deployment/trained-model
   list      List all models/deployments/frameworks/trained-models
   version   show git hash and build time of cli
   deploy    Deploy a model for scoring
   score     Score the model. Sample scoring json format -  {"modelId": "sample", "deploymentId": "sample","payload": {"fields": [],"values": []}}
   help      Enter 'bx ml help [command]' for more information about a command.
```
{: codeblock}

## 환경 정보를 사용하여 CLI 구성

CLI에서 다른 명령을 사용하려면 운영 체제에 몇 가지 환경 변수를 설정해야 합니다. Linux에서 작업하는 경우에는 ML_USERNAME 및 ML_PASSWORD의 값을 사용자의 신임 정보로 바꿔 아래 명령을 실행하십시오. 필요한 값은 {{site.data.keyword.pm_full}} 인스턴스에서 [액세스 신임 정보를 검색하는 방법에 대한 지시사항](ml_getting_access.html#retrieving-your-credentials)을 따라 얻을 수 있습니다.   

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## 모델 훈련 실행 시작

이제 CLI를 사용하여 [훈련 실행을 작성하고 시작](ml_dlaas_working_with_new_models.html)할 준비가 되었습니다. 더 자세히 알아보려는 경우에는 [샘플 신경망 모델 훈련 시작](ml_dlaas_working_with_sample_models.html)을 수행할 수 있습니다. 

### CLI 업그레이드

ML CLI의 새 버전은 [ML CLI 릴리스](https://github.ibm.com/NGP-TWC/wml-cli/releases)에서 확인하십시오. 최신 기능을 사용할 수 있도록 CLI의 최신 릴리스를 다운로드하는 것이 좋습니다. 자신의 운영 체제를 위한
최신 ML CLI 버전을 다운로드한 후에는 플러그인을 설치 제거(이전 버전을 이미 설치한 경우)한 후 새 버전을 설치해야 합니다. 

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

샘플 출력:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

다음 명령을 실행하여 새 버전을 설치하십시오. 

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

샘플 출력:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
