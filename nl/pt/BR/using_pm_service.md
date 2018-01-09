---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o serviço

Usando os métodos de modelagem na paleta de modelagem do IBM® SPSS® Modeler, você
pode derivar novas informações de seus dados e desenvolver modelos preditivos. Cada
método possui certas forças e é mais adequado para certos tipos de problemas de
aprendizado de máquina.
{: .shortdesc}

Para obter detalhes sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que
ele fornece, consulte [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Depois que os requisitos de entrada e saída de seu aplicativo
{{site.data.keyword.Bluemix_notm}} e design de ramificação de escoragem do IBM®
SPSS® Modeler são implementados, seu analista de dados pode mudar qualquer
aspecto interno da ramificação de escoragem. O
Data Analyst pode até alterar o(s) algoritmo(s) de modelo usado(s) em uma operação de atualização,
assegurando sua capacidade de realizar ajuste fino de suas análises preditivas
sem precisar regravar seus aplicativos.


## Etapas para ligar o serviço com o aplicativo {{site.data.keyword.Bluemix_notm}}

Conclua as etapas a seguir para criar seu aplicativo
{{site.data.keyword.Bluemix_notm}} e ligá-lo ao serviço.
{{site.data.keyword.pm_short}}

1. Faça download do código do aplicativo de amostra Node.js no
[repositório GitHub](https://github.com/pmservice/customer-satisfaction-prediction).

2. Use o comando `cf create-service` para criar uma instância de serviço:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Por exemplo:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   Esse comando cria uma instância de serviço do
{{site.data.keyword.pm_short}} com o plano Lite chamado my_pm_lite em seu espaço
do {{site.data.keyword.Bluemix_notm}}.

3. Use o comando `cf create-service-key` para criar credenciais de serviço:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Por exemplo:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Esse comando cria as credenciais de serviço do {{site.data.keyword.pm_short}}.

4. Use o comando `cf bind-service` para ligar a instância de
serviço `my_pm_lite` ao seu aplicativo.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   Por exemplo:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   Esse comando liga a instância de serviço do {{site.data.keyword.pm_short}}
`my_pm_lite` ao aplicativo {{site.data.keyword.Bluemix_notm}} my_app1.

5. Credenciais {{site.data.keyword.pm_short}}:

   Depois de ligar a instância de serviço do {{site.data.keyword.pm_short}}
ao seu aplicativo {{site.data.keyword.Bluemix_notm}}, as credenciais do
{{site.data.keyword.pm_short}} são incluídas na variável de ambiente
`VCAP_SERVICES`:

```
    {   
        "pm-20": {      
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   A variável de ambiente `VCAP_SERVICES` inclui as informações a seguir:

   <dl>

   <dt>plano</dt>
   <dd>O plano do {{site.data.keyword.pm_short}} que é usado no fornecimento de serviço.</dd>

   <dt>url</dt>
   <dd>O endereço da instância de serviço do {{site.data.keyword.pm_short}}.</dd>

   <dt>access_key</dt>
   <dd>O parâmetro de consulta accessKey a ser passado em todas as solicitações
para essa instância de serviço.</dd>

   </dl>

Por exemplo:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   O código de exemplo Node.js que mostra como obter a accessKey
da variável de ambiente `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
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
