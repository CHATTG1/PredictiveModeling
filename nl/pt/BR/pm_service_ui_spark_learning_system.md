---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sistema de aprendizado contínuo

O sistema de aprendizado contínuo do {{site.data.keyword.pm_full}} fornece
monitoramento automatizado de desempenho, novo treinamento e reimplementação do modelo
para assegurar a qualidade da predição.
{: shortdesc}

**Nome do cenário**: bons medicamentos para seleção de tratamento cardíaco.

**Descrição do cenário**: uma empresa biomédica que produz remédios para o coração
deseja implementar um modelo que seleciona o melhor medicamento para tratamento cardíaco. Quando surgem novas evidências sobre a eficácia do medicamento, uma atualização de modelo deve ser considerada. Um cientista de
dados prepara um modelo preditivo e o compartilha com você (o desenvolvedor). Sua tarefa
é implementar o modelo, definir a configuração de aprendizado e executar a iteração do
aprendizado para avaliar, treinar novamente e reimplementar o modelo.

## Pré-requisitos

Para trabalhar com esse exemplo, é necessário ter os serviços a seguir:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) para armazenar os dados de feedback
* Serviço do [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark)

   Use [este link](https://console.bluemix.net/catalog/services/apache-spark) para criar uma instância do Apache Spark se você ainda não tiver uma.

## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o quadro
**Heart Drug Selection** e clique no ícone (+) **Incluir modelo**.

O modelo de amostra Heart Drug Selection aparece na lista de modelos disponíveis na
guia **Modelos**.


## Configure o sistema de aprendizado contínuo para o modelo publicado

1.  Acesse a guia **Modelos** do Painel do {{site.data.keyword.pm_full}}.
2.  No menu **Ações**, clique em **Visualizar detalhes**.
3.  Role para baixo até **Detalhes do novo treinamento** e pressione
**Editar**.
4.  Deve-se fornecer as seguintes entradas:

    **Conexão de feedback**: detalhes do
{{site.data.keyword.dashdbshort}}, que são usados como entrada (dados de
feedback) para avaliação e novo treinamento do modelo.

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Use as instruções a seguir para preparar a tabela `DRUG_FEEDBACK_DATA` no {{site.data.keyword.dashdbshort}}.
    
    - Crie uma instância de serviço do [{{site.data.keyword.dashdbshort}}](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (um plano de entrada é oferecido).
    - Crie a tabela **DRUG_FEEDBACK_DATA** no **{{site.data.keyword.dashdbshort_notm}}**.
      + Faça download do arquivo [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) no repositório GitHub.
      + Clique em **Abrir o console** para dar início ao **{{site.data.keyword.dashdbshort_notm}}**.
      + Clique em **Carregar dados** e selecione o tipo de carregamento **Área de trabalho**.
      + **Arraste** o arquivo transferido por download anteriormente para a área de carregamento e clique em **Avançar**.
      + Selecione **Esquema** para importar dados e clique em **Nova
tabela**.
      + Digite um nome para a nova tabela e clique em **Avançar**.
      + Use um ponto e vírgula (;) como **separador de campo**.
      + Clique em **Avançar** para criar uma tabela com os dados transferidos por upload.

     **Nota**: Você pode incluir novos registros de feedback no banco de dados de feedback usando o
[terminal
da API de REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback).

     **Tamanho mínimo dos dados**: o número mínimo de registros no
conjunto de dados de feedback para iniciar a avaliação do modelo (parte da iteração do
sistema de aprendizado).

     ```
     20
     ```
     {: codeblock}

     **Conexão do Spark**: as credenciais de serviço do Spark
podem ser encontradas na guia **Credenciais de serviço** do painel de serviços do
{{site.data.keyword.Bluemix_notm}} Spark.

     ```
{
    "credentials": {
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

     **Limites**: o valor do limite que aciona o processo de novo treinamento. Se o valor da métrica, como o valor de precisão que é calculado durante a avaliação de modelo, for menor do que o valor do limite, ele acionará o novo treinamento.

     ```
     0.8
     ```

     **Implementar automaticamente o modelo treinado novamente**:
implemente o modelo treinado novamente se ele tiver um valor de métrica melhor do
que o implementado.
5.  Clique em **Configurar**.

## Executar iteração do sistema de aprendizado contínuo

Um modelo publicado é avaliado iterativamente. Se o valor da métrica, como o valor de precisão que é calculado durante a avaliação de modelo, for menor do que o valor do limite, ele acionará o novo treinamento. Ambos
os conjuntos de dados, treinamento e feedback, são utilizados para este processo
iterativo de novo treinamento e avaliação.

1. Para iniciar uma nova iteração, no formulário **Detalhes** do modelo, na
guia **Monitor**, clique em **Avaliar**.
3. Para verificar o resultado da iteração, acesse a tabela Eventos de avaliação ou
a visualização de Gráfico. 

   É possível visualizar informações sobre a qualidade do modelo que é calculada
com base nos dados de feedback (monitoramento). Você também pode visualizar informações
sobre a qualidade do modelo treinado novamente, que é a nova versão do modelo que foi
criada e avaliada.

4. Para ver a lista de modelos treinados automaticamente e sua qualidade, clique
em **Visualizar detalhes** > **Visão geral** e vá
para a seção **Histórico de versões**.

## Armazenamento de dados de feedback

É possível usar um
[terminal
de feedback](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) para enviar novos registros ao armazenamento de feedback que você definiu na
configuração de aprendizado de máquina. O {{site.data.keyword.dashdbshort}} é
atualmente suportado como armazenamento de dados de feedback. Se a tabela de feedback não existir, o serviço a criará. Se a tabela existir, o esquema será verificado para corresponder com o da tabela de treinamento e uma coluna extra chamada `_training` será anexada à tabela. A
coluna `_training`a é usada para marcar registros que são
consumidos durante o processo de novo treinamento.

Para obter mais informações e exemplos de um armazenamento de dados de feedback,
consulte a [Documentação da
API](pm_service_api_spark_learning_system.html).

**Nota:** A tabela de feedback é gerenciada, modificada e usada
pelo serviço {{site.data.keyword.pm_full}}.

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
