---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Política de cotas

O mecanismo {{site.data.keyword.pm_full}} impõe cotas por instância para assegurar alocação e uso apropriados de recurso. Essas políticas variam de acordo com os perfis da conta, com o uso de serviço
histórico e com a disponibilidade de recursos. A política de cotas descreve os limites que não são definidos
pelo plano de precificação e que **estão sujeitos a mudanças sem aviso prévio**. Os seguintes limites de cota de serviço estão atualmente ativos.
{: shortdesc}

## Uso do Recurso

Os recursos são limitados aos seguintes valores máximos:

-  **Máximo de modelos publicados**: 200 (Lite Plano) 1000 (Planos Standard e Professional)
-  **Tamanho máximo de modelo do Spark ML**: 200 MB
-  **Máximo de modelos implementados**: 1000 (Planos Standard e Professional)

**Nota**: Para entender os limites do Plano Lite, consulte a descrição do [Plano Lite](https://console.bluemix.net/catalog/services/machine-learning) no [{{site.data.keyword.Bluemix_notm}} Catalog](https://console.bluemix.net/catalog/).

## Limites de solicitação de serviço

Você está limitado ao número de solicitações que pode fazer por minuto para APIs específicas. Se seu aplicativo precisar de limites mais altos, [entre em contato com o Suporte ao {{site.data.keyword.Bluemix_notm}}](https://support.ng.bluemix.net/) para aumentar sua cota.

## Solicitações de implementação

Os seguintes limites se aplicam às solicitações da API de implementação:

-  **Período**: 60 segundos
-  **Limite padrão**: 10

## Solicitações de predição on-line

Os seguintes limites se aplicam às solicitações de
[predições on-line](pm_service_api_spark_building.html):

-  **Período**: 60 segundos
-  **Limite padrão**: 500

## Solicitações de gerenciamento de recurso

Os seguintes limites se aplicam ao total de solicitações combinadas nesta lista:

[Token](https://watson-ml-api.mybluemix.net/#/Token): solicitações de post, get, delete,
put, patch

[Implementações](https://watson-ml-api.mybluemix.net/#/Deployments): solicitações
de post, get, delete, put, patch

-  **Período**: 60 segundos
-  **Limite padrão**: 100

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM® Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
