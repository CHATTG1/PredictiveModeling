---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Implementando modelos on-line

Para implementar um modelo e gerar novamente a análise preditiva fazendo
solicitações de escore com relação ao modelo implementado, utilize o serviço
{{site.data.keyword.pm_full}}. O cenário a seguir fornece um exemplo de como
fazer isso.
{: shortdesc}

**Nome do cenário**: predição de linha de produto.

**Descrição do cenário**: uma empresa que vende equipamento de outdoor
quer criar um modelo que prediga o interesse do cliente em sua
linha de produto. Um
cientista de dados preparou um modelo preditivo e o compartilha com você (o
desenvolvedor). Sua tarefa é implementar o modelo e gerar previsões de interesse do
cliente, fazendo solicitações de escore com relação ao modelo implementado.

## Usando o modelo de amostra

1. Acesse a guia **Amostras** do
{{site.data.keyword.pm_full}}
Painel.
2. Na seção **Modelos de amostra**, localize o quadrado **Predição de linha de produto** e clique no ícone **Incluir modelo** (+).

O modelo de **Previsão de linha de produto** de amostra aparece na lista de modelos
disponíveis na guia **Modelos**.


## Criando a implementação on-line

1. Acesse a guia **Modelos** do Painel do {{site.data.keyword.pm_full}}.
2. No menu **Ações**, clique em **Criar implementação**.
3. No formulário **Criar implementação**, preencha os
campos **Nome**, **Descrição** e
**Tipo on-line**.
4. Clique em **Salvar**.

A implementação on-line aparece na lista de implementações disponíveis na guia
**Implementações**.

## Obtendo detalhes da implementação

É possível verificar o status, o endereço de terminal de pontuação (`Scoring Endpoint`)
e parâmetros relacionados ao modelo implementado.

1. Acesse a guia **Implementações** do {{site.data.keyword.pm_full}} Dashboard.
2. No menu **Ações**, clique em **Visualizar detalhes**.

Anote o valor `Scoring Endpoint`, que será necessário para fazer solicitações de escore.


## Fazendo solicitações de escore

Depois de criar um terminal de pontuação, é possível gerar predições fazendo
solicitações de escore. No cenário a seguir, os registros do cliente são passados para o
modelo preditivo e uma previsão de produto de esporte é retornada.

Cabeçalho de registro de amostra:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Registro de amostra:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Exemplo de solicitação:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Exemplo de saída:

```
{    "fields": ["GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

Podemos ver, por exemplo, que um executivo de 55 anos está
interessado em equipamento de montanhismo, enquanto um estudante de
23 anos está interessado em acessórios pessoais.

## Saiba mais

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM® Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
