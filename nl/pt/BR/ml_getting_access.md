---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurando o Ambiente

Para usar o {{site.data.keyword.pm_full}}, deve-se estar apto a criar o ambiente de aprendizado de máquina adequado e recuperar as credenciais que são específicas desse ambiente.Você pode criar a instância do {{site.data.keyword.Bluemix}} usando a
[interface da linha de comandos do Cloud Foundry](https://github.com/cloudfoundry/cli#getting-started) ou por meio da interface gráfica com o usuário no do {{site.data.keyword.Bluemix}} Dashboard.
{: shortdesc}

## Usando o IBM Cloud Dashboard

Para usar o {{site.data.keyword.pm_full}}, deve-se
[criar uma
instância](https://console.bluemix.net/catalog/services/machine-learning) dele em seu {{site.data.keyword.Bluemix_notm}} Dashboard.

Dependendo de seu cenário, você também pode precisar dos seguintes recursos:

- Armazenamento de objeto (implementação em lote)
- Db2 Warehouse on Cloud (implementação em lote)
- Apache Spark (implementação em lote, de fluxo e sistema de aprendizado contínuo)
- Hub de mensagens (implementação de fluxo)

Para ver como trabalhar com o Painel para criar as instâncias do
{{site.data.keyword.Bluemix_notm}} e visualizar as credenciais, assista ao vídeo
a seguir, que complementa as etapas escritas, que se seguem ao vídeo.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Se não for possível acessar o vídeo que está integrado nesta página, será possível acessá-lo no website do YouTube. (Abre em uma nova guia ou janela)"> <img src="images/video.png" alt="Ícone de vídeo"></a>IBM Watson Machine Learning: introdução</span></figcaption>
<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Este vídeo fornece uma visão geral de como configurar seu ambiente de aprendizado de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## Criando instâncias do {{site.data.keyword.Bluemix_notm}}

Para criar a instância do {{site.data.keyword.pm_full}}, execute as etapas a seguir:

1. Abra a [página de serviço](https://console.bluemix.net/catalog/services/machine-learning) do {{site.data.keyword.pm_full}}.
2. Inscreva-se ou efetue login para criar a instância de serviço.
3. Insira um nome descritivo para sua instância, escolha um espaço e selecione seu plano de dados.
4. Clique em **Criar instância**.

Sua instância é aberta.

## Recuperando suas credenciais

Para usar sua instância do {{site.data.keyword.Bluemix_notm}} (anteriormente
conhecido como Bluemix), você precisa das credenciais da instância de serviço. É possível
criar e acessar suas credenciais por meio da [CLI do
CF](using_pm_service.html) ou do {{site.data.keyword.Bluemix_notm}} Dashboard. Depois de ligar a
instância de serviço do {{site.data.keyword.pm_short}} ao seu aplicativo
{{site.data.keyword.Bluemix_notm}}, as credenciais do
{{site.data.keyword.pm_short}} são incluídas na variável de ambiente
`VCAP_SERVICES`. Para obter mais informações, consulte
[usando o serviço com o aplicativo](using_pm_service.html).

Para recuperar as credenciais da instância do {{site.data.keyword.pm_full}}, execute as etapas a seguir:

1. [Efetue login no {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Na seção **Serviços**, clique no nome do serviço cujas credenciais você deseja recuperar.
3. Na área de janela de navegação, clique em **Credenciais de serviço**.
4. Na lista de credenciais, clique em **Visualizar credenciais**.
5. Se nenhuma credencial existir para esse serviço, crie-as clicando em **Criar credenciais**.
6. Para copiar as credenciais, clique no ícone Copiar.

Para usar o serviço do {{site.data.keyword.pm_short}} em seu aplicativo,
deve-se ligar o serviço ao aplicativo {{site.data.keyword.Bluemix_notm}} e as
credenciais necessárias para executar o aplicativo são inseridas na variável ambiental
`VCAP_SERVICES`. Para obter mais informações, consulte
[Interface da linha de comandos do
Cloud Foundry](#cloud-foundry-command-line-interface).

## Usando a CLI (interface da linha de comandos) do Cloud Foundry

### Etapas para ligar o serviço com o aplicativo {{site.data.keyword.Bluemix_notm}}

É possível fazer download do
[código de
amostra](https://github.com/pmservice/product-line-prediction/blob/master/README.md) do Node.js para tentar o serviço do Machine Learning. Conclua as etapas a
seguir para criar seu aplicativo {{site.data.keyword.Bluemix_notm}} e ligar o
serviço {{site.data.keyword.pm_short}}. Estes exemplos usam Node.js, por ele ser um tempo
de execução popular. Outros podem ser usados, como iOS, Ruby, Perl ou Java.

Você também pode executar as etapas de 1 a 3 usando a interface gráfica do
{{site.data.keyword.Bluemix_notm}} em vez da ferramenta Cloud
Foundry (cf).

1. Use o comando `cf create-service` para criar uma instância de serviço:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Por exemplo, o comando a seguir cria uma instância de serviço do
{{site.data.keyword.pm_short}} com o plano Lite chamado
`my_wml_lite` em seu espaço do {{site.data.keyword.Bluemix_notm}}:

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Use o comando `cf create-service-key` para criar credenciais de serviço:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Por exemplo, o comando a seguir cria as credenciais de serviço do
{{site.data.keyword.pm_short}}:

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. Use o comando `cf bind-service` para ligar a instância de
serviço `my_wml_lite` ao seu aplicativo.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   Por exemplo, o comando a seguir liga a instância de serviço do
{{site.data.keyword.pm_short}} `my_wml_lite` ao aplicativo
{{site.data.keyword.Bluemix_notm}} `my_app1`:

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Acessando credenciais por meio da variável `VCAP_SERVICES`

Depois de ligar a instância de serviço do {{site.data.keyword.pm_short}} ao
seu aplicativo {{site.data.keyword.Bluemix}}, as credenciais do
{{site.data.keyword.pm_short}} são incluídas na variável de ambiente
`VCAP_SERVICES`:

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   A variável de ambiente `VCAP_SERVICES` inclui as informações a seguir:

<dl>
<dt>plano</dt>
<dd>o plano do {{site.data.keyword.pm_short}} que é usado no fornecimento de serviço.</dd>
<dt>url</dt><dd>o endereço da instância de serviço do {{site.data.keyword.pm_short}}.
<dt>access_key</dt><dd>somente para fluxos do IBM® SPSS® Modeler.</dd>
<dt>instance_id</dt><dd>identificador exclusivo da instância do {{site.data.keyword.pm_short}}.</dd>
<dt>username</dt><dd>o nome do usuário, que faz parte da autorização básica que é necessária para gerar um token de acesso a ser passado em todas as solicitações para essa instância de serviço. </dd>
<dt>password</dt><dd>a senha, que faz parte da autorização básica que é necessária para gerar um token de acesso a ser passado em todas as solicitações para essa instância de serviço. </dd>
</dl>

Por exemplo, é possível gerar um token de acesso usando o seguinte comando `curl`:

Exemplo de solicitação:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Output example:

       {"token":"**********"}
```
{: codeblock}

   O código do Node.js a seguir é um exemplo de como obter os valores
`username` e `password` da variável de ambiente
`VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
