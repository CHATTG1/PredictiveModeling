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

# 持续学习系统

{{site.data.keyword.pm_full}} 持续学习系统可自动监视模型性能、执行重新培训和重新部署，以确保预测质量。
{: shortdesc}

**场景名称**：选择治疗心脏疾病的最佳药物。

**场景描述**：一家生产心脏疾病药物的生物医学公司希望部署一个模型，用于选择治疗心脏疾病的最佳药物。在药物有效性方面出现新的证据时，应该考虑更新模型。为此，数据研究员准备了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，设置学习配置和运行学习迭代来评估、重新培训并重新部署模型。

## 先决条件

要使用此示例，需要具有以下服务：

* [Db2 Warehouse on Cloud](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud)，用于存储反馈数据
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服务

   如果您还没有 Apache Spark 实例，请使用[此链接](https://console.bluemix.net/catalog/services/apache-spark)进行创建。

## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到**心脏疾病药物选择**磁贴，并单击**添加模型**图标 (+)。

样本“心脏疾病药物选择”模型将显示在**模型**选项卡上的可用模型列表中。


## 为已发布的模型设置持续学习系统

1.  转至 {{site.data.keyword.pm_full}}“仪表板”的**模型**选项卡。
2.  从**操作**菜单中，单击**查看详细信息**。
3.  向下滚动到**重新培训详细信息**，然后按**编辑**。
4.  必须提供以下输入：

    **反馈连接**：{{site.data.keyword.dashdbshort}} 详细信息，将用作模型评估和重新培训的输入（反馈数据）。

    ```
    {
        "connection":{
"username": "dash102204",
            "host": "awh-yp-small02.services.dal.bluemix.net",
            "password": "NweTlYwPY6cu",
            "db": "BLUDB"
        },
    "source": {
      "type": "dashdb",
            "tablename": "DRUG_FEEDBACK_DATA"
        }
    }
    ```
    {: codeblock}

    使用以下指示信息在 {{site.data.keyword.dashdbshort}} 中准备 `DRUG_FEEDBACK_DATA` 表。
    
    - 创建 [{{site.data.keyword.dashdbshort}} 服务](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/)实例（提供了入门套餐）。
    - 在 **{{site.data.keyword.dashdbshort_notm}}** 中创建 **DRUG_FEEDBACK_DATA** 表。
      + 从 GitHub 存储库下载 [drug_feedback_data.csv](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/drug-selection/data/drug_feedback_data.csv) 文件。
      + 单击**打开控制台**以开始使用 **{{site.data.keyword.dashdbshort_notm}}**。
      + 单击**装入数据**，然后选择**桌面**装入类型。
      + 将先前下载的文件**拖动**到装入区域，然后单击**下一步**。
      + 选择**模式**以导入数据，然后单击**新建表**。
      + 输入新表的名称，然后单击**下一步**。
      + 使用分号 (;) 作为**字段分隔符**。
      + 单击**下一步**，以使用上传的数据来创建表。

     **注**：您可以使用 [REST API 端点](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)向反馈数据库添加新的反馈记录。

     **最小数据大小**：反馈数据集内至少需要有多少条记录才能开始评估模型（是学习系统迭代的一部分）。

     ```
    20
    ```
     {: codeblock}

     **Spark 连接**：Spark 服务凭证位于 {{site.data.keyword.Bluemix_notm}} Spark 服务仪表板的**服务凭证**选项卡上。

     ```
{
    "credentials": {
"tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",  
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",  
      "cluster_master_url": "https://spark.bluemix.net",  
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",  
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",  
      "plan": "ibm.SparkService.PayGoPersonal"
},
         "version":"2.0"
      }
     ```
    {: codeblock}

    **阈值**：用于触发重新培训过程的阈值。如果度量值（例如，模型评估期间计算的准确性值）小于阈值，那么将触发重新培训。

    ```
    0.8
    ```

     **自动部署经过重新培训的模型**：如果经过重新培训的模型具有的度量值比当前已部署模型更好，那么会部署经过重新培训的模型。

5.  单击**设置**。

## 运行持续学习系统迭代

对于已发布的模型，将以迭代方式进行评估。如果度量值（例如，模型评估期间计算的准确性值）小于阈值，那么将触发重新培训。两个数据集（培训和反馈）都会用于重新培训和评估的这一迭代过程。

1. 要启动新的迭代，请在模型的**详细信息**表单中的**监视**选项卡上，单击**评估**。
3. 要检查迭代结果，请转至“评估事件”表或“图表”视图。 

   可以查看有关基于反馈数据（监视）计算的模型质量的信息。还可以查看经过重新培训的模型（即已创建并评估的新模型版本）的质量相关信息。

4. 要查看经过自动培训的模型及其质量的列表，请单击**查看详细信息** > **概述**，然后转至**版本历史记录**部分。

## 反馈数据存储器

可以使用[反馈端点](http://watson-ml-api.mybluemix.net/#!/Published32Models/post_v3_wml_instances_instance_id_published_models_published_model_id_feedback)将新记录发送到机器学习配置中定义的反馈存储器。目前，支持 {{site.data.keyword.dashdbshort}} 作为反馈数据存储器。如果反馈表不存在，那么服务会创建反馈表。如果该表存在，那么会验证其模式是否与培训表的模式相匹配，并且会向该表额外附加一个名为 `_training` 的列。`_training` 列用于标记在重新培训过程中使用的记录。

有关反馈数据存储器的更多信息和示例，请参阅 [API 文档](pm_service_api_spark_learning_system.html)。

**注：**反馈表由 {{site.data.keyword.pm_full}} 服务管理、修改和使用。

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
