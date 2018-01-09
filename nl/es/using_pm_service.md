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

# Utilización del servicio

Utilizando los métodos de modelado en la paleta de modelado de IBM® SPSS® Modeler,
puede derivar nueva información desde los modelos predictivos de desarrollo y de datos. Cada método tiene determinados puntos fuertes
y resulta más adecuado para determinados tipos de problemas de aprendizaje de máquina.
{: .shortdesc}

Para obtener detalles
sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona, consulte
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Después de implementar los requisitos de entrada y salida de la aplicación {{site.data.keyword.Bluemix_notm}}
y el diseño del sistema de puntuación de IBM® SPSS® Modeler,
el analista de datos puede modificar cualquier aspecto interno de la
rama de puntuación. El analista de datos puede incluso cambiar los algoritmos del modelo utilizados en una operación de renovación, lo que garantiza la capacidad de ajustar mejor el análisis predictivo sin tener que volver a escribir las aplicaciones.


## Pasos para enlazar el servicio con la aplicación {{site.data.keyword.Bluemix_notm}}

Siga estos pasos para crear la aplicación {{site.data.keyword.Bluemix_notm}} y enlazarla al servicio {{site.data.keyword.pm_short}}.

1. Descargue el código de aplicación de ejemplo Node.js desde el [repositorio de GitHub](https://github.com/pmservice/customer-satisfaction-prediction).

2. Utilice el mandato `cf
create-service` para crear una instancia del servicio:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Por ejemplo:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   Este mandato crea una instancia de servicio {{site.data.keyword.pm_short}}
   con el plan lite llamado my_pm_lite en el espacio de {{site.data.keyword.Bluemix_notm}}.

3. Utilice el mandato `cf create-service-key` para crear credenciales de servicio:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Por ejemplo:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Este mandato crea credenciales de servicio de {{site.data.keyword.pm_short}}.

4. Utilice el mandato `cf bind-service` para enlazar la instancia del servicio
   `my_pm_lite` a su aplicación.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   Por ejemplo:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   Este mandato enlaza la instancia del servicio {{site.data.keyword.pm_short}}
   `my_pm_lite` a la aplicación de {{site.data.keyword.Bluemix_notm}} my_app1.

5. Credenciales de {{site.data.keyword.pm_short}}:

   Después de enlazar la instancia de servicio de {{site.data.keyword.pm_short}} a la aplicación {{site.data.keyword.Bluemix_notm}}, las credenciales de {{site.data.keyword.pm_short}} se añaden a la variable de entorno `VCAP_SERVICES`:

```
    {   
        "pm-20": {      
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   La variable de entorno `VCAP_SERVICES` incluye la siguiente información:

   <dl>

   <dt>plan</dt>
   <dd>El plan de {{site.data.keyword.pm_short}} que se utiliza en el suministro del servicio.</dd>

   <dt>url</dt>
   <dd>La dirección de la instancia de servicio de {{site.data.keyword.pm_short}}.</dd>

   <dt>access_key</dt>
   <dd>El parámetro de la consulta accessKey que se va a pasar en todas las solicitudes a esta instancia de servicio.</dd>

   </dl>

Por ejemplo:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Código Node.js de ejemplo que muestra cómo obtener el valor de accessKey a partir de la variable de entorno `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
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
