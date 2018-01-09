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

# 部署联机模型

通过使用 {{site.data.keyword.pm_full}} 服务，可以部署模型，然后通过对已部署的模型发出评分请求来生成预测性分析。
{: shortdesc}

**场景名称**：客户满意度预测。

**场景描述**：一家通信公司希望了解哪些客户有流失风险。
该公司要求我们提供一个解决方案来帮助回答该问题。为此，数据研究员准备了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

## 使用样本模型

1. 从 Git 存储库（[此处](https://github.com/pmservice/wml-sample-models/blob/master/spss/customer-satisfaction-prediction/model/customer-satisfaction-prediction.str)）下载样本模型。

2. 使用以下请求上传新模型：

   ```
   curl -X PUT -H "Content-Type:multipart/form-data;charset=utf-8" -F "model_file=@customer-satisfaction-prediction.str" https://ibm-watson-ml.mybluemix.net/pm/v1/model/{context_id}?accesskey={accesskey_value}
   ```
   {: codeblock}

   响应：

   ```
   {"flag":true,"message":"success to upload stream with given context id context_csp2","url":"http://pmdevlb.pmservice.ibmcloud.com:80/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr"}
```
   {: codeblock}

3. 使用以下请求刷新现有模型：

   ```
   curl -X PUT -H "Content-Type:multipart/form-data;Charset=UTF-8" -F "model_file=@customer-satisfaction-prediction.str" https://ibm-watson-ml.mybluemix.net/pm/v1/model/{context_id}?accesskey={accesskey_value}
   ```
   {: codeblock}

   响应：

   ```
   {"flag":true,"message":"success to update stream with given context id context_csp2","url":"http://pmdevlb.pmservice.ibmcloud.com:80/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr"}
```
   {: codeblock}

4. 使用以下请求来获取所有已部署模型的列表：

   ```
   curl -X GET -i -H "Content-Type:*/*" https://ibm-watson-ml.mybluemix.net/pm/v1/model?accesskey={accesskey_value}
   ```
   {: codeblock}

   响应：

   ```
   [{"stream":"customer-satisfaction-prediction.str","id":"context_csp2"}]
```
   {: codeblock}

5. 使用以下请求来下载特定已部署模型的副本：

   ```
   curl -X GET -v -H "Content-Type:*/*" https://ibm-watson-ml.mybluemix.net/pm/v1/model/{context_id}?accesskey={accesskey_value} >> output.str
   ```
   {: codeblock}

   响应（这会将模型内容下载到 output.str 文件）：

   ```
   > GET /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1

   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*

   >

   * STATE: DO => DO_DONE handle 0x600057960; line 1664 (connection #0)

   * STATE: DO_DONE => WAITPERFORM handle 0x600057960; line 1791 (connection #0)

   * STATE: WAITPERFORM => PERFORM handle 0x600057960; line 1801 (connection #0)

   { [5 bytes data]

   * HTTP 1.1 or later with persistent connection, pipelining supported

   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Disposition: attachment; filename=customer-satisfaction-prediction.str

   < Content-Type: application/octet-stream

   < Date: Fri, 17 Feb 2017 15:55:20 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 15:57:20 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 3450392319

   <
   
   0 0 0 0 0 0 0 0 --:--:-- 0:00:01 --:--:-- 0{ [5 bytes data]

   100 74410 0 74410 0 0 15966 0 --:--:-- 0:00:04 --:--:-- 15978* STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   100 99k 0 99k 0 0 21114 0 --:--:-- 0:00:04 --:--:-- 24508

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact
```
   {: codeblock}

6. 删除已部署的模型：

   ```
   curl -X DELETE -v -H "Content-Type:*/*" https://ibm-watson-ml.mybluemix.net/pm/v1/model/{context_id}?accesskey={accesskey_value}
   ```
   {: codeblock}

   响应：

   ```
   > DELETE /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1
   
   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*
   
   > 
   
   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json

   < Date: Fri, 17 Feb 2017 16:21:20 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:23:20 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 2495867153

   <
   
   * STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact

   {"flag":true,"message":"success to delete stream with specified id context_csp2"}

   Response when trying to delete a model that doesn't exist (or
   was already deleted):

   > DELETE /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1

   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*

   >

   * STATE: DO => DO_DONE handle 0x600057960; line 1664 (connection #0)

   * STATE: DO_DONE => WAITPERFORM handle 0x600057960; line 1791 (connection #0)

   * STATE: WAITPERFORM => PERFORM handle 0x600057960; line 1801 (connection #0)

   * HTTP 1.1 or later with persistent connection, pipelining supported

   < HTTP/1.1 404 Not Found

   < X-Backside-Transport: FAIL FAIL

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json

   < Date: Fri, 17 Feb 2017 16:23:15 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:25:15 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 94445277

   <
   
   * STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact

   Not Found Model:context_csp2
```
   {: codeblock}

7. 对已部署的模型评分：

   ```
   curl -X POST -v -H "Content-Type:application/json;Charset=UTF-8" -d '{"tablename":"Input data","header":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"data":[["9237-HQITU","Female",0,"No","No",2,"Yes","No","Fiber optic","No","No","No","No","No","No","Month-to-month","Yes","Electronic check",70.700,151.650,"Yes",1.000]]}' https://ibm-watson-ml.mybluemix.net/pm/v1/score/{context_id}?accesskey={accesskey_value}
   ```
   {: codeblock}

   响应：

   ```
   > POST /pm/v1/score/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1
   
   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:application/json;charset=UTF-8
   
   > Content-Length: 525

   >

   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json;charset=UTF-8

   < Date: Fri, 17 Feb 2017 16:40:05 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:42:05 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 3291552207



   [{"header":["customerID","Churn","Predicted Churn","Probability of Churn"],"data":[["9237-HQITU","Yes","Yes",0.8829830706957551]]}] 
   ```
   {: codeblock}

## 了解更多信息
 
准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 IBM® SPSS® 模型](using_pm_service.html)。


有关该 API 的更多信息，请参阅 [Spark 和 Python 模型的服务 API](pm_service_api_spark.html) 或 [IBM® SPSS® 模型的服务 API](pm_service_api_spss.html)。

有关 IBM® SPSS® Modeler 及其提供的建模算法的更多信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的更多信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
