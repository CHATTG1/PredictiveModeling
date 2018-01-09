---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Criar uma execução de treinamento

Execuções de treinamento são os princípios de organização para conduzir
experimentos de aprendizado profundo no {{site.data.keyword.pm_full}}. Um
experimento típico pode consistir em dezenas a centenas de execuções de treinamento. Cada
execução é definida individualmente e consiste nas seguintes partes: a rede neural
definido usando uma das [estruturas
de aprendizado profundo suportadas](ml_dlaas_supported_framework.html) e a configuração de como executar seu
treinamento incluindo o número de GPUs e o local do armazenamento de objeto que contém
seu conjunto de dados.
{: shortdesc}

<p align="center"><img src="images/experiment_to_training_runs_text.svg" alt="relação de experimentos para execuções de treinamento"></p>

## Criando um arquivo .zip de definição de modelo

Depois de definir a rede neural e a manipulação de dados associados usando uma das
[estruturas de aprendizado profundo
suportadas](ml_dlaas_supported_framework.html), compacte esses arquivos juntos usando o formato .zip. Por exemplo, se
o modelo foi escrito em Toch, compacte seus arquivos .lua; se em Caffe, compacte o
arquivo .prototxt; ou se em Tensorflow/Keras/MXNet, compacte seus arquivos .py. Outros
formatos de compactação, como gzip ou tar, não são suportados. Consulte a documentação
para a estrutura de aprendizado profundo que você deseja usar para preparar os arquivos de
definição de modelo.  

<!-- Supposedly this isn't true anymore >> NOTE: All model definition files must be in the first level of the zip file so ensure there are no nested directories in the zip file. -->

Por exemplo, um arquivo zip `tf-model.zip` que contém a definição de modelo para tensorflow pode conter a saída a seguir:

```
unzip -l tf-model.zip
```
{: codeblock}

Saída de amostra:

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

## Faça upload de dados de treinamento

Seus dados de treinamento devem ser
[transferidos por upload para uma instância de
serviço de armazenamento de objeto compatível](ml_dlaas_object_store.html). As credenciais da instância de
armazenamento de objeto serão usadas abaixo em seu arquivo manifest abaixo. O
armazenamento de objeto também é usado para armazenar o modelo treinado no final da sua
execução de treinamento.

## Criando um arquivo manifest de treinamento

O manifest é um arquivo formatado YAML que contém campos diferentes que descrevem
o modelo a ser treinado, incluindo a estrutura de aprendizado profundo a ser usada, a
configuração do armazenamento de objeto de nuvem, os requisitos de recurso e vários
argumentos (incluindo hiperparâmetros) necessários para a execução do modelo durante o
treinamento e teste. Abaixo, descrevemos os diferentes campos do arquivo de treinamento
de modelo, continuando nosso exemplo de reconhecimento de caligrafia tensorflow.

* `model_definition.name`: é possível fornecer qualquer valor para o nome para ajudar a identificar sua tarefa de treinamento após ela ser ativada. No entanto, isso não precisa ser exclusivo - o serviço receberá um ID de modelo exclusivo para cada tarefa de treinamento ativada.
* `model_definition.description`: este é outro campo que pode ser usado para descrever a tarefa.
* `model_definition.author`: opcional. Forneça o nome do autor e o endereço de e-mail sob as chaves *name* e *email*.
* `model_definition.framework`: este campo fornece informações específicas da estrutura; o nome e a versão devem corresponder a uma das [estruturas de aprendizado profundo suportadas](ml_dlaas_supported_framework.html).
    - `model_definition.framework.name`: nome de estrutura
    - `model_definition.framework.version`: versão da estrutura.
* `model_definition.execution`: este campo fornece informações sobre o comando para ativar o treinamento.
    - `model_definition.execution.command`: este campo identifica o arquivo de programa principal, com quaisquer argumentos que aprendizado profundo precisa executar.
    - `model_definition.execution.resource`: este campo especifica os recursos que serão alocados para treinamento e deve ser um dos valores a seguir - `small` (1 GPU), `medium` (2 GPUs), `large` (4 GPUs)
* `training_data_reference`: essa seção especifica uma lista dos armazenamentos de objeto dos quais os arquivos de dados usados para treinar o modelo são carregados. Atualmente, essa lista deve conter um e apenas um armazenamento de objeto, com a seguinte definição:
    - `connection`: as variáveis de conexão para o armazenamento de dados.
    - `source.type`: tipo de armazenamento de dados; atualmente, isso só pode ser configurado para s3 ou bluemix_objectstore. Use `s3` se a sua instância de armazenamento de objetos for *Cloud Object Storage (IaaS)* e `bluemix_objectstore` se a sua instância de armazenamento de objetos for *Object Storage OpenStack Swift for Bluemix*.
    - `source.bucket`: o depósito onde os dados de treinamento residem.
* `training_results_reference`: essa seção especifica o armazenamento de objeto no qual os arquivos de modelo resultantes e os logs serão armazenados após o treinamento concluído.
    - `connection`: as variáveis de conexão para o armazenamento de dados. A lista de variáveis de conexão suportadas é dependente do tipo de armazenamento de dados.
    - `target.type`: tipo de armazenamento de dados; atualmente, isso só pode ser configurado para s3 ou bluemix_objectstore. Use `s3` se a sua instância de armazenamento de objetos for *Cloud Object Storage (IaaS)* e `bluemix_objectstore` se a sua instância de armazenamento de objetos for *Object Storage OpenStack Swift for Bluemix*.
    - `target.bucket`: o depósito no qual os resultados do treinamento serão gravados.

Por exemplo, o seguinte arquivo de definição de treinamento de modelo pode ser usado para definir uma tarefa para treinar um modelo tensorflow:

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

em que `convolutional_network.py` é o programa tensorflow (que faz parte do zip de definição de modelo) a ser executado, enquanto o restante são argumentos para o programa. Os valores de argumentos de programa `--trainImagesFile train-images-idx3-ubyte.gz`, `--trainLabelsFile train-labels-idx1-ubyte.gz`, `--testImagesFile t10k-images-idx3-ubyte.gz`, `--testLabelsFile t10k-labels-idx1-ubyte.gz` se referem aos caminhos de conjunto de dados no contêiner de armazenamento de objeto `tf_training_data`. Os argumentos de programa `-- trainingIters 20000` e `-- learningRate 0,001` passam os valores de hiperparâmetros.

**Nota**: o treinamento de arquivos de definição de configuração ou modelo se refere a arquivos transferidos por upload para a instância de armazenamento de objeto; as referências devem usar caminhos relativos, conforme mostrado acima.

**Nota**: antes do início do treinamento, todos os arquivos no compartimento de dados de treinamento são transferidos por download para o ambiente de treinamento operado pelo serviço. Para evitar sobrecarga/atraso de transferência de arquivos desnecessários, mantenha os arquivos não usados para treinar arquivos em depósitos separados.

**Nota**: no exemplo acima, o armazenamento de objeto usado para fornecer os dados e armazenar o modelo resultante é o *Armazenamento de objeto de nuvem (IaaS)*. Por outro lado, se o armazenamento de objeto que está sendo usada fosse *Object Storage OpenStack Swift para Bluemix*, as chaves de conexão seriam diferentes; um manifest de exemplo é fornecido abaixo:

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

**Nota** para conexões *Object Storage Open Stack Swift para Bluemix*, o mapeamento entre nomes de chave de suas credenciais de armazenamento de objeto e os nomes de chave requeridos no manifest:

| {{site.data.keyword.Bluemix_notm}} Chave de credencial  | Chave de credenciais de manifest de treinamento |
|----------------------------------------------------|----------------------------------------|
|auth_url |auth_url |
|username |user_name |
|password |password |
|projectId |project_id |
|região |região |
|domainName |domain_name |
{: caption="Tabela 1. {{site.data.keyword.Bluemix_notm}} e chaves de credencial de manifest de treinamento" caption-side="top"}

## Enviar uma execução de treinamento

Depois de preparar o arquivo .zip de definição de modelo e os arquivos de configuração de treinamento, envie a tarefa usando o comando `bx ml train`: `bx ml trem <path-to-model-definition-zip> <path-to-model-configuration-yaml>` 

```
bx ml train tf-model.zip job.yaml
```
{: codeblock}

Saída de amostra:

Quando o comando é enviado com êxito, um ID de modelo exclusivo é retornado. Por exemplo, a saída a seguir mostra um valor `Model-ID` de `training-DOl4q2LkR`:

```
Starting to train ...
OK
Model-ID is 'training-DOl4q2LkR'
```

# Monitorar uma execução de treinamento

Para listar todas as tarefas de treinamento (concluídas ou não), use o comando da CLI `bx ml list trained-models`

```
bx ml list trained-models
```
{: codeblock}

Saída de amostra:

```
Fetching the list of trained models ...
SI No   Name                       guid                 status    submitted-at
1       tf-mnist                   training-DOl4q2LkR   pending   2017-10-26T11:16:51Z

1 records found.
OK
List all trained-models successful
```
{: codeblock}

**Nota**: o serviço só manterá os detalhes das tarefas de treinamento por 7 dias; após esse tempo, eles serão removidos e não aparecerão nessa lista.

Para monitorar uma tarefa específica, use o comando `bx ml show trained-models <model-id>`:

```
bx ml show trained-models training-DOl4q2LkR
```
{: codeblock}

Saída de amostra:

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

**Nota**: existe atualmente um problema conhecido com tarefas
com falha que desaparecem da lista e mostra a saída do comando da CLI, como se a tarefa
tivesse sido excluída. Esse problema será corrigido, mas enquanto isso, se você vir uma
tarefa de treinamento desaparecer, verifique os arquivos de log de treinamento conforme
explicado abaixo para descobrir por que a tarefa falhou.

Quando uma tarefa é concluída com sucesso (ou com falha), os arquivos de modelo treinado e os logs devem ser gravadas no depósito de armazenamento de objeto de nuvem especificado na configuração `training_results_reference` dentro do arquivo de definição de treinamento de modelo, em uma pasta com o mesmo nome do ID de modelo.

## Excluir uma execução de treinamento

Para excluir uma tarefa de treinamento (isso não removerá o modelo treinado e os
logs que foram enviados para sua instância de armazenamento de objeto, mas removerá todo o
histórico da tarefa de treinamento do serviço).

```
bx ml delete trained-models training-DOl4q2LkR
```
{: codeblock}


Saída de amostra:

```
Deleting the trained model 'training-DOl4q2LkR' ...
OK
Delete trained-models successful
```
{: codeblock}
