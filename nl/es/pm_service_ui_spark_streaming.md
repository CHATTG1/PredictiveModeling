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

# Despliegue de modelos en modalidad continua

Puede utilizar el servicio {{site.data.keyword.pm_full}} para desplegar el modelo y
generar análisis predictivo realizando solicitudes de puntuación en el modelo en modalidad continua desplegado.
{: shortdesc}

**Nombre de caso de ejemplo**: Análisis de sentimiento.

**Descripción del caso de ejemplo**: Una agencia de marketing quiere saber la opinión general sobre un determinado tema. La agencia quiere que desarrollemos un modelo que clasifique una
declaración como POSITIVA o
NEGATIVA. Un experto en datos ha preparado un modelo predictivo y lo ha compartido con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo y generar análisis predictivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

Consulte este [documento](https://github.com/pmservice/tweet-sentiment-prediction) para obtener más información.

## Requisitos previos

Para trabajar con este ejemplo, debe tener los recursos siguientes:

* Detalles del tema [Message Hub](https://console.bluemix.net/catalog/services/message-hub), que se utilizan como entrada (texto de tuits) para el modelo y el almacenamiento para la salida del modelo (resultados de la predicción). Asegúrese de que se creen dos temas: tema de entrada con texto de tuits y tema de salida.
* Credenciales de la instancia de servicio de [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Utilice [este enlace](https://console.bluemix.net/catalog/services/apache-spark) para crear una.


## Utilización del modelo de ejemplo

1. Vaya al separador **Ejemplos** del panel de control de {{site.data.keyword.pm_full}}.
2. En la sección **Modelos de ejemplo**, busque el mosaico **Predicción de opiniones**
   y pulse el icono Añadir modelo (+).

El modelo de ejemplo de Predicción de opiniones aparece en la lista
de modelos disponibles en el separador Modelos.


## Creación de un despliegue en modalidad continua con IBM Message Hub

1.  Vaya al separador **Modelos** del panel de control de {{site.data.keyword.pm_full}}.
2.  En el menú **Acciones**, pulse **Crear despliegue**.
3.  En el formulario de **Crear despliegue**, introduzca el **nombre**, la **descripción** y el **tipo de secuencia**.
4.  Debe proporcionar las entradas siguientes:

    **Conexión de entrada**: Detalles de temas de IBM Message Hub, que se utilizarán como entrada (tuits) para el modelo y almacenamiento de la salida del modelo (resultados de predicción).

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
   "source":{
        "topic":"sinput",
         "type":"kafka"
      }
  }
    ```
    {: codeblock}

    **Conexión de salida**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
   "target":{
       "topic":"soutput",
         "type":"kafka"
      }
 }
    ```
    {: codeblock}

    **Conexión Spark**: Credenciales del servicio Spark, que encontrará en el separador Credenciales de servicio del panel de control del servicio {{site.data.keyword.Bluemix_notm}} Spark.

     ```
{
     "credentials":{
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",  
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",  
      "cluster_master_url": "https://spark.bluemix.net",  
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",  
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",  
      "plan": "ibm.SparkService.PayGoPersonal"
     },
         "version":"2.0"
}
     ```
     {: codeblock}

5. Pulse **Guardar**.

El resultado de la predicción se envía al tema MessageHub definido (conexión de salida).

## Obteniendo detalles del despliegue

Puede comprobar el estado y los parámetros relacionados con el modelo desplegado.

1. Vaya al separador **Despliegues** del Panel de control de {{site.data.keyword.pm_full}}.
2. En el menú **Acciones**, pulse **Ver detalles**.

## Suprimiendo un despliegue en modalidad continua

Puede suprimir el despliegue si ya no es necesario ejecutando una
consulta como la del siguiente ejemplo.

1. Vaya al separador **Despliegues** del Panel de control de {{site.data.keyword.pm_full}}.
2. En el menú **Acciones**, pulse **Suprimir**.

## Más información

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que
proporciona, consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado
que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
