---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de REST (novo) <span class='tag--beta'>Beta</span>

Para fornecer uma única experiência do usuário, o serviço
{{site.data.keyword.pm_full}} fornece uma
[API de REST](http://watson-ml-api.mybluemix.net/) que é compatível com
outras estruturas suportados. É possível usar as mesmas solicitações de API para modelos
SPSS® e para outras estruturas, como scikit-learn, XGboost ou Spark MLlib.
{: shortdesc}

**Nota**: este recurso está em beta. Se estiver interessado em participar, inclua-se na lista de espera! Para obter mais informações, veja: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

Usando a API de REST do serviço {{site.data.keyword.pm_full}}, é possível
criar a implementação on-line e gerar a análise preditiva fazendo solicitações de escore
com relação ao modelo implementado. Para se familiarizar com a API de REST, use o
seguinte cenário de satisfação do cliente e as
[anotações
de amostra do Jupyter](https://dataplatform.ibm.com/analytics/notebooks/3c2ef65c-7d0e-4ff7-ab11-578ef2a46d66/view?access_token=21517d9a2f59b1ff5e386982b3fa03d7b41cff53a22bd30b5dc7785535b0b80d) que usam comandos bash para executar o
cenário.

**Nome do cenário**: predição de satisfação do cliente

**Descrição do cenário**: uma empresa de telecomunicações quer saber
quais clientes estão prestes a sair. O modelo apresentado prevê a perda de
clientes. Um cientista de dados desenvolve um modelo preditivo e o compartilha com você
(o desenvolvedor). Sua tarefa é implementar
o modelo e gerar análise preditiva fazendo solicitações de
pontuação em relação ao modelo implementado.

## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o quadro
**Previsão da satisfação do cliente (SPSS MODEL)** e clique
no ícone (+) **Incluir modelo**.

O modelo de previsão de satisfação do cliente aparece na lista de modelos disponíveis na guia **Modelos**.

## Gerando o token de acesso

Gere um token de acesso usando o nome de usuário e a senha disponíveis na guia
Credenciais de serviço da instância de serviço do IBM Watson Machine Learning.

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

Use a chamada API a seguir para obter detalhes da sua instância, que incluem os valores a seguir:

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

Forneça **published_models** `url` para
executar a chamada API a seguir para obter os detalhes do modelo:

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
            "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8",
            "created_at":"2017-11-06T11:34:17.991Z",
            "modified_at":"2017-11-06T11:34:18.227Z"
         },
   "entity":{
            "runtime_environment":"spss-modeler-17.0",
            "learning_configuration_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_configuration",
            "author":{
               "name":"IBM",
            "email":""
            },
            "name":"Predição da satisfação do cliente",
            "description":"Prediz a satisfação do cliente no modelo de perda de clientes de telecomunicações",
            "label_col":"Perda de clientes",
            "learning_iterations_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/learning_iterations",
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
            "feedback_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/feedback",
            "latest_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb",
               "created_at":"2017-11-06T11:34:18.227Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments"
            },
            "evaluation_metrics_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/evaluation_metrics",
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

Anote a `url` de **implementações** que é necessária para criar a implementação on-line na etapa a seguir.

## Criando a implementação on-line

Use a chamada API a seguir para criar uma implementação
on-line do seu modelo preditivo.

Exemplo de solicitação:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" -d '{"name": "Predição da satisfação do cliente", "description": "Implementação da predição da satisfação do cliente", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


Exemplo de saída:

```
{
   "metadata" : {
      "guid":"1466867a-99aa-4708-a22c-f4db06caacf8", "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8", "created_at":"2017-11-06T12:03:05.080Z", "modified_at":"2017-11-06T12:04:10.766Z"
   },
   "entity":{
      "runtime_environment":"spss-modeler-17.0", "name":"Customer Satisfaction Deployment", "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online", "description":"My 1st Deployment", "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         }, "name":"Customer Satisfaction Prediction", "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8", "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8", "description":"Predicts customer satisfaction in telco churn model", "created_at":"2017-11-06T12:01:04.394Z"
      },
      "model_type":"spss-modeler-model-17.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb", "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb", "created_at":"2017-11-06T11:34:18.227Z" }
   }
}
```
{: codeblock}

## Obtendo detalhes da implementação

É possível verificar o status, o endereço de terminal de pontuação (`scoring_url`) e
parâmetros relacionados ao modelo implementado.

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Exemplo de saída:

```
{
   "count":1,
   "resources":[
      {
         "metadata" : {
            "guid":"1466867a-99aa-4708-a22c-f4db06caacf8", "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8", "created_at":"2017-11-06T12:03:05.080Z", "modified_at":"2017-11-06T12:04:10.766Z"
         },
   "entity":{
            "runtime_environment":"spss-modeler-17.0", "name":"Customer Satisfaction Deployment", "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/deployments/1466867a-99aa-4708-a22c-f4db06caacf8/online", "description":"My 1st Deployment", "published_model":{
               "author": {
                  "name":"IBM",
            "email":""
               }, "name":"Customer Satisfaction Prediction", "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/74806c17-7e0d-4824-a6d6-e0a07a82e979/published_models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8", "guid":"c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8", "description":"Predicts customer satisfaction in telco churn model", "created_at":"2017-11-06T12:01:04.394Z"
            },
            "model_type":"spss-modeler-model-17.0",
            "status":"ACTIVE",
            "type":"online",
            "deployed_version":{
               "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/c6ee8ec5-51ad-443a-92aa-2ac9a3ddb3b8/versions/3e4c768e-1346-4fc5-9e75-7e277b858fbb", "guid":"3e4c768e-1346-4fc5-9e75-7e277b858fbb", "created_at":"2017-11-06T11:34:18.227Z" }
         }
      }
   ]
}
```
{: codeblock}

## Fazendo solicitações de escore

Como seu terminal de pontuação foi criado (`scoring_url`), agora
é possível gerar predições fazendo solicitações de pontuação. No cenário a seguir, os registros do cliente são passados para o modelo preditivo e a
predição da satisfação é retornada.

Cabeçalho de registro de amostra:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges
```
{: codeblock}


Registro do cliente de amostra:

```
"3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768
```
{: codeblock}


Exemplo de solicitação:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"values":[["3638-WEABW","Female",0,"Yes","No",58,"Yes","Yes","DSL","No","Yes","No","Yes","No","No","Two year","Yes","Credit card (automatic)",59.9,3505.1,"No",2.768]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Exemplo de saída:

```
{
   "fields": [
      "customerID",
      "Churn",
      "Predicted Churn",
      "Probability of Churn"
   ],
   "values":[
      [
         
         "3638-WEABW",
         "No",
         "No",
         0.0526309571556145
      ]
   ]
}

```
{: codeblock}

No resultado, você pode ver que esse cliente está satisfeito.

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM® Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
