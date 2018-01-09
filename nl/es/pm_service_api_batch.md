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

# API de trabajo por lotes del servicio Machine Learning para modelos IBM SPSS Modeler

La API de trabajo por lotes para el servicio {{site.data.keyword.pm_full}} da soporte a las
tareas de larga ejecución relacionadas con el entrenamiento de modelo, la evaluación de modelo
y la puntuación por lotes. Esta API gestiona dos tipos de activos: los archivos continuos de IBM® SPSS®
Modeler utilizados en los trabajos por lotes y las definiciones de trabajo
enviadas.
{: shortdesc}

Para cada archivo continuo de SPSS® Modeler que haya cargado,
es posible que haya trabajos de muchos tipos enviados. Si existe la posibilidad de que un trabajo actualice el contenido del archivo continuo de Modeler, el archivo modificado se incluirá en los archivos adjuntos que resultan del trabajo. En un tipo de trabajo de evaluación de modelo, todos los archivos de evaluación generados se encontrarán en los archivos adjuntos resultantes del trabajo.

**Nota**: Los datos que aparecen en el panel de control solo están relacionados con las predicciones en tiempo real, como los datos de la característica de carga.

## Supresión de trabajos

Puede suprimir trabajos. Si se está ejecutando un trabajo, suprimir un trabajo cancela en primer lugar el mismo. Utilice el mandato `DELETE` con `job ID`. Puede cancelar más de un trabajo a la vez pasando varios ID.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

El resultado indica cuántos trabajos a los que hace referencia el ID de la solicitud, se han suprimido. Si este número no coincide con la lista pasada en la solicitud, debe inspeccionar el estado de los
trabajos individuales.

## Comprobación del estado de un trabajo

Puede obtener el estado de su `job ID` en cualquier momento utilizando el mandato `GET`:


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

El archivo JSON que se devuelve indica el estado de trabajo y, si el trabajo se ha completado
correctamente, el URL de datos que puede utilizar para obtener todo el contenido generado.

## Reenvío de un trabajo

Para reenviar un trabajo, utilice el mandato `PUT` con `job ID`. Para reenviar un trabajo, este no debe estar en ejecución.

Si el ID al que hace referencia no existe, la llamada siguiente crea un nuevo trabajo. El estado de retorno es de 201 frente a 200 (indicando cuál de ellos
se ha producido). Debe pasar el archivo JSON de definición de trabajo que se va a utilizar
en esta ejecución.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Reenvíe el trabajo con el valor `content_type` establecido en "application/json". Debe incluir el archivo JSON de definición de trabajo nuevo o actualizado en el cuerpo de la solicitud.

## Envío de un trabajo en relación un archivo continuo de Modeler cargado

Para poner un trabajo en la cola de ejecución, utilice el mandato `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Envíe el trabajo con el valor `content_type` establecido en "application/json". Debe incluir el archivo JSON de definición de trabajo nuevo o actualizado en el cuerpo de la solicitud.

Esta solicitud se devuelve inmediatamente e indica que es correcta si la definición de trabajo
se ha colocado en la cola de ejecución. No existe ninguna prueba de la capacidad de ejecutar correctamente la secuencia de Modeler tal como se ha configurado en la definición de trabajo. El trabajo lo ejecutará uno de los
servidores de trabajo de {{site.data.keyword.pm_short}} de la nube, y puede
supervisar su estado. Si está efectuando el entrenamiento de modelo e indica
que desea realizar una renovación automática, el trabajo sustituirá el archivo continuo original
de Modeler cuando el trabajo se ejecute correctamente.

## Carga de un archivo continuo para utilizarlo en trabajos

**Nota**: El panel de control de {{site.data.keyword.pm_short}} solo se utiliza para la puntuación en tiempo real. No puede utilizarlo para ejecutar trabajos (puntuación por lotes).

Para que un archivo continuo de Modeler sea accesible para los trabajos, utilice el mandato `PUT`:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

con content_type "multipart/form-data" que pasa el archivo en la solicitud.

El nombre exclusivo utilizado (`file ID` en una llamada `PUT`) es a lo que se hará referencia en una llamada `DELETE` a la API de archivo, así como la referencia de modelo en las definiciones de trabajos. Tenga en cuenta que el usuario controla totalmente el espacio de nombres de los archivos que se utiliza en los trabajos. El mandato `PUT` de un archivo en el mismo `file ID` sustituye de manera implícita la copia actual.

Para generar una lista con todos los archivos cargados para los trabajos, utilice un mandato `GET`, pero omita el parámetro `file ID`. Para recuperar un archivo específico, utilice un mandato `GET` con el parámetro `file ID`. También puede suprimir un archivo cargado emitiendo un mandato `DELETE`. Esta acción provoca errores en las ejecuciones de trabajos pendientes que hagan referencia al archivo.

Ejemplo de solicitud:

```
    Content-Type: multipart/form-data
    Parámetros:
        Parámetros de formulario:
            model_file: archivo de modelo a cargar
        Parámetros de vía de acceso:
            contextId: identificador único que se asigna a su modelo o una referencia al modelo desplegado para renovar
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando el despliegue se ejecuta correctamente:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }      
```
{: codeblock}

Respuesta cuando el despliegue falla:

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

## Tipos de trabajo

**Entrenamiento**: este tipo de trabajo indica que deben ejecutarse todos los nodos terminales del "generador de modelos" en la secuencia de Modeler. Tras la finalización correcta del trabajo, habrá una secuencia de Modeler actualizada con los nuggets de modelos recién entrenados en los resultados del trabajo que se podrán recuperar. Si el archivo continuo de Modeler
tiene enlaces de los nodos de generación de modelos a los nuggets de modelos entrenados en la evaluación y las ramas de puntuación, se producirá una renovación de esos nodos.

**Evaluación**: este tipo de trabajo desencadena la ejecución de todos los nodos terminales del "generador de documentos" (especialmente, de los separadores Graphs y Output del cliente Modeler) que generan contenido de archivo de informe estático que se puede volver a pasar al solicitante. La rama de puntuación no se considera parte de este tipo de trabajo.

**Renovación automática**: una versión del tipo de trabajo `TRAINING` en la que, si se realiza correctamente, se actualizará el archivo continuo de Modeler original en la lista de archivos de proceso por lotes. Se presuponen la evaluación y decisión explícita de un suceso de actualización de secuencias de Modeler desplegado para la puntuación en tiempo real y no se cubren en la renovación automática en este momento.

**Puntuación por lotes**: ejecución del nodo terminal al que ha aplicado la opción Utilizar como rama de puntuación, que indica que es la rama de puntuación de este diseño de secuencia de Modeler. La definición de trabajo debe especificar los detalles de exportación y origen.

**Ejecutar secuencia**: la ejecución es similar a pulsar el icono verde de ejecución en
Modeler con la opción Ejecutar este script seleccionada en el separador
Ejecución de las propiedades de la secuencia. El uso cubre la necesidad de realizar una ejecución mediante script del entrenamiento de modelo u otros tipos de trabajo. Todo el control dinámico del script debe gestionarse con los parámetros de secuencia y los valores de parámetro deben pasarse en la definición de trabajo.

## Estado de trabajo

**Pendiente**: se ha enviado la definición del trabajo pero todavía no se ha reclamado en ningún servidor de trabajo.

**Ejecución**: se ha reclamado la definición del trabajo en un servidor de trabajo y se está ejecutando.

**Cancelación**: se está cancelando el trabajo.

**Cancelado**: se ha cancelado el trabajo.

**Error**: no se ha podido ejecutar el trabajo. Los detalles sobre el motivo de este error se proporcionan con la llamada GET sobre el estado del trabajo.

**Correcto**: el trabajo se ha ejecutado correctamente. Cualquier resultado relacionado con este suceso se devuelve en el JSON de la llamada GET sobre el estado del trabajo.

## Detalles de la API de trabajo

`POST /v1/jobs/{id}`

Descripción: envía una definición de trabajo para su ejecución. Provocará un error si el `ID de trabajo` ya existe.

Tipos de contenido:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` especificado por el usuario. Debe ser exclusivo de una instancia del servicio {{site.data.keyword.pm_short}}:

```
@PathParam("id")
```
{: codeblock}

Definición de trabajo JSON (consulte JSON de definición de trabajo):

```
@BodyParam
```
{: codeblock}

Respuestas:

Correcto. El trabajo se envía para su ejecución y se devuelve el JSON que describe el trabajo enviado.

```
@ApiResponse(code = 200)
```
{: codeblock}

Error de trabajo existente. Ya se ha enviado un trabajo con este ID. El mensaje devuelto describe este error.

```
@ApiResponse(code = 409)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

Descripción: crea o actualiza un trabajo. Si un trabajo con este ID no existe, lo crea; en caso contrario, lo actualiza (esta acción vuelve a enviar el trabajo para su ejecución).

Tipos de contenido:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` especificado por el usuario:

```
@PathParam("id")
```
{: codeblock}

Definición de trabajo JSON (consulte JSON de definición de trabajo):

```
@BodyParam
```
{: codeblock}

Respuestas:

Correcto. El trabajo se actualiza y se envía para su ejecución. Devuelve el JSON que describe el trabajo enviado.

```
@ApiResponse(code = 200)
```
{: codeblock}

Correcto. El trabajo se crea y se envía para su ejecución. Devuelve el JSON que describe el trabajo enviado.

```
@ApiResponse(code = 201)
```
{: codeblock}

Otro error. JSON de excepción devuelto.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

Descripción: devuelve una lista de todos los trabajos definidos en esta instancia del servicio Machine Learning.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

Respuestas:

Correcto. Devuelve la matriz JSON de todas las definiciones de trabajo:

```
@ApiResponse(code = 200)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

Descripción: solicita la devolución de una definición de trabajo específica.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` enviado:

```
@PathParam("id")
```
{: codeblock}

Respuestas:

Correcto. Devuelve el JSON que describe el trabajo al que se hace referencia:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error de trabajo no encontrado. Detalles del mensaje devuelto:

```
@ApiResponse(code = 404)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

Descripción: recupera el estado de un trabajo específico.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

`job ID` enviado:

```
@PathParam("id")
```
{: codeblock}

Respuestas:

Correcto. Devuelve el JSON que describe el estado del trabajo:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error de trabajo no encontrado; se devuelven los detalles en el mensaje:

```
@ApiResponse(code = 404)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

Descripción: suprime uno o varios trabajos. Se cancelará el trabajo si está en ejecución.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

Lista de `job ID` o `job ID` enviados con valores de ID separados por coma (,)

```
@PathParam("id" o “id,id,id”)
```
{: codeblock}

Respuestas:

Correcto. Devuelve el número de trabajos suprimidos:

```
@ApiResponse(code = 200)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON de definición de trabajo

El JSON de definición de trabajo contiene las siguientes secciones generales:

Tipo de trabajo y referencia de modelo predictivo

**Nota**: Los datos que se muestran en el panel de control solo están relacionados con las predicciones en tiempo real, incluidos los datos de la característica de descarga.

### Tipos de acción

**TRAINING**: ejecuta el entrenamiento de nodo generador de modelos o los nuggets de modelos de renovación. La secuencia de Modeler actualizada se recupera en los resultados del trabajo.

**EVALUATION**: ejecuta los nodos de análisis y generador de documentos que evalúan el modelo entrenado.

**AUTO_REFRESH**: lleva a cabo la acción TRAINING y actualiza el contenido del archivo en el espacio de carga de archivos de proceso por lotes.

**BATCH_SCORE**: ejecuta la rama de puntuación y exporta los datos resultantes solicitados por la definición de trabajo.

**RUN_STREAM**: ejecuta la secuencia de Modeler tal como se indica en las propiedades de secuencia; en la mayoría de los casos, se utiliza cuando se necesita una ejecución mediante script.

Model: el ID tal como se especifica en la acción de carga de archivos para la referencia de trabajo por lotes. Para obtener más información, consulte Carga de un archivo continuo para utilizarlo en trabajos. Observe que se utiliza `/pm/v1/file`.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Recuerde que el ID debe ser el mismo que el `file ID` utilizado en la API `PUT`. name no es necesario aunque, para el entrenamiento de modelo y la renovación automáticas, el resultado del trabajo se guardará con el nombre definido aquí. Si name no está definido, el servicio {{site.data.keyword.pm_short}} generará
el resultado de acuerdo con las reglas de denominación predefinidas.

### Valores de trabajo

Son necesarios todos los valores para ejecutar este trabajo.

```
"setting": {
     … Valores de exportación
     … Valores de importación
     … Valores de parámetro
     … Valores de control de documento
}
```
{: codeblock}

### Definiciones de conectividad de base de datos

Tipo de base de datos: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL, ProgressOpenEdge, Salesforce,  SQLServer, Sybase, SybaseIQ, Teradata.

Conectividad tal como se especifica en la instancia de servicio de base de datos, muchos tipos de base de datos requieren que se pasen valores específicos a "Options".

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### Configuración de nodo de origen

Haga referencia a la tabla y conectividad de BD utilizadas para originar una rama determinada en la secuencia de Modeler tal como se identifica en el nombre de nodo de origen.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

table es el nombre de la tabla de base de datos. Esta tabla se utiliza para alterar temporalmente el nodo de origen de la secuencia. node es el nombre de origen de datos para la secuencia. dbRef hace referencia a la conectividad de base de datos definida en el objeto dbDefinitions y table es la tabla de base de datos utilizada como entrada de una rama determinada en la secuencia de Modeler. node identifica el nodo de origen original en la secuencia de Modeler que se va sustituir con un nodo de origen de base de datos construido con estos parámetros proporcionados.

### Configuración de nodo de exportación

Haga referencia a la tabla y conectividad de BD utilizadas para persistir datos en una rama determinada de la secuencia de Modeler tal como se identifica en el nombre de nodo terminal.

El método de control utilizado en la persistencia de datos se comunica mediante el atributo insertMode:

**Append**: la tabla debe existir y ser compatible para la inserción

**Create**: se crea la tabla (se devuelve un error si ya existe)

**Drop**: si la tabla a la que se hace referencia ya existe, se elimina y se vuelve a crear

**Refresh**: se suprimen las filas existentes de la tabla antes de insertar filas nuevas
   
table es el nombre de la tabla de base de datos en la que registrar el resultado del trabajo.
node es el nombre de nodo terminal para la secuencia. De forma similar a la configuración de nodo de origen, node identifica el nodo de salida original en la secuencia de Modeler que se va sustituir con un nodo de exportación de base de datos construido con estos parámetros proporcionados.

**Nota**: la configuración de nodo de origen y la configuración de nodo de exportación son importantes para que el trabajo se efectúe correctamente.

### Alteraciones temporales de los valores de parámetro

Para alterar temporalmente los valores predeterminados de los parámetros de nivel de secuencia antes de la ejecución del trabajo, puede especificar los pares nombre/valor para utilizarlos en la definición del trabajo.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Al ejecutar un tipo de trabajo de evaluación, se devuelve el documento de evaluación en el formato de informe deseado

Todos los documentos generados a partir de la ejecución de trabajos de evaluación se empaquetan en un archivo .zip. Solo se ejecutan los nodos del generador de documentos compatibles con el valor reportFormat y su devolverá su contenido tras la correcta ejecución del trabajo.

Los siguientes formatos están soportados: HTML, JPG, PNG, RTF, SAV, TAB y XML.

```
"reportFormat": "HTML"
```
{: codeblock}

### Ejemplo completo de JSON

```
{ 
     "action": "TRAINING", 
     "model": { 
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Detalles de la API de trabajo por lotes

En las secciones siguientes encontrará detalles de la API de gestión de archivos de IBM® SPSS® Modeler para trabajos por lotes.

`PUT /v1/file/{id}`

Descripción: carga un archivo continuo de IBM® SPSS® Modeler para su uso en tareas por lotes.

**Nota**: si ya existe un archivo almacenado con el ID especificado, este se sobrescribirá.

### Tipos de contenido

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### Parámetros

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey") 
```
{: codeblock}

`file ID` especificado por el usuario. Debe ser exclusivo en el repositorio de la instancia de servicio:

```
@PathParam("id") 
```
{: codeblock}

### Respuestas

Correcto. Devuelve el URL que se puede utilizar para descargar el archivo del repositorio de la instancia de servicio:

```
@ApiResponse(code = 200)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

Descripción: suprime el archivo almacenado con el ID especificado del repositorio de la instancia de servicio.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")
```
{: codeblock}

ID especificado por el usuario para el archivo durante la carga:

```
@PathParam("id") 
```
{: codeblock}

Respuestas:

Correcto. El archivo especificado se ha suprimido:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error. Un archivo almacenado con este ID no existía en el repositorio de la instancia de servicio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

Descripción: devuelve una lista de los ID de todos los archivos que se están gestionando en el repositorio de la instancia de servicio para el procesamiento del trabajo por lotes.

Tipos de contenido:

```
@Produces({“application/json”})
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")  
```
{: codeblock}

Respuestas:

Correcto. Devuelve una matriz JSON de valores `file ID`:

```
@ApiResponse(code = 200)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

Descripción: recupera el archivo continuo de IBM® SPSS® Modeler almacenado para su uso en el procesamiento de tareas por lotes con el ID especificado.

Tipos de contenido:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parámetros:

Clave de acceso devuelta como credencial en el suministro o enlace:

```
@QueryParam("accesskey")  
```
{: codeblock}

ID especificado por el usuario para el archivo durante la carga:

```
@PathParam("id") 
```
{: codeblock}

Respuestas:

Correcto. Devuelve el archivo de IBM® SPSS® Modeler:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error. Un archivo almacenado con este ID no existía en el repositorio de la instancia de servicio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Otro error. JSON de excepción devuelto:

```
@ApiResponse(code = 500)
```
{: codeblock}

## Información adicional

Para ver un ejemplo de adopción de un trabajo por lotes en un cuaderno, consulte [De la secuencia SPSS a la puntuación por lotes con Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

Para obtener más información sobre el JSON de definición de trabajo, consulte [JSON de definición de trabajo](#job-definition-json).
