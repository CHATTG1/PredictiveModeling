---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Instalando a interface da linha de comandos (CLI)

Para conduzir experimentos de aprendizado profundo usando o {{site.data.keyword.pm_full}}, primeiro [configure seu ambiente](ml_getting_access.html); em seguida, instale a interface da linha de comandos (CLI) que suporta a criação e o monitoramento de execuções de treinamento.
{: shortdesc}

## Instale a interface da linha de comandos

1.  Instale a [interface da
linha de comandos](http://clis.ng.bluemix.net/ui/home.html), que, como suas instruções explicam, requer que você primeiro
instale a [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html).
2.  Faça download de uma versão do plug-in da CLI na página
[Liberações de CLI de
ML](https://github.ibm.com/NGP-TWC/wml-cli/releases) deste repositório. Verifique se tem a versão correta de plataforma.
3. Instale o plug-in da CLI (por exemplo, no OSX, é transferido por download como ml_cli_plugin_osx)

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Amostra da saída:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  Verifique se o plug-in da CLI está instalado

   ```
   bx ml help
   ```
   {: codeblock}

Amostra da saída:

```
NAME:
   bx ml - Manage machine learning lifecycle on Bluemix
USAGE:
   bx ml command [arguments...] [command options]
COMMANDS:
   show      Get detailed information about models/deployments/trained-models
   train     Start training a model
   delete    Delete a deployment/trained-model
   list      List all models/deployments/frameworks/trained-models
   version   show git hash and build time of cli
   deploy    Deploy a model for scoring
   score     Score the model. Sample scoring json format -  {"modelId": "sample", "deploymentId": "sample","payload": {"fields": [],"values": []}}
   help      Enter 'bx ml help [command]' for more information about a command.
```
{: codeblock}

## Configure a CLI com as informações do seu ambiente

Para usar os outros comandos da CLI, deve-se configurar algumas variáveis de ambiente em seu sistema operacional. Se você estiver trabalhando no Linux, execute os comandos abaixo substituindo os valores para ML_USERNAME e ML_PASSWORD por suas credenciais. Você pode obter os valores necessários seguindo as [instruções para recuperar suas credenciais de acesso](ml_getting_access.html#retrieving-your-credentials) para sua instância do {{site.data.keyword.pm_full}}.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## Iniciar uma execução de treinamento de modelo

Agora você está pronto para
[criar e iniciar suas execuções de
treinamento](ml_dlaas_working_with_new_models.html) usando a CLI. Ou, para aprender mais, você pode
[começar a treinar um modelo de rede
neural de amostra](ml_dlaas_working_with_sample_models.html).

### Upgrade da CLI

Marque [Liberações de
CLI de ML](https://github.ibm.com/NGP-TWC/wml-cli/releases) para novas versões da CLI de ML. Encorajamos você a fazer download da
liberação mais recente da CLI para ter os recursos mais atualizados. Depois de fazer download da versão mais recente da CLI de ML para seu sistema operacional, desinstale o plug-in (se já tiver uma versão mais antiga instalada) e, em seguida, instale a nova versão.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Amostra da saída:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Instale a nova versão usando o comando a seguir:

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Amostra da saída:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
