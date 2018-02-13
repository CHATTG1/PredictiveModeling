---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installing the python client library

## Common API client library

To use the {{site.data.keyword.pm_full}} service with the Python programming language, you can use PyPI to install the `watson-machine-learning-client` client library.

```
pip install watson-machine-learning-client==0.3.701
```
Note: **0.3.701** version of the client contains support for training and experiments.

{: codeblock}


For more details how this is done, you can look at the:
* [TensorFlow with experiments notebook](https://dataplatform.ibm.com/analytics/notebooks/76f98b75-33b3-4bc0-925f-68e18d21c8f2/view?access_token=782249398be11c183dc1cd4d1ab64c1e41ddfbe4895718713f79c9e99a8f752f) that demonstrate the following techniques:
  * TensorFlow experiments
  * model persistence
  * model deployment (online)
  * scoring

Note: experiments are recommended way of models training.

* [TensorFlow training notebook](https://dataplatform.ibm.com/analytics/notebooks/bf0150bf-a239-4de6-809c-2368c62b4494/view?access_token=4cf4726e821fa2806a39faacd693c050b70142488f1d83f6ef5c6f08b8d5685b) that demonstrate the following techniques:
  * TensorFlow model training
  * model persistence
  * model deployment (online)
  * scoring


The documentation for the common API client library is available [here](http://wml-api-pyclient-dev.mybluemix.net/).

### Restrictions

* The common API client is in **beta**.
* Only Python 3 is supported.
