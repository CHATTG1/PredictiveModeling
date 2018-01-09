---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Estruturas suportadas

O serviço {{site.data.keyword.pm_full}} suporta as seguintes estruturas
para desenvolvimento e validação de modelo como parte do gerenciamento de ciclo de vida
do modelo.
{: shortdesc}

## Spark MLlib

O serviço {{site.data.keyword.pm_full}} suporta Spark MLlib para o serviço
on-line, em lote (beta), implementação de fluxo (beta) e pontuação. Somente versões e ambientes de tempo de execução específicos são suportados.

### Recursos

* Tipos de implementação:
  * On-line
  * Em lote com armazenamento de objeto, DB2 Warehouse on Cloud e DB2 on Cloud
  * Fluxo com hub de mensagens
*  Sistema de aprendizado contínuo

### Versões suportadas

*  Spark 2.0

### Restrições

  *  Somente modelos de classificação e de regressão são suportados.
  *  Os modelos que contêm referências a transformadores customizados, funções
definidas pelo usuário e classes não são suportados.
  * Implementações em lote e de fluxo usam o serviço IBM Cloud Apache Spark. Dependendo
do plano de serviço associado, observe as seguintes limitações no número de tarefas paralelas spark-submit
([Usando
a documentação do spark-submit](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite: apenas uma tarefa spark-submit por vez
    * Reservado a empresas: 150 tarefas spark-submit por vez

## Scikit-learn

Há suporte para scikit-learn para o serviço de escoragem e implementação on-line do {{site.data.keyword.pm_full}}. Somente versões e ambientes de tempo de execução específicos são suportados.

### Recursos

* Tipos de implementação:
  * On-line

### Versões suportadas

- Tempo de execução do Anaconda 4.2.x for Python 3.5
- Scikit-Learn 0.17

### Tempo de execução Python

O tempo de execução Python para modelos que precisam ser implementados usando o serviço de escoragem e implementação on-line do WML é o Python 3,5.

Em alguns casos, em que os modelos foram criados usando o tempo de execução do Python 2.7, a implementação poderá falhar devido a incompatibilidade entre o Python2.7 e o Python3.5.

### Pacotes Python suportados

Uma lista de pacotes Python que são suportados pelo serviço WML Online Deployment and Scoring pode ser localizada [aqui](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restrições

Há algumas restrições, que podem incluir algumas ou todas as limitações a seguir:

* Somente modelos de classificação e de regressão são suportados.
* Os modelos podem conter referências somente a esses pacotes (e à versão do pacote) suportados pelo
tempo de execução do Anaconda 4.2.x for Python 3.5.
* O terminal "schema" retornado durante a solicitação de implementação não está implementado.
* Os modelos que requerem o Pandas Dataframe como o tipo de entrada para a API "predict()" não são
suportados.
* Os modelos que contêm referências a transformadores customizados, funções definidas pelo usuário e classes não são suportados.

## XGBoost

Há suporte para o XGBoost para o serviço de escoragem e implementação on-line do {{site.data.keyword.pm_full}}. Somente versões e ambientes de tempo de execução específicos são suportados.

### Recursos

* Tipos de implementação:
  * On-line

### Versões suportadas

- XGBoost 0.6
- Tempo de execução do Anaconda 4.2.x for Python 3.5
- Scikit-Learn 0.17

### Tempo de execução Python

O tempo de execução Python para modelos que precisam ser implementados usando o serviço de implementação
e pontuação on-line do WML é o Python 3.5.

Em alguns casos, em que os modelos foram criados usando o tempo de execução do Python 2.7, a implementação poderá falhar devido a incompatibilidade entre o Python2.7 e o Python3.5.

### Pacotes Python suportados

Uma lista de pacotes Python que são suportados pelo serviço WML Online Deployment and Scoring pode ser localizada [aqui](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restrições

Há algumas restrições, que incluem algumas ou todas as limitações a seguir:

* Os modelos podem conter referências somente a esses pacotes (e à versão do pacote) suportados pelo
tempo de execução do Anaconda 4.2.x for Python 3.5.
* O terminal "schema" retornado durante a solicitação de implementação não está implementado.
* A probabilidade de predição não é retornada como uma resposta de solicitação de pontuação neste
estágio.
* Os modelos que contêm referências a transformadores customizados, funções definidas pelo usuário e classes não são suportados.

## SPSS Modeler

Há suporte para os fluxos do IBM® SPSS® Modeler para o serviço de escoragem, implementação em lote e on-line do {{site.data.keyword.pm_full}}. Somente versões e ambientes de tempo de execução específicos são suportados.

### Recursos

* Tipos de implementação:
  * On-line
  * Em lote
* Novo treinamento
* Fluxo de execução

### Versões suportadas

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Restrições

As restrições a seguir existem para os fluxos do IBM® SPSS® Modeler:

*  Fluxos que são criados usando o editor de fluxo do IBM® Data Science Experience não são suportados.
*  Quando uma ramificação de pontuação é preparada para uso em pontuação em tempo
real, os dados de entrada vindos da solicitação de escore devem substituir o nó de origem
projetado na ramificação de pontuação e a saída da análise preditiva resultante deve
fluir de volta para o fluxo de resposta (substituindo efetivamente o nó terminal no
design de ramificação de pontuação).
*  Como a ramificação de escoragem é preparada para execução em tempo real no
{{site.data.keyword.Bluemix_short}}, ela não pode requerer uma conexão com um
serviço externo. Por exemplo, um design de ramificação de escoragem do IBM Analytical
Decision Management não pode conter referências a regras ou modelos armazenados em um
repositório do IBM® SPSS® Collaboration and Deployment Services.
*  A execução de uma ramificação de escoragem para escoragem em tempo real no
{{site.data.keyword.Bluemix_short}} não pode requerer um serviço externo. Por exemplo, não é possível implementar e pontuar com relação a algoritmos de modelo que requeiram um armazenamento de dados IBM® SPSS® Analytic Server e Apache Hadoop em tempo real.
*  O {{site.data.keyword.pm_short}} suporta script Python integrado no Modeler. Há algumas restrições devido ao método usado para processar fluxos antes que
eles sejam executados no {{site.data.keyword.pm_short}}. Normalmente, se
um usuário optar pelo controle da execução do fluxo, eles referenciarão o nó terminal da
ramificação. Para {{site.data.keyword.pm_short}}, quando processamos o fluxo, identificamos os nós do JSON que serão substituídos e, em seguida, executamos a substituição antes que o fluxo seja executado. Isso faz com que o
fluxo falhe no script porque os nós de entrada e exportação referenciados não existem mais. A
solução é usar o ID de outro nó para identificar exclusivamente a ramificação durante a
execução. Isso assegura que o fluxo seja executado conforme definido no script Python
integrado.

## Saiba mais

Para obter mais informações sobre o suporte atual para modelos preditivos treinados pelo IBM® SPSS® Analytic Server, consulte a seção [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) do [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
