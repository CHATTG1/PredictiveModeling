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

# IBM SPSS Modeler 模型的 Machine Learning 服务批处理作业 API

{{site.data.keyword.pm_full}} 服务的批处理作业 API 支持与模型培训、模型评估和批量评分相关的长期运行的任务。此 API 可管理两种资产类型：用于批处理作业的 IBM® SPSS® Modeler 流文件和提交的作业定义。
{: shortdesc}

对于您上传的每个 SPSS ® Modeler 流文件，可能已经提交有许多类型的大量作业。如果作业可能会更新 Modeler 流文件内容，那么修改后的文件可能会包含在作业结果附件中。在模型评估作业类型中，生成的所有评估文件将会位于作业结果附件中。

**注**：仪表板中显示的数据仅与实时预测相关，例如通过上传功能上传的数据。

## 删除作业

可以删除作业。如果作业正在运行，那么删除作业操作会先取消该作业。请使用带 `job ID` 的 `DELETE` 命令。通过传递多个标识，可以一次取消多个作业。

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

返回的结果会指示删除了多少个在请求中通过标识引用的作业。如果此数字与请求中传递的列表不匹配，那么必须检查各个作业的状态。

## 检查作业的状态

通过使用 `GET` 命令，可以在任意点获取 `job ID` 的状态：


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

返回的 JSON 文件会指示作业状态，如果作业成功完成，那么会提供可用于获取所有已生成文件内容的数据 URL。

## 重新提交作业

要重新提交作业，请使用带 `job ID` 的 `PUT` 命令。要重新提交作业，该作业不能正在运行。

如果引用的标识不存在，那么以下调用将创建新作业。返回状态为 201 或 200（指示发生了其中哪种情况）。必须传递要在此执行中使用的作业定义 JSON 文件。

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

重新提交作业，其中 `content_type` 值设置为“application/json”。必须在请求的主体中包含新的或更新的作业定义 JSON 文件。

## 根据已上传的 Modeler 流文件提交作业

要将作业放入执行队列，请使用 `PUT` 命令：

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

提交作业，其中 `content_type` 值设置为“application/json”。必须在请求的主体中包含新的或更新的作业定义 JSON 文件。

如果作业定义已放入执行队列中，那么此请求会立即返回并指示成功。不会对成功执行 Modeler 流的能力进行测试，如作业定义中所配置。
该作业将由云中的某个 {{site.data.keyword.pm_short}} 作业服务器执行，并且可以监视其状态。如果您在执行模型培训并指示要自动刷新，那么该作业在成功执行后将替换原始 Modeler 流文件。

## 上传要在作业中使用的流文件

**注**：{{site.data.keyword.pm_short}} 仪表板仅用于实时评分。您无法将其用于运行作业（批量评分）。

要使 Modeler 流文件可供作业访问，请使用 `PUT` 命令：

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

同时通过 content_type "multipart/form-data"，在请求中传递文件。


所使用的唯一名称（`PUT` 调用中的 `file ID`）是您要在对文件 API 的 `DELETE` 调用中引用的名称，以及作业定义中的模型引用。请注意，在作业中使用的文件的名称空间完全受您控制。对相同 `file ID` 下的文件执行 `PUT` 命令会隐式替换当前副本。

要生成针对作业上传的所有文件的列表，请使用 `GET` 命令，但省略 `file ID` 参数。要检索特定文件，请使用带 `file ID` 参数的 `GET` 命令。您还可以通过发出 `DELETE` 命令来删除已上传的文件。这会导致引用该文件的任何暂挂作业执行中发生错误。

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

## 作业类型

**培训**：此作业类型指示所有“模型构建器”终端节点都应该在 Modeler 流中执行。作业成功完成之后，具有新培训模型块的更新 Modeler 流将处于可检索的作业结果中。
如果 Modeler 流文件在评估和评分分支中具有从模型构建节点到受训模型块的链接，那么将会导致刷新那些节点。


**评估**：此作业类型会触发执行所有“文档构建器”终端节点（主要来自 Modeler 客户端的“图形”和“输出”选项卡），以生成可传递回调用者的静态报告文件内容。评分分支不会视为此作业类型的一部分。

**自动刷新**：作业成功完成时，将更新批处理文件列表中原始 Modeler 流文件的 `TRAINING` 作业类型版本。假设使用针对实时评分部署的 Modeler 流刷新事件相关的评估和明确决策，但此时并未包含在自动刷新中。


**批量评分**：执行已应用“用作评分分支”选项的终端节点，指示这是此 Modeler 流设计中的评分分支。作业定义必须指定“导出”以及“评分”详细信息。

**运行流**：执行方式类似于以下情况：已选中流属性“执行”选项卡上的“运行此脚本”选项，并单击 Modeler 中的绿色“运行”图标。使用情况涵盖需要模型培训或其他作业类型的脚本执行。
脚本的所有动态控制必须由流参数（在作业定义中传递参数值）处理。


## 作业状态

**暂挂**：作业定义已提交，但尚未由作业服务器声明加以执行。

**正在运行**：作业定义已由作业服务器声明，并且正在执行。

**正在取消**：作业正在取消过程中。

**已取消**：作业已取消。

**失败**：作业失败。失败原因的详细信息在对作业状态执行 GET 时返回。

**成功**：作业成功运行。针对此事件连接的任何结果会在作业状态的 GET 的 JSON 中返回。

## 作业 API 详细信息

`POST /v1/jobs/{id}`

描述：提交作业定义以便执行。如果 `job ID` 已存在，那么将会发生错误。

内容类型：

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的 `job ID`。对于 {{site.data.keyword.pm_short}} 服务实例必须唯一：

```
@PathParam("id")
```
{: codeblock}

JSON 作业定义（请参阅作业定义 JSON）：

```
@BodyParam
```
{: codeblock}

响应：

成功。已提交作业以便执行，并返回描述所提交作业的 JSON。


```
@ApiResponse(code = 200)
```
{: codeblock}

“作业已存在”错误。已提交具有此标识的作业。返回的消息会描述此错误。


```
@ApiResponse(code = 409)
```
{: codeblock}

其他错误。返回异常的 JSON

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

描述：创建或更新作业。如果具有此标识的作业不存在，那么将会创建该作业；否则，更新该作业（有效地重新提交该作业以便执行）。


内容类型：

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

JSON 作业定义（请参阅作业定义 JSON）：

```
@BodyParam
```
{: codeblock}

响应：

成功。已更新并提交作业以便执行。返回描述所提交作业的 JSON。


```
@ApiResponse(code = 200)
```
{: codeblock}

成功。已创建并提交作业以便执行。返回描述所提交作业的 JSON。


```
@ApiResponse(code = 201)
```
{: codeblock}

其他错误。返回异常的 JSON。

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

描述：返回此 Machine
Learning 服务实例上所有已定义作业的列表。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

响应：

成功。返回所有作业定义的 JSON 数组：


```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

描述：请求返回特定作业定义。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回描述所参考作业的 JSON：


```
@ApiResponse(code = 200)
```
{: codeblock}

“找不到作业”错误。返回的消息的详细信息：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

描述：检索特定作业的状态。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的 `job ID`：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回描述作业状态的 JSON：


```
@ApiResponse(code = 200)
```
{: codeblock}

“找不到作业”错误，返回的消息的详细信息：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

描述：删除一个或多个作业。如果作业正在运行，那么将取消该作业。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的 `job ID` 或 `job ID` 列表，其中 ID 值以逗号 (,) 分隔

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

响应：

成功。返回已删除作业的数量：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

## 作业定义 JSON

作业定义 JSON 包含以下一般部分：

作业类型和预测模型参考

**注**：仪表板中显示的数据仅与实时预测相关，包括通过上传功能上传的数据。

### 操作类型

**TRAINING** – 执行用于培训或刷新模型块的模型构建器节点。会在作业结果中检索更新的 Modeler 流。

**EVALUATION** – 执行用于评估经过培训的模型的文档构建器和分析节点。

**AUTO_REFRESH** – 执行 TRAINING 并更新批量文件上传空间中的文件内容。

**BATCH_SCORE** – 如作业定义所指示，执行评分分支并导出生成的数据。

**RUN_STREAM** – 如流属性中所指示，执行 Modeler 流；最常用于需要脚本执行的情况。

模型：在文件上传操作中针对批处理作业参考指定的标识。有关更多信息，请参阅“上传要在作业中使用的流文件”。
请注意，使用的是 `/pm/v1/file`。

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

请注意，ID 应该与 `PUT` API 中使用的 `file ID` 相同。name 不是必要的，但是对于模型培训和自动刷新，将会使用在这里定义的名称来保存作业结果。
如果未定义 name，那么 {{site.data.keyword.pm_short}} 服务将会根据预定义的命名规则生成结果。

### 作业设置

运行此作业所需的所有设置。


```
"setting": {          
… Export settings
… Import settings
… Parameter value settings
… Document control settings
}
```
{: codeblock}

### 数据库连接定义

数据库类型：ApacheHive、DashDB、DB2、Greenplum、Impala、Informix、MySQL、Oracle、PostgreSQL、ProgressOpenEdge、Salesforce、SQLServer、Sybase、SybaseIQ 和 Teradata。

数据库服务实例中指定的连接，许多数据库类型需要在“Options”中传递特定设置

```
"dbDefinitions":{
"db1":{
"type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### 源节点设置

用于在源节点名称所识别的 Modeler 流中查找给定分支源的参考数据库连接和表。


```
"inputs": [
     {
          "odbc": {
"dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

table 是数据库表名称。此表用于覆盖流源节点。
node 是流的数据源名称。dbRef 参考 dbDefinitions 对象定义的数据库连接，而 table 是用作 Modeler 流中给定分支输入的数据库表。
node 识别 Modeler 流中的原始源节点，该节点要由使用这些提供参数所构造的数据库源节点替换。


### 导出节点设置

用于在终端节点名称所识别的 Modeler 流中持久存储给定分支的数据的参考数据库连接和表。


通过 insertMode 属性连接持久存储数据时使用的控制方法：

**Append** – 表必须存在且适合插入

**Create** – 创建表（如果表已经存在，将返回错误）

**Drop** – 如果引用的表存在，会将其废弃并重新创建

**Refresh** – 在插入新行之前先从表删除现有的行
   
table 是要将作业结果写入的数据库表名称。node 是流的终端节点名称。与源节点设置类似，node 识别 Modeler 流中的原始输出节点，该节点要由使用这些提供参数所构造的数据库导出节点替换。


**注**：导出节点设置和源节点设置对于成功运行作业都非常重要。

### 参数值覆盖

要在作业执行之前覆盖流级别参数的缺省值，您可以指定要在作业定义中使用的名称/值对。


```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

执行评估作业类型时返回的评估文档的预期报告格式

通过执行评估作业生成的所有文档会捆绑到一个 .zip 文件中。只有能够支持所指示 reportFormat 的文档构建器节点才会执行，并在成功执行作业之后返回其内容。


支持以下格式：HTML、JPG、PNG、RTF、SAV、TAB 和 XML。

```
"reportFormat": "HTML"
```
{: codeblock}

### 完成 JSON 示例

```
{ 
     "action": "TRAINING", 
     "model": { 
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
"dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## 批处理作业 API 详细信息

以下各部分提供了批处理作业 IBM® SPSS® Modeler 文件管理 API 详细信息。

`PUT /v1/file/{id}`

描述：上传要用于批处理作业的 IBM® SPSS® Modeler 流文件。

**注**：如果在指定标识下已经存储有文件，那么会覆盖该文件。

### 内容类型

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### 参数

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的 `file ID`。在服务实例存储库中必须唯一：


```
@PathParam("id")
```
{: codeblock}

### 响应

成功。返回可用于从服务实例存储库中下载文件的 URL：


```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

描述：从服务实例存储库删除指定标识下存储的文件。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

上传时文件的用户指定标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。已删除指定的文件：

```
@ApiResponse(code = 200)
```
{: codeblock}

错误。服务实例存储库中不存在此标识下存储的文件：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

描述：返回服务实例存储库中管理的所有文件的标识列表，以进行批处理作业处理。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

响应：

成功。返回 `file ID` 值的 JSON 数组：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

描述：检索在指定标识下存储用于批处理作业处理的 IBM® SPSS® Modeler 流文件。

内容类型：

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

上传时文件的用户指定标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回 IBM® SPSS® Modeler 文件：

```
@ApiResponse(code = 200)
```
{: codeblock}

错误。服务实例存储库中不存在此标识下存储的文件：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

## 了解更多信息

有关在配置页中采用批处理作业的示例，请参阅[从 SPSS 流到使用 Python 的批量评分](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37)。

有关作业定义 JSON 的更多信息，请参阅[作业定义 JSON](#job-definition-json)。
