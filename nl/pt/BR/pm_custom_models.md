---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desenvolvimento e persistência do modelo customizado

Usando o serviço {{site.data.keyword.pm_full}}, é possível implementar um modelo e gerar análise preditiva criando solicitações de pontuação com relação ao modelo implementado.
{: shortdesc}

## Trabalhando com modelos customizados

### Nome do cenário: Predição da linha de produto.

### Descrição do cenário:

Nosso cliente comanda uma das cadeias de lojas mais famosas da Europa. Eles gostariam que nós descobríssemos os interesses dos clientes em sua linha de produtos, como acessórios pessoais, equipamento de acampamento e proteção para atividades externas.
Um cientista de dados desenvolve um modelo preditivo e o compartilha com você (o desenvolvedor). Sua tarefa é implementar o modelo e gerar análise preditiva fazendo solicitações de pontuação em relação ao modelo implementado.

### Desenvolvimento e persistência do modelo customizado

#### Usando o Data Science Experience

Use o [Data Science Experience](https://console.bluemix.net/catalog/services/data-science-experience) para criar modelos customizados. Depois de se inscrever, conecte-se para concluir as etapas a seguir.

1. Crie uma organização e um espaço. Na primeira vez que você se conectar, deverá fornecer estas informações. Clique em **Continuar** para aceitar os
valores padrão.
2. Após a organização ser criada, acesse **Projetos** e clique em **Novo
projeto**.
3. Especifique um nome e uma descrição para o seu projeto e
clique em **Criar**. O nome do projeto que você especificou é usado como o nome do contêiner de destino.
4. Após o projeto ser criado, será possível executar uma das tarefas a seguir:
   
   *  **incluir anotações** e iniciar o desenvolvimento de seus próprios modelos ou fazer upload de uma das anotações de amostra a seguir:
        *  [Desenvolvendo modelos Spark MLlib com o Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Desenvolvendo modelos Spark MLlib com o Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Desenvolvendo modelos scikit-learn com o Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **incluir modelos** e começar a desenvolver seus próprios modelos usando o assistente de modelo.


#### Usando o ambiente local

Você também pode utilizar o ambiente de sua escolha para desenvolver o modelo e
depois publicar, implementar e pontuar usando a [biblioteca do cliente da
API comum]() do Watson Machine Learning disponível em pypi.
Para obter mais informações sobre a biblioteca do cliente, consulte a amostra de
[anotações](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125)
e a [documentação](pm_service_client_library.html).

**Nota:** a biblioteca do cliente da API comum está em
**beta**.

### Implementação e pontuação do modelo customizado

É possível usar a API para implementar e pontuar em modelos on-line, em lote e de fluxo.

*  [Implementando modelos on-line](pm_service_api_spark_online.html)
*  [Implementando modelos em lote](pm_service_api_spark_batch.html)
*  [Implementando modelos de fluxo](pm_service_api_spark_streaming.html)

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM® Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
