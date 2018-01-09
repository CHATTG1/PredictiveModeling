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

# 开发利用已部署 SPSS 模型的应用程序

可以使用 IBM® SPSS® 模型来开发 {{site.data.keyword.pm_full}} 应用程序。  
{: shortdesc}

*  [使用已部署的预测模型评分](#scoring-with-a-deployed-predictive-model)

*  [检索已部署预测模型的元数据](#retrieving-metadata-for-a-deployed-predictive-model)

*  [检索此服务的 Web 应用程序描述语言 (WADL) 摘要](#retrieving-the-web-application-description-language-wadl-summary-of-this-service)

## 使用已部署的预测模型评分

使用以下 API 调用可发布输入数据，供已部署的模型用于在评分结果中生成并返回预测性分析。

```
POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}
```
{: codeblock}

请求示例：

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this score request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

对之前请求成功响应的示例：

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the score result, a json array, eg.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

评分请求失败时的响应：

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

## 检索已部署预测模型的元数据

使用以下 API 调用可检索已部署 IBM® SPSS® Modeler 流的评分分支的元数据。不要为此方法提供请求主体。

```
GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score
```
{: codeblock}

请求示例：

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this metadata retrieval request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

对之前请求成功响应的示例：

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the metadata (formatted for readability in this document) on the scoring branch eg.
    [
      {
        flag: true
        message: 
          "Model Score Input Metadata: 
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
<table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata> 
          Model Score Output Metadata:
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
<table xsi:type="nodeImpl" tag="id32CPAJBGJFG" name="Flat File" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
            <field measurementLevel="FLAG" storageType="STRING" name="$C-beer_beans_pizza"/>
            <field measurementLevel="CONTINUOUS" storageType="DOUBLE" name="$CC-beer_beans_pizza"/>
          </table>
          </metadata>"
      }
    ]
```
{: codeblock}

评分请求失败时的响应：

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

## 检索此服务的 Web 应用程序描述语言 (WADL) 摘要

使用以下 API 调用可检索此服务的 WADL。

```
OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl
```
{: codeblock}

请求示例：

```
Content-Type: */*
```
{: codeblock}

WADL 请求成功时的响应：

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score using the model with given context ID" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <request>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

WADL 请求失败时的响应：

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
