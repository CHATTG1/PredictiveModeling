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

# 예측 분석 애플리케이션 빌드

{{site.data.keyword.pm_full}} 서비스를 사용하여 Node.js 애플리케이션을 배치할 수 있습니다.
{: shortdesc}

**시나리오 이름**: 제품 라인 예측

**시나리오 설명**: 고객은 유럽에서 가장 유명한 체인점 중 하나를
운영하고 있습니다. 개인 용품, 캠핑 장비 및 아웃도어 보호 장비와 같은 제품 라인에 관해서 고객의 관심 분야를 확인할 수 있도록
사용자에게 요청합니다. 데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
배치된 모델에 대해 스코어 요청을 작성하여 스포츠 제품 라인을 추천하는 Node.js 애플리케이션을 준비하고 배치하는 것입니다. 

1. {{site.data.keyword.Bluemix_short}}에 로그온하여 {{site.data.keyword.pm_full}}의 인스턴스를 작성하십시오. 
2. {{site.data.keyword.pm_full}} 대시보드를 실행하십시오. 
3. **샘플** 탭으로 이동하십시오. 
4. **샘플 모델** 섹션에서 **제품 라인 예측** 타일을 찾아 **모델 추가** 아이콘(+)을 클릭하십시오. 샘플 제품 라인 예측 모델이 **모델** 탭의 사용 가능한 모델 목록에 표시됩니다. 

   **참고**: 샘플 대신에 사용자 고유의 Data Science Experience 프로젝트 및 모델을 사용하려는 경우, {{site.data.keyword.pm_short}} 저장소에
사용자 정의 모델을 유지하고 있어야 합니다. REST API 또는 클라이언트 라이브러리를 사용하여 이를 수행할 수 있습니다.
세부사항은 사용자 정의 모델의 개발 및 지속성을 참조하십시오. 

5. **조치** 메뉴에서 **배치 작성**을 클릭하여 제품 라인 예측 모델을 온라인 배치로 배치하십시오. 
6. 배치 이름을 지정하고(예: 제품 라인 예측) 저장을 클릭하십시오. 이제 배치 목록에 제품 라인 예측 배치가 표시됩니다. 
7. 조치 메뉴에서 배치에 대한 세부사항 보기를 선택하십시오.
세부사항 분할창에 표시된 스코어링 엔드포인트가 스코어링 요청을 실행하는 데 사용됩니다. 
8. REST API를 통해 스코어링 요청을 실행하려면, 스코어링 엔드포인트 및 인증 토큰이 필요합니다.
자세한 정보는 온라인 모델 배치를 참조하십시오. 
9. 다음 위치에 있는 샘플 Node.js 애플리케이션을 사용하여 다양한 실험을 수행하십시오. 
   https://github.com/pmservice/product-line-prediction.

   샘플 애플리케이션 코드를 {{site.data.keyword.Bluemix_short}} 영역에 자동으로 배치하려면,
샘플 탭으로 이동하여 샘플 애플리케이션 섹션에서 제품 라인 예측 애플리케이션을 선택하고 (+) 아이콘을 클릭하여 이를 배치하십시오.
DeployToBluemix 양식에서 프롬프트가 표시되는 경우 인증하십시오. 

   이제 모든 앱 섹션의 {{site.data.keyword.Bluemix_short}} 대시보드에 샘플 애플리케이션이 표시됩니다. 

10. 세부사항을 보려는 애플리케이션을 클릭하십시오. 여기에서 인스턴스 수 또는 메모리 할당 등의 애플리케이션 세부사항을 수정할 수 있습니다. 
11. 애플리케이션을 {{site.data.keyword.pm_short}} 인스턴스에 바인드하십시오. **연결** 탭에서 **기존 항목 연결**을 클릭하십시오. 다시 스테이징된 후
애플리케이션이 서비스 인스턴스에 연결됩니다. 
12. 라우트 주소를 사용하여 애플리케이션을 실행하십시오. 
13. 해당 애플리케이션의 제품 라인 예측 배치를 선택하십시오. 이제 샘플 레코드 및 예측 결과를 사용할 준비가 되었습니다. 
    
## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
