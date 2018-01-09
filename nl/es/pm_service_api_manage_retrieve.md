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

# Recuperación de una lista de todos los modelos desplegados actualmente

Recupere un resumen de todos los modelos actualmente desplegados en esta instancia del servicio de {{site.data.keyword.pm_full}}.
{: shortdesc}

```
GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}
```
{: codeblock}

Ejemplo de solicitud:

```
    Content-Type: */*
    Parámetros:
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando la solicitud de resumen de un modelo desplegado se ejecuta correctamente:

```
    Content-Type: application/json
    Status code: 200
    body: una matriz vacía si no se ha desplegado ningún modelo o un resumen de los modelos desplegados...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Respuesta cuando la solicitud de resumen de un modelo desplegado falla:

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
