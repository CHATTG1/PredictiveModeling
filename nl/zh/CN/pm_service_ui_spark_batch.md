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

# 部署批处理模型

通过使用 {{site.data.keyword.pm_full}} 服务，可以部署模型，然后通过对已部署的模型发出评分请求来生成预测性分析。
{: shortdesc}


**场景名称**：客户满意度预测。

**场景描述**：一家通信公司希望了解哪些客户有流失风险。
展示的模型预测客户流失率。为此，数据研究员开发了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

## 先决条件

要使用此示例，需要具有以下服务：

* [Object Storage](https://console.bluemix.net/catalog/services/object-storage) 实例详细信息，将用作模型的输入（要评分的客户数据）以及模型输出的存储。请从[此处](https://raw.githubusercontent.com/pmservice/wml-sample-models/master/spark/customer-satisfaction-prediction/data/scoreInput.csv)下载样本输入数据 .csv 文件。您应该将输入文件添加到 Object Storage 实例。
* [Apache Spark](https://console.bluemix.net/catalog/services/apache-spark) 服务实例凭证。可以使用[此链接](https://console.bluemix.net/catalog/services/apache-spark)来创建凭证。


## 使用样本模型

1.  转至 {{site.data.keyword.pm_full}}“仪表板”的“样本”选项卡。
2.  在“样本模型”部分中，找到“客户满意度预测”磁贴，并单击“添加模型”图标 (+)。

样本“客户满意度预测”模型将显示在“模型”选项卡上的可用模型列表中。

## 使用 Object Storage 创建批量部署

1.  转至 {{site.data.keyword.pm_full}}“仪表板”的“模型”选项卡。
2.  从**操作**菜单中，单击**创建部署**。
3.  在“创建部署”表单中，提供“名称”、“描述”和“批量类型”。
4.  必须提供以下输入：

    **输入连接**：Object Storage 详细信息，将用作模型输入（要评分的客户数据），以及模型输出的存储（在本例中为 results.csv，此文件会自动创建）。

    ```
       {
          "source":{
"fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **输出连接**

    ```
       {
          "target":{
"fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}

    **Spark 连接**：Spark 服务凭证位于 {{site.data.keyword.Bluemix_short}} Spark 服务仪表板的“服务凭证”选项卡上。

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

5.  单击**保存**。

预测结果会保存到 IBM Object Storage 中的 .csv 文件。下面是样本行。

输入文件预览：

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

输出文件预览：

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## 获取部署详细信息

可以检查状态以及与已部署模型相关的参数。

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的“部署”选项卡。
2. 从**操作**菜单中，单击**查看详细信息**。

## 删除批量部署

如果不再需要该部署，那么可以使用查询将其删除（如以下样本所示）。

1. 转至 {{site.data.keyword.pm_full}}“仪表板”的“部署”选项卡。
2. 从**操作**菜单中，单击**删除**。

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。

有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
