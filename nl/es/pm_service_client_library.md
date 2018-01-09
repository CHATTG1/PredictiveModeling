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

# Bibliotecas de clientes

## Biblioteca de cliente API común <span class='tag--beta'>Beta</span>

Para utilizar el servicio de IBM Watson Machine Learning con el lenguaje de programación Python, puede utilizar PyPI para instalar la biblioteca de cliente `watson-machine-learning-client`.

Para obtener más detalles sobre cómo se hace esto, puede consultar los cuadernos de ejemplo [cuaderno de Spark MLlib](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) o [el cuaderno de scikit-learn](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a) que demuestran las siguientes técnicas:

* persistencia de modelo
* despliegue en línea
* puntuación utilizando el cliente de API común

La documentación para la biblioteca de cliente de API común está disponible [aquí](http://wml-api-pyclient.mybluemix.net/).

### Restricciones

* El cliente de API común está en **beta**.
* Antes de poder utilizar el cliente de API común, debe instalar al menos uno de los paquetes siguientes: XGboost, scikit-learn, o pyspark.
* Solo se da soporte a Python 3.

## Biblioteca de cliente de API de repositorio

Los derivadores de la biblioteca de cliente para las API REST del repositorio del servicio de Machine Learning están desarrollados en los lenguajes de programación Python y Scala.

Para obtener más información sobre el uso de las bibliotecas de cliente de `repositorio` para presentar la persistencia del modelo al repositorio de Watson Machine Learning, consulte los [cuadernos de ejemplo](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7).

Para obtener más información sobre las bibliotecas de repositorio, consulte los temas siguientes:

* [Documentación de Scala Repository Module](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Documentación de Python Repository Module](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### Restricciones

Las bibliotecas de repositorio solo están disponibles al utilizar [IBM Data Science Experience](https://datascience.ibm.com).
