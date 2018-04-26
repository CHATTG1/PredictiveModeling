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

{{site.data.keyword.pm_full}} = [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com). 
{: shortdesc}

有关 {{site.data.keyword.pm_short}} 及其提供的建模算法的更多信息，请参阅 [{{site.data.keyword.DSX_full}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics)。

{{site.data.keyword.pm_full}} 是一种 IBM Cloud 服务，支持用户执行两种基本的机器学习操作：培训和评分。

- **培训**是用于优化算法的过程，使算法可以通过数据集进行学习。此操作的输出称为模型。模型包含数学表达式的学习系数。
- **评分**是使用经过培训的模型来预测结果的操作。评分操作的输出是包含预测值的另一个数据集。

{{site.data.keyword.pm_full}} 旨在满足两个主要角色的需求：

- 数据研究员：创建利用数据转换和机器学习算法的机器学习管道。他们通常使用配置页或外部工具来培训和评估其模型。数据研究员往往与数据工程师协作来探索并理解数据。
- 开发者：构建智能应用程序，以使用机器学习模型输出的预测。

虽然培训是机器学习过程中的关键步骤，但 {{site.data.keyword.pm_full}} 也支持通过部署模型并随时间逐渐从模型中获取实际业务值以及通过模型的所有迭代来简化模型运行。

## 相关链接

有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [Spark 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。