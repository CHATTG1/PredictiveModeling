---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Client libraries

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_client_library.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


## Common API client library

To use the {{site.data.keyword.pm_full}} service with the Python programming language, you can use PyPI to install the `watson-machine-learning-client` client library.

```
pip install watson-machine-learning-client
```
{: codeblock}


For more details how this is done, you can follow the examples in these notebooks:
* [Spark MLlib notebook](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550)
* [Scikit-learn notebook ](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a)
* [Deep Learning notebooks](ml_dlaas_tutorial_pyclient)

that demonstrate the following techniques:

* model training
* experiments
* model persistence
* learning system
* online deployment
* scoring using the common API client

The documentation for the common API client library is available [here](http://wml-api-pyclient.mybluemix.net/).

### Restrictions
* Only Python 3 is supported.

## Repository API client library

Client library wrappers for the repository REST APIs of the {{site.data.keyword.pm_short}} service are developed in the Python and Scala programming languages.

For more information about using `repository` client libraries to present model persistence to the {{site.data.keyword.pm_full}} repository, see the [sample notebooks](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7).

For more information about the repository libraries, see the following topics:

* [Scala Repository Module documentation](http://watson-ml-libs.mybluemix.net/repository-scala/#com.ibm.analytics.ngp.repository.package)
* [Python Repository Module documentation](http://watson-ml-libs.mybluemix.net/repository-python/#package/)

### Restrictions

Repository libraries are available only when you use [IBM Data Science Experience](https://datascience.ibm.com).
