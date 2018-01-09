---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Implementando modelos de fluxo

É possível usar o serviço {{site.data.keyword.pm_full}} para implementar o
modelo e gerar análise preditiva fazendo solicitações de escore com relação ao modelo
de fluxo implementado.
{: shortdesc}

**Nome do cenário**: análise de sentimentos.

**Descrição do cenário**: uma agência de marketing quer saber a
impressão sobre um tópico específico. A agência gostaria que nós desenvolvêssemos um
modelo que classifique uma instrução como POSITIVA ou NEGATIVA. Um cientista de
dados prepara um modelo preditivo e o compartilha com você (o desenvolvedor). Sua tarefa é implementar
o modelo e gerar análise preditiva fazendo solicitações de
pontuação em relação ao modelo implementado.

Consulte este [documento](https://github.com/pmservice/tweet-sentiment-prediction) para obter mais informações.

## Pré-requisitos

Para trabalhar com este exemplo, você precisa dos seguintes recursos:

* Detalhes do tópico
[Hub de
mensagens](https://console.bluemix.net/catalog/services/message-hub), que será usado como entrada (texto do tweet) para o modelo e
armazenamento da saída do modelo (resultados de predição). Certifique-se de que dois tópicos sejam criados: entrada com texto de tweet e tópico de saída
* Credenciais da instância de serviço do [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Use [este link](https://console.bluemix.net/catalog/services/apache-spark) para criar um.


## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o quadro
**Predição de impressão** e clique no ícone (+) Incluir modelo.

O modelo de amostra Predição de impressão aparece na lista de modelos disponíveis na
guia Modelos.


## Criando uma implementação de fluxo com o IBM Message Hub

1.  Acesse a guia **Modelos** do Painel do {{site.data.keyword.pm_full}}.
2.  No menu **Ações**, clique em **Criar implementação**.
3.  No formulário **Criar implementação**, preencha os campos
**Nome**, **Descrição** e **Tipo de fluxo**.
4.  Deve-se fornecer as seguintes entradas:

    **Conexão de entrada**: detalhes do tópico IBM Message
Hub, que são usados como entrada (tweets) para o modelo e armazenamento da saída do modelo
(resultados de predição).

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
        "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
        "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
        "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
        "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
        "user":"Dv5kEVNNsbuJ9RFE",
        "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
     "source":{
        "topic":"sinput",
        "type":"kafka"
     }
  }
    ```
    {: codeblock}

    **Conexão de saída**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
        "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
        "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
        "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
        "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
        "user":"Dv5kEVNNsbuJ9RFE",
        "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
    "target":{
       "topic":"soutput",
       "type":"kafka"
    }
 }
    ```
    {: codeblock}

    **Conexão do Spark**: as credenciais de serviço do Spark
podem ser encontradas na guia Credenciais de serviço do painel de serviços do
{{site.data.keyword.Bluemix_notm}} Spark.

     ```
{
     "credentials":{
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
       "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
       "cluster_master_url": "https://spark.bluemix.net",
       "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
       "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
       "plan": "ibm.SparkService.PayGoPersonal"
     },
     "version":"2.0"
}
     ```
     {: codeblock}

5. Clique em **Salvar**.

O resultado da predição é enviado para o tópico MessageHub definido (conexão de saída).

## Obtendo detalhes da implementação

É possível verificar o status e os parâmetros relacionados ao modelo implementado.

1. Acesse a guia **Implementações** do {{site.data.keyword.pm_full}}
   Dashboard.
2. No menu **Ações**, clique em **Visualizar detalhes**.

## Excluindo um fluxo de implementação

Será possível excluir a implementação, se ela não for mais necessária, executando
uma consulta como a amostra a seguir.

1. Acesse a guia **Implementações** do {{site.data.keyword.pm_full}}
   Dashboard.
2. No menu **Ações**, clique em **Excluir**.

## Saiba mais

Pronto para começar? Para criar uma instância de serviço ou ligar um aplicativo,
consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html)
ou [Usando o serviço com modelos IBM® SPSS®](using_pm_service.html).
Para obter mais informações sobre a API, consulte [API de serviço para modelos Spark e
Python](pm_service_api_spark.html) ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).
Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que
ele fornece, consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).
Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de
modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
