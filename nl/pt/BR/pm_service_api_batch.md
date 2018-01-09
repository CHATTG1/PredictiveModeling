---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de tarefa em lote do serviço Machine Learning para modelos IBM SPSS Modeler

A API de tarefa em lote para o serviço {{site.data.keyword.pm_full}}
suporta as tarefas de longa execução que estão relacionadas ao treinamento de modelo,
avaliação de modelo e escoragem de lote. Essa API gerencia dois tipos de ativos: os
arquivos de fluxo do IBM® SPSS® Modeler que são usados nas tarefas em lote e as
definições de tarefa que são enviadas.
{: shortdesc}

Para cada arquivo de fluxo do SPSS® Modeler que você transfere por upload, pode
haver muitas tarefas de muitos tipos que são enviadas. Se uma tarefa tiver o potencial
para atualizar o conteúdo do arquivo de fluxo do Modeler, o arquivo modificado será
incluído nos anexos do resultado da tarefa. Em um tipo de tarefa de avaliação de modelo,
todos os arquivos de avaliação que são gerados estão nos anexos de resultados da tarefa.

**Nota**: os dados que aparecem no painel estão relacionados apenas
a predições em tempo real, como os dados do recurso de upload.

## Excluindo tarefas

É possível excluir tarefas. Se uma tarefa estiver em execução, a exclusão de uma tarefa primeiramente a cancela. Use o comando `DELETE` com o `job ID`. É
possível cancelar mais de uma tarefa por vez transmitindo múltiplos IDs.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

O retorno indica quantas tarefas, que são referenciadas por ID na solicitação, são excluídos. Se esse número não corresponder à lista passada na solicitação, deve-se
inspecionar o status de tarefas individuais.

## Verificando o status de uma tarefa

É possível obter o status de seu `job ID` em qualquer ponto usando o
comando `GET`:


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

O arquivo JSON que é retornado indica o status da tarefa e, se a tarefa foi
concluída com êxito, uma URL de dados que você pode usar para obter todo o conteúdo de
arquivo gerado.

## Reenviar uma tarefa

Para reenviar uma tarefa, utilize o comando `PUT` com o `job ID`. Para reenviar uma tarefa, ela não deve estar em execução.

Se o ID que você referenciar não existir, a chamada a seguir criará uma nova tarefa. O
status de retorno é 201 versus 200 (indicando qual desses ocorreu). Deve-se passar o
arquivo JSON de definição de tarefa a ser usado nessa execução.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Reenvie a tarefa com o valor `content_type` configurado como "application/json". Deve-se incluir o arquivo JSON de definição de tarefa novo ou atualizado no corpo da solicitação.

## Enviar uma tarefa em um arquivo de fluxo do modelador transferido por upload

Para colocar uma tarefa na fila de execução, use o comando `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Envie a tarefa com o valor `content_type` configurado como "application/json". Deve-se incluir o arquivo JSON de definição de tarefa novo ou atualizado no corpo da solicitação.

Essa solicitação retorna imediatamente e indica sucesso se a definição de tarefa
foi colocada na fila de execução. Não há teste da capacidade de executar com sucesso o fluxo
do Modeler conforme configurado em sua definição de tarefa. A tarefa será executada por
um dos servidores de tarefa {{site.data.keyword.pm_short}} na nuvem, e você pode
monitorar seu status. Se você estiver executando treinamento de modelo e indicando que deseja uma atualização automática, a tarefa substituirá o arquivo de fluxo do Modeler original após a execução bem-sucedida da tarefa.

## Fazer upload de um arquivo de fluxo para usar em suas tarefas

**Nota**: o painel do {{site.data.keyword.pm_short}} é somente para pontuação em tempo real. Não é possível usá-lo para executar tarefas (escoragem de
lote).

Para tornar um arquivo de fluxo do Modeler acessível para tarefas, use o comando `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

com content_type "multipart/form-data" passando o arquivo na
solicitação.

O nome exclusivo usado (`file ID` em uma chamada `PUT`) é
aquele que você usa como referência em uma chamada `DELETE` na API do arquivo e
também como a referência de modelo em suas definições de tarefa. Observe que você tem total controle do
namespace dos arquivos usados em suas tarefas. O comando `PUT` de um arquivo com o
mesmo `file ID` substitui implicitamente a cópia atual.

Para gerar uma lista de todos os arquivos transferidos por upload para suas tarefas, use um comando
`GET`, mas omita o parâmetro `file ID`. Para recuperar um arquivo
específico, use um comando `GET` com o parâmetro `file ID`. Você também pode excluir um arquivo transferido por upload emitindo um comando `DELETE`. Isso causa erros na execução de quaisquer tarefas pendentes que façam referência ao arquivo.

Exemplo de solicitação:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a implementação for bem-sucedida:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }      
```
{: codeblock}

Resposta quando a implementação falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## Tipos de Job

**Treinamento**: este tipo de tarefa indica que todos os nós de terminal do
"construtor de modelo" devem ser executados no fluxo do Modeler. Após a
conclusão bem-sucedida da tarefa, um fluxo atualizado do Modeler com nuggets do modelo
recém-treinado estará nos resultados da tarefa que podem ser recuperados. Se o arquivo de
fluxo do Modeler tiver links dos nós de construção de modelo para os nuggets do modelo
treinado nas ramificações de avaliação e escoragem, isso causará uma atualização desses
nós.

**Avaliação**: este tipo de tarefa aciona a execução de todos os nós de terminal do
"construtor de documento" (principalmente nas guias Gráficos e Saída no cliente do Modeler) que geram o
conteúdo do arquivo de relatório estático que pode ser transmitido de volta para o responsável pela chamada. A ramificação de escoragem não é considerada como parte desse
tipo de tarefa.

**Atualização automática**: uma versão do tipo de
tarefa `TRAINING`, em que o arquivo de fluxo original do
Modeler na lista de arquivos em lote é atualizado quando a tarefa é concluída com sucesso. A decisão de
avaliação e explícita sobre um evento de atualização dos fluxos do Modeler implementados
para escoragem em tempo real é assumida e não coberta na atualização automática neste
momento.

**Pontuação em lote**: execução do nó do terminal em que você selecionou a opção Usar
como ramificação de pontuação, indicando que esta é a ramificação de pontuação neste design de fluxo do
Modeler. A definição de tarefa deve especificar
os detalhes de Exportação, bem como os de Origem.

**Executar fluxo**: a execução é semelhante a clicar no ícone verde "executar" no Modeler com a opção Executar este script selecionada na guia Execução das propriedades de fluxo. São
necessárias coberturas de uso para execução do script do treinamento de modelo ou outros
tipos de tarefa. Todo o controle dinâmico do script deve ser manipulado pelos parâmetros
do fluxo, com os valores de parâmetro passados na definição de tarefa.

## Status do Trabalho

**Pendente**: a definição de tarefa foi enviada, mas ainda não foi solicitada por um
servidor de tarefa para execução.

**Em execução**: a definição de tarefa foi solicitada por um servidor de tarefa
e está em execução.

**Cancelando**: a tarefa está no processo de cancelamento.

**Cancelada**: a tarefa foi cancelada.

**Com falha**: a tarefa falhou. Os detalhes sobre a causa da falha são
retornados em GET no status da tarefa.

**Sucesso**: a tarefa foi executada com sucesso. Qualquer resultado comunicado para
esse evento é retornado no JSON do GET no status da tarefa.

## Detalhes da API de tarefa

`POST /v1/jobs/{id}`

Descrição: envia uma definição de tarefa para execução. Um
erro resultará se o `job ID` já existir.

Tipos de conteúdo:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` especificado pelo usuário. Deve ser exclusivo para uma instância de serviço do {{site.data.keyword.pm_short}}:

```
@PathParam("id")
```
{: codeblock}

Definição de tarefa JSON (veja JSON de definição de tarefa):

```
@BodyParam
```
{: codeblock}

Respostas:

Sucesso. A tarefa é enviada para execução e retorna o JSON que descreve a tarefa
enviada.

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa já existente. Uma tarefa com esse ID já foi enviada. A mensagem
retornada descreve esse erro.

```
@ApiResponse(code = 409)
```
{: codeblock}

Outro erro. JSON de exceção retornada

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT
/v1/jobs/{id}`

Descrição: criar ou atualizar uma tarefa. Se uma tarefa
com esse ID não existir, será criada; caso contrário, será atualizada (o que, efetivamente,
a reenvia para execução).

Tipos de conteúdo:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` especificado pelo usuário:

```
@PathParam("id")
```
{: codeblock}

Definição de tarefa JSON (veja JSON de definição de tarefa):

```
@BodyParam
```
{: codeblock}

Respostas:

Sucesso. A tarefa foi atualizada e enviada para execução. Retorna o JSON que descreve
a tarefa enviada.

```
@ApiResponse(code = 200)
```
{: codeblock}

Sucesso. A tarefa foi criada e enviada para execução. Retorna o JSON que descreve
a tarefa enviada.

```
@ApiResponse(code = 201)
```
{: codeblock}

Outro erro. JSON de exceção retornada.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET
/v1/jobs`

Descrição: retorna uma lista de todas as tarefas definidas nesta instância de
serviço de Machine Learning.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

Respostas:

Sucesso. Retorna a matriz JSON de todas as definições de tarefa:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET
/v1/jobs/{id}`

Descrição: solicitar o retorno de uma definição de tarefa específica.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` enviado:

```
@PathParam("id")
```
{: codeblock}

Respostas:

Sucesso. Retorna o JSON que descreve a tarefa referenciada:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa não localizada. Detalhes na mensagem
retornada:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET
/v1/jobs/{id}/status`

Descrição: recuperar o status de uma tarefa específica.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` enviado:

```
@PathParam("id")
```
{: codeblock}

Respostas:

Sucesso. Retorna o JSON que descreve o status da
tarefa:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa não localizada, detalhes na mensagem
retornada:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE
/v1/jobs/{ids}`

Descrição: excluir uma ou mais tarefas. Cancelará a
tarefa se ela estiver em execução no momento.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

Listas de `job ID` ou `job ID` com valores de ID
divididos por uma vírgula (,)

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Respostas:

Sucesso. Retorna o número de tarefas excluídas:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON de definição de tarefa

O JSON de definição de tarefa contém as seções gerais a seguir:

Referência de tipo de tarefa e modelo preditivo

**Nota**: os dados que são mostrados no painel se referem somente a
predições em tempo real, incluindo os dados do recurso de upload.

### Tipos de Ações

**TRAINING** – Executa o nó de construção de modelo treinando ou
atualizando os nuggets do modelo. O fluxo do Modeler atualizado é recuperado nos
resultados da tarefa.

**EVALUATION** – Executa os nós de análise e construtor de documento
avaliando o modelo treinado.

**AUTO_REFRESH** - executa TRAINING e atualiza o conteúdo de arquivo no espaço
de upload de arquivo em lote.

**BATCH_SCORE** – Executa a ramificação de escoragem e exporta os dados
resultantes conforme direcionado pela definição de tarefa.

**RUN_STREAM** – Executa o fluxo do Modeler conforme indicado nas
propriedades do fluxo; muitas vezes usado quando é necessária a execução do script.

Modelo: ID conforme especificado na ação de upload de arquivo para a referência de
tarefa em lote. Para obter mais informações, veja Fazendo upload de um arquivo de fluxo
para usar em suas tarefas. Observe que `/pm/v1/file` é usado.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Observe que o ID deve ser o mesmo que o `file ID` usado na
API `PUT`. O nome não é necessário, mas para treinamento e atualização automática
de modelo, o resultado da tarefa será salvo usando o nome definido
aqui. Se o nome não for definido, o serviço {{site.data.keyword.pm_short}} gerará
o resultado de acordo com as regras de nomenclatura predefinidas.

### Definições do job

Todas as configurações requeridas para executar esta
tarefa.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### Definições de conectividade do banco de dados

Tipo de banco de dados: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL,
ProgressOpenEdge, Salesforce, SQLServer, Sybase, SybaseIQ, Teradata.

Conectividade conforme especificada na instância de serviço do BD, muitos tipos de BD
requerem que configurações específicas sejam passadas em
‘Options’

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### Configurações do nó de origem

Conectividade e tabela de BD de referência usadas para dar origem a uma
determinada ramificação no fluxo do Modeler, conforme identificado pelo nome do nó de
origem.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

A tabela é o nome da tabela de banco de dados. Essa tabela é usada para
substituir o nó de origem do fluxo. O nó é o nome da origem de dados
para o fluxo. O dbRef referencia a conectividade do DB definida
pelo objeto dbDefinitions e a tabela é a tabela de banco de dados usada
como a entrada da ramificação específica no fluxo do Modeler. O nó
identifica o nó de origem original no fluxo do Modeler a ser
substituído por um nó de origem do DB construído com esses parâmetros
fornecidos.

### Exportar configurações de nó

Conectividade e tabela de BD de referência usadas para persistir dados
de uma determinada ramificação no fluxo do Modeler, conforme identificado pelo nome do nó
do terminal.

O método de controle usado ao persistir dados é comunicado por meio do
atributo insertMode:

**Append** – a tabela deve existir e ser compatível para inserção

**Create** – a tabela é criada (um erro será retornado se ela já existir)

**Drop** – se a tabela referenciada existir, ela será eliminada e recriada

**Refresh** – as linhas existentes da tabela serão excluídas antes de
inserir novas linhas
   
A tabela é o nome da tabela de banco de dados na qual gravar os resultados da tarefa.
O nó é o nome do nó terminal para o fluxo. Semelhante às
configurações do nó de origem, nó identifica o nó de saída original no
fluxo do Modeler a ser substituído por um nó de exportação do DB construído
com esses parâmetros fornecidos.

**Nota**: as configurações do nó de exportação e as configurações do nó de origem são
importantes para que sua tarefa seja executada com sucesso.

### Substituições de valor de parâmetro

Para substituir os valores padrão dos parâmetros de nível de fluxo antes da
execução da tarefa, é possível especificar os pares de nome/valor a serem usados na
definição de tarefa.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Formatos de relatório desejados para documento de avaliação retornado ao
executar um tipo de tarefa de avaliação

Todos os documentos gerados da execução da tarefa de Avaliação são
empacotados em um arquivo .zip. Somente nós do construtor de documento capazes
de suportar o reportFormat indicado são executados e têm
seu conteúdo retornado após a execução bem-sucedida da tarefa.

Os seguintes formatos são suportados: HTML, JPG, PNG, RTF, SAV, TAB e XML.

```
"reportFormat": "HTML"
```
{: codeblock}

### Exemplo completo de JSON

```
{ 
     "action": "TRAINING", 
     "model": { 
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Detalhes da API de tarefa em lote

As seções a seguir fornecem detalhes da API de gerenciamento de arquivo do IBM®
SPSS® Modeler da tarefa em lote.

`PUT
/v1/file/{id}`

Descrição: faz upload de um arquivo de fluxo do IBM® SPSS® Modeler para uso em tarefas em lote.

**Nota**: se houver um arquivo já armazenado sob o ID especificado, ele será
sobrescrito.

### Tipos de conteúdo

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### Parâmetros

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey") 
```
{: codeblock}

`file ID` especificado pelo usuário. Deve ser exclusivo no repositório de
instância de serviço:

```
@PathParam("id") 
```
{: codeblock}

### Respostas

Sucesso. Retorna a URL que pode ser usada para fazer download do arquivo do
repositório da instância de serviço:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE
/v1/file/{id}`

Descrição: exclui o arquivo armazenado com o ID especificado do
repositório de instância de serviço.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID especificado pelo usuário para o arquivo quando transferido por upload:

```
@PathParam("id") 
```
{: codeblock}

Respostas:

Sucesso. O arquivo especificado foi excluído:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro. Um arquivo armazenado sob este ID não existia no repositório de instância de
serviço:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET
/v1/file/`

Descrição: retorna uma lista dos IDs de todos os arquivos que estão sendo gerenciados
no repositório de instância de serviço para processamento de tarefa em lote.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")  
```
{: codeblock}

Respostas:

Sucesso. Retorna uma matriz JSON de valores de `file ID`:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET
/v1/file/{id}`

Descrição: recupera o arquivo de fluxo do IBM® SPSS® Modeler armazenado para uso no processamento da tarefa em lote sob o ID especificado.

Tipos de conteúdo:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")  
```
{: codeblock}

ID especificado pelo usuário para o arquivo quando transferido por upload:

```
@PathParam("id") 
```
{: codeblock}

Respostas:

Sucesso. Retorna o arquivo do IBM® SPSS® Modeler:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro. Um arquivo armazenado sob este ID não existia no repositório de instância de
serviço:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

## Saiba mais

Para obter um exemplo de adoção de tarefa em lote em uma anotação, consulte
[Do fluxo do SPSS para a escoragem de lote com Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

Para obter mais informações sobre o JSON de definição de tarefa, veja [JSON
de definição de tarefa](#job-definition-json).
