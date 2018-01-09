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

# 创建培训运行

培训运行是用于在 {{site.data.keyword.pm_full}} 中执行深度学习试验的组织原则。一个典型的试验可能包含数十个到数百个培训运行。每个运行均单独定义；运行由以下部分组成：神经网络（使用其中一个[支持的深度学习框架](ml_dlaas_supported_framework.html)进行定义）和配置（用于设置培训运行方式，包括 GPU 数和包含数据集的 Object Storage 的位置）。
{: shortdesc}

<p align="center"><img src="images/experiment_to_training_runs_text.svg" alt="试验与培训运行的关系"></p>

## 创建模型定义 .zip 文件

使用其中一个[支持的深度学习框架](ml_dlaas_supported_framework.html)定义神经网络和关联的数据处理后，请将这些文件一起打包成 .zip 格式。例如，如果模型是用 Torch 编写的，请将 .lua 文件打包在一起；如果是使用 Caffe 编写的，请压缩 .prototxt 文件；或者如果是用 Tensorflow/Keras/MXNet 编写的，请压缩 .py 文件。不支持其他压缩格式，例如 gzip 或 tar。请查阅要使用的深度学习框架的文档，以便准备模型定义文件。  

<!-- Supposedly this isn't true anymore >> NOTE: All model definition files must be in the first level of the zip file so ensure there are no nested directories in the zip file. -->

例如，包含 tensflow 模型定义的 zip 文件 `tf-model.zip` 可能包含以下输出：

```
unzip -l tf-model.zip
```
{: codeblock}

样本输出：

```
Archive:  tf-model.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     7094  09-21-2017 11:38   convolutional_network.py
     5486  09-19-2017 13:49   input_data.py
---------                     -------
    12580                     2 files
```
{: codeblock}

## 上传培训数据

培训数据必须[上传到兼容的 Object Storage 服务实例](ml_dlaas_object_store.html)。下面的清单文件将使用该 Object Storage 实例中的凭证。对象存储还用于在培训运行结束时存储经过培训的模型。

## 创建培训清单文件

清单是一个 YAML 格式的文件，含有用于描述要培训的模型的不同字段，包括要使用的深度学习框架、Cloud Object Storage 配置、资源需求以及在培训和测试期间执行模型所需的多个自变量（包括超参数）。下面将描述用于深度学习的模型培训文件的不同字段，以继续 tensorflow 手写体识别示例。

* `model_definition.name`：可以提供任何值来命名，以帮助在培训作业启动后对其进行识别。但是，此名称不必唯一，因为服务会为每个启动的培训作业分配唯一的模型标识。
* `model_definition.description`：这是可用于描述作业的另一个字段。
* `model_definition.author`：可选。在 *name* 和 *email* 键下提供作者姓名和电子邮件地址。
* `model_definition.framework`：此字段提供特定于框架的信息，其中名称和版本必须与[支持的深度学习框架](ml_dlaas_supported_framework.html)中的其中一个框架相匹配。
    - `model_definition.framework.name`：框架的名称
    - `model_definition.framework.version`：框架的版本。
* `model_definition.execution`：此字段提供有关用于启动培训的命令的信息。
    - `model_definition.execution.command`：此字段确定主程序文件以及深度学习需要执行的任何自变量。
    - `model_definition.execution.resource`：此字段指定将分配用于培训的资源，其值应为 `small`（1 个 GPU）、`medium`（2 个 GPU）或 `large`（4 个 GPU）
* `training_data_reference`：此部分指定对象存储的列表，用于培训模型的数据文件将从这些对象存储中装入。当前，此列表应该包含一个且仅包含一个对象存储，其定义如下：
    - `connection`：数据存储器的连接变量。
    - `source.type`：数据存储器的类型，目前只能设置为 s3 或 bluemix_objectstore。如果 Object Storage 实例为 *Cloud Object Storage (IaaS)*，请使用 `s3`；如果 Object Storage 实例为 *Object Storage OpenStack Swift for Bluemix*，请使用 `bluemix_objectstore`。
    - `source.bucket`：培训数据所在的存储区。
* `training_results_reference`：此部分指定将在培训完成后存储所生成模型文件和日志的对象存储。
    - `connection`：数据存储器的连接变量。支持的连接变量列表取决于数据存储器类型。
    - `target.type`：数据存储器的类型，目前只能设置为 s3 或 bluemix_objectstore。如果 Object Storage 实例为 *Cloud Object Storage (IaaS)*，请使用 `s3`；如果 Object Storage 实例为 *Object Storage OpenStack Swift for Bluemix*，请使用 `bluemix_objectstore`。
    - `target.strucket`：将向其写入培训结果的存储区。

例如，可以使用以下模型培训定义文件来定义用于培训 tensflow 模型的作业：

```
model_definition:
  framework:
    name: tensorflow
    version: 1.2-py3
  name: tf-mnist-showtest1
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 2000000
    resource: small
training_data:
- connection:
    endpoint_url: <auth-url>
    aws_access_key_id: <username>
    aws_secret_access_key: <password>
  source:
    bucket: mnist-training-data
    type: s3
training_results:
  connection:
    endpoint_url: <auth-url>
    aws_access_key_id: <username>
    aws_secret_access_key: <password>
  target:
    bucket: mnist-training-models
    type: s3
```
{: codeblock]

其中，`convolutional_network.py` 是要执行的 tensflow 程序（它是模型定义 zip 的一部分），其余项是该程序的自变量。程序自变量 `--trainImagesFile train-images-idx3-ubyte.gz`、`--trainLabelsFile train-labels-idx1-ubyte.gz`、`--testImagesFile t10k-images-idx3-ubyte.gz` 和 `--testLabelsFile t10k-labels-idx1-ubyte.gz` 的值引用对象存储容器 `tf_training_data` 中的数据集路径。程序自变量 `--trainingIters 20000` 和 `--learningRate 0.001` 用于传递超参数的值。

**注**：培训配置或模型定义文件引用上传到 Object Storage 实例的文件时，引用应使用如上所示的相对路径。

**注**：培训开始前，培训数据存储区中的所有文件都会下载到服务所运行的培训环境中。为了避免不必要的文件传输开销/延迟，请将不用于文件培训的文件保存在其他存储区中。

**注**：在上面的示例中，用于提供数据并存储所生成模型的对象存储是 *Cloud Object Storage (IaaS)*。但是，如果使用的对象存储是 *Object Storage Open Stack for Bluemix*，那么连接键会不同，下面提供了示例清单：

```
model_definition:
  framework:
    name: tensorflow
    version: 1.2-py3
  name: tf-mnist-showtest1
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  execution:
    command: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz
      --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz
      --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001
      --trainingIters 2000000
    resource: small
training_data_reference:
- connection:
    auth_url: <auth-url>
    user_name: <username>
    password: <password>
    region: <region>
    domain_name: <domain-name>
    project_id: <project-id>
  source:
    bucket: mnist-training-data
    type: bluemix_objectstore
training_results_reference:
  connection:
    auth_url: <auth-url>
    user_name: <username>
    password: <password>
    region: <region>
    domain_name: <domain-name>
    project_id: <project-id>
  target:
    bucket: mnist-training-models
    type: bluemix_objectstore
```
{: codeblock]

**注**对于 *Object Storage Open Stack for Bluemix* 连接，应执行“对象存储”凭证中的键名与清单中所需键名之间的映射：

| {{site.data.keyword.Bluemix_notm}} 凭证键| 培训清单凭证键|
|----------------------------------------------------|----------------------------------------|
|auth_url |auth_url |
|username |user_name |
|password |password |
|projectId |project_id |
|region |region |
|domainName |domain_name |
{: caption="表 1. {{site.data.keyword.Bluemix_notm}} 和培训清单凭证键" caption-side="top"}

## 提交培训运行

准备好模型定义 .zip 和培训配置文件后，使用 `bx ml train` 命令提交作业：`bx ml train <path-to-model-definition-zip> <path-to-model-configuration-yaml>` 

```
bx ml train tf-model.zip job.yaml
```
{: codeblock}

样本输出：

成功提交该命令后，将返回唯一的模型标识。例如，以下输出显示 `Model-ID` 的值为 `training-DOl4q2LkR`：

```
Starting to train ...
OK
Model-ID is 'training-DOl4q2LkR'
```

# 监视培训运行

要列出所有培训作业（无论是否完成），请使用 CLI 命令 `bx ml list trained-models`：

```
bx ml list trained-models
```
{: codeblock}

样本输出：

```
Fetching the list of trained models ...
SI No   Name                       guid                 status    submitted-at
1       tf-mnist                   training-DOl4q2LkR   pending   2017-10-26T11:16:51Z

1 records found.
OK
List all trained-models successful
```
{: codeblock}

**注**：对于培训作业的详细信息，服务只会保留 7 天，在该时间之后，这些作业会除去并且不会显示在此列表中。

要监视特定作业，请使用 CLI 命令 `bx ml show trained-models <model-id>`：

```
bx ml show trained-models training-DOl4q2LkR
```
{: codeblock}

样本输出：

```
Fetching the trained model details with MODEL-ID 'training-DOl4q2LkR' ...
ModelId        training-DOl4q2LkR
url            /v3/models/training-DOl4q2LkR
Name           tf-mnist
State          running
Submitted_at   2017-10-26T11:10:37Z
OK
Show trained-models details successful
```
{: codeblock}

**注**：目前的已知问题是，失败作业不显示在列表中，并且会显示 CLI 命令输出，如同该作业已删除一样。此问题未来将予以更正，但在此之前，如果看到培训作业不显示，请按照下面的说明检查培训日志文件以了解作业失败的原因。

作业成功完成（或失败）后，经过培训的模型文件和日志应该会写入模型培训定义文件的 `traininging_results_reference` 设置中指定的 Cloud Object Storage 存储区，位于与模型标识同名的文件夹下。

## 删除培训运行

要删除培训作业（这不会除去已向 Object Storage 实例输出的经过培训的模型和日志，而是从服务中除去培训作业的所有历史记录），请执行以下命令：

```
bx ml delete trained-models training-DOl4q2LkR
```
{: codeblock}


样本输出：

```
Deleting the trained model 'training-DOl4q2LkR' ...
OK
Delete trained-models successful
```
{: codeblock}
