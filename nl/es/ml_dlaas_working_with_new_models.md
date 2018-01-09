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

# Crear una ejecución de entrenamiento

Las ejecuciones de entrenamiento son el principio organizativo para realizar experimentos de aprendizaje profundo en {{site.data.keyword.pm_full}}. Un experimento típico puede constar de decenas a cientos de ejecuciones de entrenamiento. Cada ejecución se define individualmente y consta de las siguientes partes: la red neuronal definida mediante uno de los [marcos de aprendizaje profundo soportado](ml_dlaas_supported_framework.html) y la configuración para cómo ejecutar su entrenamiento, como el número de GPU y la ubicación del almacenamiento objeto que contiene el conjunto de datos.
{: shortdesc}

<p align="center"><img src="images/experiment_to_training_runs_text.svg" alt="relación de experimentos para entrenar ejecuciones"></p>

## Creación de un archivo .zip de definición de modelo

Después de definir la red neuronal y el manejo de datos asociado utilizando uno de los [marcos de aprendizaje profundo soportados](ml_dlaas_supported_framework.html), empaquete estos archivos utilizando el formato .zip. Por ejemplo, si el modelo se ha escrito en Torch, empaquete los archivos .lua; si está en Caffe, comprima el archivo .prototxt; o si está en Tensorflow/Keras/MXNet, comprima los archivos .py. Otros formatos de compresión, como gzip o tar, no están soportados. Consulte la documentación para el marco de Deep Learning que desee utilizar para preparar los archivos de definición de modelo.  

<!-- Supposedly this isn't true anymore >> NOTE: All model definition files must be in the first level of the zip file so ensure there are no nested directories in the zip file. -->

Por ejemplo, un archivo zip `tf-model.zip` que contiene la definición de modelo para tensorflow puede contener la salida siguiente:

```
unzip -l tf-model.zip
```
{: codeblock}

Salida de ejemplo:

```
Archive:  tf-model.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     7094  09-21-2017 11:38   convolutional_network.py
     5486  09-19-2017 13:49   input_data.py
---------                     -------
    12580                     2 files
```
{: codeblock}

## Cargar datos de entrenamiento

Los datos de entrenamiento deben [cargarse a una instancia de servicio compatible con Object Storage](ml_dlaas_object_store.html). Las credenciales de dicha instancia de Object Storage se utilizarán a continuación en el archivo de manifiesto siguiente. El almacén de objetos también se utiliza para almacenar el modelo entrenado al final de su ejecución de entrenamiento.

## Creación de un archivo de manifiesto de entrenamiento

El manifiesto es un archivo con formato YAML que contiene distintos campos que describen el modelo de entrenamiento, incluido el marco de aprendizaje profundo a utilizar, la configuración del almacenamiento de objetos en la nube, los requisitos de recursos, y varios argumentos (incluidos hiperparámetros) necesarios para la ejecución de modelos durante el entrenamiento y las pruebas. A continuación se describen los distintos campos del archivo de entrenamiento de modelos para el aprendizaje profundo, continuando nuestro ejemplo de reconocimiento de escritura manual del flujo del tensor.

* `model_definition.name`: Puede proporcionar cualquier valor para poner nombre al trabajo de entrenamiento para ayudar a identificarlo una vez que se haya iniciado. Sin embargo, esto no tiene que ser exclusivo: el servicio asignará un modelo-id único para cada trabajo de entrenamiento iniciado.
* `model_definition.description`: Este es otro campo que puede utilizar para describir el trabajo.
* `model_definition.author`: Opcional. Proporcione el nombre del autor y la dirección de correo electrónico bajo las teclas *nombre* y *correo electrónico*.
* `model_definition.framework`: Este campo proporciona la información de infraestructura específica; el nombre y la versión deben coincidir con una de las [Infraestructuras de aprendizaje profundo soportadas](ml_dlaas_supported_framework.html).
    - `model_definition.framework.name`: Nombre de marco
    - `model_definition.framework.version`: Versión del marco.
* `model_definition.execution`: Este campo proporciona información sobre el mandato para iniciar el entrenamiento.
    - `model_definition.execution.command`: Este campo identifica el archivo de programa principal junto con cualquier argumento que necesite ejecutar el aprendizaje profundo.
    - `model_definition.execution.resource`: Este campo especifica los recursos que se asignarán para el entrenamiento y que deberían ser uno de los valores siguientes: `small` (1 GPU), `medium` (2 GPU), `large` (4 GPU)
* `training_data_reference`: Esta sección especifica una lista de los almacenes de objetos desde donde se cargan los archivos de datos utilizados para entrenar el modelo. Actualmente esta lista debe contener uno y solo uno de los almacenes de objetos, con la definición siguiente:
    - `connection`: Las variables de conexión para el almacén de datos.
    - `source.type`: Tipo de almacén de datos, actualmente solo puede establecerse en s3 o bluemix_objectstore. Utilice `s3` si la instancia de Object Storage es *Cloud Object Storage (IaaS)* y `bluemix_objectstore` si la instancia de Object Storage es *Object Storage OpenStack Swift for Bluemix*.
    - `source.bucket`: El grupo donde residen los datos de entrenamiento.
* `training_results_reference`: Esta sección especifica el almacén de objetos donde se almacenarán los archivos y los registros de modelo resultantes una vez que se complete el entrenamiento.
    - `connection`: Las variables de conexión para el almacén de datos. La lista de variables de conexión soportadas dependen del tipo de almacén de datos.
    - `target.type`: Tipo de almacén de datos, actualmente solo puede establecerse en s3 o bluemix_objectstore. Utilice `s3` si la instancia de Object Storage es *Cloud Object Storage (IaaS)* y `bluemix_objectstore` si la instancia de Object Storage es *Object Storage OpenStack Swift for Bluemix*.
    - `target.bucket`: El grupo donde se grabarán los resultados de entrenamiento.

Por ejemplo, el siguiente archivo de definición de entrenamiento de modelos se puede utilizar para definir un trabajo para entrenar un modelo de flujo de tensor:

```
model_definition:
  framework:
    name: tensorflow
    version: 1.2-py3
  name: tf-mnist-showtest1
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 2000000
    resource: small
training_data:
- connection:
    endpoint_url: <auth-url>
    aws_access_key_id: <username>
    aws_secret_access_key: <password>
  source:
    bucket: mnist-training-data
    type: s3
training_results:
  connection:
    endpoint_url: <auth-url>
    aws_access_key_id: <username>
    aws_secret_access_key: <password>
  target:
    bucket: mnist-training-models
    type: s3
```
{: codeblock]

donde `convolutional_network.py` es el programa de flujo de tensor (que forma parte del zip de definición de modelo) a ejecutar mientras el restante son argumentos para el programa. Los valores de argumentos de programa `--trainImagesFile train-images-idx3-ubyte.gz`, `--trainLabelsFile train-labels-idx1-ubyte.gz`, `--testImagesFile t10k-images-idx3-ubyte.gz`, `--testLabelsFile t10k-labels-idx1-ubyte.gz` hacen referencia a las vías de acceso de conjunto de datos del contenedor del almacén de objetos `tf_training_data`. Los argumentos de programa `--trainingIters 20000` y `--learningRate 0.001` pasan los valores de hiperparámetros.

**Nota**: Cuando la configuración de entrenamiento o los archivos de definición de modelo hacen referencia a los archivos cargados en la instancia de Object Storage, las referencias deberían utilizar vías de acceso relativas como se muestra más arriba.

**Nota**: Antes de que empiece el entrenamiento, se descargarán todos los archivos del grupo de datos de entrenamiento en el entorno de entrenamiento operado por el servicio. Para evitar la sobrecarga/retraso de archivos de transferencia innecesarios, mantenga los archivos no utilizados para archivos de entrenamiento en grupos separados.

**Nota**: En el ejemplo anterior, el almacén de objetos utilizado para proporcionar los datos y almacenar el modelo resultante es *Cloud Object Storage (IaaS)*. Si, por otro lado, el almacén de objetos que se utiliza es *Object Storage Open Stack Swift for Bluemix*, las claves de conexión podrían ser diferentes. A continuación se muestra un manifiesto de ejemplo:

```
model_definition:
  framework:
    name: tensorflow
    version: 1.2-py3
  name: tf-mnist-showtest1
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 2000000
    resource: small
training_data_reference:
- connection:
    auth_url: <auth-url>
    user_name: <username>
    password: <password>
    region: <region>
    domain_name: <domain-name>
    project_id: <project-id>
  source:
    bucket: mnist-training-data
    type: bluemix_objectstore
training_results_reference:
  connection:
    auth_url: <auth-url>
    user_name: <username>
    password: <password>
    region: <region>
    domain_name: <domain-name>
    project_id: <project-id>
  target:
    bucket: mnist-training-models
    type: bluemix_objectstore
```
{: codeblock]

**Nota**: Para conexiones *Object Storage Open Stack Swift for Bluemix*, la correlación entre nombres de claves desde las credenciales de Object Store y los nombres de claves necesarios en el manifiesto:

| {{site.data.keyword.Bluemix_notm}} Clave de credenciales | Clave de credenciales de manifiesto de entrenamiento |
|----------------------------------------------------|----------------------------------------|
|auth_url |auth_url |
|username |user_name |
|password |password |
|projectId |project_id |
|region |region |
|domainName |domain_name |
{: caption="Tabla 1. {{site.data.keyword.Bluemix_notm}} y claves de credenciales de manifiesto de entrenamiento" caption-side="top"}

## Enviar una ejecución de entrenamiento

Después de preparar el .zip de definición de modelo y los archivos de configuración de entrenamiento, envíe el trabajo utilizando el mandato `bx ml train`: `bx ml train <path-to-model-definition-zip> <path-to-model-configuration-yaml>` 

```
bx ml train tf-model.zip job.yaml
```
{: codeblock}

Salida de ejemplo:

Cuando el mandato se ha enviado satisfactoriamente, se devolverá un ID de modelo exclusivo. Por ejemplo, la salida siguiente muestra un valor `Model-ID` de `training-DOl4q2LkR`:

```
Starting to train ...
OK
Model-ID is 'training-DOl4q2LkR'
```

# Supervisar una ejecución de entrenamiento

Para listar todos los trabajos de entrenamiento (se hayan completado o no), utilice el mandato cli `bx ml list trained-models`

```
bx ml list trained-models
```
{: codeblock}

Salida de ejemplo:

```
Fetching the list of trained models ...
SI No   Name                       guid                 status    submitted-at
1       tf-mnist                   training-DOl4q2LkR   pending   2017-10-26T11:16:51Z

1 records found.
OK
List all trained-models successful
```
{: codeblock}

**Nota**: El servicio sólo conservará detalles de los trabajos de entrenamiento para 7 días después de que se eliminen y no aparecerán en esta lista.

Para supervisar un trabajo concreto, utilice el mandato cli `bx ml show trained-models <model-id>`:

```
bx ml show trained-models training-DOl4q2LkR
```
{: codeblock}

Salida de ejemplo:

```
Fetching the trained model details with MODEL-ID 'training-DOl4q2LkR' ...
ModelId        training-DOl4q2LkR
url            /v3/models/training-DOl4q2LkR
Name           tf-mnist
State          running
Submitted_at   2017-10-26T11:10:37Z
OK
Show trained-models details successful
```
{: codeblock}

**Nota**: En este momento hay un problema conocido con trabajos fallidos que desaparecen de la lista y que muestran el resultado de mandatos de CLI, como si el trabajo se hubiera suprimido. Este problema se corregirá pero, mientras tanto, si ve que desaparece un trabajo de entrenamiento, compruebe los archivos de registro de entrenamiento como se explica a continuación para averiguar por qué ha fallado el trabajo.

Cuando un trabajo haya finalizado satisfactoriamente (o no), los archivos y los registros del modelo entrenado se deberían grabar en el paquete de Cloud Object Storage especificado en el valor `training_results_reference` dentro del archivo de definición de entrenamiento de modelos, en una carpeta con el mismo nombre que el ID de modelo.

## Suprimir una ejecución de entrenamiento

Para suprimir un trabajo de entrenamiento (esto no elimina el modelo ni los registros de entrenamiento que salieron a la instancia de Object Storage, sino que elimina todo el historial del trabajo de entrenamiento del servicio).

```
bx ml delete trained-models training-DOl4q2LkR
```
{: codeblock}


Salida de ejemplo:

```
Deleting the trained model 'training-DOl4q2LkR' ...
OK
Delete trained-models successful
```
{: codeblock}
