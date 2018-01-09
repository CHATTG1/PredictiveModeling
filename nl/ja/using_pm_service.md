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

# サービスの使用

IBM® SPSS® Modeler のモデル作成パレットにあるモデリング手法を使用してデータから新しい情報を引き出したり、予測モデルを開発したりできます。各手法によって、強みが異なり、適した機械学習問題の種類も異なります。
{: .shortdesc}

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

お使いの {{site.data.keyword.Bluemix_notm}} アプリケーションや IBM® SPSS® Modeler スコアリング枝設計の入出力要件が実装されたら、データ・アナリストはスコアリング枝の内部を変更できます。データ・アナリストは、ユーザーがアプリケーションを書き換えることなく予測分析を微調整できる機能を確保しながら、最新表示操作で使用されるモデル・アルゴリズムまでも変更することができます。



## サービスを {{site.data.keyword.Bluemix_notm}} アプリケーションとバインドする手順

{{site.data.keyword.Bluemix_notm}} アプリケーションを作成し、それを {{site.data.keyword.pm_short}} サービスにバインドするには、以下の手順を実行します。

1. [GitHub リポジトリー](https://github.com/pmservice/customer-satisfaction-prediction)から Node.js サンプル・アプリケーション・コードをダウンロードします。

2. `cf create-service` コマンドを使用して、サービス・インスタンスを作成します。


   ```
   cf create-service pm-20 lite {local naming}
   ```
   {: codeblock}

   例:

   ```
   cf create-service pm-20 lite my_pm_lite
   ```
   {: codeblock}

   このコマンドは、{{site.data.keyword.Bluemix_notm}} スペースに named my_pm_lite という名前で、ライト・プランを使用する 1 つの {{site.data.keyword.pm_short}} サービス・インスタンスを作成します。

3. `cf create-service-key` コマンドを使用して、サービス資格情報を作成します。

   ```
cf create-service-key "{service instance name}" {vcap key name}```
   {: codeblock}

   例:

   ```
cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```
   {: codeblock}

   このコマンドにより、{{site.data.keyword.pm_short}} サービス資格情報が作成されます。

4. `cf bind-service` コマンドを使用して、サービス・インスタンス `my_pm_lite` をアプリケーションにバインドします。

   ```
cf bind-service AppName my_pm_service```
   {: codeblock}

   例:

   ```
   cf bind-service my_app1 my_pm_lite
   ```
   {: codeblock}

   このコマンドは、{{site.data.keyword.pm_short}} サービス・インスタンス `my_pm_lite` を {{site.data.keyword.Bluemix_notm}} アプリケーション my_app1 にバインドします。

5. {{site.data.keyword.pm_short}} 資格情報:

   {{site.data.keyword.pm_short}} サービス・インスタンスをお使いの {{site.data.keyword.Bluemix_notm}} アプリケーションにバインドすると、{{site.data.keyword.pm_short}} 資格情報が `VCAP_SERVICES` 環境変数に追加されます。

```
    {   
        "pm-20": {
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "lite",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    }
```
{: codeblock}

   `VCAP_SERVICES` 環境変数には以下の情報が含まれます。


   <dl>

   <dt>plan</dt>
   <dd>サービス・プロビジョニングで使用される {{site.data.keyword.pm_short}} プラン。
</dd>

   <dt>url</dt>
   <dd>{{site.data.keyword.pm_short}} サービス・インスタンスのアドレス。</dd>

   <dt>access_key</dt>
   <dd>このサービス・インスタンスへのすべての要求に渡される照会パラメーター accessKey。</dd>

   </dl>

例:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   以下は、`VCAP_SERVICES` 環境変数から accessKey を取得する方法を示す Node.js サンプル・コードです。

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}

## 詳細はこちら

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[IBM® SPSS® モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API について詳しくは、[Spark モデルおよび Python モデル用のサービス API](pm_service_api_spark.html) または [IBM® SPSS® モデル用のサービス API] (pm_service_api_spss.html) を参照してください。

IBM® SPSS® Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
