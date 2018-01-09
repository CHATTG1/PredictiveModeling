---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Instalación de la interfaz de línea de mandatos (CLI)

Para realizar experimentos de aprendizaje profundo mediante {{site.data.keyword.pm_full}}, primero [configure el entorno](ml_getting_access.html) y, a continuación, instale la interfaz de línea de mandatos (CLI) que da soporte a la creación y la supervisión de ejecuciones de entrenamiento.
{: shortdesc}

## Instalar la interfaz de línea de mandatos

1.  Instale la [interfaz de línea de mandatos](http://clis.ng.bluemix.net/ui/home.html), que, como explican sus instrucciones, requiere que instale en primer lugar la [CLI](https://console.stage1.ng.bluemix.net/docs/starters/install_cli.html).
2.  Descargue una versión del plug-in de CLI desde la página [Releases de CLI de ML](https://github.ibm.com/NGP-TWC/wml-cli/releases) de este repositorio. Asegúrese de obtener la versión correcta para su plataforma.
3. Instale el plug-in de CLI (por ejemplo, en OSX se descargó como ml_cli_plugin_osx)

   ```
   bx plugin install ml_cli_plugin_osx
   ```
   {: codeblock}

   Salida de ejemplo:

   ```
   Installing binary...
   OK
   Plug-in 'machine-learning 1.0.0' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
   ```

4.  Verifique que esté instalado el plug-in de CLI

   ```
   bx ml help
   ```
   {: codeblock}

Salida de ejemplo:

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

## Configure la CLI con la información del entorno

Para utilizar el resto de los mandatos de la CLI, debe establecer algunas variables de entorno en el sistema operativo. Si está trabajando en Linux, ejecute los mandatos siguientes al sustituir los valores para ML_USERNAME y ML_PASSWORD con sus credenciales. Puede obtener los valores necesarios siguiendo las [instrucciones para recuperar las credenciales de acceso](ml_getting_access.html#retrieving-your-credentials) a la instancia de {{site.data.keyword.pm_full}}.  

```
export ML_INSTANCE=11111111-aaaa-2222-bbbb-333333333333
export ML_USERNAME=44444444-cccc-5555-dddd-666666666666
export ML_PASSWORD=77777777-eeee-8888-ffff-999999999999
export ML_ENV=<url from credentials>
```

## Iniciar una ejecución de entrenamiento del modelo

Ahora está listo para [crear e iniciar las ejecuciones de entrenamiento](ml_dlaas_working_with_new_models.html) mediante la CLI. O bien, para obtener más información, puede [empezar a entrenar un modelo de red neuronal de ejemplo](ml_dlaas_working_with_sample_models.html).

### Actualizar CLI

Consulte [Releases de CLI de ML](https://github.ibm.com/NGP-TWC/wml-cli/releases) para obtener nuevas versiones de la CLI de ML. Le animamos a descargar la última versión de la CLI para tener las características más actualizadas. Una vez que descargue
la versión más reciente de la CLI de ML para su sistema operativo, debe desinstalar el plug-in (si ya tiene una versión anterior
instalada) y, luego, instalar la nueva versión.

```
bluemix plugin uninstall "machine-learning"
```
{: codeblock}

Salida de ejemplo:

```
Uninstalling plug-in 'deep-learning'...
Uninstalling plug-in 'machine-learning'...
OK
Plug-in 'machine-learning' was successfully uninstalled.
```

Instale la versión nueva ejecutando el siguiente mandato:

```
bluemix plugin install ml_cli_plugin_osx
```
{: codeblock}

Salida de ejemplo:

```
Installing binary...
OK
Plug-in 'machine-learning 1.0.1' was successfully installed into /Users/username/.bluemix/plugins/machine-learning. Use 'bx plugin show machine-learning' to show its details.
```
{: codeblock}
