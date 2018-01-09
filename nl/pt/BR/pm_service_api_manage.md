---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gerenciando os modelos SPSS implementados

É possível usar a API de serviço do {{site.data.keyword.pm_full}} para fazer
upload de um arquivo que contenha a ramificação de escoragem do IBM® SPSS® Modeler a ser
implementada. Depois de transferido por upload, ele é disponibilizado para dados de
armazenamento em seus aplicativos.
{: shortdesc}

Especificamente, é possível executar as tarefas a seguir:

*  [Implementando ou atualizando um modelo preditivo](#deploying-or-refreshing-a-predictive-model)
*  [Recuperando uma lista de todos os modelos atualmente implementados](#retrieving-a-list-of-all-currently-deployed-models)
*  [Fazendo download de uma cópia de um arquivo de modelo implementado específico](#downloading-a-copy-of-a-specific-deployed-model-file)
*  [Excluindo um modelo preditivo implementado](#deleting-a-deployed-predictive-model)

## Implementando ou atualizando um modelo preditivo

Cada
arquivo de modelo recebe um ID de contexto como um alias conveniente a ser usado para
fazer referência ao modelo implementado em chamadas de serviço subsequentes. Se existir um modelo para um ID de contexto, ele será substituído seguindo a chamada `PUT` como um meio de atualizar a análise preditiva em uso por seus aplicativos.

Exemplo de solicitação:

```
PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Parâmetros de solicitação:

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

## Recuperando uma lista de todos os modelos atualmente implementados

Utilize a chamada API a seguir para recuperar um resumo de todos os modelos que
estão atualmente implementados nessa instância de serviço.

Exemplo de solicitação:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}
```
{: codeblock}

Parâmetros de solicitação:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a solicitação para um resumo de modelo implementado for bem-sucedida:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Resposta quando a solicitação para um resumo de modelo implementado falhar:

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

## Fazendo download de uma cópia de um arquivo de modelo implementado específico

Use a chamada API a seguir para fazer download de uma cópia de um arquivo de
modelo implementado específico.

Exemplo de solicitação:

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Parâmetros de solicitação:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a solicitação de download for bem-sucedida:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Resposta quando a solicitação de download falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}

## Excluindo um modelo preditivo implementado

Use a chamada API a seguir para excluir o modelo preditivo da instância de serviço Machine Learning. Depois
de executar essa chamada, o modelo preditivo não ficará mais disponível para
download ou dados de pontuação em seus aplicativos.

Exemplo de solicitação:

```
DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}
```
{: codeblock}

Parâmetros de solicitação:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a remoção de implementação for bem-sucedida:

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

Resposta quando a remoção de implementação falhar:

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

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
