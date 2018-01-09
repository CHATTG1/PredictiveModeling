---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Implementando modelos em lote

Usando o serviço {{site.data.keyword.pm_full}}, é possível implementar um modelo e gerar análise preditiva criando solicitações de pontuação com relação ao modelo implementado.
{: shortdesc}


**Nome do cenário**: predição de satisfação do cliente.

**Descrição do cenário**: uma empresa de telecomunicações quer saber
quais clientes estão prestes a sair. O modelo apresentado prevê a perda de
clientes. Um cientista de dados desenvolve um modelo preditivo e o compartilha com você
(o desenvolvedor). Sua tarefa é implementar
o modelo e gerar análise preditiva fazendo solicitações de
pontuação em relação ao modelo implementado.

**Nota:** Você também pode utilizar as
[anotações](https://apsportal.ibm.com/analytics/notebooks/5e4963d9-faea-455d-a7db-ff6302d1d8f5/view?access_token=5d23d36be72dea35ebbde9b4b5f4a16d0053ee898f1ab2ab73cf1301ce9322be)
de amostra python, que criam modelo de amostra e predizem a perda de clientes.

## Pré-requisitos

Para trabalhar com esse exemplo, deve-se ter os seguintes recursos:

* Detalhes da instância de [Armazenamento de objetos](https://console.bluemix.net/catalog/services/object-storage), que são usados como entrada (dados do cliente para pontuação) para o modelo e armazenamento para a saída do modelo. O
arquivo .csv de dados de entrada de amostra pode ser transferido por download [aqui](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv). É necessário incluir o arquivo de entrada em sua instância de armazenamento de objetos.
* Credenciais da instância de serviço do [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). É possível usar [este link](https://console.bluemix.net/catalog/services/apache-spark) para criar um.



## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o quadro
**Predição da satisfação do cliente** e clique no ícone (+)
**Incluir modelo**.

O modelo de previsão de satisfação do cliente aparece na lista de modelos disponíveis na guia **Modelos**.

## Gerando o token de acesso

Gere um token de acesso utilizando o usuário e a senha disponíveis na guia
Credenciais de serviço da instância de serviço do {{site.data.keyword.pm_full}}.

Exemplo de solicitação:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Exemplo de saída:

```
{"token":"**********"}
```
{: codeblock}

Use o comando do terminal a seguir para designar seu valor do token para o token de variável de ambiente:

```
token="<token_value>"
```
{: codeblock}

## Trabalhando com modelos publicados

Use a chamada API a seguir para obter os detalhes da instância, que incluem os
itens a seguir:

* Valor da `url` dos modelos publicados
* Valor da `url` das implementações
* informações de uso

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Exemplo de saída:

```
{
   "metadata" : {
      "guid":"87452a37-6a8f-4d59-bf88-59c66b5463e4",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
      "created_at":"2017-06-23T08:31:52.026Z",
      "modified_at":"2017-06-23T08:31:52.026Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
      },
      "usage":{ },
      "plan_id":"5325f63a-683a-47f0-a04e-97e371385588",
      "account_id":"b56398ea52f470c3173f4cf3bef5cc7e",
      "status":"Active",
      "organization_guid":"3e658178-a60c-48b8-8be9-bf58cc821656",
      "region":"us-south",
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}}/deployments"
      }, "space_guid":"c3ea6205-b895-48ad-bb55-6786bc712c24", "plan":"lite"
   }
}
```
{: codeblock}

Ao fornecer o valor **published_models** `url`,
é possível usar a chamada API a seguir para obter os detalhes do modelo:

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

Exemplo de saída:

```
{
   "count":1,
   "resources":[
      {
         "metadata" : {
            "guid":"dc46315a-c30e-46a3-8e30-33518e6f7976",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976",
            "created_at":"2017-03-21T13:49:38.711Z",
            "modified_at":"2017-03-21T13:49:38.802Z"
         },
   "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
            "email":""
            },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts Telco customer churn.",
            "label_col":"Churn",
            "training_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Churn",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/dc46315a-c30e-46a3-8e30-33518e6f7976/versions/658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "guid":"658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "created_at":"2017-03-21T13:49:38.802Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976/deployments"
            },
            "input_data_schema":{
               "type": "struct",
    "fields": [
      {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata" : {

                     },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  }
               ]
            }
         }
      }
   ]
}
```
{: codeblock}

Observe que o valor da `url` de **implementações** que é necessário para criar a implementação em lote a seguir.

## Criando uma implementação em lote com o Object Storage

Para usar uma chamada API REST para criar uma implementação
em lote do seu modelo preditivo, forneça os seguintes detalhes:

*  O token de acesso criado na etapa anterior
*  Credenciais de serviço do Spark, que podem ser localizadas na guia Credenciais de serviço do painel do serviço {{site.data.keyword.Bluemix_notm}} Spark. 
Antes de fazer a solicitação de implementação, as credenciais do Spark devem ser
decodificadas como base64 e passadas no cabeçalho de uma solicitação
`curL` como X-Spark-Service-Instance.

   Dependendo do sistema operacional que você está usando, deve-se emitir um dos comandos de terminal a seguir para executar a decodificação base64 e designá-la à variável de ambiente.

   No sistema operacional **macOS**, use o comando a seguir:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Nos sistemas operacionais **Microsoft Windows** ou **Linux**, deve-se usar o parâmetro `--wrap=0` com o comando `base64` para executar a decodificação de base64:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

*  Detalhes do Object Storage, que serão usados como entrada (dados do
cliente para pontuação) para o modelo e armazenamento para a saída do modelo
(results.csv neste caso, que é criado automaticamente).
*  Para criar uma implementação, use o valor `url` de **implementações** da seção anterior.


Exemplo de solicitação:

```
curl -v -XPOST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"Customer Satisfaction Prediction",
      "type": "batch",
      "description": "Batch Deployment",
       "input":{
          "source":{
             "container":"batchjob",
             "filename":"TelcoCustomerData.csv",
             "fileformat":"csv",
             "inferschema":"1",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       },
       "output":{
          "target":{
             "container":"batchjob",
             "filename":"result.csv",
             "fileformat":"csv",
             "writemode":"write",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b283cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       }
    }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

Exemplo de saída:

```
{
   "metadata" : {
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

**Nota**: também é possível usar o Painel para criar uma implementação de
lote.

## Obtendo detalhes da implementação

É possível verificar o status e os parâmetros relacionados ao modelo de implementação usando o valor `url` de **metadados**.
Exemplo de solicitação:

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemplo de saída:

```
{
   "metadata" : {
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"COMPLETED",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

O resultado da predição é salvo em um arquivo .csv no IBM Object Storage. Consulte a linha de amostra a seguir para obter um exemplo da saída.

Visualização de arquivo de entrada:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Visualização de arquivo de saída:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}

## Excluindo uma implementação em lote

Para excluir a implementação, use a consulta a seguir:

Exemplo de solicitação:

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemplo de saída:

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
