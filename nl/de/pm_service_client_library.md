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

# Clientbibliotheken

## Allgemeine API-Clientbibliothek (<span class='tag--beta'>Betaversion</span>)

Um den IBM Watson Machine Learning-Service mit der Programmiersprache Python zu verwenden, können Sie mit PyPI die Clientbibliothek `watson-machine-learning-client` installieren.

Weitere Informationen zur Vorgehensweise finden Sie in den Beispiel-Notizbüchern [Spark MLlib-Notizbuch](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) oder [scikit-learn-Notizbuch](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a), in denen die folgenden Verfahren vorgeführt werden:

* Modellpersistenz
* Onlinebereitstellung
* Scoring mit dem allgemeinen API-Client

Die Dokumentation für die allgemeine API-Clientbibliothek ist [hier](http://wml-api-pyclient.mybluemix.net/) verfügbar.

### Einschränkungen

* Die allgemeine API-Clientbibliothek ist in der **Betaversion** verfügbar.
* Bevor Sie den allgemeinen API-Client verwenden können, müssen Sie mindestens eines der folgenden Pakete installieren: XGboost, scikit-learn oder pyspark.
* Nur Python 3 wird unterstützt.

## Clientbibliothek für Repository-API

Clientbibliotheks-Wrapper für die Repository-REST-APIs des Machine Learning-Service werden in den Programmiersprachen Python und Scala entwickelt.

Weitere Informationen zur Verwendung der `Repository`-Clientbibliotheken zur Darstelllung der Modellpersistenz für das Watson Machine Learning-Repository finden Sie in den [Beispiel-Notebooks](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7 ).

Weitere Informationen zu den Repository-Bibliotheken finden Sie in den folgenden Themen:

* [Scala Repository Module - Dokumentation](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python Repository Module - Dokumentation](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### Einschränkungen

Repository-Bibliotheken sind nur verfügbar, wenn Sie [IBM Data Science Experience](https://datascience.ibm.com) verwenden.
