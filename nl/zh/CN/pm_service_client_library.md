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

# 客户机库

## 公共 API 客户机库 <span class='tag--beta'>Beta</span>

要将 IBM Watson Machine Learning 服务与 Python 编程语言一起使用，可以使用 PyPI 来安装 `watson-machine-learning-client` 客户机库。

有关如何执行此操作的更多详细信息，可以查看 [Spark MLlib 配置页](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550)或 [scikit-learn 配置页](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a)样本配置页，其中说明了以下方法：

* 模型持久性
* 联机部署
* 使用公共 API 客户机进行评分

公共 API 客户机库的文档在[此处](http://wml-api-pyclient.mybluemix.net/)提供。

### 限制

* 公共 API 客户机为 **beta** 版本。
* 必须至少安装以下一个软件包才能使用公共 API 客户机：XGboost、scikit-learn 或 pyspark。
* 仅支持 Python 3。

## 存储库 API 客户机库

Machine Learning 服务的存储库 REST API 的客户机库包装程序是用 Python 和 Scala 编程语言开发的。

有关使用 `repository` 客户机库向 Watson Machine Learning 存储库提供模型持久性的更多信息，请参阅[样本配置页](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)。

有关 repository 库的更多信息，请参阅以下主题：

* [Scala 存储库模块文档](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Python 存储库模块文档](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### 限制

仅当使用 [IBM Data Science Experience](https://datascience.ibm.com) 时，repository 库才可用。
