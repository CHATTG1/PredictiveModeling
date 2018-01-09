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

# 딥 러닝을 위한 오브젝트 저장소 설정

{{site.data.keyword.pm_full}} 서비스와 이 서비스의 일부인 딥 러닝 실험에 대한 작업을 수행하려면 Cloud Object Storage(control.softlayer.com) 또는 Object Storage OpenStack Swift(console.bluemix.net) 인스턴스에 액세스할 수 있어야 합니다. 사용자는 훈련 데이터를 제공하고 훈련된 모델 및 로그를 저장하기 위한 Cloud Object Storage 버킷을 정의해야 하며, 이를 위해 동일한 또는 별도의 Cloud Object Storage 인스턴스를 사용할 수 있습니다.
{: shortdesc}

## Cloud Object Storage 인스턴스 작성

1. [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) 페이지로 이동하여 다음 옵션 중 하나를 선택하십시오. 

   - 비암호화/무료(Lite) 스토리지: [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage)(console.bluemix.net)
   - 암호화/유료 서비스: [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage)(control.softlayer.com)
   
2. Object Storage 인스턴스를 구성하고 **작성**을 클릭하십시오. 
3. 서비스가 작성되고 나면 사이드 메뉴에서 **서비스 신임 정보**를 클릭하여 Object Storage 인스턴스에 액세스하는 데 필요한 API URL, 사용자 이름 및 비밀번호와 같은 신임 정보를 얻으십시오. 

## Cloud Object Storage(IaaS) 인스턴스에 액세스

Cloud Object Storage(IaaS) 서비스는 S3 호환 가능 API를 제공하며 호환되는 클라이언트 애플리케이션을 사용하여 액세스할 수 있습니다. 

## Amazon Web Services(AWS) 명령행 인터페이스(CLI)

1. `pip install awscli`를 사용하여 [AWS CLI](https://aws.amazon.com/cli/)를 설치하십시오. 
2. AWS 신임 정보 파일(Linux의 경우 ~/.aws/credentials에 있음)에 Cloud Object Storage(IaaS)의 신임 정보를 구성하십시오. 

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

위 프로파일 및 신임 정보의 Cloud Object Storage 엔드포인트를 지정해, AWS CLI를 사용하여 버킷 및 파일을 나열하고 업데이트할 수 있습니다. 예를 들어, 인스턴스의 버킷을 나열하려면 다음 명령을 실행하십시오. 

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Bluemix용 Object Storage OpenStack Swift 인스턴스에 액세스

Swift CLI 또는 클라이언트 라이브러리(예: 이 오브젝트 저장소에 액세스하기 위한 Swift Python 클라이언트 라이브러리)를 사용할 수 있습니다. 세부사항은 [이 문서](https://console.bluemix.net/docs/services/ObjectStorage/index.html)를 참조하십시오. 

## 데이터 형식화

서로 다른 프레임워크는 서로 다른 형식의 훈련 및 테스트 데이터 세트를 필요로 합니다. 예를 들어, Caffe는 LevelDB 또는 LMDB 형식의 데이터 세트를 필요로 하는 반면 Torch는 Torch 전용 형식의 데이터 세트를 필요로 합니다. IBM에서는 데이터가 이미 특정 프레임워크에서 필요로 하는 형식으로 되어 있다고 가정합니다. 원시 데이터를 특정 프레임워크에서 필요로 하는 형식으로 변환하는 방법에 대한 세부사항은 이 문서의 범위를 벗어납니다. 추가 정보가 필요한 경우에는 딥 러닝 프레임워크에 대한 문서를 참조하십시오. 
