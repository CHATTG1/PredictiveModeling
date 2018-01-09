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

# 设置用于深度学习的对象存储

要使用 {{site.data.keyword.pm_full}} 服务以及属于该服务的深度学习试验，您必须有权访问 Cloud Object Storage (control.softlayer.com) 或 Object Storage OpenStack Swift (console.bluemix.net) 实例。您必须定义 Cloud Object Storage 存储区以提供培训数据、存储经过培训的模型和日志，为此，您既可以使用相同的 Cloud Object Storage 实例，也可以使用或不同的实例。
{: shortdesc}

## 创建 Cloud Object Storage 实例

1. 转至 [{{site.data.keyword.Bluemix_short}}Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) 页面并选择以下某个选项：

   - 非加密但免费（轻量）的存储器：[Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - 加密但不免费的服务：[Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. 配置 Object Storage 实例，然后单击**创建**。
3. 创建服务后，在侧面菜单中，单击**服务凭证**以获取凭证（例如，API URL、用户名和密码）来访问 Object Storage 实例。

## 访问 Cloud Object Storage (IaaS) 实例

Cloud Object Storage (IaaS) 服务提供与 S3 兼容的 API，并且可以使用任何兼容的客户机应用程序对该服务进行访问。

## Amazon Web Services (AWS) 命令行界面 (CLI)

1. 使用 `pip install awscli` 安装 [AWS CLI](https://aws.amazon.com/cli/)
2. 在 AWS 凭证文件（对于 linux，位于 ~/.aws/credentials）中配置 Cloud Object Storage (IaaS) 的凭证。

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

可以使用 AWS CLI 来列出和更新存储区及文件，并通过凭证指定以上概要文件和 Cloud Object Storage 端点。例如，要列出实例中的存储区，请运行以下命令：

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## 访问 Object Storage OpenStack Swift for Bluemix 实例

可以使用 Swift CLI 或客户机库（例如，Swift Python 客户机库）来访问此对象存储。有关更多详细信息，请参阅[文档](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## 数据格式设置

不同的框架需要不同格式的培训和测试数据集。例如，Caffe 需要 LevelDB 或 LMDB 格式的数据集，而 Torch 需要 Torch 专有格式的数据集。我们假定数据已经是特定框架所需的格式。有关如何将原始数据转换为特定于框架的格式的详细信息不属于本文档的范围。请查阅要使用的深度学习框架的文档，以获取更多信息。
