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

# 开发和持久存储定制模型

通过使用 {{site.data.keyword.pm_full}} 服务，可以部署模型，然后通过对已部署的模型发出评分请求来生成预测性分析。
{: shortdesc}

## 使用定制模型

### 场景名称：产品系列预测。

### 场景描述：

客户在欧洲运营着最著名的连锁店之一。他们希望我们弄清楚顾客对其产品系列（例如，“个人辅助用具”、“露营装备”和“户外防护用品”）的兴趣。为此，数据研究员开发了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

### 开发和持久存储定制模型

#### 使用 Data Science Experience

使用 [Data Science Experience](https://console.bluemix.net/catalog/services/data-science-experience) 创建定制模型。注册后，必须登录才能完成以下步骤。

1. 创建组织和空间。第一次登录时，必须提供这些信息。单击**继续**以接受缺省值。
2. 在创建组织后，转至**项目**，然后单击**新建项目**。
3. 指定项目的名称和描述，然后单击**创建**。您指定的项目名称将用作目标容器名称。
4. 创建项目后，可以执行以下某项任务：
   
   *  **添加配置页**，并开始开发您自己的模型，也可以上传以下某个样本配置页：
        *  [使用 Python 开发 Spark MLlib 模型](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)
        *  [使用 Scala 开发 Spark MLlib 模型](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)
        *  [使用 Python 开发 scikit-learn 模型](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)
   * **添加模型**，并使用模型向导开始开发自己的模型。


#### 使用本地环境

您还可以使用所选择的环境，通过在 pypi 上提供的 Watson Machine Learning [公共 API 客户机库]()来开发模型，并在日后发布、部署和评分。有关客户机库的更多信息，请参阅样本[配置页](https://dataplatform.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550&cm_mc_uid=30670837705115063231884&cm_mc_sid_50200000=1509364125)和[文档](pm_service_client_library.html)。

**注：**公共 API 客户机库为 **beta** 版本。

### 定制模型的部署和评分

可以使用该 API 对联机、批处理和流式模型进行部署和评分。

*  [部署联机模型](pm_service_api_spark_online.html)
*  [部署批处理模型](pm_service_api_spark_batch.html)
*  [部署流式模型](pm_service_api_spark_streaming.html)

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM® Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
