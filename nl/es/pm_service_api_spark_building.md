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

# Creación de aplicaciones de análisis predictivo

Puede utilizar el servicio {{site.data.keyword.pm_full}} para desplegar una aplicación Node.js.
{: shortdesc}

**Nombre del caso de ejemplo**: Predicción de línea de productos.

**Descripción del caso de ejemplo**: Nuestro cliente es el propietario de una de las cadenas de grandes almacenes más famosas de Europa. Quieren que estudiemos los intereses de sus clientes en relación con su línea de productos, como Accesorios personales, Equipamiento de camping y Protección aire libre.
Un experto en datos desarrolla un modelo predictivo y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en preparar y desplegar la aplicación Node.js que recomienda líneas de productos deportivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

1. Inicie sesión en {{site.data.keyword.Bluemix_short}} y cree una instancia de {{site.data.keyword.pm_full}}.
2. Inicie el panel de control de {{site.data.keyword.pm_full}}.
3. Vaya al separador **Ejemplos**.
4. En la sección **Modelos de ejemplo**, busque el mosaico **Predicción de línea de productos**
   y pulse el icono **Añadir modelo** (+). El modelo
   Predicción de línea de producto de ejemplo aparece en la lista de modelos
   disponibles en el separador **Modelos**.

   **Nota**: Si desea utilizar su propio proyecto y modelos de Data Science Experience
   en lugar de los ejemplos, debe guardar de forma permanente un modelo personalizado en el repositorio de
   {{site.data.keyword.pm_short}}. Para ello puede utilizar la API REST o bibliotecas de cliente. Para obtener información más detallada, consulte Desarrollo y permanencia del modelo personalizado.

5. En el menú **Acción**, pulse **Crear despliegue** para desplegar el
   modelo de Predicción de línea de productos como un despliegue en línea.
6. Especifique el nombre del despliegue (por ejemplo, Predicción de línea de productos) y pulse Guardar. Ahora debería ver el despliegue Predicción de línea de productos en la lista Despliegues.
7. En el menú Acción, seleccione Ver detalles para su despliegue.
   El Punto final de puntuación que se muestra en el panel de detalles se utilizará para ejecutar solicitudes de puntuación.
8. Para ejecutar solicitudes de puntuación mediante la API REST, se necesita un punto final de puntuación y una señal de autorización. Para obtener más información, consulte Despliegue de modelos en línea.
9. Experimente con la aplicación Node.js de ejemplo, que se encuentra en la siguiente ubicación:
   https://github.com/pmservice/product-line-prediction.

   Para desplegar automáticamente el código de aplicación de ejemplo en el espacio de
   {{site.data.keyword.Bluemix_short}}, vaya al separador Ejemplos y, en la sección Aplicaciones de
   ejemplo, seleccione la aplicación Predicción de línea de productos
   y despliéguela pulsando el icono (+).
   Autentíquese en el formulario de DeployToBluemix si se le solicita.

   Ahora debería ver la aplicación de ejemplo en el Panel de control de {{site.data.keyword.Bluemix_short}}
   en la sección Todas las apps.

10. Pulse en la aplicación para ver sus detalles. Aquí puede
    modificar los detalles de la aplicación, como por ejemplo el número de instancias o la
    asignación de memoria.
11. Enlace la aplicación con la instancia de
    {{site.data.keyword.pm_short}}. En el separador **Conexiones**, pulse **Conectar existente**.
    Después de volverse a transferir, la aplicación se conecta a la instancia de servicio.
12. Ejecute la aplicación utilizando la dirección de ruta.
13. En la aplicación, seleccione su despliegue de Predicción de línea de productos. Ahora está listo para utilizar los registros de muestra
    y los resultados de la predicción.
    
## Información adicional

¿Preparado para ponerse en marcha? Para crear una instancia de servicio o enlazar
una aplicación, consulte [Utilización del servicio con modelos Spark y Python](using_pm_service_dsx.html) o
[Utilización del servicio con modelos IBM® SPSS®](using_pm_service.html).

Para obtener más información sobre la API, consulte [API del servicio para modelos Spark y Python](pm_service_api_spark.html) o [API del servicio para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obtener más información sobre IBM® SPSS® Modeler y los algoritmos de modelado que proporciona,
consulte [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obtener más información sobre IBM Data Science Experience y los algoritmos de modelado que proporciona, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
