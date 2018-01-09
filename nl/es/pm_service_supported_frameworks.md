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

# Infraestructuras soportadas

El servicio de {{site.data.keyword.pm_full}} da soporte a las siguientes infraestructuras para el desarrollo y la validación de modelos como parte de la gestión del ciclo de vida de modelo.
{: shortdesc}

## Spark MLlib

El servicio de {{site.data.keyword.pm_full}} da soporte a Spark MLlib para el servicio En línea, Por lotes (beta), Despliegue continuo (beta) y Puntuación. Solo se soportan versiones y entornos de tiempo de ejecución específicos.

### Prestaciones

* Tipos de despliegue:
  * En línea
  * Por lotes con Object Storage, DB2 Warehouse on Cloud, y DB2 on Cloud
  * Continuo con Message Hub
*  Sistema de aprendizaje continuo

### Versiones soportadas

*  Spark 2.0

### Restricciones

  *  Solo se soportan los modelos de clasificación y regresión.
  *  Los modelos que contienen referencias a transformadores personalizados, funciones definidas por el usuario y clases no están soportados.
  * Los despliegues Por lotes y Continuos utilizan el servicio de IBM Cloud Apache Spark. Dependiendo del plan de servicio suscrito, observe las siguientes limitaciones en el número de trabajos de spark-submit paralelos ([Utilizando la documentación de spark-submit](https://console.bluemix.net/docs/services/AnalyticsforApacheSpark/index-gentopic2.html#genTopProcId3)).
    * Lite: solo un trabajo spark-submit a la vez
    * Reserved Enterprise: 150 trabajos spark-submit a la vez

## Scikit-learn

Se ofrece soporte para scikit-learn para el servicio de despliegue en línea y puntuación de {{site.data.keyword.pm_full}}. Solo se soportan versiones y entornos de tiempo de ejecución específicos.

### Prestaciones

* Tipos de despliegue:
  * En línea

### Versiones soportadas

- Anaconda 4.2.x para tiempo de ejecución Python 3.5
- Scikit-Learn 0.17

### Tiempo de ejecución Python

La ejecución de Python para modelos que deben desplegarse utilizando el servicio de despliegue en línea y puntuación de WML es Python 3.5.

En algunos casos, si los modelos se han creado en el tiempo de ejecución Python 2.7, es posible que el despliegue falle debido a incompatibilidades entre Python2.7 y Python3.5.

### Paquetes de Python soportados

Consulte una lista de paquetes de Python soportados por el servicio de despliegue en línea y puntuación de WML [aquí](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restricciones

Existen algunas restricciones, que puede incluir algunas de las siguientes limitaciones o todas:

* Solo se soportan los modelos de clasificación y regresión.
* Los modelos solo pueden contener referencias a los paquetes (y versión de paquete) soportados por Anaconda 4.2.x para el tiempo de ejecución Python 3.5.
* El punto final "schema" devuelto durante la solicitud de despliegue no se ha implementado.
* Los modelos que requieren el tipo de entrada Pandas Dataframe para la API "predict()" no se soportan.
* Los modelos que contienen referencias a transformadores personalizados, funciones definidas por el usuario y clases no están soportados.

## XGBoost

Se ofrece soporte para XGBoost para el servicio de despliegue en línea y puntuación de {{site.data.keyword.pm_full}}. Solo se soportan versiones y entornos de tiempo de ejecución específicos.

### Prestaciones

* Tipos de despliegue:
  * En línea

### Versiones soportadas

- XGBoost 0.6
- Anaconda 4.2.x para tiempo de ejecución Python 3.5
- Scikit-Learn 0.17

### Tiempo de ejecución Python

Python 3.5 es el tiempo de ejecución Python para modelos que se debe desplegar utilizando el servicio de despliegue en línea y puntuación de WML.

En algunos casos, si los modelos se han creado en el tiempo de ejecución Python 2.7, es posible que el despliegue falle debido a incompatibilidades entre Python2.7 y Python3.5.

### Paquetes de Python soportados

Consulte una lista de paquetes de Python soportados por el servicio de despliegue en línea y puntuación de WML [aquí](https://docs.continuum.io/anaconda/packages/old-pkg-lists/4.2.0/py35).

### Restricciones

Existen algunas restricciones, que incluyen algunas o todas las limitaciones siguientes:

* Los modelos solo pueden contener referencias a los paquetes (y versión de paquete) soportados por Anaconda 4.2.x para el tiempo de ejecución Python 3.5.
* El punto final "schema" devuelto durante la solicitud de despliegue no se ha implementado.
* La probabilidad de predicción no se devuelve como respuesta a la solicitud de puntuación en esta etapa.
* Los modelos que contienen referencias a los transformadores personalizados, las funciones definidas por el usuario y las clases no están soportados.

## SPSS Modeler

Se ofrece soporte para las secuencias de IBM® SPSS® Modeler para el servicio de despliegue en línea y por lotes y puntuación de {{site.data.keyword.pm_full}}. Solo se soportan versiones y entornos de tiempo de ejecución específicos.

### Prestaciones

* Tipos de despliegue:
  * En línea
  * Por lotes
* Reentrenamiento
* Ejecutar secuencia

### Versiones soportadas

*  IBM® SPSS® Modeler 17.1
*  IBM® SPSS® Modeler 18.0

### Restricciones

Existen las siguientes restricciones para las secuencias de IBM® SPSS® Modeler:

*  Las secuencias creadas mediante IBM® Data Science Experience Flow Editor no están soportadas.
*  Cuando se prepara una rama de puntuación para su uso en puntuaciones en tiempo real, los datos de entrada procedentes de la solicitud de puntuación deben sustituir al nodo de origen diseñado en la rama de puntuación y el resultado del análisis predictivo debe volver atrás al flujo de respuesta (reemplazando de forma efectiva el nodo terminal en el diseño de rama de puntuación).
*  Dado que la rama de puntuación está preparada para la ejecución en tiempo real en {{site.data.keyword.Bluemix_short}}, no necesita una conexión a un servicio externo. Por ejemplo, un diseño de rama de puntuación de IBM Analytical Decision Management no puede incluir referencias a reglas o modelos almacenados en un repositorio de IBM® SPSS® Collaboration and Deployment Services.
*  La ejecución de una rama de puntuación para la puntuación en tiempo real en {{site.data.keyword.Bluemix_short}} no necesita un servicio externo. Por ejemplo, no puede desplegar ni puntuar en algoritmos de modelo que requieran un almacén de datos de IBM® SPSS® Analytic Server y Apache Hadoop en tiempo real.
*  {{site.data.keyword.pm_short}} da soporte a los scripts Python incluidos en Modeler. Existen unas pocas restricciones debido al método usado para procesar secuencias antes de ejecutarlas en {{site.data.keyword.pm_short}}. Normalmente, si un usuario decide controlar la ejecución de la secuencia, hará referencia al nodo terminal de la rama. Para
{{site.data.keyword.pm_short}}, al procesar la secuencia, identificamos los nodos de JSON que se alterarán temporalmente y, a continuación, realizaremos la sustitución antes de que se ejecute la secuencia. Ello provoca que la secuencia falle en el script porque los nodos de exportación y la entrada a la que se hace referencia ya no existen. La solución pasa por utilizar el ID de otro nodo para identificar la rama de forma exclusiva durante la ejecución. Así se garantiza que la secuencia se ejecuta tal como se define en el script Python incluido.

## Información adicional

Para obtener más información sobre el soporte actual para los modelos predictivos entrenados por IBM® SPSS® Analytic Server, consulte la sección [IBM® SPSS® Analytic Server](https://www.ibm.com/support/knowledgecenter/SSWLVY) de [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/).
