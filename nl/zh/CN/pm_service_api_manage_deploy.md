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

# 部署或刷新预测模型

要使用此服务部署或刷新预测模型，请使用 API 调用来上传包含使用 IBM® SPSS® Modeler 开发的评分分支的文件。此文件可用于对应用程序中的数据进行评分。为了方便在后续服务调用中引用已部署的模型，系统会为每个模型文件提供一个 contextID 来作为别名。如果存在与 contextID 对应的模型，那么此 PUT 调用会替换该模型，从而来刷新应用程序正在使用的预测性分析。
{: shortdesc}

```
PUT http://{PA Bluemix load balancer URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound application}
```
{: codeblock}

请求示例：

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

部署成功时的响应：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

部署失败时的响应：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## 了解更多信息

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
