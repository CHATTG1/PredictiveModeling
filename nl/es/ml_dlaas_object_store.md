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

# Configure un almacén de objetos para el aprendizaje profundo

Para trabajar con el servicio de {{site.data.keyword.pm_full}} y los experimentos de aprendizaje profundo que forman parte de dicho servicio, debe tener acceso a una instancia de Cloud Object Storage (control.softlayer.com) o de Object Storage OpenStack Swift (console.bluemix.net). Debe definir grupos de Cloud Object Storage para proporcionar los datos de entrenamiento y para almacenar los registros y el modelo de entrenamiento, y puede utilizar las mismas instancias u otras distintas de Cloud Object Storage para este propósito.
{: shortdesc}

## Creación de una instancia de Cloud Object Storage

1. Vaya a la página [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) y seleccione una de las siguientes opciones:

   - Almacenamiento no cifrado pero gratuito (Lite): [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - Servicio cifrado pero no libre: [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. Configure la instancia de Object Storage y pulse **Crear**.
3. Una vez que se cree el servicio, en el menú lateral, pulse **Credenciales de servicio** para obtener las credenciales, como por ejemplo el URL de la API, el nombre de usuario y la contraseña para acceder a la instancia de Object Storage.

## Acceso a la instancia de Cloud Object Storage (IaaS)

El servicio de Cloud Object Storage (IaaS) proporciona API compatibles con S3 y a las que se puede acceder utilizando cualquier aplicación cliente compatible.

## Interfaz de línea de mandatos (CLI) de Amazon Web Services (AWS)

1. Instale [CLI de AWS](https://aws.amazon.com/cli/) utilizando `pip install awscli`
2. Configure las credenciales para Cloud Object Storage (IaaS) en el archivo de credenciales de AWS (para linux, ubicado en ~/.aws/credentials).

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

Puede utilizar la CLI de AWS para enumerar y actualizar grupos y archivos, especificando el perfil anterior y el punto final de Cloud Object Storage desde las credenciales. Por ejemplo, para enumerar los grupos de la instancia, ejecute el mandato siguiente:

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Acceso al Object Storage OpenStack Swift para la instancia de Bluemix

Puede utilizar la CLI de swift o las bibliotecas de cliente (por ejemplo, la biblioteca de cliente de swift python para acceder a este objectstore). Para obtener más detalles, consulte [la documentación](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## Formato de datos

Distintas infraestructuras requieren entrenar y probar conjuntos de datos en diferentes formatos. Por ejemplo, Caffe requiere conjuntos de datos en formato LevelDB o LMDB, mientras que Torch requiere conjuntos de datos en el formato propietario Torch. Suponemos que los datos ya están en el formato que necesita la infraestructura específica. Los detalles sobre cómo convertir datos en bruto en formato específico de la infraestructura sobrepasan el ámbito de este documento. Consulte la documentación para la infraestructura de aprendizaje profundo que desee utilizar para obtener más información.
