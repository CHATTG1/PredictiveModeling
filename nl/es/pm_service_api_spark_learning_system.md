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

# Sistema de aprendizaje continuo

El servicio de {{site.data.keyword.pm_full}} incluye un sistema de aprendizaje continuo. Los sistemas de aprendizaje continuo proporcionan supervisión automatizada de rendimiento, reentrenamiento y redespliegue de modelo para garantizar la calidad de las predicciones.
{: shortdesc}

**Nombre del escenario**: La mejor selección de fármacos para problemas coronarios.

**Descripción del escenario**: Una empresa biomédica que fabrica fármacos para problemas coronarios desea desplegar un modelo que seleccione el mejor fármaco para el tratamiento de enfermedades coronarias. El modelo se debe actualizar cuando surjan nuevas pruebas que demuestren la eficacia del fármaco. Un experto en datos prepara un modelo predictivo y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo, definir la configuración de aprendizaje y ejecutar la iteración de aprendizaje para evaluar, reentrenar y redesplegar el modelo.

**Nota**: También puede practicar con [un cuaderno de python de ejemplo](https://dataplatform.ibm.com/analytics/notebooks/57bd0753-ccee-42bd-9d11-099a981e4fbe/view?access_token=40b77775b209dab516811a695ba1d5dbcab2dfb260c910daf3d985c9d4570325) que crea un modelo de ejemplo, configura el sistema de aprendizaje y ejecuta la iteración de aprendizaje.

## Requisitos previos

Para trabajar con los ejemplos, debe tener los siguientes servicios:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) para almacenar datos de comentarios
* Credenciales de la instancia de servicio de [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Puede utilizar [este enlace](https://console.bluemix.net/catalog/services/apache-spark) para crear una.

## Utilización del modelo de ejemplo

1. Vaya al separador **Ejemplos** del panel de control de {{site.data.keyword.pm_full}}.
2. En la sección **Modelos de ejemplo**, busque el mosaico Selección de fármacos para problemas coronarios y pulse el icono **Añadir modelo** (+).

El modelo de **Selección de fármacos para problemas coronarios** de ejemplo aparece en la lista de modelos disponibles en el separador **Modelos**.

## Generación de la señal de acceso

Genere una señal de acceso que utilice el usuario y la contraseña disponibles en el separador Credenciales de servicio de la instancia de servicio de {{site.data.keyword.pm_full}}.

Ejemplo de solicitud:

```
curl --basic --user <username>:<password> https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Ejemplo de salida:

```
{"token":"**********"}
```
{: codeblock}

Utilice el siguiente mandato de terminal para asignar el valor de la señal a la señal de la variable de entorno:

```
token="<token_value>"
```
{: codeblock}

## Cómo trabajar con modelos publicados

Utilice la siguiente llamada de API para obtener detalles de la instancia, como, por ejemplo:

* dirección `url` de modelos publicados
* dirección `url` de despliegues
* información de uso

Ejemplo de solicitud:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Ejemplo de salida:

```
{
   "metadata":{
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

Al obtener la dirección `url` **published_models**, utilice la siguiente llamada de API para obtener los detalles del modelo:

Ejemplo de solicitud:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer  $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models
```
{: codeblock}

Ejemplo de salida:

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


## Configure el sistema de aprendizaje continuo para un modelo publicado

Para configurar el sistema de aprendizaje continuo para el modelo, debe realizar las tareas siguientes:

1.  [Preparar la cabecera de autorización](#prepare-the-authorization-header).
2.  [Preparar el conjunto de datos de los comentarios](#prepare-the-feedback-data-set).
3.  [Preparar la carga útil de configuración](#prepare-the-configuration-payload).

Después de completar la preparación de la cabecera de autorización, el conjunto de datos de los comentarios y la carga útil de la configuración, podrá comenzar a iterar el sistema de aprendizaje continuo. Para obtener más información, consulte [Ejecute la iteración del sistema de aprendizaje continuo](#run-continuous-learning-system-iteration).

### Preparar la cabecera de autorización

Para preparar la cabecera de autorización que combina la señal de {{site.data.keyword.pm_full}} y las credenciales de instancia de Spark, proporcione los detalles siguientes:

*  La señal de acceso creada en el paso anterior
*  Las credenciales del servicio Spark, que pueden encontrarse en el separador Credenciales de servicio del panel de control del servicio Spark de {{site.data.keyword.Bluemix_notm}}. Antes de realizar la solicitud de despliegue, las credenciales de Spark deben estar codificadas como base64. Se pasan en el campo X-Spark-Service-Instance de la cabecera de la solicitud.

   Dependiendo del sistema operativo que utilice, debe emitir uno de los siguientes mandatos de terminal para realizar la codificación base64 y asignarla a la variable de entorno.

   En el sistema operativo **macOS**, utilice el mandato siguiente:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   En los sistemas operativos **Microsoft Windows** o **Linux**, debe utilizar el parámetro `--wrap=0` con el mandato `base64` para realizar la codificación base64:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

### Preparar el conjunto de datos de comentarios

El sistema de aprendizaje requiere una conexión a los datos de entrenamiento (los datos que se utilizan en el entrenamiento del modelo), así como los datos de comentarios. El almacén de datos de comentarios se utiliza para supervisar y reentrenar el modelo cuando lo necesite. {{site.data.keyword.dashdbshort}} está soportado como un almacén de datos de comentarios. El servicio de {{site.data.keyword.pm_short}} gestiona, modifica y utiliza la tabla de comentarios.
Para preparar la tabla de **DRUG_FEEDBACK_DATA** en **{{site.data.keyword.dashdbshort}}**, debe completar los pasos siguientes:

1. Cree una instancia de [{{site.data.keyword.dashdbshort}} Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (se ofrece un plan de entrada).
2. Cree la tabla `DRUG_FEEDBACK_DATA` en **{{site.data.keyword.dashdbshort}}**.
   1. Desde el repositorio de GitHub, descargue el archivo [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv).
   2. Para empezar con **{{site.data.keyword.dashdbshort_notm}}**, pulse el icono **Abrir la consola**.
   3. Seleccione **Cargar datos** y el tipo de carga **Escritorio**.
   4. **Arrastre** el archivo previamente descargado y pulse **Siguiente**.
   5. Para importar datos, pulse **Esquema** y, a continuación, pulse **Nueva tabla**.
   6. En el tipo de campo **nueva tabla**, escriba un nombre y pulse **Siguiente**.
   7. Utilice punto y coma (;) como **separador de campo**.
   8. Pulse **Siguiente** para crear la tabla con los datos cargados.

**Nota**: Puede añadir nuevos registros de comentarios a la base de datos de comentarios utilizando este [punto final de API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback). Para obtener más información, consulte [Sección de almacén de datos de comentarios](#feedback-data-store).

### Preparación de la carga útil de configuración

Para especificar la configuración de aprendizaje, debe utilizar el siguiente punto final:

```
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_configuration
```
{: codeblock}

Para finalizar la carga útil, debe definir los valores de los parámetros siguientes:

<dl><dt>feedback_data_reference</dt>
<dd>conexión y fuente del almacén de datos de comentarios</dd>
<dt>min_feedback_data_size</dt>
<dd>número mínimo de registros en el conjunto de datos de comentarios para iniciar la iteración del sistema de aprendizaje continuo
</dd>
<dt>auto_retrain</dt>
<dd>[never, always, conditionally] especifica cuándo se desencadena el proceso de reentrenamiento. Cuando se establece en [conditionally], se desencadena el proceso de reentrenamiento cuando la calidad del modelo es inferior al valor de umbral especificado.
</dd>
<dt>auto_redeploy</dt>
<dd>[never, always, conditionally] especifica cuándo se debe desplegar el modelo reentrenado. Cuando se establece en [conditionally], se desencadena el nuevo despliegue del modelo cuando la calidad del modelo que se acaba de reentrenar sea mejor que la del modelo desplegado.</dd></dl>

Ejemplo de solicitud:

```
curl -v -X PUT \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer $token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
         "definition": {
           "method": "multiclass",
           "metrics": [
             {
               "name": "accuracy",
               "threshold": 0.8
             }
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

Ejemplo de salida:

```
{
   "auto_retrain":"conditionally",
   "auto_redeploy":"never",
      "evaluation_definition": {
    "method": "multiclass",
    "metrics": [
      {
        "name": "accuracy",
        "threshold": 0.8
      }
    ]
   },
   "feedback_data_reference":{
      "connection":{
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
      "credentials":{
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

**Nota**: El ejemplo utiliza valores predeterminados para los parámetros `auto_retrain` y `auto_redeploy`. Para obtener más información sobre estos parámetros, consulte [documentación de la API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/put_v3_wml_instances_instance_id_published_models_published_model_id_learning_configuration) para el parámetro `learning_configuration`.

## Ejecute la iteración del sistema de aprendizaje continuo

Para comenzar la iteración del sistema de aprendizaje, utilice el siguiente método de API REST. Durante la iteración, el modelo publicado se evalúa. Si la precisión evaluada está por debajo del valor de umbral especificado, se desencadenará el reentrenamiento del modelo. Se utilizarán tanto los conjuntos de datos de entrenamiento como de comentarios para el reentrenamiento y la evaluación.

Ejemplo de solicitud:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Ejemplo de salida:

```
La solicitud se ha realizado y se ha creado un recurso como resultado.
```
{: codeblock}

Para comprobar el estado de la iteración, emita la siguiente solicitud:

Ejemplo de solicitud:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/learning_iterations
```
{: codeblock}

Ejemplo de salida:

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

También se puede obtener la lista de métricas de evaluación y valores emitiendo la siguiente solicitud:

Ejemplo de solicitud:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/evaluation_metrics
```
{: codeblock}

Ejemplo de salida:

```
{
  "count": 1,
  "resources": [
    {
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
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
      "phase": "setup",
      "values": [
        {
          "name": "accuracy",
          "value": 0.870968,
          "threshold": 0.8
        }
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

## Almacén de datos de comentarios

Puede utilizar un [punto final de comentarios](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) para enviar registros nuevos (registros que no se han utilizado en el proceso de entrenamiento) para almacenar información que se ha definido en la configuración de aprendizaje. El almacén de datos de comentarios se utiliza para supervisar y reentrenar el modelo cuando lo necesite. {{site.data.keyword.dashdbshort}} está soportado como un almacén de datos de comentarios. Si la tabla de información no existe, la creará el servicio. Si la tabla existe, el esquema se verificará para que coincida con la tabla de entrenamiento y se añadirá a la tabla una columna adicional denominada `_training`. La columna adicional se utiliza para marcar los registros que se consumen en el proceso de reentrenamiento.

**Nota:** El servicio de {{site.data.keyword.pm_short}} gestiona, modifica y utiliza la tabla de comentarios.

Puede enviar un nuevo registro de información al almacén de datos de comentarios emitiendo la solicitud siguiente:

Ejemplo de solicitud:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: Bearer $token" \
-d '{
   "fields":[
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

## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
