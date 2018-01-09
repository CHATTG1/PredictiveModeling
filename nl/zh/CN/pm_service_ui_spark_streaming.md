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

# 部署流式模型

可以使用 {{site.data.keyword.pm_full}} 服务来部署模型，并通过对已部署的流式模型发出评分请求来生成预测性分析。
{: shortdesc}

**场景名称**：观点分析。

**场景描述**：市场营销代理希望了解关于特定主题的观点。
该代理希望我们开发出一种模型，用于将阐述分类为 POSITIVE 或 NEGATIVE。为此，数据研究员准备了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

请参阅此[文档](https://github.com/pmservice/tweet-sentiment-prediction)以获取更多信息。

## 先决条件

要使用此示例，需要具有以下资源：

* [Message Hub](https://console.bluemix.net/catalog/services/message-hub) 主题详细信息，将用作模型输入（推文文本），以及模型输出的存储（预测结果）。确保创建两个主题：具有推文文本的输入以及输出主题。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服务实例凭证。请使用[此链接](https://console.bluemix.net/catalog/services/apache-spark)来创建凭证。


## 使用样本模型

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**样本**选项卡。
2. 在**样本模型**部分中，找到**观点预测**磁贴，并单击“添加模型”图标 (+)。

“观点预测”样本模型将显示在“模型”选项卡上的可用模型列表中。


## 使用 IBM Message Hub 创建流式部署

1.  转至 {{site.data.keyword.pm_full}}“仪表板”的**模型**选项卡。
2.  从**操作**菜单中，单击**创建部署**。
3.  在**创建部署**表单中，填写**名称**、**描述**和**流类型**字段。
4.  必须提供以下输入：

    **输入连接**：IBM Message Hub 主题详细信息，将用作模型输入（推文），以及模型输出的存储（预测结果）。

    ```
  {
     "connection":{
"kafka_brokers_sasl":[
"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
         "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
         "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
         "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
         "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
         "user":"Dv5kEVNNsbuJ9RFE",
         "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
      },
         "source":{
            "topic":"sinput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **输出连接**

    ```
 {
    "connection":{
"kafka_brokers_sasl":[
"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
         "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
         "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
         "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
         "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
         "user":"Dv5kEVNNsbuJ9RFE",
         "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
      },
   "target":{
      "topic":"soutput",
         "type":"kafka"
      }
   }
    ```
    {: codeblock}

    **Spark 连接**：Spark 服务凭证位于 {{site.data.keyword.Bluemix_notm}} Spark 服务仪表板的“服务凭证”选项卡上。

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

5. 单击**保存**。

预测结果将发送到定义的 MessageHub 主题（输出连接）。

## 获取部署详细信息

可以检查状态以及与部署的模型相关的参数。

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**部署**选项卡。
2. 从**操作**菜单中，单击**查看详细信息**。

## 删除流式部署

如果不再需要该部署，那么可以通过运行查询将其删除（如以下样本所示）。

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的**部署**选项卡。
2. 从**操作**菜单中，单击**删除**。

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。

有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
