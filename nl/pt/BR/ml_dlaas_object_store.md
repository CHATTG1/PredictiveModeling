---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurar um armazenamento de objeto para aprendizado profundo

Para trabalhar com o serviço {{site.data.keyword.pm_full}} e os experimentos de aprendizado profundo que fazem parte desse serviço, deve-se ter acesso a uma instância de armazenamento de objeto de nuvem (control.softlayer.com) ou Object Storage OpenStack Swift (console.bluemix.net). Deve-se definir depósitos de armazenamento de objeto de nuvem para fornecer os dados de treinamento e armazenar o modelo treinado e os logs. Para esse propósito é possível usar as mesmas instâncias de armazenamento de objeto de nuvem ou instâncias separadas.
{: shortdesc}

## Criando uma instância de armazenamento de objeto de nuvem

1. Acesse a página [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) e selecione uma das opções a seguir:

   - Armazenamento não criptografado, mas livre (Lite): [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - Serviço criptografado, mas não livre: [Armazenamento de objeto de nuvem](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. Configure sua instância de armazenamento de objeto e clique em **Criar**.
3. Após o serviço ser criado, no menu lateral, clique em **Credenciais de serviço** para obter as credenciais, como a URL da API, o nome de usuário e a senha para acessar a instância de armazenamento de objeto.

## Acessando a instância de armazenamento de objeto de nuvem (IaaS)

O serviço de armazenamento de objeto de nuvem (IaaS) fornece APIs compatíveis com
S3 e pode ser acessado usando qualquer aplicativo cliente compatível.

## Interface da linha de comandos (CLI) do Amazon Web Services (AWS)

1. Instale a [CLI do AWS](https://aws.amazon.com/cli/) usando `pip install awscli`
2. Configure as credenciais para o armazenamento de objeto de nuvem (IaaS) em seu arquivo de credenciais AWS (para linux, localizado em ~/.aws/credentials).

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

É possível usar a CLI do AWS para listar e atualizar depósitos e arquivos,
especificando o perfil acima e o terminal de armazenamento de objeto de nuvem de suas
credenciais. Por exemplo, para listar os depósitos na instância, execute o comando a
seguir:

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Acessando o Object Storage OpenStack Swift para a instância do Bluemix

É possível usar a CLI ou as bibliotecas do cliente swift (por exemplo, a
biblioteca do cliente python swift para acessar esse armazenamento de objeto). Para obter
mais detalhes, consulte
[a
documentação](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## Formatação de dados

Diferentes estruturas requerem treinamento e teste de conjuntos de dados em
formatos diferentes. Por exemplo, o Caffe requer conjuntos de dados no formato LevelDB ou
LMDB, enquanto o Torch requer conjuntos de dados no formato do proprietário Torch. Presumimos
que os dados já estejam no formato exigido pela estrutura específica. Os detalhes sobre
como converter dados brutos em formato de estrutura específica estão fora do escopo deste
documento. Consulte a documentação para a estrutura de aprendizado profundo que você
deseja usar para obter informações adicionais.
