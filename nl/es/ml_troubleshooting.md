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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolución de problemas

A continuación, encontrará las respuestas a las preguntas sobre resolución de problemas más comunes sobre el uso de {{site.data.keyword.pm_full}}.
{: shortdesc}

## Obtención de ayuda y soporte para Machine Learning
{: #gettinghelp}

Si tiene problemas o preguntas al utilizar {{site.data.keyword.pm_short}}, puede obtener ayuda buscando información o planteando preguntas a través de un foro. También puede abrir una incidencia de soporte.

Al utilizar los foros para formular una pregunta, etiquete la pregunta de forma que la puedan ver los equipos de desarrollo de {{site.data.keyword.pm_short}}.

Si tiene preguntas técnicas sobre {{site.data.keyword.pm_short}}, publique su pregunta en <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> y etiquete la pregunta con "ibm-bluemix" y "machine-learning".

Para preguntas sobre el servicio y sobre las instrucciones de iniciación, utilice el foro <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Incluya las etiquetas "machine-learning" y "bluemix".
Consulte [Obtención de ayuda](https://console.bluemix.net/docs/support/index.html#getting-help) para obtener más detalles sobre el uso de los foros.

Para obtener información sobre cómo abrir una incidencia de soporte de IBM o sobre los niveles de soporte y la gravedad de las incidencias, consulte [Cómo obtener soporte](https://console.bluemix.net/docs/support/index.html#contacting-support).

## No se ha proporcionado la señal de autorización.
{: #ts_missing_authorization_token}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La señal de autorización no se ha proporcionado en la cabecera `Authorization`.
{: tsCauses}
 
Pase la señal de autorización en la cabecera `Authorization`.
{: tsResolve}
       
## Señal de autorización no válida.
{: #ts_invalid_authorization_token}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La señal de autorización que se ha proporcionado no se puede descodificar ni analizar.
{: tsCauses}
 
Pase la señal de autorización correcta en la cabecera `Authorization`.
{: tsResolve}
       
## La señal de autorización y el valor instance_id utilizados en la solicitud no son iguales.
{: #ts_not_matching_authorization_token}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La señal de autorización que se ha utilizado no está generada para la instancia de servicio en la que se ha utilizado.
{: tsCauses}
 
Pase la señal de autorización en la cabecera `Authorization` correspondiente a la instancia de servicio que se está utilizando.
{: tsResolve}
       
## La señal de autorización ha caducado.
{: #ts_expired_authorization_token}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La señal de autorización ha caducado.
{: tsCauses}
 
Pase una señal de autorización no caducada en la cabecera `Authorization`.
{: tsResolve}
       
## La clave pública necesaria para la autenticación no está disponible.
{: #ts_missing_public_key}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Es un problema del servicio interno.
{: tsCauses}
 
El equipo de soporte debe ser el encargado de solucionar este problema.
{: tsResolve}
       
## La operación ha agotado el tiempo de espera después de {{timeout}}
{: #ts_operation_timeout}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
El tiempo de espera se ha agotado durante la ejecución de la operación solicitada.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo.
{: tsResolve}
       
## Excepción no controlada de tipo {{type}} con {{status}}
{: #ts_unhandled_exception_with_status}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Es un problema del servicio interno.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo. Si vuelve a ocurrir, el equipo de soporte debe ser el encargado de solucionar este problema.
{: tsResolve}
       
## Excepción no controlada de tipo {{type}} con {{response}}
{: #ts_unhandled_exception_with_response}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Es un problema del servicio interno.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo. Si vuelve a ocurrir, el equipo de soporte debe ser el encargado de solucionar este problema.
{: tsResolve}
       
## Excepción no controlada de tipo {{type}} con {{json}}
{: #ts_unhandled_exception_with_json}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Es un problema del servicio interno.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo. Si vuelve a ocurrir, el equipo de soporte debe ser el encargado de solucionar este problema.
{: tsResolve}
       
## Excepción no controlada de tipo {{type}} con {{message}}
{: #ts_unhandled_exception_with_message}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Es un problema del servicio interno.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo. Si vuelve a ocurrir, el equipo de soporte debe ser el encargado de solucionar este problema.
{: tsResolve}
       
## No se ha encontrado el objeto solicitado.
{: #ts_not_found}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
No se ha encontrado el recurso de solicitud.
{: tsCauses}
 
Asegúrese de hacer referencia al recurso existente.
{: tsResolve}
       
## La base de datos subyacente ha notificado demasiadas solicitudes.
{: #ts_too_many_cloudant_requests}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
El usuario ha enviado demasiadas solicitudes en un intervalo de tiempo determinado.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo.
{: tsResolve}
       
 ## La definición de la evaluación no se ha definido en artifactModelVersion ni en el despliegue. Se debe especificar \" +\n      \"al menos en uno de los dos sitios.
{: #ts_missing_evaluation_definition}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La configuración de aprendizaje no contiene toda la información necesaria
{: tsCauses}
 
Proporcione `definition` en `learning configuration`
{: tsResolve}
       
## La evaluación requiere la configuración de aprendizaje especificada para el modelo.
{: #ts_missing_learning_configuration}
 
No se puede crear `learning iteration`.
{: tsSymptoms}
 
No se ha definido `learning configuration` para el modelo.
{: tsCauses}
 
Cree `learning configuration` e intente volver a crear `learning iteration`.
{: tsResolve}
       
## La evaluación requiere que se proporcione la instancia spark en la cabecera `X-Spark-Service-Instance`
{: #ts_missing_spark_definition_for_evaluation}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
No está toda la información necesaria en `learning configuration`
{: tsCauses}
 
Proporcione `spark_service` en la configuración de aprendizaje o en la cabecera `X-Spark-Service-Instance`
{: tsResolve}
       
## El modelo no contiene ninguna versión.
{: #ts_missing_latest_model_version}
 
No se puede crear el despliegue ni de establecer la configuración de aprendizaje.
{: tsSymptoms}
 
Hay una incoherencia relacionada con la permanencia del modelo.
{: tsCauses}
 
Intente volver a persistir el modelo e intente realizar la acción de nuevo.
{: tsResolve}
       
## La operación de parche solo puede modificar configuración de aprendizaje existente.
{: #ts_patch_non_existing_learning_configuration}
 
No se puede invocar el método REST API del parche para aplicar el parche a la configuración de aprendizaje.
{: tsSymptoms}
 
No se ha definido `learning configuration` para este modelo o el modelo no existe.
{: tsCauses}
 
Asegúrese de que el modelo exista y ya tenga definida la configuración de aprendizaje.
{: tsResolve}
       
## La operación de parche espera exactamente una operación de sustitución.
{: #ts_patch_multiple_ops}
 
No se puede aplicar el parche al despliegue.
{: tsSymptoms}
 
La carga útil del parche contiene más de una operación o la operación de parche es distinta a `replace`.
{: tsCauses}
 
Utilizar únicamente la operación `replace` en la carga útil del parche
{: tsResolve}
       
## En la carga útil proporcionada faltan los campos necesarios: FIELD o los valores de los campos están dañados.
{: #ts_invalid_request_payload}
 
No se puede procesar una acción relacionada con el acceso al conjunto de datos subyacente.
{: tsSymptoms}
 
El acceso al conjunto de datos no se ha definido correctamente.
{: tsCauses}
 
Corrija la definición de acceso del conjunto de datos.
{: tsResolve}
       
## El método de evaluación proporcionado: METHOD no se soporta. Valores soportados: VALUE.
{: #ts_evaluation_method_not_supported}
 
No se puede crear la configuración de aprendizaje.
{: tsSymptoms}
 
Se ha utilizado un método de evaluación incorrecto para crear la configuración de aprendizaje.
{: tsCauses}
 
Utilice uno de los métodos de evaluación soportados siguientes: `regression`, `binary`, `multiclass`.
{: tsResolve}
       
## Solo puede haber una evaluación activa por modelo. La solicitud no se ha podido completar porque hay una evaluación activa: {{url}}
{: #ts_active_evaluation_conflict}
 
No se puede crear otra iteración de aprendizaje.
{: tsSymptoms}
 
Solo puede haber una evaluación en ejecución para el modelo.
{: tsCauses}
 
Consulte la evaluación en ejecución o espere hasta que termine e inicie la nueva.
{: tsResolve}
       
## El tipo de despliegue {{type}} no se soporta.
{: #ts_not_supported_deployment_type}
 
No se puede crear el despliegue.
{: tsSymptoms}
 
No se ha utilizado un tipo de despliegue soportado.
{: tsCauses}
 
Se debería haber utilizado un tipo de despliegue soportado.
{: tsResolve}
       
## Entrada incorrecta: ({{message}})
{: #ts_deserialization_error}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Se ha producido un problema con el análisis de JSON.
{: tsCauses}
 
Asegúrese de que se analiza el JSON adecuado en la solicitud.
{: tsResolve}
       
## Datos insuficientes: no se ha podido calcular la métrica {{name}}
{: #ts_missing_metric}
 
La iteración de aprendizaje ha fallado
{: tsSymptoms}
 
No se ha podido calcular el valor de la métrica con el umbral definido porque no había suficientes datos de comentarios
{: tsCauses}
 
Revise y mejore los datos del origen de datos `feedback_data_ref` en `learning configuration`
{: tsResolve}
       
## Para el tipo {{type}}, la instancia spark se debe proporcionar en la cabecera `X-Spark-Service-Instance`
{: #ts_missing_prediction_spark_definition}
 
No se puede crear el despliegue
{: tsSymptoms}
 
Los despliegues `batch` y `streaming` necesitan que se proporcione la instancia spark
{: tsCauses}
 
Proporcione la instancia spark en la cabecera `X-Spark-Service-Instance`
{: tsResolve}
       
## La acción {{action}} ha fallado con el mensaje {{message}}
{: #ts_http_client_error}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Se ha producido un problema al invocar el servicio subyacente.
{: tsCauses}
 
Si hay instrucciones para solucionar el problema, sígalas. Póngase en contacto con el equipo de soporte si no hay instrucciones en el mensaje o no resuelven el problema.
{: tsResolve}
       
## La vía de acceso `{{path}}` no está permitida. La única vía de acceso permitida para la secuencia de parche es `/status`
{: #ts_wrong_stream_patch_path}
 
No se puede aplicar el parche al despliegue continuo.
{: tsSymptoms}
 
Se ha utilizado una vía de acceso incorrecta para aplicar el parche al despliegue `stream`.
{: tsCauses}
 
Aplique el parche al despliegue `stream` con la opción de vía de acceso soportada `/status` (permite iniciar/detener el procesamiento de secuencia.)
{: tsResolve}
       
## La operación de parche no está permitida para instancias de tipo `{{$type}}`
{: #ts_patch_not_supported}
 
No se puede aplicar el parche al despliegue.
{: tsSymptoms}
 
Se está aplicando el parche a un tipo de despliegue incorrecto.
{: tsCauses}
 
Aplique el parche al tipo de despliegue `stream`.
{: tsResolve}
       
## La conexión de datos `{{data}}` no es válida para feedback_data_ref
{: #ts_invalid_feedback_data_connection}
 
No se puede crear `learning configuration` para el modelo.
{: tsSymptoms}
 
Se ha utilizado un origen de datos no soportado al definir feedback_data_ref.
{: tsCauses}
 
Utilice solo el tipo de origen de datos soportado `dashdb`
{: tsResolve}
       
## La vía de acceso {{path}} no está permitida. La única vía de acceso permitida para el modelo de parche es `/deployed_version/url` o `/deployed_version/href` para V2
{: #ts_patch_model_path_not_allowed}
 
No hay ninguna opción para aplicar el parche al modelo.
{: tsSymptoms}
 
Se ha utilizado una vía de acceso incorrecta durante la aplicación del parche al modelo.
{: tsCauses}
 
Aplique el parche al modelo con la vía de acceso soportada que permite actualizar la versión del modelo desplegado.
{: tsResolve}
       
## Error de análisis: {{msg}}
{: #ts_parsing_error}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
La carga útil solicitada no se ha podido analizar correctamente.
{: tsCauses}
 
Asegúrese de que la carga útil solicitada sea correcta y se pueda analizar correctamente.
{: tsResolve}
       
## Entorno de tiempo de ejecución para el modelo seleccionado: {{env}} no se soporta para `learning configuration`. Entornos soportados: [{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
No hay ninguna opción para crear `learning configuration`
{: tsSymptoms}
 
El modelo para el que se ha intentado crear `learning_configuration` no se soporta.
{: tsCauses}
 
Cree `learning configuration` para el modelo que tiene el tiempo de ejecución soportado.
{: tsResolve}
       
## El plan actual \'{{plan}}\' solo permite {{limit}} despliegues
{: #ts_deployments_plan_limit_reached}
 
No se puede crear el despliegue.
{: tsSymptoms}
 
El plan actual ha alcanzado el límite de despliegues.
{: tsCauses}
 
Actualice a un plan que no tenga esa limitación.
{: tsResolve}
       
## La definición de la conexión a base de datos no es válida ({{code}})
{: #ts_sql_error}
 
No se puede utilizar la funcionalidad `learning configuration`.
{: tsSymptoms}
 
La definición de la conexión a base de datos no es válida.
{: tsCauses}
 
Intente arreglar el problema descrito por `code` que ha devuelto la base de datos subyacente.
{: tsResolve}
       
## Se han producido problemas al conectar con la base de datos del {{system}}
{: #ts_stream_tcp_error}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Se ha producido un problema al conectar con el sistema subyacente. Es posible que se trate de un problema temporal.
{: tsCauses}
 
Intente invocar la operación deseada de nuevo. Si vuelve a ocurrir, póngase en contacto con el equipo de soporte.
{: tsResolve}
       
## Error al extraer la cabecera de X-Spark-Service-Instance: ({{message}})
{: #ts_spark_header_deserialization_error}
 
No se puede invocar la API REST que necesita credenciales de Spark
{: tsSymptoms}
 
Se ha producido un problema con la decodificación base 64 o con el análisis de las credenciales de Spark.
{: tsCauses}
 
Asegúrese de que las credenciales de Spark estaban correctamente codificadas en base 64. Para obtener más información, consulte la documentación.
{: tsResolve}
       
## Esta funcionalidad está prohibida para los usuarios que no sean de la versión beta.
{: #ts_not_beta_user}
 
La API REST solicitada no se puede invocar correctamente.
{: tsSymptoms}
 
La API REST que se ha invocado actualmente está en versión beta.
{: tsCauses}
 
Si le interesa participar, póngase en lista de espera. Para obtener más información, consulte la documentación.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
La API REST no se puede invocar correctamente.
{: tsSymptoms}
 
Se ha producido un problema al invocar el servicio subyacente.
{: tsCauses}
 
Si hay instrucciones para solucionar el problema, sígalas. Póngase en contacto con el equipo de soporte si no hay instrucciones en el mensaje o no resuelven el problema.
{: tsResolve}
       
## Se ha superado el límite de tasa.
{: #ts_rate_limit_exceeded}
 
Se ha superado el límite de tasa.
{: tsSymptoms}
 
Se ha superado el límite de tasa del plan actual.
{: tsCauses}
 
Para solucionar este problema, adquiera otro plan con un límite de tasa superior
{: tsResolve}
       
## El parámetro de consulta `{{paramName}}` tiene un valor no válido: {{value}}
{: #ts_invalid_query_parameter_value}
 
Se ha producido un error de validación porque se ha pasado un valor de parámetro de consulta incorrecto.
{: tsSymptoms}
 
Error al obtener el resultado de la consulta.
{: tsCauses}
 
Corrija el valor del parámetro de consulta. Para obtener más información, consulte la documentación.
{: tsResolve}
       
## Tipo de señal no válido: {{type}}
{: #ts_invalid_token_type}
 
Error relacionado con el tipo de señal.
{: tsSymptoms}
 
Error en la autorización.
{: tsCauses}
 
La señal debe empezar por el prefijo `Bearer`
{: tsResolve}
       
## Formato de señal no válido. Se debe utilizar el formato de señal portadora.
{: #ts_invalid_token_format}
 
Error relacionado con el formato de señal.
{: tsSymptoms}
 
Error en la autorización.
{: tsCauses}
 
La señal debe ser una señal portadora y debe empezar por el prefijo `Bearer`
{: tsResolve}

## Falta el archivo JSON de entrada o no es válido: 400
{: #os_invalid_input}

El siguiente mensaje aparece cuando se intenta puntuar en línea: **Falta el archivo JSON de entrada o no es válido**.
{: tsSymptoms}

Este mensaje se muestra cuando la carga útil de entrada de puntuación no coincide con el tipo de entrada previsto que se necesita para puntar el modelo. Concretamente, puede ser por los motivos siguientes:

- La carga útil de entrada está vacía.
- El esquema de carga útil de entrada no es válido.
- Los tipos de datos de entrada no coinciden con los tipos de datos previstos.
{: tsCauses}

Corrija la carga útil de entrada. Asegúrese de que la carga útil tenga la sintaxis correcta, un esquema válido y tipos de datos adecuados. Una vez aplicadas las correcciones, intente puntuar en línea de nuevo. Para problemas con la sintaxis, verifique el archivo JSON utilizando el mandato `jsonlint`.
{: tsResolve}

## La señal de autorización ha caducado: 401
{: #os_expired_authorization_token}

El siguiente mensaje aparece cuando se intenta puntuar en línea: **La autorización ha fallado**.
{: tsSymptoms}

Este mensaje aparece cuando la señal que se utiliza para puntuar ha caducado.
{: tsCauses}

Vuelva a generar la señal para esta instancia de {{site.data.keyword.pm_full}} y vuelva a intentarlo. Si el problema persiste, póngase en contacto con el soporte de IBM.
{: tsResolve}

## Identificación de despliegue desconocida:404
{: #os_unkown_depid}

El siguiente mensaje aparece cuando se intenta puntar en línea una **identificación de despliegue desconocida**. {: osSymptoms}

Este mensaje aparece cuando el ID de despliegue que se utiliza para puntuar no existe.
{: tsCauses}

Asegúrese de haber proporcionado el ID de despliegue correcto. De lo contrario, despliegue el modelo con el ID de despliegue e intente puntar de nuevo.
{: tsResolve}

## Error de servidor interno:500
{: #os_internal_error}

El siguiente mensaje aparece cuando se intenta puntuar en línea: **Error de servidor interno**  {: osSymptoms}

Este mensaje aparece cuando falla el flujo de datos en sentido descendente del que depende la puntuación en línea.
{: tsCauses}

Espere e intente puntuar en línea de nuevo. Si vuelve a fallar, póngase en contacto con el soporte de IBM.
{: tsResolve}
              
