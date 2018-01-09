---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informações iniciais
{: #WMLgettingstarted}

O {{site.data.keyword.pm_full}} é um serviço IBM Cloud que permite aos usuários executar duas operações fundamentais de aprendizado de máquina: treinamento e pontuação.
{: shortdesc}

- **Treinamento** é o processo de refinar um algoritmo para que
ele possa aprender com um conjunto de dados. O resultado dessa operação é chamado de
modelo. Um modelo abrange os coeficientes aprendidos de expressões matemáticas.
- **Pontuação** é a operação de prever um resultado usando um
modelo treinado. O resultado da operação de pontuação é outro conjunto de dados contendo
valores preditos.

O {{site.data.keyword.pm_full}} é projetado para atender às necessidades de dois personagens principais:

- Cientistas de dados: criam pipelines de aprendizado de máquina que utilizam transformações de dados e algoritmos de aprendizado de máquina. Eles geralmente usam notebooks ou ferramentas externas para treinar e avaliar seus modelos. Os cientistas de dados frequentemente colaboram com engenheiros de dados para explorar e entender os dados.
- Desenvolvedores: constroem aplicativos inteligentes que usam as predições exibidas por modelos de aprendizado de máquina.

Embora o treinamento seja uma etapa crítica no processo de aprendizado de máquina, o {{site.data.keyword.pm_full}} permite simplificar o funcionamento de seus modelos, implementando-os e obtendo o real valor de negócios deles ao longo do tempo e através de todas as suas iterações.

## Pré-requisitos

Para usar o {{site.data.keyword.pm_full}}, no catálogo do
{{site.data.keyword.Bluemix_short}}, deve-se criar a
[instância
de serviço aqui](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/). Essa configuração permite executar as tarefas a seguir:

## Etapas

1. [Configurar o ambiente de aprendizado de máquina.](ml_getting_access.html)
1. [Criar e armazenar um modelo](pm_custom_models.html).
2. [Implementar um modelo](pm_service_api_spark_online.html).
3. Use o `scoring endpoint` do modelo implementado em seu aplicativo para [obter predições.](pm_service_api_spark_building.html)

## Usando o aprendizado de máquina com o Data Science Experience

O {{site.data.keyword.pm_full}} é integrado ao IBM Data Science Experience. É possível usar bibliotecas de cliente da API de aprendizado de máquina com anotações do
Data Science Experience; deve-se ter uma instância de aprendizado de máquina para usar o construtor de modelos e o editor de fluxo.

## Usando aprendizado de máquina com o SPSS Modeler

O {{site.data.keyword.pm_full}} é integrado com o IBM® SPSS® Modeler. É possível usar a API de aprendizado de máquina para alavancar algoritmos matemáticos avançados.


## Usando o aprendizado de máquina com seu ambiente

O {{site.data.keyword.pm_full}} pode ser usado como solução híbrida vinculando seu ambiente local com a nuvem. É possível usar a API de aprendizado de
máquina para publicar seus modelos, implementar e pontuar. Para obter mais informações, consulte [DSX: modo híbrido](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b).

## Sobre

O serviço {{site.data.keyword.pm_full}} é um conjunto de APIs REST que
podem ser chamadas de qualquer linguagem de programação.

O foco do serviço {{site.data.keyword.pm_full}} é a implementação, mas
você pode utilizar o IBM® SPSS® Modeler ou o IBM® Data Science Experience para criar e
trabalhar com modelos e pipelines. O SPSS® Modeler e o Data Science Experience que usam
Spark MLlib e Python scikit-learn oferecem vários métodos de modelagem que são extraídos do
aprendizado de máquina, inteligência artificial e estatísticas.

## Links relacionados

Pronto para começar? Para criar uma instância de um serviço ou ligar
um aplicativo, veja [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou
[Usando o serviço com modelos SPSS](using_pm_service.html).

Para obter mais informações sobre a API, consulte [API de serviço para modelos Spark e Python](pm_service_api_spark.html) ou [API de serviço para modelos SPSS](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
