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

# Configuración del entorno de aprendizaje de máquina y recuperación de sus credenciales

Para utilizar IBM Watson Machine Learning, debe poder crear el entorno de aprendizaje de máquina correcto y recuperar las credenciales que son específicas de ese entorno.

## Configuración de su entorno

Para utilizar IBM Watson Machine Learning, debe configurar instancias de los siguientes elementos en el panel de control de IBM Cloud:

- Watson Machine Learning
- Object Storage
- Apache Spark

Además de las instancias obligatorias, algunos de los cuadernos y guías de aprendizaje también utilizan las instancias siguientes. Para utilizar las guías de aprendizaje, debe configurar estos servicios también.

- Python Flask
- Natural Language Understanding
- Deep learning experiments

### Creación de instancias de IBM Cloud

Vea este vídeo para ver cómo crear las instancias de IBM Cloud y ver las credenciales. A continuación, siga los pasos después del vídeo para configurar su propio entorno. Consulte la sección <a href="#retrieving-your-credentials">Recuperación de las credenciales</a> para obtener los pasos para ver sus credenciales.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>Figura 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="Si no puede acceder al vídeo incluido en esta página, puede acceder al mismo desde el sitio web de YouTube. (Se abre en un nuevo separador o ventana)">    <img src="images/video.png" alt="Icono de vídeo"></a>IBM Watson Machine Learning: Cómo empezar</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>Este vídeo proporciona una descripción general sobre cómo configurar el entorno de aprendizaje de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Para crear instancias de Watson Data Platform, debe realizar los pasos siguientes:

1. [Inicie una sesión en IBM Cloud](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. En el panel de navegación, pulse **Datos y análisis**.

   Verá una pantalla centrada en servicios de datos. Puede volver aquí periódicamente para trabajar con los servicios de datos y análisis desde una página fácil de utilizar.

3. Pulse el separador **Análisis** y, a continuación, pulse **Instancias**.
4. Pulse **Nueva instancia**.
5. Configure instancias y almacenamiento de datos.

   Especifique un nombre descriptivo para la instancia, elija un espacio, y seleccione su plan de datos (encontrará la comparación de planes y más detalles sobre los precios en esta página). IBM Cloud rellena automáticamente el nombre de almacenamiento de objetos con su nombre de instancia. Es probable que necesite almacenamiento de datos para el proyecto de Spark, de modo que elija un espacio y un plan aquí también.

6. Pulse **Crear instancia**.

Se abrirá la instancia.

### Añadir instancias de Bluemix existentes para un proyecto de Data Science Experience

Visualice este vídeo para ver cómo crear un proyecto y configurarlo para utilizar Watson Machine Learning.

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>Figura 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="Si no puede acceder al vídeo incluido en esta página, puede acceder al mismo desde el sitio web de YouTube. (Se abre en un nuevo separador o ventana)">    <img src="images/video.png" alt="Icono de vídeo"></a>IBM Watson Machine Learning: Crear un proyecto para Watson Machine Learning</span></figcaption>
<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>Este vídeo proporciona una descripción general sobre cómo configurar el entorno de aprendizaje de máquina.</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Si ya tiene instancias, pero no las ha enlazado a un proyecto en Data Science Experience, debe realizar los pasos siguientes:

1. [Inicie sesión en IBM Data Science Experience](https://datascience.ibm.com).
2. Pulse **Proyectos** > **Ver todos los proyectos**.
3. Pulse el separador **Valores**.
4. Para añadir un servicio, en el panel **Servicios asociados**, pulse **añadir servicio asociado**, seleccione un servicio, complete la información de configuración, y pulse **Guardar**.
5. Para añadir una señal de acceso, realice los pasos siguientes:
   6. En el panel **Señales de acceso**, pulse **crear nueva señal**.
   7. En el recuadro **Nombre**, escriba un nombre.
   8. En el recuadro **Rol de acceso para proyecto**, seleccione un rol, ya sea **Visor** o **Editor**.
   9. Pulse **Añadir**.

6. Para conectarse a un repositorio GitHub, en el **URL de repositorio**, escriba la ubicación de repositorio. Si no tiene una señal de acceso personal ya configurada para acceder a GitHub, debe crearla ahora pulsando **Valores**. Cuando haya terminado, pulse **Añadir**.


## Recuperar las credenciales

Para utilizar las instancias, los servicios y los modelos de Bluemix en cuadernos y aplicaciones, debe poder insertar sus credenciales, señales, y puntos finales de anotación en el código que se utiliza para el proceso.

1. [Inicie sesión en Bluemix](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter).
2. Desplácese hasta la sección **Servicios** y pulse el nombre de servicio cuyas credenciales desee recuperar.
3. En el panel de navegación, pulse **Credenciales de servicio**.
4. En la lista de credenciales, pulse **Ver credenciales**.
5. Si no existen credenciales para este servicio, puede crearlas pulsando **Crear credenciales**.
6. Para copiar las credenciales, pulse el icono de copia.

Dependiendo del servicio, puede tener distintos campos, como por ejemplo `username` y `password`. Debe copiar los valores como aparecen para utilizarlos en el código.

## Información adicional

[Una guía de aprendizaje rápida de aprendizaje profundo](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

