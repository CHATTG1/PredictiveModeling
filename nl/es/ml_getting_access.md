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

# Configuración de su entorno

Para utilizar {{site.data.keyword.pm_full}}, debe poder crear el entorno de aprendizaje de máquina correcto y recuperar las credenciales específicas de ese entorno. Puede crear la instancia de {{site.data.keyword.Bluemix}} ya sea mediante la [interfaz de línea de mandatos de Cloud Foundry](https://github.com/cloudfoundry/cli#getting-started) o mediante la interfaz gráfica de usuario en el {{site.data.keyword.Bluemix}} Dashboard.
{: shortdesc}

## Utilización de IBM Cloud Dashboard

Para utilizar {{site.data.keyword.pm_full}}, debe [crear una instancia](https://console.bluemix.net/catalog/services/machine-learning) del mismo en el {{site.data.keyword.Bluemix_notm}} Dashboard.

Dependiendo del caso de ejemplo, puede que también necesite los recursos siguientes:

- Object Storage (despliegue por lotes)
- Db2 Warehouse on Cloud (despliegue por lotes)
- Apache Spark (despliegue continuo y por lotes, y sistema de aprendizaje continuo)
- Message Hub (despliegue continuo)

Para ver cómo trabajar con el Dashboard para crear las instancias de {{site.data.keyword.Bluemix_notm}} y ver credenciales, vea el siguiente vídeo, que complementa los pasos escritos, que siguen al vídeo.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Si no puede acceder al vídeo incluido en esta página, puede acceder al mismo desde el sitio web de YouTube. (Se abre en un nuevo separador o ventana)">    <img src="images/video.png" alt="Icono de vídeo"></a>IBM Watson Machine Learning: Cómo empezar</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Este vídeo proporciona una descripción general sobre cómo configurar el entorno de aprendizaje de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

## Creación de instancias de {{site.data.keyword.Bluemix_notm}}

Para crear la instancia de {{site.data.keyword.pm_full}}, debe realizar los pasos siguientes:

1. Abra la [página de servicio](https://console.bluemix.net/catalog/services/machine-learning) de {{site.data.keyword.pm_full}}.
2. Regístrese o inicie sesión para crear la instancia de servicio.
3. Especifique un nombre descriptivo para la instancia, elija un espacio y seleccione el plan de datos.
4. Pulse **Crear instancia**.

Se abrirá la instancia.

## Recuperar las credenciales

Para utilizar la instancia de {{site.data.keyword.Bluemix_notm}} (anteriormente conocida como Bluemix), necesita las credenciales de la instancia de servicio. Puede crear y acceder a las credenciales mediante [CLI CF](using_pm_service.html) o el {{site.data.keyword.Bluemix_notm}} Dashboard. Después de enlazar la instancia del servicio {{site.data.keyword.pm_short}} a la aplicación {{site.data.keyword.Bluemix_notm}}, las credenciales de {{site.data.keyword.pm_short}} se añaden a la variable de entorno `VCAP_SERVICES`. Para obtener más información, consulte [uso del servicio con la aplicación](using_pm_service.html).

Para recuperar las credenciales de instancia de {{site.data.keyword.pm_full}}, debe efectuar los pasos siguientes:

1. [Inicie sesión en {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. En la sección **Servicios**, pulse el nombre de servicio cuyas credenciales desee recuperar.
3. En el panel de navegación, pulse **Credenciales de servicio**.
4. En la lista de credenciales, pulse **Ver credenciales**.
5. Si no existen credenciales para este servicio, créelas pulsando **Crear credenciales**.
6. Para copiar las credenciales, pulse el icono de copia.

Para utilizar el servicio de {{site.data.keyword.pm_short}} en la aplicación, debe enlazar el servicio a la aplicación {{site.data.keyword.Bluemix_notm}} y las credenciales necesarias para ejecutar la aplicación están insertadas en la variable de entorno de `VCAP_SERVICES`. Para obtener más información, consulte [interfaz de línea de mandatos de Cloud Foundry](#cloud-foundry-command-line-interface).

## Uso de CLI (interfaz de línea de mandatos) de Cloud Foundry

### Pasos para enlazar el servicio con la aplicación {{site.data.keyword.Bluemix_notm}}

Puede descargar el [código de ejemplo](https://github.com/pmservice/product-line-prediction/blob/master/README.md) Node.js para probar el servicio Machine
Learning. Siga estos pasos para crear la aplicación {{site.data.keyword.Bluemix_notm}} y enlazar el servicio de {{site.data.keyword.pm_short}}. En estos ejemplos se utiliza Node.js porque es un tiempo de ejecución muy usado. Se pueden utilizar otros, como por ejemplo iOS, Ruby, Perl o Java.

También puede realizar los pasos 1 a 3 utilizando la interfaz gráfica de {{site.data.keyword.Bluemix_notm}} en lugar de la herramienta de Cloud Foundry (cf).

1. Utilice el mandato `cf
create-service` para crear una instancia del servicio:

   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   Por ejemplo, el mandato siguiente crea una instancia de servicio de {{site.data.keyword.pm_short}}
   con el plan lite denominado `my_wml_lite` en su espacio de {{site.data.keyword.Bluemix_notm}}:

   ```
   cf create-service pm-20 lite my_wml_lite
   ```
   {: codeblock}

2. Utilice el mandato `cf create-service-key` para crear credenciales de servicio:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Por ejemplo, el mandato siguiente crea credenciales de servicio de {{site.data.keyword.pm_short}}:

   ```
   cf create-service-key "IBM {{site.data.keyword.pm_full}} - my instance" Credentials-1
   ```
   {: codeblock}

3. Utilice el mandato `cf bind-service` para enlazar la instancia de servicio
   `my_wml_lite` a su aplicación.

   ```
   cf bind-service {AppName} my_wml_lite
   ```
   {: codeblock}

   Por ejemplo, el mandato siguiente enlaza la instancia de servicio de {{site.data.keyword.pm_short}}
   `my_wml_lite` a `my_app1` de la aplicación de {{site.data.keyword.Bluemix_notm}}:

   ```
   cf bind-service my_app1 my_wml_lite
   ```
   {: codeblock}

### Acceso a credenciales mediante la variable `VCAP_SERVICES`

Después de enlazar la instancia de servicio de {{site.data.keyword.pm_short}} a la aplicación {{site.data.keyword.Bluemix}}, las credenciales de {{site.data.keyword.pm_short}} se añaden a la variable de entorno `VCAP_SERVICES`:

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "lite",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   La variable de entorno `VCAP_SERVICES` incluye la siguiente información:

<dl>
<dt>plan</dt>
<dd>el plan de {{site.data.keyword.pm_short}} que se utiliza en el suministro de servicio.</dd>
<dt>url</dt><dd>la dirección de la instancia de servicio de {{site.data.keyword.pm_short}}.
<dt>access_key</dt><dd>solo para secuencias de IBM® SPSS® Modeler.</dd>
<dt>instance_id</dt><dd>identificador de instancia exclusivo de {{site.data.keyword.pm_short}}.</dd>
<dt>username</dt><dd>el nombre de usuario, que forma parte de la autorización básica que se necesita para generar una señal de acceso a pasar en todas las solicitudes a esta instancia de servicio.</dd>
<dt>password</dt><dd>la contraseña, que forma parte de la autorización básica que se necesita para generar una señal de acceso a pasar en todas las solicitudes a esta instancia de servicio. </dd>
</dl>

Por ejemplo, puede generar una señal de acceso utilizando el siguiente mandato `curl`:

Ejemplo de solicitud:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Ejemplo de salida:

       {"token":"**********"}
```
{: codeblock}

   El siguiente código Node.js es un ejemplo de cómo obtener los valores
   `username` y `password` desde la variable de entorno de `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
