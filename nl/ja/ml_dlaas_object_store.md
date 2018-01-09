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

# 深層学習用のオブジェクト・ストアのセットアップ

{{site.data.keyword.pm_full}} サービスと、このサービスの一部である深層学習試験を使用した作業を行うには、Cloud Object Storage (control.softlayer.com) インスタンスまたは Object Storage OpenStack Swift (console.bluemix.net) インスタンスにアクセスできる必要があります。トレーニング・データを提供するためとトレーニング済みモデルおよびログを保管するために、クラウド・オブジェクト・ストレージ・バケットを定義する必要があり、この目的のために、同じクラウド・オブジェクト・ストレージ・インスタンスを使用することも、別々のインスタンスを使用することもできます。
{: shortdesc}

## クラウド・オブジェクト・ストレージ・インスタンスの作成

1. [{{site.data.keyword.Bluemix_short}} オブジェクト・ストレージ](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage)のページに進み、以下のいずれかのオプションを選択します。

   - 暗号化されないが無料 (ライト) ストレージ: [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - 暗号化されるが有料サービス: [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. オブジェクト・ストレージ・インスタンスを構成し、**「作成」**をクリックします。
3. サービスが作成された後、サイド・メニューで**「サービス資格情報」**をクリックして、オブジェクト・ストレージ・インスタンスにアクセスするための資格情報 (例えば、API URL、ユーザー名、およびパスワード) を取得します。

## Cloud Object Storage (IaaS) インスタンスへのアクセス

Cloud Object Storage (IaaS) サービスは、S3 互換 API を提供し、任意の互換クライアント・アプリケーションを使用してアクセス可能です。

## Amazon Web Services (AWS) コマンド・ライン・インターフェース (CLI)

1. `pip install awscli` を使用して [AWS CLI](https://aws.amazon.com/cli/) をインストールします。
2. AWS 資格情報ファイル (Linux の場合は ~/.aws/credentials にあります) 内に Cloud Object Storage (IaaS) の資格情報を構成します。

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

AWS CLI を使用して、バケットおよびファイルをリストおよび更新できます。その際、上記のプロファイルと、資格情報からのクラウド・オブジェクト・ストレージ・エンドポイントを指定します。例えば、インスタンス内のバケットをリストするには、以下のコマンドを実行します。

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Object Storage OpenStack Swift for Bluemix インスタンスへのアクセス

swift CLI またはクライアント・ライブラリーを使用できます (例えば、swift python クライアント・ライブラリーを使用してこのオブジェクト・ストアにアクセスできます)。詳しくは、[資料](https://console.bluemix.net/docs/services/ObjectStorage/index.html)を参照してください。

## データ・フォーマット

トレーニングおよびテストするデータ・セットのフォーマットはフレームワークによって異なります。例えば、Caffe ではデータ・セットは LevelDB または LMDB フォーマットでなければならず、Torch ではデータ・セットは Torch 専有フォーマットでなければなりません。この資料では、データは特定のフレームワークで必要とされるフォーマットに既になっていると想定しています。生データをフレームワーク固有のフォーマットに変換する方法の詳細は、本資料の範囲外です。詳しくは、使用する深層学習フレームワークの資料を参照してください。
