---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sistema de aprendizado contínuo

O serviço {{site.data.keyword.pm_full}} inclui um sistema de aprendizado
contínuo. Os sistemas de aprendizado contínuo fornecem monitoramento automatizado de
desempenho, novo treinamento e reimplementação do modelo para assegurar a qualidade da
predição.
{: shortdesc}

**Nome do cenário**: bons medicamentos para seleção de tratamento cardíaco.

**Descrição do cenário**: uma empresa biomédica que produz remédios para o coração
deseja implementar um modelo que seleciona o melhor medicamento para tratamento cardíaco. Quando surgem novas evidências sobre a eficácia do medicamento, uma atualização de modelo deve ser considerada. Um
cientista de dados preparou um modelo preditivo e o compartilha com você (o
desenvolvedor). Sua tarefa é implementar o modelo, definir a configuração do aprendizado
e executar a iteração para avaliar, treinar novamente e reimplementar o modelo.

**Nota**: também é possível utilizar
[anotações
de amostra python](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325), que criam um modelo de amostra, configuram o
sistema de aprendizado e executam iteração de aprendizado.

## Pré-requisitos

Para trabalhar com as amostras, deve-se ter os seguintes serviços:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) para armazenar dados de feedback
* Credenciais da instância de serviço do [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). É possível usar [este link](https://console.bluemix.net/catalog/services/apache-spark) para criar um.

## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o ladrilho da Seleção de
Medicamento do Coração e clique no ícone **Incluir modelo** (+).

O modelo de amostra **Heart Drug Selection** aparece na lista de modelos disponíveis na guia **Modelos**.

## Gerando o token de acesso

Gere um token de acesso que utilize o usuário e a senha disponíveis na guia
Credenciais de serviço da instância de serviço do {{site.data.keyword.pm_full}}.

Exemplo de solicitação:

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
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

Utilize a chamada de API a seguir para obter detalhes de sua instância, como:

* endereço `url` de modelos publicados
* endereço `url` de implementações
* informações de uso

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Exemplo de saída:

```
{
   "metadata" : {
      "guid":"360c510b-012c-4793-ae3f-063410081c3e",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e",
      "created_at":"2017-08-04T09:15:48.344Z",
      "modified_at":"2017-08-22T08:34:47.759Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models"
      },
      "usage":{
         "expiration_date":"2017-09-01T00:00:00.000Z",
         "computation_time":{
            "limit":18000,
            "current":4
         },
         "model_count":{
            "limit":200,
            "current":2
         },
         "prediction_count":{
            "limit":5000,
            "current":16
         },
         "deployment_count":{
            "limit":5,
            "current":1
         }
      },
      "plan_id":"0f2a3c2c-456b-40f3-9b19-726d2740b11c",
      "status":"Active",
      "organization_guid":"b0e61605-a82e-4f03-9e9f-2767973c084d",
      "region":"us-south",
      "account":{
         "id":"f52968f3dbbe7b0b53e15743d45e5e90",
         "name":"Umit Cakmak's Account",
         "type":"TRIAL"
      },
      "owner":{
         "ibm_id":"31000292EV",
         "email":"umit.cakmak@pl.ibm.com",
         "user_id":"43e0ee0e-6bfb-48fc-bcd8-d61e40d19253",
         "country_code":"POL",
         "beta_user":true
      },
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/deployments"
      },
      "space_guid":"4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "plan":"standard"
   }
}
```
{: codeblock}

Depois de obter o endereço **published_models**
`url`, use a chamada API a seguir para obter os detalhes do modelo:

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

Exemplo de saída:

```
{  
   "count":1,
   "resources":[  
      {  
         "metadata":{  
      "guid":"361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d",
            "created_at":"2017-08-22T08:34:47.597Z",
            "modified_at":"2017-08-22T08:34:47.691Z"
         },
   "entity":{  
      "runtime_environment":"spark-2.0",
            "author":{  
               "name":"IBM",
            "email":""
            },
            "name":"Best Heart Drug Selection",
            "label_col":"DRUG",
            "training_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"DRUG"
                     },
                     "type":"string",
                     "name":"DRUG",
                     "nullable":true
                  }
               ],
               "type":"struct"
            },
            "latest_version":{  
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "guid":"8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3",
               "created_at":"2017-08-22T08:34:47.691Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{  
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/360c510b-012c-4793-ae3f-063410081c3e/published_models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/deployments"
            },
            "input_data_schema":{  
               "fields":[  
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"AGE"
                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"SEX"
                     },
                     "type":"string",
                     "name":"SEX",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"BP"
                     },
                     "type":"string",
                     "name":"BP",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":0,
                        "name":"CHOLESTEROL"
                     },
                     "type":"string",
                     "name":"CHOLESTEROL",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"NA"
                     },
                     "type":"decimal(12,6)",
                     "name":"NA",
                     "nullable":true
                  },
                  {  
                     "metadata":{  
      "scale":6,
                        "name":"K"
                     },
                     "type":"decimal(13,6)",
                     "name":"K",
                     "nullable":true
                  }
               ],
               "type":"struct"
            }
         }
      }
}
```
{: codeblock}


## Configure o sistema de aprendizado contínuo para um modelo publicado

Para configurar o sistema de aprendizado contínuo para seu modelo, execute as tarefas a seguir:

1.  [Preparar o cabeçalho de autorização](#prepare-the-authorization-header).
2.  [Preparar o conjunto de dados de feedback](#prepare-the-feedback-data-set).
3.  [Preparar a carga útil de configuração](#prepare-the-configuration-payload).

Depois de concluir a preparação do cabeçalho de autorização, o conjunto de dados de
feedback e a carga útil de configuração, você poderá iniciar a iteração de seu sistema de
aprendizado contínuo. Para obter mais informações, consulte
[Executar iteração do sistema de
aprendizado contínuo](#run-continuous-learning-system-iteration).

### Preparar o cabeçalho de autorização

Para preparar o cabeçalho de autorização que combina as credenciais de token do
{{site.data.keyword.pm_full}} e de instância do Spark, forneça os detalhes a
seguir:

*  O token de acesso criado na etapa anterior
*  Credenciais de serviço do Spark, que podem ser localizadas na guia Credenciais de serviço do painel do serviço {{site.data.keyword.Bluemix_notm}} Spark. Antes
de fazer a solicitação de implementação, as credenciais do Spark devem ser
codificadas como base64. Elas são passadas no cabeçalho da solicitação
no campo X-Spark-Service-Instance.

   Dependendo do sistema operacional que você está utilizando, um dos seguintes
comandos de terminal deve ser emitido para executar codificação base64 e designá-la à variável de ambiente.

   No sistema operacional **macOS**, use o comando a seguir:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   Nos sistemas operacionais **Microsoft Windows** ou
**Linux**, deve-se usar o parâmetro `--wrap=0` com o
comando `base64` para executar a codificação base64:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### Preparar o conjunto de dados de feedback

O sistema de aprendizado requer uma conexão com os dados de treinamento (os dados
que são usados no treinamento de modelo), bem como os dados de feedback. O armazenamento de dados de feedback é usado para monitorar e treinar novamente seu modelo quando necessário. {{site.data.keyword.dashdbshort}} é suportado como um armazenamento de dados de feedback. A
tabela de feedback é gerenciada, modificada e usada pelo serviço {{site.data.keyword.pm_short}}.
Para preparar a tabela **DRUG_FEEDBACK_DATA** no
**{{site.data.keyword.dashdbshort}}**, deve-se concluir as
etapas a seguir:

1. Crie uma instância de serviço do [{{site.data.keyword.dashdbshort}}](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (um plano de entrada é oferecido).
2. Crie a tabela `DRUG_FEEDBACK_DATA` no **{{site.data.keyword.dashdbshort}}**.
   1. No repositório GitHub, faça download do arquivo [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv).
   2. Para dar início ao
**{{site.data.keyword.dashdbshort_notm}}**, clique no ícone
**Abrir o console**.
   3. Selecione **Carregar dados** e o tipo de carregamento **Área de
trabalho**.
   4. **Arraste** o arquivo transferido por download anteriormente e pressione **Avançar**.
   5. Para importar dados, clique em **Esquema** e, em seguida, clique em **Nova tabela**.
   6. No campo **Nova tabela**, digite um nome e clique em
**Avançar**.
   7. Use um ponto e vírgula (;) para o **Separador de campo**.
   8. Clique em **Avançar** para criar a tabela com os dados transferidos por upload.

**Nota**: você pode incluir novos registros de feedback no banco de dados de feedback usando este
[terminal
de API de REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback). Para obter mais informações, consulte a
[seção Armazenamento de dados de feedback](#feedback-data-store).

### Preparar a carga útil de configuração

Para especificar a configuração de aprendizado, use o terminal a seguir:

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Para finalizar a carga útil, defina os valores dos parâmetros a seguir:

<dl><dt>feedback_data_reference</dt>
<dd>conexão e origem de armazenamento de dados de feedback</dd>
<dt>min_feedback_data_size</dt>
<dd>número mínimo de registros no conjunto de dados de feedback para iniciar a iteração do sistema de aprendizado contínuo
</dd>
<dt>auto_retrain</dt>
<dd>[nunca, sempre, condicionalmente] especifica quando o processo de novo treinamento é
acionado. Quando configurado como [condicionalmente], aciona o processo de novo
treinamento quando a qualidade do modelo é menor que o valor do limite especificado.
</dd>
<dt>auto_redeploy</dt>
<dd>[nunca, sempre, condicionalmente] especifica quando o modelo treinado novamente deve
ser implementado. Quando configurado como [condicionalmente], aciona a reimplementação do
modelo quando a qualidade do modelo recém-treinado é melhor que a qualidade do modelo
implementado atualmente.</dd></dl>

Exemplo de solicitação:

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
         "definition": {
           "method": "multiclass", "metrics": [ {
               "name": "accuracy", "threshold": 0.8 }
           ]
         },
         "feedback_data_reference": {
           "connection": {
            "db": "BLUDB",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "username": "***",
            "password": "***"
           },
    "source": {
            "tablename": "DRUG_FEEDBACK_DATA",
            "type": "dashdb"
           }
          },
          "min_feedback_data_size": 10,
          "auto_retrain": "conditionally",
          "auto_redeploy": "never",
          "last_training_record": 0
        }' \
    https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Exemplo de saída:

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass", "metrics": [ {
        "name": "accuracy", "threshold": 0.8 }
    ]
   },
   "feedback_data_reference":{
      "connection": {
         "db":"BLUDB",
         "host":"awh-yp-small02.services.dal.bluemix.net",
         "username":"***",
         "password":"***"
      },
      "source":{
         "tablename":"DRUG_FEEDBACK_DATA",
         "type":"dashdb"
      }
   },
   "min_feedback_data_size":10,
   "spark_service":{
      "credentials": {
         "tenant_id":"***",
         "cluster_master_url":"https://spark.bluemix.net",
         "tenant_id_full":"***",
         "tenant_secret":"***",
         "instance_id":"***",
         "plan":"ibm.SparkService.PayGoPersonal"
      },
         "version":"2.0"
   },
   "training_data_reference": {
   "connection": {
     "db": "BLUDB",
     "host": "dashdb-entry-yp-dal09-08.services.dal.bluemix.net",
     "password": "***",
     "username": "***"
   },
    "source": {
     "tablename": "DRUG_TRAIN_DATA_UPDATED",
     "type": "dashdb"
    }
   }
}
```
{: codeblock}

**Nota**: o exemplo usa valores padrão para os parâmetros
`auto_retrain` e `auto_redeploy`. Para obter mais
informações sobre esses parâmetros, consulte a
[Documentação
da API de REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration) para o parâmetro learning_configuration.

## Executar iteração do sistema de aprendizado contínuo

Para iniciar a iteração do sistema de aprendizado, use o método da API de REST a
seguir. Durante iteração, o modelo publicado é avaliado. Se a precisão avaliada for menor
que o valor do limite especificado, o novo treinamento do modelo será acionado. Os
conjuntos de dados de treinamento e de feedback são usados para novo treinamento e avaliação.

Exemplo de solicitação:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Exemplo de saída:

```
A solicitação foi preenchida e resultou em um novo recurso que
está
sendo criado.
```
{: codeblock}

Para verificar o status da iteração, emita a seguinte solicitação:

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Exemplo de saída:

```
{
  "count": 1,
  "resources": [
    {
      "entity":{
        "status": {
          "state": "INITIALIZED"
        },
        "model_version": {
          "url": "https://ibm-watson-ml-svt.stage1.mybluemix.net/v2/artifacts/models/71dc688a-ebda-4174-9574-e8805059e08f/versions/de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d",
          "created_at": "2017-10-30T15:45:12.926Z",
          "guid": "de0df5a6-32c6-408b-bf94-d7b0e3a7fc2d"
        },
      "spark_service":{
          "credentials": {
            "tenant_id_full": "***",
            "tenant_secret": "***",
            "tenant_id": "***",
            "instance_id": "***",
            "plan": "ibm.SparkService.PayGoPersonal",
            "cluster_master_url": "https://spark.bluemix.net"
          },
         "version":"2.0"
        },
        "published_model": {
          "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f",
          "guid": "71dc688a-ebda-4174-9574-e8805059e08f"
        }
      },
      "metadata": {
        "url": "https://ibm-watson-ml.mybluemix.net/v3/wml_instances/ff558a0e-af34-40e9-9f13-2d51b1a4c8bb/published_models/71dc688a-ebda-4174-9574-e8805059e08f/learning_iterations/a308838b-445f-45b8-9fbf-1c3dd1b392c1",
        "created_at": "2017-10-30T15:54:30.657Z",
        "guid": "a308838b-445f-45b8-9fbf-1c3dd1b392c1"
      }
    }
  ]
}
```
{: codeblock}

Também é possível obter a lista de métricas e valores de avaliação emitindo a seguinte solicitação:

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

Exemplo de saída:

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup", "values": [ {
          "name": "accuracy", "value": 0.870968, "threshold": 0.8 }
      ],
      "timestamp": "2017-08-07T10:11:02.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
      {
      "phase": "monitoring",
      "values": [
        {
          "name": "accuracy",
          "value": 0.75,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/8dfe9548-57ec-4768-8d3e-6c1e7e5cdfc3"
    },
    {
      "phase": "setup", "values": [ {
          "name": "accuracy", "value": 0.870968, "threshold": 0.8 }
      ],
      "timestamp": "2017-08-08T10:11:12.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    },    
    {
      "phase": "training",
      "values": [
        {
          "name": "accuracy",
          "value": 0.88281694,
          "threshold": 0.8
        }
      ],
      "timestamp": "2017-08-08T10:11:22.000Z",
      ""model_version_url": "https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/361d0edb-c5c9-4b8c-b353-d968e7dc0b8d/versions/e390cd8a-e043-4da6-b3e8-1d2d02d971fb"
    }
  ]
}
```
{: codeblock}

## Armazenamento de dados de feedback

É possível usar um
[terminal
de feedback](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) para enviar novos registros (registros que não foram usados no processo
de treinamento) para o armazenamento de feedback que é definido na configuração de
aprendizado. O armazenamento de dados de feedback é usado para monitorar e treinar novamente seu modelo quando necessário. {{site.data.keyword.dashdbshort}} é suportado como um armazenamento de dados de feedback. Se a tabela de feedback não existir, o serviço a criará. Se a tabela existir, o esquema será verificado para corresponder com o da tabela de treinamento e uma coluna extra chamada `_training` será anexada à tabela. A coluna adicional é usada para marcar registros que são consumidos no processo de novo treinamento.

**Nota:** a tabela de feedback é gerenciada, modificada e usada
pelo serviço {{site.data.keyword.pm_short}}.

Você pode enviar um novo registro de feedback para o armazenamento de dados de
feedback emitindo a solicitação a seguir:

Exemplo de solicitação:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields": [
      "AGE",
      "SEX",
      "BP",
      "CHOLESTEROL",
      "NA",
      "K",
      "DRUG"
   ],
   "values":[
      [
         
         16,
         "M",
         "HIGH",
         "NORMAL",
         0.58301000000000003,
         0.033884999999999998,
         "drugY"
      ]
   ]
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/feedback
```
{: codeblock}

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
