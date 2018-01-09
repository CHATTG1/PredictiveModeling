---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Iniciación
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} es un servicio de IBM Cloud que permite a los usuarios realizar dos operaciones fundamentales de aprendizaje de máquina: entrenamiento y puntuación.
{: shortdesc}

- **Entrenamiento** es el proceso de refinación de un algoritmo para que pueda aprender de un conjunto de datos. El resultado de esta operación se denomina un modelo. Un modelo engloba los coeficientes aprendidos de expresiones matemáticas.
- **Puntuación** es la operación de predecir un resultado mediante un modelo entrenado. El resultado de la operación de puntuación es otro conjunto de datos que contiene los valores pronosticados.

{{site.data.keyword.pm_full}} está diseñado para satisfacer las necesidades de dos personajes principales:

- Los científicos de datos: Crear interconexiones de aprendizaje de máquina que aprovechan las transformaciones de datos y los algoritmos de aprendizaje de máquina. Se suele utilizar cuadernos o herramientas externas para entrenar y evaluar sus modelos. Los científicos de datos suelen colaborar con ingenieros de datos para explorar y comprender los datos.
- Los desarrolladores: Crear aplicaciones inteligentes que utilizan el resultado de las predicciones mediante modelos de aprendizaje de máquina.

Aunque el entrenamiento es un paso fundamental en el proceso de aprendizaje de máquina, {{site.data.keyword.pm_full}} le permite agilizar el funcionamiento de los modelos al desplegarlos y obtener el valor real de su negocio a lo largo del tiempo y en todas sus iteraciones.

## Requisitos previos

Para utilizar {{site.data.keyword.pm_full}}, desde el catálogo de {{site.data.keyword.Bluemix_short}}, debe crear la [instancia de servicio aquí](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/). Esta configuración permite realizar las tareas siguientes:

## Pasos

1. [Configure el entorno de aprendizaje de la máquina.](ml_getting_access.html)
1. [Cree y almacene un modelo](pm_custom_models.html).
2. [Despliegue un modelo](pm_service_api_spark_online.html).
3. Utilice el valor `scoring endpoint` en la aplicación para [obtener predicciones.](pm_service_api_spark_building.html)

## Uso de Machine Learning con Data Science Experience

{{site.data.keyword.pm_full}} está integrado con IBM Data Science Experience. Puede utilizar bibliotecas de cliente de API de Machine Learning en cuadernos de Data Science Experience; debe tener una instancia de Machine Learning para utilizar Model Builder y Flow Editor.

## Uso de Machine Learning con SPSS Modeler

{{site.data.keyword.pm_full}} se integra con IBM® SPSS® Modeler. Puede utilizar la API de Machine Learning para aprovechar los algoritmos matemáticos avanzados.


## Uso de Machine Learning con su entorno

{{site.data.keyword.pm_full}} puede utilizarse como una solución híbrida que enlaza el entorno local con la nube. Puede utilizar la API de Machine Learning para publicar los modelos, desplegar y puntuar. Para obtener más información, consulte [DSX: Hybrid Mode](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b).

## Acerca de

El servicio {{site.data.keyword.pm_full}} es un conjunto de API REST que se pueden invocar desde cualquier lenguaje de programación.

El servicio {{site.data.keyword.pm_full}} se centra en el despliegue, pero puede utilizar IBM® SPSS® Modeler o IBM® Data Science Experience para crear y trabajar con modelos y conductos. Tanto SPSS® Modeler como Data Science Experience que utilicen Spark MLlib y Python scikit-learn ofrecen diversos métodos de modelado derivados de aprendizaje de máquina, inteligencia artificial y estadísticas.

## Enlaces relacionados

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o [Utilización del servicio con modelos SPSS](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos
SPSS](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
