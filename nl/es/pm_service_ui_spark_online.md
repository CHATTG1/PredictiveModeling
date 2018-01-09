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

# Despliegue de modelos en línea

Para desplegar un modelo y generar análisis predictivo realizando solicitudes de puntuación en el modelo desplegado, utilice el servicio de {{site.data.keyword.pm_full}}. El siguiente caso de ejemplo proporciona un ejemplo de cómo hacer esto.
{: shortdesc}

**Nombre del caso de ejemplo**: Predicción de línea de productos.

**Descripción del caso de ejemplo**: Una empresa que vende equipamiento para actividades al aire libre desea crear un modelo que ofrezca una previsión del interés de los clientes en su línea de productos. Un experto en datos prepara un modelo predictivo y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo y generar predicciones mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

## Utilización del modelo de ejemplo

1. Vaya al separador **Ejemplos** del panel de control de {{site.data.keyword.pm_full}}.
2. En la sección **Modelos de ejemplo**, busque el mosaico **Predicción de línea de productos**
   y pulse el icono **Añadir modelo** (+). 

El modelo **Predicción de línea de productos** de ejemplo aparece en la lista
de modelos disponibles en el separador **Modelos**.


## Creación del despliegue en línea

1. Vaya al separador **Modelos** del panel de control de {{site.data.keyword.pm_full}}.
2. En el menú **Acciones**, pulse **Crear despliegue**.
3. En el formulario de **Crear despliegue**, introduzca el **nombre**, la **descripción** y el **tipo de despliegue en línea**.
4. Pulse **Guardar**.

El despliegue en línea aparece en la lista de despliegues disponibles en el separador **Despliegues**.

## Obtención de detalles del despliegue

Puede comprobar el estado, la dirección de punto final de puntuación (`Scoring Endpoint`) y los parámetros relacionados con el modelo desplegado.

1. Vaya al separador **Despliegues** del panel de control de {{site.data.keyword.pm_full}}.
2. En el menú **Acciones**, pulse **Ver detalles**.

Tenga en cuenta el valor `Punto final de puntuación`, que es necesario para hacer solicitudes de puntuación.


## Cómo realizar solicitudes de puntuación

Una vez que cree un punto final de puntuación, puede generar predicciones realizando solicitudes de puntuación. En el siguiente caso de ejemplo, los registros del cliente se pasan al modelo de predicción y se devuelve una predicción sobre productos deportivos.

Cabecera del registro de ejemplo:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Registro de ejemplo:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Ejemplo de solicitud:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Ejemplo de salida:

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

Podemos ver, por ejemplo, que un ejecutivo de 55 años está interesado en equipamiento de montañismo (Mountaineering Equipment), mientras que un estudiante de 23 años está interesado en accesorios personales (Personal Accessories).

## Información adicional

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM® Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
