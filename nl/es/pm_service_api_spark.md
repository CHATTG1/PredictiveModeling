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

# API REST

El servicio de {{site.data.keyword.pm_short}} es un conjunto de API REST que puede invocar desde cualquier
lenguaje de programación y que permite integrar de análisis de Data Science
Experience en sus aplicaciones. Enlace sus aplicaciones
{{site.data.keyword.Bluemix_short}} a la instancia del servicio {{site.data.keyword.pm_short}} y
genere el análisis predictivo que necesitan las aplicaciones para mejorar
el servicio a los usuarios. Gestione sus modelos en el [panel de control de administración](pm_service_ui_spark.html) y utilice el panel de control para crear en línea, ejecutar por lotes o ejecutar en modalidad continua despliegues integrados con sus aplicaciones.
{: shortdesc}

Desarrolle aplicaciones sobre los modelos de Spark, Python, e IBM® SPSS® desplegados en una instancia de servicio
mediante las potentes [API REST](https://watson-ml-api.mybluemix.net/):

*  Recupere los metadatos correspondientes a un determinado modelo de predicción
*  Despliegue modelos y gestione modelos desplegados
    *  Despliegue en línea (puntuación)
    *  Despliegue por lotes (con soporte de IBM Cloud Object Storage y {{site.data.keyword.dashdbshort}})
    *  Despliegue continuo (con soporte a IBM Cloud Message Hub)
*  Recupere los metadatos correspondientes a un determinado despliegue
*  Supervise y reentrene los modelos desplegados mediante el sistema de aprendizaje continuo
*  Genere análisis predictivos realizando solicitudes de puntuación a los modelos desplegados

Para obtener más información, consulte los ejemplos siguientes del uso de la API REST:

*  [Despliegue de modelos en línea](pm_service_api_spark_online.html)
*  [Puntuación de modelos en línea](pm_service_api_develop_score.html)
*  [Despliegue de modelos por lotes](pm_service_api_spark_batch.html)
*  [Despliegue de modelos continuos](pm_service_api_spark_streaming.html)
*  [Sistema de aprendizaje continuo](pm_service_api_spark_learning_system.html)

## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
