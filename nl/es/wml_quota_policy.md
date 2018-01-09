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

# Política de cuota

El motor de {{site.data.keyword.pm_full}} aplica cuotas por instancia para garantizar una asignación y uso correctos de los recursos. Estas políticas varían en función de los perfiles de cuentas, el uso histórico del servicio y la disponibilidad de los recursos. La política de cuota describe los límites que no están definidos por el plan de precios y **están sujetos a cambios sin previo aviso**. En estos momentos, están activos los siguientes límites de cuota de servicio:
{: shortdesc}

## Uso de recursos

Los recursos están limitados a los siguientes valores máximos:

-  **Número máximo de modelos publicados**: 200 (plan Lite) 1000 (plan estándar y plan profesional)
-  **Tamaño máximo del modelo Spark ML**: 200 MB
-  **Número máximo de modelos desplegados**: 1000 (plan estándar y plan profesional)

**Nota**: Para entender los límites del plan Lite, consulte la descripción del [Plan Lite](https://console.bluemix.net/catalog/services/machine-learning) en el [Catálogo de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/catalog/).

## Límites de las solicitudes de servicio

El número de solicitudes que se pueden realizar por minuto a API específicas es limitado. Si la aplicación necesita ampliar los límites, [póngase en contacto con el soporte de {{site.data.keyword.Bluemix_notm}}](https://support.ng.bluemix.net/) para aumentar la cuota correspondiente.

## Solicitudes de despliegue

Se aplican los siguientes límites a las solicitudes de API de despliegue:

-  **Periodo**: 60 segundos
-  **Límite predeterminado**: 10

## Solicitudes de predicciones en línea

Se aplican los siguientes límites a las solicitudes de [predicciones en línea](pm_service_api_spark_building.html):

-  **Periodo**: 60 segundos
-  **Límite predeterminado**: 500

## Solicitudes de gestión de recursos

Se aplican los siguientes límites al número total combinado de solicitudes de la lista siguiente:

[Señal](https://watson-ml-api.mybluemix.net/#/Token): solicitudes post, get, delete, put, patch

[Despliegues](https://watson-ml-api.mybluemix.net/#/Deployments): solicitudes post, get, delete, put, patch

-  **Periodo**: 60 segundos
-  **Límite predeterminado**: 100

## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM® Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
