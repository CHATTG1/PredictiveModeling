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

# Despliegue de modelos por lotes

Mediante el servicio de {{site.data.keyword.pm_full}}, puede desplegar un modelo y
generar análisis predictivo realizando solicitudes de puntuación en el
modelo desplegado.
{: shortdesc}


**Nombre del caso de ejemplo**: Predicción de la satisfacción del cliente.

**Descripción del caso de ejemplo**: Una empresa de telecomunicaciones quiere saber qué clientes tienen riesgo de abandono. El modelo presentado predice la rotación de clientes. Un experto en datos desarrolla un modelo predictivo y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo y generar análisis predictivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

## Requisitos previos

Para trabajar con este ejemplo, necesita los siguientes servicios:

* Detalles de la instancia de [Object Storage](https://console.bluemix.net/catalog/services/object-storage), que se utilizan como entrada (datos del cliente que se puntuarán) para el modelo y el almacenamiento para la salida del modelo. Descargue el archivo .csv de datos de entrada de ejemplo desde [aquí](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv). Debe añadir el archivo de entrada a la instancia de Object Storage.
* Credenciales de la instancia de servicio de [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark). Puede utilizar [este enlace](https://console.bluemix.net/catalog/services/apache-spark) para crear una.


## Utilización del modelo de ejemplo

1.  Vaya al separador Ejemplos del Panel de control de {{site.data.keyword.pm_full}}.
2.  En la sección Modelos de ejemplo, busque el mosaico Predicción de la satisfacción del cliente y pulse el icono Añadir modelo (+).

El modelo Predicción de la satisfacción del cliente de ejemplo aparece
en la lista de modelos disponibles en el separador Modelos.

## Creación de un despliegue por lotes con Object Storage

1.  Vaya al separador Modelos del Panel de control de {{site.data.keyword.pm_full}}.
2.  En el menú **Acciones**, pulse **Crear despliegue**.
3.  En el formulario de Crear despliegue, introduzca el nombre, la descripción y el tipo de lote.
4.  Debe proporcionar las entradas siguientes:

    **Conexión de entrada**: Detalles de Object Storage, que se utilizarán como entrada (datos del cliente que se puntuarán) para el modelo y almacenamiento de la salida del modelo (results.csv en este caso, que se crea automáticamente).

    ```
       {
          "source":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Conexión de salida**

    ```
       {
          "target":{
             "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Conexión Spark**: Credenciales del servicio Spark, que encontrará en el separador Credenciales de servicio del panel de control del servicio {{site.data.keyword.Bluemix_short}} Spark.

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

5.  Pulse **Guardar**.

El resultado de la predicción se guarda en un archivo .csv en IBM Object Storage. A continuación se muestra una fila de ejemplo.

Vista previa del archivo de entrada:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Vista previa del archivo de salida:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## Obteniendo detalles del despliegue

Puede comprobar el estado y los parámetros relacionados con el modelo desplegado.

1. Vaya al separador Despliegues del Panel de control de {{site.data.keyword.pm_full}}.
2. En el menú **Acciones**, pulse **Ver detalles**.

## Suprimiendo un despliegue por lotes

Puede suprimir el despliegue si ya no es necesario mediante una consulta como la del siguiente ejemplo.

1. Vaya al separador Despliegues del Panel de control de {{site.data.keyword.pm_full}}.

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
