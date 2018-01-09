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

# Desarrollo y permanencia del modelo personalizado

Mediante el servicio de {{site.data.keyword.pm_full}}, puede desplegar un modelo y
generar análisis predictivo realizando solicitudes de puntuación en el
modelo desplegado.
{: shortdesc}

## Cómo trabajar con modelos personalizados

### Nombre del caso de ejemplo: Predicción de línea de productos.

### Descripción del caso de ejemplo:

Nuestro cliente es el propietario de una de las cadenas de grandes almacenes más famosas de Europa. Quieren que estudiemos los intereses de sus clientes en relación con su línea de productos, como Accesorios personales, Equipamiento de camping y Protección aire libre.
Un experto en datos desarrolla un modelo predictivo y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo y generar análisis predictivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

### Desarrollo y permanencia del modelo personalizado

#### Uso de Data Science Experience

Utilice [Data Science
Experience](https://console.bluemix.net/catalog/services/data-science-experience) para crear modelos personalizados. Después de registrarse, debe iniciar sesión para completar los pasos siguientes.

1. Cree una organización y un espacio. La primera vez que inicie sesión, debe proporcionar esta información. Pulse **Continuar** para aceptar los valores predeterminados.
2. Una vez creada la organización, vaya a **Proyectos** y pulse **Nuevo proyecto**.
3. Especifique un nombre y una descripción para el proyecto y pulse **Crear**. El nombre de proyecto que especifique se utilizará como
   nombre del contenedor de destino.
4. Una vez creado el proyecto, puede realizar una de las siguientes tareas:
   
   *  **añadir cuadernos** y empezar a desarrollar sus propios modelos, o bien puede cargar uno de los siguientes cuadernos de ejemplo:

        *  [Desarrollo de modelos Spark con Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [Desarrollo de modelos Spark con Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [Desarrollo de modelos scikit-learn con Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **añadir modelos** y empezar a desarrollar sus propios modelos utilizando el asistente de modelos.


#### Uso del entorno local

También puede utilizar el entorno que elija para desarrollar el modelo y publicarlo posteriormente, desplegar y puntuar utilizando la [biblioteca de cliente de API común]() de Watson Machine Learning disponible en pypi.
Para obtener más información sobre la biblioteca de cliente, consulte el [cuaderno](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125) y la [documentación](pm_service_client_library.html) de ejemplo.

**Nota:** La biblioteca de cliente de API común está en **beta**.

### Despliegue y puntuación del modelo personalizado

Puede utilizar la API para desplegar y puntuar los modelos en línea, por lotes y continuos.

*  [Despliegue de modelos en línea](pm_service_api_spark_online.html)
*  [Despliegue de modelos por lotes](pm_service_api_spark_batch.html)
*  [Despliegue de modelos en modalidad continua](pm_service_api_spark_streaming.html)

## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM® Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
