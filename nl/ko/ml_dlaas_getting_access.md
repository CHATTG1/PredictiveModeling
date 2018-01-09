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

# 기계 학습 환경 설정 및 신임 정보 검색

IBM Watson Machine Learning을 사용하려면 적절한 기계 학습 환경을 작성하고 해당 환경의 신임 정보를 검색할 수 있어야 합니다. 

## 환경 설정

IBM Watson Machine Learning을 사용하려면 IBM Cloud 대시보드에서 다음 항목의 인스턴스를 설정해야 합니다. 

- Watson Machine Learning
- Object Storage
- Apache Spark

일부 노트북 및 튜토리얼은 필수 인스턴스 외에 다음 인스턴스 또한 사용합니다. 튜토리얼을 사용하려면 이러한 서비스 또한 설정해야 합니다. 

- Python Flask
- Natural Language Understanding
- 딥 러닝 실험

### IBM Cloud 인스턴스 작성

IBM Cloud 인스턴스를 작성하고 신임 정보를 보는 방법을 보려면 이 비디오를 보십시오. 그 후, 비디오의 내용에 따라 단계를 수행하여 자신의 환경을 설정하십시오. 신임 정보를 보는 방법의 단계는 <a href="#retrieving-your-credentials">신임 정보 검색</a> 절을 참조하십시오. 

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>그림 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="이 페이지에 임베드된 비디오에 액세스할 수 없는 경우에는 유튜브 웹 사이트에서 이 비디오에 액세스할 수 있습니다(새 탭 또는 창에서 열림). ">    <img src="images/video.png" alt="비디오 아이콘"></a>IBM Watson Machine Learning: 시작하기</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>이 비디오는 기계 학습 환경을 설정하는 방법에 대한 개요를 제공합니다. </span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Watson Data Platform 인스턴스를 작성하려면 다음 단계를 수행해야 합니다. 

1. [IBM Cloud에 로그인하십시오](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter). 
2. 탐색 패널에서 **데이터 & 분석**을 클릭하십시오. 

   데이터 서비스를 중심으로 화면이 표시됩니다. 사용자는 주기적으로 이 화면으로 돌아와 사용하기 쉬운 하나의 페이지에서 데이터 및 분석 서비스에 대해 작업할 수 있습니다. 

3. **분석** 탭을 클릭한 후 **인스턴스**를 클릭하십시오. 
4. **새 인스턴스**를 클릭하십시오. 
5. 인스턴스 및 데이터 스토리지를 구성하십시오. 

   인스턴스를 나타내는 이름을 입력하고, 영역을 선택하고, 데이터 플랜(플랜 비교 및 가격 책정 세부사항은 이 페이지에 있음)을 선택하십시오. IBM Cloud는 자동으로 인스턴스 이름을 오브젝트 스토리지 이름으로 채웁니다. Spark 프로젝트의 데이터 스토리지도 필요하므로, 이 또한 여기서 영역 및 플랜을 선택하십시오. 

6. **인스턴스 작성**을 클릭하십시오. 

인스턴스가 열립니다. 

### Data Science Experience의 프로젝트에 기존 Bluemix 인스턴스 추가

프로젝트를 작성하고 Watson Machine Learning을 사용하도록 설정하는 방법을 보려면 이 비디오를 보십시오. 

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>그림 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="이 페이지에 임베드된 비디오에 액세스할 수 없는 경우에는 유튜브 웹 사이트에서 이 비디오에 액세스할 수 있습니다(새 탭 또는 창에서 열림). ">    <img src="images/video.png" alt="비디오 아이콘"></a>IBM Watson Machine Learning: Watson Machine Learning의 프로젝트 작성</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>이 비디오는 기계 학습 환경을 설정하는 방법에 대한 개요를 제공합니다. </span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

인스턴스가 이미 있으나 이를 Data Science Experience의 프로젝트에 링크하지 않은 경우에는 다음 단계를 수행해야 합니다. 

1. [IBM Data Science Experience](https://datascience.ibm.com)에 로그온하십시오. 
2. **프로젝트** > **모든 프로젝트 보기**를 클릭하십시오. 
3. **설정** 탭을 클릭하십시오. 
4. 서비스를 추가하려면 **연관된 서비스** 패널에서 **연관된 서비스 추가**를 클릭하고 서비스를 선택한 후, 구성 정보를 완료한 다음 **저장**을 클릭하십시오. 
5. 액세스 토큰을 추가하려면 다음 단계를 수행하십시오. 
   6. **액세스 토큰** 패널에서 **새 토큰 작성**을 클릭하십시오. 
   7. **이름** 상자에 이름을 입력하십시오. 
   8. **프로젝트에 대한 액세스 역할** 상자에서 **열람자** 또는 **편집자**를 선택하십시오. 
   9. **추가**를 클릭하십시오. 

6. GitHub 저장소에 연결하려면 **저장소 URL**에 저장소 위치를 입력하십시오. GitHub 액세스를 위해 이미 설정되어 있는 개인 액세스 토큰이 없는 경우에는 **설정**을 클릭하여 지금 이를 작성해야 합니다. 완료한 후에는 **추가**를 클릭하십시오. 


## 신임 정보 검색

노트북 및 애플리케이션에서 Bluemix 인스턴스, 서비스 및 모델을 사용하려면 처리에 사용되는 코드에 자신의 신임 정보, 토큰 및 스코어링 엔드포인트를 삽입할 수 있어야 합니다. 

1. [Bluemix에 로그인하십시오](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter). 
2. **서비스** 섹션으로 스크롤하여 검색할 신임 정보를 포함하는 서비스의 이름을 클릭하십시오. 
3. 탐색 분할창에서 **서비스 신임 정보**를 클릭하십시오. 
4. 신임 정보 목록에서 **신임 정보 보기**를 클릭하십시오. 
5. 이 서비스에 대한 신임 정보가 존재하지 않는 경우에는 **신임 정보 작성**을 클릭하여 이를 작성할 수 있습니다. 
6. 신임 정보를 복사하려면 복사 아이콘을 클릭하십시오. 

서비스에 따라, `username` 및 `password` 등의 다른 필드가 있을 수 있습니다. 이들을 코드에서 사용하려면 값을 표시되어 있는 대로 복사해야 합니다. 

## 자세히 보기

[딥 러닝에 대한 빠른 튜토리얼](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

