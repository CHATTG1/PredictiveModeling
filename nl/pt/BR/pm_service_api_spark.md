---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de REST

O serviço {{site.data.keyword.pm_short}} é um conjunto de APIs de REST que você pode chamar de qualquer linguagem de programação e que permitem a integração da análise do Data Science Experience em seus aplicativos. Ligue seus aplicativos {{site.data.keyword.Bluemix_short}} à instância de serviço do {{site.data.keyword.pm_short}} e gere a análise preditiva que seus aplicativos precisam para entregar valor mais alto aos seus usuários. Gerencie modelos no [painel de
administração](pm_service_ui_spark.html) e use o painel para criar implementações on-line, em lote ou de fluxo integradas com seus
aplicativos.
{: shortdesc}

Desenvolva aplicativos nos modelos Spark, Python e IBM® SPSS® que são
implementados em uma instância de serviço por meio das poderosas
[APIs de REST](https://watson-ml-api.mybluemix.net/):

*  Recupere os metadados para um determinado modelo preditivo
*  Implemente modelos e gerencie modelos implementados
    *  Implementação on-line (pontuação)
    *  Implementação em lote (suporte ao IBM Cloud Object Storage e {{site.data.keyword.dashdbshort}})
    *  Implementação de fluxo (suporte a IBM Cloud Message Hub)
*  Recupere os metadados para uma determinada implementação
*  Monitore e treine novamente os modelos implementados usando o sistema de aprendizado contínuo
*  Gere análise preditiva fazendo solicitações de pontuação em
modelos implementados

Para obter mais informações, consulte os seguintes exemplos de uso da API de REST:

*  [Implementando modelos on-line](pm_service_api_spark_online.html)
*  [Pontuando modelos on-line](pm_service_api_develop_score.html)
*  [Implementando modelos em lote](pm_service_api_spark_batch.html)
*  [Implementando modelos de fluxo](pm_service_api_spark_streaming.html)
*  [Sistema de aprendizado contínuo](pm_service_api_spark_learning_system.html)

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
