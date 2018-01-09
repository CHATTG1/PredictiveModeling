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

# 設定物件儲存庫以進行深度學習

若要使用 {{site.data.keyword.pm_full}} 服務以及屬於該服務的深度學習實驗，您必須可以存取 Cloud Object Storage (control.softlayer.com) 或 Object Storage OpenStack Swift (console.bluemix.net) 實例。您必須定義 Cloud Object Storage 儲存區來提供訓練資料，以及儲存訓練模型及日誌，而且您可以基於此目的使用相同或不同的 Cloud Object Storage 實例。
{: shortdesc}

## 建立 Cloud Object Storage 實例

1. 移至 [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) 頁面，然後選取下列其中一個選項：

   - 未加密但免費（精簡）的儲存空間：[Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - 加密但非免費的服務：[Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. 配置 Object Storage 實例，然後按一下**建立**。
3. 建立服務之後，請在側邊功能表中按一下**服務認證**來取得認證（例如 API URL、使用者名稱及密碼），以存取 Object Storage 實例。

## 存取 Cloud Object Storage (IaaS) 實例

Cloud Object Storage (IaaS) 服務提供 S3 相容 API，而且可以使用任何相容用戶端應用程式進行存取。

## Amazon Web Services (AWS) 指令行介面 (CLI)

1. 使用 `pip install awscli` 來安裝 [AWS CLI](https://aws.amazon.com/cli/)
2. 在 AWS 認證檔（若為 Linux，位於 ~/.aws/credentials）中，配置 Cloud Object Storage (IaaS) 的認證。

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

您可以使用 AWS CLI 來列出及更新儲存區和檔案，並指定上述設定檔以及您認證中的 Cloud Object Storage 端點。例如，若要列出實例中的儲存區，請執行下列指令：

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## 存取 Object Storage OpenStack Swift for Bluemix 實例

您可以使用 swift CLI 或用戶端程式庫（例如，swift python 用戶端程式庫用來存取此物件儲存庫）。如需詳細資料，請參閱[文件](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## 資料格式

不同的架構需要不同格式的訓練及測試資料集。例如，Caffe 需要 LevelDB 或 LMDB 格式的資料集，而 Torch 需要 Torch 專有格式的資料集。假設資料已採用特定架構所需的格式。如何將原始資料轉換為架構特定格式的詳細資料不在本文件的範圍內。如需相關資訊，請參閱您想要使用的深度學習架構文件。
