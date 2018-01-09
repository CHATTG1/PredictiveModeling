---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Construindo aplicativos de analítica preditiva

É possível usar o serviço {{site.data.keyword.pm_full}} para implementar um aplicativo Node.js.
{: shortdesc}

**Nome do cenário**: predição de linha de produto.

**Descrição do cenário**: nosso cliente está cuidando de uma das cadeias de
lojas mais famosas da Europa. Eles
gostariam que nós descobríssemos os interesses dos clientes em
sua linha de produtos, como acessórios pessoais,
equipamento de acampamento e proteção para atividades externas.
Um Cientista de dados desenvolve um modelo preditivo e o
compartilha com você (o desenvolvedor). Sua tarefa é preparar e
implementar o aplicativo Node.js que recomenda linhas de
produtos esportivos fazendo solicitações de pontuação em relação
ao modelo implementado.

1. Efetue logon no {{site.data.keyword.Bluemix_short}} e crie uma
instância do {{site.data.keyword.pm_full}}.
2. Ative o {{site.data.keyword.pm_full}} Dashboard.
3. Acesse a guia **Amostras**.
4. Na seção **Modelos de amostra**, localize o quadrado **Predição de linha de produto** e clique no ícone **Incluir modelo** (+). O
modelo de Previsão de linha de produto de amostra aparece na lista de modelos disponíveis
na guia **Modelos**.

   **Nota**: se você quiser usar seu próprio projeto e modelos
do Data Science Experience, em vez das amostras, deverá persistir um modelo customizado
no repositório do {{site.data.keyword.pm_short}}. É possível fazer isso usando a API REST ou as bibliotecas do
cliente. Para obter
detalhes, veja Desenvolvimento e persistência do modelo customizado.

5. No menu **Ação**, clique em **Criar
implementação** para implementar o modelo de Previsão de linha de produto como uma
implementação on-line.
6. Especifique o nome da implementação (por exemplo, Predição de linha de
produto) e clique em Salvar. Agora, você deverá ver a implementação de
Predição de linha de produto na lista Implementações.
7. No menu Ação, selecione Visualizar detalhes para a sua implementação.
   O Terminal de pontuação apresentado na área de janela de detalhes será
usado para executar solicitações de escore.
8. Para executar solicitações de pontuação por meio da API
REST, são necessários um terminal de pontuação e um token de autorização. Para obter mais
informações, veja Implementando modelos on-line.
9. Experimente com o aplicativo Node.js de amostra, que está localizado no seguinte local:
   https://github.com/pmservice/product-line-prediction.

   Para implementar automaticamente o código do aplicativo de amostra em seu
   espaço do {{site.data.keyword.Bluemix_short}}, acesse a guia Amostras e, na
   seção Aplicativos de amostra, selecione o aplicativo Product Line Prediction
   e implemente-o clicando no ícone (+).
   Autentique-se no formulário DeployToBluemix, se solicitado.

   Você deverá ver agora o aplicativo de amostra no
{{site.data.keyword.Bluemix_short}} Dashboard, na seção Todos os apps.

10. Clique no aplicativo para visualizar os seus detalhes. Aqui você pode
modificar detalhes do aplicativo, como o número de instâncias ou alocação de memória.
11. Ligue seu aplicativo com a instância do {{site.data.keyword.pm_short}}. Na
guia **Conexões**, clique em **Conectar existente**.
    Após o
reestabelecimento, seu aplicativo está conectado à instância de serviço.
12. Execute o aplicativo usando o endereço da rota.
13. No aplicativo, selecione sua implementação de Previsão de
Linha de Produto. Agora você está pronto para usar os registros de amostra e os resultados de predição.
    
## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
