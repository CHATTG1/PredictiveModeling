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

El sistema de aprendizaje continuo de {{site.data.keyword.pm_full}} proporciona supervisión automatizada de rendimiento, reentrenamiento y redespliegue de modelo para garantizar la calidad de la predicción.
{: shortdesc}

**Nombre del escenario**: La mejor selección de fármacos para problemas coronarios.

**Descripción del escenario**: Una empresa biomédica que fabrica fármacos para problemas coronarios desea desplegar un modelo que seleccione el mejor fármaco para el tratamiento de enfermedades coronarias. El modelo se debe actualizar cuando surjan nuevas pruebas que demuestren la eficacia del fármaco. Un experto en datos ha preparado un modelo predictivo y lo ha compartido con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo, definir la configuración de aprendizaje y ejecutar la iteración de aprendizaje para evaluar, reentrenar y redesplegar el modelo.

## Requisitos previos

Para trabajar con este ejemplo, necesita los siguientes servicios:

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud) para almacenar los datos de comentarios
* Servicio de [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark)

   Utilice [este enlace](https://console.bluemix.net/catalog/services/apache-spark) para crear una instancia de Apache Spark si todavía no tiene ninguna.

## Utilización del modelo de ejemplo

1. Vaya al separador **Ejemplos** del panel de control de {{site.data.keyword.pm_full}}.
2. En la sección **Modelos de ejemplo**, busque el mosaico **Selección de fármacos para problemas coronarios**
   y pulse el icono **Añadir modelo** (+).

El modelo de Selección de fármacos para problemas coronarios de ejemplo aparece en la lista de modelos disponibles en el separador **Modelos**.


## Establecimiento del sistema de aprendizaje continuo para el modelo publicado

1.  Vaya al separador **Modelos** del panel de control de {{site.data.keyword.pm_full}}.
2.  En el menú **Acciones**, pulse **Ver detalles**.
3.  Desplácese hacia abajo hasta **Detalles de reentrenamiento** y pulse **Editar**.
4.  Debe proporcionar las entradas siguientes:

    **Conexión de comentarios**: Detalles de {{site.data.keyword.dashdbshort}}, que se utilizarán como entrada (datos de comentarios) para la evaluación y el reentrenamiento del modelo.

    ```
    {
        "connection": {
            "username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
            "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    Utilice las siguientes instrucciones para preparar la tabla `DRUG_FEEDBACK_DATA` en {{site.data.keyword.dashdbshort}}.
    
    - Cree una instancia de [{{site.data.keyword.dashdbshort}} Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) (se ofrece un plan de entrada).
    - Cree la tabla **DRUG_FEEDBACK_DATA** en **{{site.data.keyword.dashdbshort_notm}}**.
      + Descargue el archivo [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) desde el repositorio GitHub.
      + Pulse **Abrir la consola** para empezar con **{{site.data.keyword.dashdbshort_notm}}**.
      + Pulse **Cargar datos** y seleccione el tipo de carga **Escritorio**.
      + **Arrastre** el archivo descargado anteriormente al área de carga y pulse **Siguiente**.
      + Seleccione **Esquema** para importar datos y pulse **Nueva tabla**.
      + Escriba un nombre para la nueva tabla y pulse **Siguiente**.
      + Utilice punto y coma (;) como **separador de campo**.
      + Pulse **Siguiente** para crear una tabla con los datos cargados.

     **Nota**: Puede añadir nuevos registros de comentarios a la base de datos de comentarios utilizando el [punto final de la API REST](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback).

     **Tamaño mínimo de datos**: El número mínimo de registros del conjunto de datos de comentarios para empezar la evaluación del modelo (parte de la iteración del sistema de aprendizaje).

     ```
     20
     ```
     {: codeblock}

     **Conexión Spark**: Credenciales del servicio Spark, que encontrará en el separador **Credenciales de servicio** del panel de control del servicio {{site.data.keyword.Bluemix_notm}} Spark.

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

     **Umbrales**: El valor de umbral que desencadena el proceso de reentrenamiento. Si el valor de métrica, como por ejemplo el valor de precisión que se calcula durante la evaluación de modelos es inferior al valor de umbral, se desencadena el reentrenamiento.
     ```
     0.8
     ```

     **Desplegar automáticamente el modelo reentrenado**: Despliegue el modelo reentrenado si tiene un valor de métrica mejor que el desplegado.

5.  Pulse **Configurar**.

## Ejecute la iteración del sistema de aprendizaje continuo

Un modelo publicado se evalúa de forma iterativa. Si el valor de métrica, como por ejemplo el valor de precisión que se calcula durante la evaluación de modelos es inferior al valor de umbral, se desencadena el reentrenamiento. Ambos conjuntos de datos, el entrenamiento y los comentarios, se utilizan para este proceso iterativo de reentrenamiento y evaluación.

1. Para iniciar una nueva iteración, desde el formulario **Detalles** del modelo, separador **Supervisar**, pulse **Evaluar**.
3. Para comprobar el resultado de la iteración, vaya a la tabla Eventos de evaluación o a la vista Gráfico. 

   Puede ver información sobre la calidad del modelo calculada en función de los datos de comentarios (supervisión). También puede ver información sobre la calidad del modelo reentrenado, que es la versión nueva del modelo que se ha creado y evaluado.

4. Para ver la lista de modelos entrenados automáticamente y la calidad correspondiente, pulse **Ver detalles** > **Visión general** y vaya a la sección **Historial de versiones**.

## Almacén de datos de comentarios

Puede utilizar un [punto final de comentarios](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback) para enviar registros nuevos al almacén de comentarios que ha definido en la configuración de aprendizaje de máquina. {{site.data.keyword.dashdbshort}} está soportado actualmente como un almacén de datos de comentarios. Si la tabla de información no existe, la creará el servicio. Si la tabla existe, el esquema se verificará para que coincida con la tabla de entrenamiento y se añadirá a la tabla una columna adicional denominada `_training`. La columna `_training` se utiliza para marcar los registros que se consumen en el proceso de reentrenamiento.

Para obtener más información y ejemplos de un almacén de datos de comentarios, consulte la [documentación de la API](pm_service_api_spark_learning_system.html).

**Nota:** El servicio de {{site.data.keyword.pm_full}} gestiona, modifica y utiliza la tabla de comentarios.

## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
