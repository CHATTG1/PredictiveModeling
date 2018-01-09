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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 故障诊断

下面是对使用 {{site.data.keyword.pm_full}} 的相关常见故障诊断问题的解答。
{: shortdesc}

## 获取 Machine Learning 的帮助和支持
{: #gettinghelp}

如果在使用 {{site.data.keyword.pm_short}} 时遇到问题或有疑问，您可以通过在论坛中搜索相关信息或进行提问来获取帮助。您还可以开具支持凭单。

在使用论坛提问时，请对问题进行标记，以便 {{site.data.keyword.pm_short}} 开发团队能看到您的问题。

如果有关于 {{site.data.keyword.pm_short}} 的技术问题，请在 <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 上发布问题，并使用“ibm-bluemix”和“machine-learning”标记您的问题。

有关服务和入门指示信息的问题，请使用 <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 论坛。请包含“machine-learning”和“bluemix”标记。请参阅[获取帮助](https://console.bluemix.net/docs/support/index.html#getting-help)，以获取有关如何使用论坛的更多详细信息。

有关开具 IBM 支持凭单的信息，或有关支持级别和凭单严重性的信息，请参阅[联系支持人员](https://console.bluemix.net/docs/support/index.html#contacting-support)。

## 尚未提供授权令牌。
{: #ts_missing_authorization_token}
 
无法成功调用 REST API。
{: tsSymptoms}
 
在 `Authorization` 头中未提供授权令牌。
{: tsCauses}
 
在 `Authorization` 头中传递授权令牌。
{: tsResolve}
       
## 授权令牌无效。
{: #ts_invalid_authorization_token}
 
无法成功调用 REST API。
{: tsSymptoms}
 
无法对已提供的授权令牌进行解码或解析。
{: tsCauses}
 
在 `Authorization` 头中传递正确的授权令牌。
{: tsResolve}
       
## 授权令牌不同于请求中使用的 instance_id。
{: #ts_not_matching_authorization_token}
 
无法成功调用 REST API。
{: tsSymptoms}
 
对于使用授权令牌的服务实例，不会生成已使用的授权令牌。
{: tsCauses}
 
在与所使用服务实例相对应的 `Authorization` 头中传递授权令牌。
{: tsResolve}
       
## 授权令牌已到期。
{: #ts_expired_authorization_token}
 
无法成功调用 REST API。
{: tsSymptoms}
 
授权令牌已到期。
{: tsCauses}
 
在 `Authorization` 头中传递未到期授权令牌。
{: tsResolve}
       
## 认证所需的公用密钥不可用。
{: #ts_missing_public_key}
 
无法成功调用 REST API。
{: tsSymptoms}
 
这是内部服务问题。
{: tsCauses}
 
此问题需要由支持团队来解决。
{: tsResolve}
       
## 操作在 {{timeout}} 后超时
{: #ts_operation_timeout}
 
无法成功调用 REST API。
{: tsSymptoms}
 
执行请求的操作期间发生超时。
{: tsCauses}
 
请再次尝试调用所需的操作。
{: tsResolve}
       
## 未处理类型为 {{type}} 且状态为 {{status}} 的异常
{: #ts_unhandled_exception_with_status}
 
无法成功调用 REST API。
{: tsSymptoms}
 
这是内部服务问题。
{: tsCauses}
 
请再次尝试调用所需的操作。如果此问题多次出现，那么需要由支持团队来解决。
{: tsResolve}
       
## 未处理类型为 {{type}} 且响应为 {{response}} 的异常
{: #ts_unhandled_exception_with_response}
 
无法成功调用 REST API。
{: tsSymptoms}
 
这是内部服务问题。
{: tsCauses}
 
请再次尝试调用所需的操作。如果此问题多次出现，那么需要由支持团队来解决。
{: tsResolve}
       
## 未处理类型为 {{type}} 且 JSON 为 {{json}} 的异常
{: #ts_unhandled_exception_with_json}
 
无法成功调用 REST API。
{: tsSymptoms}
 
这是内部服务问题。
{: tsCauses}
 
请再次尝试调用所需的操作。如果此问题多次出现，那么需要由支持团队来解决。
{: tsResolve}
       
## 未处理类型为 {{type}} 且消息为 {{message}} 的异常
{: #ts_unhandled_exception_with_message}
 
无法成功调用 REST API。
{: tsSymptoms}
 
这是内部服务问题。
{: tsCauses}
 
请再次尝试调用所需的操作。如果此问题多次出现，那么需要由支持团队来解决。
{: tsResolve}
       
## 找不到请求的对象。
{: #ts_not_found}
 
无法成功调用 REST API。
{: tsSymptoms}
 
找不到请求的资源。
{: tsCauses}
 
确保您引用的是现有资源。
{: tsResolve}
       
## 底层数据库报告了太多请求。
{: #ts_too_many_cloudant_requests}
 
无法成功调用 REST API。
{: tsSymptoms}
 
用户在给定时间内发送了太多请求。
{: tsCauses}
 
请再次尝试调用所需的操作。
{: tsResolve}
       
 ## 评估的定义在 artifactModelVersion 和部署中均未定义。至少需要\" +\n      \"在其中一个位置指定该定义。
{: #ts_missing_evaluation_definition}
 
无法成功调用 REST API。
{: tsSymptoms}
 
学习配置未包含所有必需信息
{: tsCauses}
 
在 `learning configuration` 中提供 `definition`
{: tsResolve}
       
## 评估需要为模型指定的学习配置。
{: #ts_missing_learning_configuration}
 
无法创建 `learning iteration`。
{: tsSymptoms}
 
没有为该模型定义 `learning configuration`。
{: tsCauses}
 
创建 `learning configuration`，然后再次尝试创建 `learning iteration`。
{: tsResolve}
       
## 评估需要在 `X-Spark-Service-Instance` 头中提供 Spark 实例
{: #ts_missing_spark_definition_for_evaluation}
 
无法成功调用 REST API。
{: tsSymptoms}
 
`learning configuration` 中未包含所有必需信息
{: tsCauses}
 
在 Learning Configuration 或 `X-Spark-Service-Instance` 头中提供 `spark_service`
{: tsResolve}
       
## 模型不包含任何版本。
{: #ts_missing_latest_model_version}
 
无法创建部署，也无法设置学习配置。
{: tsSymptoms}
 
存在与模型持久性相关的不一致。
{: tsCauses}
 
请再次尝试持久存储该模型，然后再次尝试执行该操作。
{: tsResolve}
       
## 补丁操作只能修改现有学习配置。
{: #ts_patch_non_existing_learning_configuration}
 
无法调用补丁 REST API 方法来修补学习配置。
{: tsSymptoms}
 
不存在为此模型设置的 `learning configuration`，或模型不存在。
{: tsCauses}
 
确保该模型存在，并且已经设置学习配置。
{: tsResolve}
       
## 补丁操作需要恰好一个 replace 操作。
{: #ts_patch_multiple_ops}
 
无法对部署进行修补。
{: tsSymptoms}
 
补丁有效内容包含多个操作，或者补丁操作不同于 `replace`。
{: tsCauses}
 
在补丁有效内容中仅使用一个操作，即 `replace` 操作
{: tsResolve}
       
## 给定的有效内容缺少必填字段 FIELD，或这些字段的值已损坏。
{: #ts_invalid_request_payload}
 
无法处理与底层数据集访问权相关的操作。
{: tsSymptoms}
 
未正确定义对数据集的访问权。
{: tsCauses}
 
更正数据集的访问权定义。
{: tsResolve}
       
## 不支持提供的评估方法：METHOD。支持的值：VALUE。
{: #ts_evaluation_method_not_supported}
 
无法创建学习配置。
{: tsSymptoms}
 
用于创建学习配置的评估方法不正确。
{: tsCauses}
 
使用支持的评估方法，即下列其中一项：`regression`、`binary` 和 `multiclass`。
{: tsResolve}
       
## 每个模型只能有一个活动评估。请求无法完成，因为存在现有活动评估：{{url}}
{: #ts_active_evaluation_conflict}
 
无法创建其他学习迭代
{: tsSymptoms}
 
模型只能有一个正在运行的评估。
{: tsCauses}
 
查看已在运行的评估，或等待其结束，然后启动新的评估。
{: tsResolve}
       
## 不支持部署类型 {{type}}。
{: #ts_not_supported_deployment_type}
 
无法创建部署。
{: tsSymptoms}
 
使用的是不支持的部署类型。
{: tsCauses}
 
应该使用支持的部署类型。
{: tsResolve}
       
## 输入不正确：({{message}})
{: #ts_deserialization_error}
 
无法成功调用 REST API。
{: tsSymptoms}
 
解析 JSON 时发生问题。
{: tsCauses}
 
确保在请求中传递的 JSON 正确。
{: tsResolve}
       
## 数据不充分 - 无法计算度量 {{name}}
{: #ts_missing_metric}
 
学习迭代失败
{: tsSymptoms}
 
由于反馈数据不充分，无法计算具有所定义阈值的度量的值
{: tsCauses}
 
复查并改进 `learning configuration` 中数据源 `feedback_data_ref` 的数据
{: tsResolve}
       
## 对于类型 {{type}}，必须在 `X-Spark-Service-Instance` 头中提供 Spark 实例
{: #ts_missing_prediction_spark_definition}
 
无法创建部署
{: tsSymptoms}
 
`batch` 和 `streaming` 部署都需要提供 Spark 实例
{: tsCauses}
 
在 `X-Spark-Service-Instance` 头中提供 Spark 实例
{: tsResolve}
       
## 操作 {{action}} 失败，返回消息 {{message}}
{: #ts_http_client_error}
 
无法成功调用 REST API。
{: tsSymptoms}
 
调用底层服务时发生问题。
{: tsCauses}
 
如果有关于如何解决此问题的建议，请遵循该建议进行操作。如果消息中没有任何建议或者建议无法解决此问题，请联系支持团队。
{: tsResolve}
       
## 不允许使用路径 `{{path}}`。唯一允许的补丁流路径为 `/status`
{: #ts_wrong_stream_patch_path}
 
无法修补流式部署。
{: tsSymptoms}
 
用于修补 `stream` 部署的路径不正确。
{: tsCauses}
 
使用支持的路径选项（即 `/status`，它允许启动/停止流式处理）修补 `stream` 部署。
{: tsResolve}
       
## 类型为 `{{$type}}` 的实例不允许执行补丁操作
{: #ts_patch_not_supported}
 
无法修补部署。
{: tsSymptoms}
 
正在修补的部署类型不正确。
{: tsCauses}
 
修补 `stream` 部署类型。
{: tsResolve}
       
## 数据连接 `{{data}}` 对于 feedback_data_ref 无效
{: #ts_invalid_feedback_data_connection}
 
无法为模型创建 `learning configuration`。
{: tsSymptoms}
 
定义 feedback_data_ref 时使用了不支持的数据源。
{: tsCauses}
 
仅使用支持的数据源类型，即 `dashdb`
{: tsResolve}
       
## 不允许使用路径 {{path}}。唯一允许的补丁模型路径为 `/deployed_version/url`，对于 V2，为 `/deployed_version/href`
{: #ts_patch_model_path_not_allowed}
 
没有用于修补模型的选项。
{: tsSymptoms}
 
修补模型期间使用的路径不正确。
{: tsCauses}
 
修补具有支持路径的模型，以允许更新已部署模型的版本。
{: tsResolve}
       
## 解析失败：{{msg}}
{: #ts_parsing_error}
 
无法成功调用 REST API。
{: tsSymptoms}
 
无法成功解析请求的有效内容。
{: tsCauses}
 
确保请求有效内容正确并且可以正确解析。
{: tsResolve}
       
## `learning configuration` 不支持所选模型 {{env}} 的运行时环境。支持的环境：[{{supported_envs}}]。
{: #ts_runtime_env_not_supported}
 
没有用于创建 `learning configuration` 的选项
{: tsSymptoms}
 
不支持尝试为其创建 `learning_configuration` 的模型。
{: tsCauses}
 
为具有受支持运行时的模型创建 `learning configuration`。
{: tsResolve}
       
## 当前套餐“{{plan}}”仅允许 {{limit}} 部署
{: #ts_deployments_plan_limit_reached}
 
无法创建部署。
{: tsSymptoms}
 
已达到当前套餐的部署数限制。
{: tsCauses}
 
升级到没有此类限制的套餐。
{: tsResolve}
       
## 数据库连接定义无效 ({{code}})
{: #ts_sql_error}
 
无法利用 `learning configuration` 功能。
{: tsSymptoms}
 
数据库连接定义无效。
{: tsCauses}
 
请尝试解决由底层数据库返回的 `code` 所描述的问题。
{: tsResolve}
       
## 连接底层 {{system}} 时发生问题
{: #ts_stream_tcp_error}
 
无法成功调用 REST API。
{: tsSymptoms}
 
连接到底层系统期间发生问题。这可能是临时网络问题。
{: tsCauses}
 
请再次尝试调用所需的操作。如果此问题多次出现，请联系支持团队。
{: tsResolve}
       
## 抽取 X-Spark-Service-Instance 头时出错：({{message}})
{: #ts_spark_header_deserialization_error}
 
无法调用需要 Spark 凭证的 REST API
{: tsSymptoms}
 
对 Spark 凭证进行基本 64 位解码或解析时发生问题。
{: tsCauses}
 
确保对正确的 Spark 凭证以正确方式进行基本 64 位编码。有关更多信息，请参阅文档。
{: tsResolve}
       
## 禁止非 Beta 用户使用此功能。
{: #ts_not_beta_user}
 
无法成功调用所需的 REST API。
{: tsSymptoms}
 
调用的 REST API 当前为 Beta 版本。
{: tsCauses}
 
如果您想要参与，请将您自己加入等待列表。详细信息位于相关文档中。
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
无法成功调用 REST API。
{: tsSymptoms}
 
调用底层服务时发生问题。
{: tsCauses}
 
如果有关于如何解决此问题的建议，请遵循该建议进行操作。如果消息中没有任何建议或者建议无法解决此问题，请联系支持团队。
{: tsResolve}
       
## 超过了速率限制。
{: #ts_rate_limit_exceeded}
 
超过了速率限制。
{: tsSymptoms}
 
超过了当前套餐的速率限制。
{: tsCauses}
 
要解决此问题，请获取具有更大速率限制的其他套餐
{: tsResolve}
       
## 查询参数 `{{paramName}}` 的值 {{value}} 无效
{: #ts_invalid_query_parameter_value}
 
因查询参数的值不正确而导致验证错误。
{: tsSymptoms}
 
获取查询的结果时出错。
{: tsCauses}
 
更正查询参数值。详细信息位于相关文档中。
{: tsResolve}
       
## 令牌类型 {{type}} 无效
{: #ts_invalid_token_type}
 
有关令牌类型的错误。
{: tsSymptoms}
 
授权出错。
{: tsCauses}
 
令牌应该以 `Bearer` 前缀开头
{: tsResolve}
       
## 令牌格式无效。应该使用不记名令牌格式。
{: #ts_invalid_token_format}
 
有关令牌格式的错误。
{: tsSymptoms}
 
授权出错。
{: tsCauses}
 
令牌应该为不记名令牌，并应该以 `Bearer` 前缀开头
{: tsResolve}

## 输入 JSON 文件缺失或无效：400
{: #os_invalid_input}

尝试联机评分时显示以下消息：**输入 JSON 文件缺失或无效**。
{: tsSymptoms}

评分输入有效内容与对模型评分所需的预期输入类型不匹配时，会显示此消息。具体来说，以下原因可能适用：

- 输入有效内容为空。
- 输入有效内容模式无效。
- 输入数据类型与所需的数据类型不匹配。
{: tsCauses}

更正输入有效内容。确保有效内容的语法正确、模式有效且数据类型正确。进行更正后，请再次尝试联机评分。对于语法问题，请使用 `jsonlint` 命令来验证 JSON 文件。
{: tsResolve}

## 授权令牌已到期：401
{: #os_expired_authorization_token}

尝试联机评分时，会显示以下消息：**授权失败**。
{: tsSymptoms}

用于评分的令牌到期时，会显示此消息。
{: tsCauses}

重新生成此 {{site.data.keyword.pm_full}} 实例的令牌，然后重试。如果仍看到此问题，请联系 IBM 支持。
{: tsResolve}

## 未知部署标识：404
{: #os_unkown_depid}

尝试联机评分时，会显示以下消息：**未知部署标识**。
{: osSymptoms}

用于评分的部署标识不存在时，会显示此消息。
{: tsCauses}

确保提供的是正确的部署标识。如果不是，请使用该部署标识部署模型，然后再次尝试对其评分。
{: tsResolve}

## 内部服务器错误：500
{: #os_internal_error}

尝试联机评分时，会显示以下消息：**内部服务器错误**{: osSymptoms}

如果联机评分所依赖的下游数据流发生故障，那么会显示此消息。
{: tsCauses}

等待一段时间后，再次尝试联机评分。如果再次发生故障，请联系 IBM 支持。
{: tsResolve}
              
