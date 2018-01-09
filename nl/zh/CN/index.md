---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 入门
{: #WMLgettingstarted}

{{site.data.keyword.pm_full}} 是一种 IBM Cloud 服务，支持用户执行两种基本的机器学习操作：培训和评分。
{: shortdesc}

- **培训**是用于优化算法的过程，使算法可以通过数据集进行学习。此操作的输出称为模型。模型包含数学表达式的学习系数。
- **评分**是使用经过培训的模型来预测结果的操作。评分操作的输出是包含预测值的另一个数据集。

{{site.data.keyword.pm_full}} 旨在满足两个主要角色的需求：

- 数据研究员：创建利用数据转换和机器学习算法的机器学习管道。他们通常使用配置页或外部工具来培训和评估其模型。数据研究员往往与数据工程师协作来探索并理解数据。
- 开发者：构建智能应用程序，以使用机器学习模型输出的预测。

虽然培训是机器学习过程中的关键步骤，但 {{site.data.keyword.pm_full}} 也支持通过部署模型并随时间逐渐从模型中获取实际业务值以及通过模型的所有迭代来简化模型运行。

## 先决条件

要在 {{site.data.keyword.Bluemix_short}}“目录”中使用 {{site.data.keyword.pm_full}}，必须在此处创建[服务实例](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/)。通过此设置，可以执行以下任务：

## 步骤

1. [设置 Machine Learning 环境](ml_getting_access.html)。
1. [创建和存储模型](pm_custom_models.html)。
2. [部署模型](pm_service_api_spark_online.html)。
3. 使用应用程序中已部署的模型“`评分端点`”来[获取预测](pm_service_api_spark_building.html)。

## 将 Machine Learning 与 Data Science Experience 一起使用

{{site.data.keyword.pm_full}} 可与 IBM Data Science Experience 相集成。可以在 Data Science Experience 配置页中使用 Machine Learning API 客户机库；必须具有 Machine Learning 实例才能使用模型构建器和流编辑器。

## 将 Machine Learning 与 SPSS Modeler 一起使用

{{site.data.keyword.pm_full}} 与 IBM® SPSS® Modeler 相集成。可以使用 Machine Learning API 来利用高级数学算法。


## 将 Machine Learning 用于环境

{{site.data.keyword.pm_full}} 可以用作将本地环境与云链接在一起的混合解决方案。可以使用 Machine Learning API 来发布模型、进行部署和评分。有关更多信息，请参阅 [DSX：混合模式](https://medium.com/ibm-data-science-experience/dsx-hybrid-mode-91b580450c5b)。

## 关于

{{site.data.keyword.pm_full}} 服务是一组 REST API，可通过任何编程语言进行调用。

{{site.data.keyword.pm_full}} 服务的重点是部署，但可以使用 IBM® SPSS® Modeler 或 IBM® Data Science Experience 来编写和使用模型及管道。SPSS® Modeler 和 Data Science Experience（利用 Spark MLlib 和 Python scikit-learn）提供了多种建模方法，这些方法源自机器学习、人工智能和统计信息。

## 相关链接

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 SPSS 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [Spark 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
