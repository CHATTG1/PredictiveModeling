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

# Puntuación con un modelo de predicción desplegado

Puede utilizar el servicio {{site.data.keyword.pm_full}} para publicar los datos de entrada que utilizará el modelo desplegado mediante el uso de una llamada de API. Puede utilizar este método para generar y devolver el análisis predictivo en los resultados de la puntuación.
{: shortdesc}

```
POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

Ejemplo de solicitud:

```
    Content-Type: application/json;charset=UTF-8
    Parámetros:
        Parámetros de vía de acceso:
            contextId: el identificador del modelo desplegado que se va a utilizar para procesar esta solicitud de puntuación
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
        Cuerpo: datos de entrada, serie json, ...
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Ejemplo de una respuesta correcta a la solicitud anterior:

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: el resultado de una puntuación, matriz json, ...
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Respuesta cuando la solicitud de puntuación falla:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }  
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
