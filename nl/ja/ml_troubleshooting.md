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

# トラブルシューティング

{{site.data.keyword.pm_full}} の使用に関する一般的なトラブルシューティングの質問と回答を示します。
{: shortdesc}

## Machine Learning のヘルプの利用およびサポート
{: #gettinghelp}

{{site.data.keyword.pm_short}} の使用時に問題または質問がある場合、フォーラムを通した情報検索または質問によってヘルプを利用できます。また、サポート・チケットをオープンすることも可能です。

フォーラムで質問する際には、{{site.data.keyword.pm_short}} 開発チームによって認識されるように、質問にタグを付けてください。

{{site.data.keyword.pm_short}}に関する技術的な質問がある場合は、<a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">スタック・オーバーフロー <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に質問を投稿し、質問に「ibm-bluemix」と「machine-learning」のタグを付けてください。

サービスと開始手順に関する質問については、<a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> フォーラムを使用してください。「machine-learning」と「bluemix」のタグを付けてください。
フォーラムの利用について詳しくは、[ヘルプの利用](https://console.bluemix.net/docs/support/index.html#getting-help)を参照してください。

IBM サポート・チケットのオープンや、サポート・レベルおよびチケットの重大度について詳しくは、[「サポートへの問い合わせ」](https://console.bluemix.net/docs/support/index.html#contacting-support)を参照してください。

## 許可トークンが提供されていませんでした。
{: #ts_missing_authorization_token}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
`Authorization` ヘッダー内に許可トークンが提供されていませんでした。
{: tsCauses}
 
許可トークンを `Authorization` ヘッダーに入れて渡してください。
{: tsResolve}
       
## 無効な許可トークン。
{: #ts_invalid_authorization_token}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
提供された許可トークンをデコードまたは構文解析できません。
{: tsCauses}
 
正しい許可トークンを `Authorization` ヘッダーに入れて渡してください。
{: tsResolve}
       
## 許可トークンと、要求で使用された instance_id が同じでありません。
{: #ts_not_matching_authorization_token}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
使用された許可トークンは、その使用対象だったサービス・インスタンス用に生成されていません。
{: tsCauses}
 
使用中のサービス・インスタンスに対応する許可トークンを `Authorization` ヘッダーに入れて渡してください。
{: tsResolve}
       
## 許可トークンは有効期限切れです。
{: #ts_expired_authorization_token}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
許可トークンは有効期限切れです。
{: tsCauses}
 
期限が切れていない許可トークンを `Authorization` ヘッダーに入れて渡してください。
{: tsResolve}
       
## 認証に必要な公開鍵が使用可能ではありません。
{: #ts_missing_public_key}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
これは内部サービス問題です。
{: tsCauses}
 
この問題はサポート・チームによって修正される必要があります。
{: tsResolve}
       
## {{timeout}} 後に操作がタイムアウトになりました
{: #ts_operation_timeout}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
要求された操作を実行中にタイムアウトが発生しました。
{: tsCauses}
 
操作を再起動してみてください。
{: tsResolve}
       
## {{status}} でタイプ {{type}} の処理されない例外
{: #ts_unhandled_exception_with_status}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
これは内部サービス問題です。
{: tsCauses}
 
操作を再起動してみてください。何度も発生する場合、この問題はサポート・チームによって修正される必要があります。
{: tsResolve}
       
## {{response}} でタイプ {{type}} の処理されない例外
{: #ts_unhandled_exception_with_response}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
これは内部サービス問題です。
{: tsCauses}
 
操作を再起動してみてください。何度も発生する場合、この問題はサポート・チームによって修正される必要があります。
{: tsResolve}
       
## {{json}} でタイプ {{type}} の処理されない例外
{: #ts_unhandled_exception_with_json}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
これは内部サービス問題です。
{: tsCauses}
 
操作を再起動してみてください。何度も発生する場合、この問題はサポート・チームによって修正される必要があります。
{: tsResolve}
       
## {{message}} でタイプ {{type}} の処理されない例外
{: #ts_unhandled_exception_with_message}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
これは内部サービス問題です。
{: tsCauses}
 
操作を再起動してみてください。何度も発生する場合、この問題はサポート・チームによって修正される必要があります。
{: tsResolve}
       
## 要求されたオブジェクトが見つかりませんでした。
{: #ts_not_found}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
要求リソースが見つかりませんでした。
{: tsCauses}
 
既存のリソースを参照していることを確認してください。
{: tsResolve}
       
## 基礎にあるデータベースが、要求が多すぎることを報告しました。
{: #ts_too_many_cloudant_requests}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
一定の時間内にユーザーが送信した要求が多すぎました。
{: tsCauses}
 
操作を再起動してみてください。
{: tsResolve}
       
 ## 評価の定義が、artifactModelVersion と deployment のどちらにも定義されていません。少なくともいずれかの場所に指定される必要があります。
{: #ts_missing_evaluation_definition}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
Learning Configuration に、必要なすべての情報が含まれていません。
{: tsCauses}
 
`learning configuration` 内に `definition` を指定してください。
{: tsResolve}
       
## 評価は、モデルの learning configuration が指定されていることを必要とします。
{: #ts_missing_learning_configuration}
 
`learning iteration` を作成する可能性がありません。
{: tsSymptoms}
 
モデルの `learning configuration` が定義されていません。
{: tsCauses}
 
`learning configuration` を作成し、`learning iteration` の作成を再試行してください。
{: tsResolve}
       
## 評価は、`X-Spark-Service-Instance` ヘッダー内に spark インスタンスが指定されることを必要とします。
{: #ts_missing_spark_definition_for_evaluation}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
`learning configuration` 内に必要なすべての情報がありません。
{: tsCauses}
 
Learning Configuration 内または `X-Spark-Service-Instance` ヘッダー内に `spark_service` を指定してください。
{: tsResolve}
       
## モデルにバージョンが含まれていません。
{: #ts_missing_latest_model_version}
 
デプロイメントを作成する可能性も learning configuration を設定する可能性もありません。
{: tsSymptoms}
 
モデルの永続性に関する不整合があります。
{: tsCauses}
 
モデルの永続化を再試行し、アクションの実行を再試行してください。
{: tsResolve}
       
## パッチ操作は既存の learning configuration のみを変更できます。
{: #ts_patch_non_existing_learning_configuration}
 
learning configuration のパッチを行うために patch REST API メソッドを起動する可能性がありません。
{: tsSymptoms}
 
このモデルに対して `learning configuration` が設定されていないか、モデルが存在しません。
{: tsCauses}
 
モデルが存在し、既に learning configuration が設定されていることを確認してください。
{: tsResolve}
       
## パッチ操作は、1 つの置換操作を予期しています。
{: #ts_patch_multiple_ops}
 
デプロイメントにパッチを行うことができません。
{: tsSymptoms}
 
パッチのペイロードに複数の操作が含まれているか、パッチ操作が `replace` 以外です。
{: tsCauses}
 
パッチのペイロードでは、`replace` 操作である 1 つのみの操作を使用してください。
{: tsResolve}
       
## 指定されたペイロードに必須フィールド: FIELD が欠落しているか、フィールドの値が破損しています。
{: #ts_invalid_request_payload}
 
基礎にあるデータ・セットへのアクセスに関連したアクションを処理する可能性がありません。
{: tsSymptoms}
 
データ・セットへのアクセスが適切に定義されていません。
{: tsCauses}
 
データ・セットのアクセス定義を修正してください。
{: tsResolve}
       
## 指定された評価メソッド: METHOD はサポートされていません。サポートされる値: VALUE。
{: #ts_evaluation_method_not_supported}
 
learning configuration を作成する可能性がありません。
{: tsSymptoms}
 
正しくない評価メソッドが learning configuration を作成するために使用されました。
{: tsCauses}
 
サポートされる評価メソッドを使用してください。`regression`、`binary`、`multiclass` のいずれかです。
{: tsResolve}
       
## モデル当たり 1 つのみの評価をアクティブにできます。既存のアクティブ評価が原因で要求を完了できませんでした: {{url}}
{: #ts_active_evaluation_conflict}
 
別の learning iteration を作成する可能性がありません。
{: tsSymptoms}
 
モデル当たり 1 つの評価のみを実行中にすることができます。
{: tsCauses}
 
既に実行中の評価を見るか、または、それが終了するまで待ってから新しい評価を開始してください。
{: tsResolve}
       
## デプロイメント・タイプ {{type}} はサポートされていません。
{: #ts_not_supported_deployment_type}
 
デプロイメントを作成する可能性がありません。
{: tsSymptoms}
 
サポートされていないデプロイメント・タイプが使用されました。
{: tsCauses}
 
サポートされているデプロイメント・タイプが使用される必要があります。
{: tsResolve}
       
## 入力が正しくありません: ({{message}})
{: #ts_deserialization_error}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
JSON の構文解析で問題があります。
{: tsCauses}
 
正しい JSON が要求で渡されていることを確認してください。
{: tsResolve}
       
## 不十分なデータ - メトリック {{name}} を計算できませんでした
{: #ts_missing_metric}
 
learning iteration が失敗しました。
{: tsSymptoms}
 
フィードバック・データが不十分であるため、しきい値が定義されたメトリックの値を計算できませんでした。
{: tsCauses}
 
`learning configuration` 内でデータ・ソース `feedback_data_ref` のデータを確認して改善してください。
{: tsResolve}
       
## タイプ {{type}} の場合、`X-Spark-Service-Instance` ヘッダー内に spark インスタンスが指定される必要があります
{: #ts_missing_prediction_spark_definition}
 
デプロイメントを作成できません
{: tsSymptoms}
 
`batch` デプロイメントおよび `streaming` デプロイメントは、spark インスタンスの指定を必要とします
{: tsCauses}
 
`X-Spark-Service-Instance` ヘッダー内に spark インスタンスを指定してください
{: tsResolve}
       
## アクション {{action}} がメッセージ {{message}} で失敗しました
{: #ts_http_client_error}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
基礎にあるサービスの起動で問題がありました。
{: tsCauses}
 
問題を修正するための提案があればそれに従ってください。メッセージ中に提案がないか、提案に従っても問題が解決しない場合は、サポート・チームにお問い合わせください。
{: tsResolve}
       
## パス `{{path}}` は許可されません。パッチ・ストリームに許可されるパスは `/status` のみです
{: #ts_wrong_stream_patch_path}
 
ストリーム・デプロイメントのパッチを行う可能性がありません。
{: tsSymptoms}
 
`stream` デプロイメントのパッチに、正しくないパスが使用されました。
{: tsCauses}
 
`stream` デプロイメントのパッチには、サポートされているパス・オプションである `/status` を使用してください (これはストリーム処理の開始/停止を許可します)。
{: tsResolve}
       
## タイプ `{{$type}}` のインスタンスにはパッチ操作は許可されません
{: #ts_patch_not_supported}
 
デプロイメントのパッチを行う可能性がありません。
{: tsSymptoms}
 
パッチの対象にしようとしているデプロイメント・タイプが正しくありません。
{: tsCauses}
 
`stream` デプロイメント・タイプにパッチを行ってください。
{: tsResolve}
       
## データ接続 `{{data}}` は feedback_data_ref には無効です
{: #ts_invalid_feedback_data_connection}
 
モデルの `learning configuration` を作成する可能性がありません。
{: tsSymptoms}
 
feedback_data_ref を定義するときに、サポートされていないデータ・ソースが使用されました。
{: tsCauses}
 
サポートされる唯一のデータ・ソース・タイプである `dashdb` を使用してください
{: tsResolve}
       
## パス {{path}} は許可されません。パッチ・モデルに許可されるパスは `/deployed_version/url` または `/deployed_version/href` (V2 の場合) のみです
{: #ts_patch_model_path_not_allowed}
 
モデルのパッチを行うオプションがありません。
{: tsSymptoms}
 
モデルのパッチを実行中に、正しくないパスが使用されました。
{: tsCauses}
 
デプロイされたモデルのバージョンを更新することを許可する、サポートされているパスを使用して、モデルのパッチを行ってください。
{: tsResolve}
       
## 構文解析失敗: {{msg}}
{: #ts_parsing_error}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
要求されたペイロードを正常に構文解析できませんでした。
{: tsCauses}
 
要求ペイロードが正しいことと、正しく構文解析できることを確認してください。
{: tsResolve}
       
## 選択されたモデルのランタイム環境: {{env}} は、`learning configuration` にはサポートされません。サポートされる環境: [{{supported_envs}}]。
{: #ts_runtime_env_not_supported}
 
`learning configuration` を作成するオプションがありません
{: tsSymptoms}
 
`learning_configuration` を作成しようとしたモデルはサポートされていません。
{: tsCauses}
 
サポートされるランタイムを持つモデルの `learning configuration` を作成してください。
{: tsResolve}
       
## 現在のプラン「{{plan}}」では {{limit}} デプロイメントのみが許可されます
{: #ts_deployments_plan_limit_reached}
 
デプロイメントを作成する可能性がありません。
{: tsSymptoms}
 
現在のプランでのデプロイメント数の限度に達しました。
{: tsCauses}
 
そのような制限のないプランにアップグレードしてください。
{: tsResolve}
       
## データベース接続定義が無効です ({{code}})
{: #ts_sql_error}
 
`learning configuration` 機能を利用する可能性がありません。
{: tsSymptoms}
 
データベース接続定義が無効です。
{: tsCauses}
 
基礎にあるデータベースによって返された `code` で示されている問題を修正してください。
{: tsResolve}
       
## 基礎にある {{system}} への接続中に問題がありました
{: #ts_stream_tcp_error}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
基礎にあるシステムへの接続中に問題がありました。一時的なネットワーク問題である可能性があります。
{: tsCauses}
 
操作を再起動してみてください。何度も発生する場合、サポート・チームにお問い合わせください。
{: tsResolve}
       
## X-Spark-Service-Instance ヘッダーの抽出でエラー: ({{message}})
{: #ts_spark_header_deserialization_error}
 
Spark 資格情報を必要とする REST API を起動する可能性がありません
{: tsSymptoms}
 
base-64 デコードまたは Spark 資格情報の構文解析で問題があります。
{: tsCauses}
 
正しい Spark 資格情報が base-64 で正しくエンコードされていることを確認してください。詳しくは、資料を参照してください。
{: tsResolve}
       
## この機能は非ベータ・ユーザーには禁止されています。
{: #ts_not_beta_user}
 
要求された REST API を正常に呼び出せません。
{: tsSymptoms}
 
呼び出された REST API は現在はベータ版です。
{: tsCauses}
 
参加をご希望の場合は、ご自身を待機リストに追加してください。詳細は資料に記載されています。
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
REST API を正常に呼び出すことができません。
{: tsSymptoms}
 
基礎にあるサービスの起動で問題がありました。
{: tsCauses}
 
問題を修正するための提案があればそれに従ってください。メッセージ中に提案がないか、提案に従っても問題が解決しない場合は、サポート・チームにお問い合わせください。
{: tsResolve}
       
## 速度制限を超えました。
{: #ts_rate_limit_exceeded}
 
速度制限を超えました。
{: tsSymptoms}
 
現在のプランの速度制限を超えました。
{: tsCauses}
 
この問題を解決するには、速度制限がもっと大きい別のプランを獲得してください。
{: tsResolve}
       
## 無効な照会パラメーター `{{paramName}}` 値: {{value}}
{: #ts_invalid_query_parameter_value}
 
正しくない照会パラメーター値が渡されたため、妥当性検査エラーが発生しました。
{: tsSymptoms}
 
照会結果の取得でエラー。
{: tsCauses}
 
照会パラメーター値を修正してください。詳細は資料に記載されています。
{: tsResolve}
       
## 無効なトークン・タイプ: {{type}}
{: #ts_invalid_token_type}
 
トークン・タイプに関するエラー。
{: tsSymptoms}
 
許可でのエラー。
{: tsCauses}
 
トークンは `Bearer` 接頭部で始まる必要があります
{: tsResolve}
       
## 無効なトークン・フォーマット。ベアラー・トークン・フォーマットが使用される必要があります。
{: #ts_invalid_token_format}
 
トークン・フォーマットに関するエラー。
{: tsSymptoms}
 
許可でのエラー。
{: tsCauses}
 
トークンはベアラー・トークンである必要があり、`Bearer` 接頭部で始まる必要があります
{: tsResolve}

## 入力 JSON ファイルが欠落しているか、無効です: 400
{: #os_invalid_input}

オンラインでのスコアリングを試行中に次のメッセージが表示されます: **入力 JSON ファイルが欠落しているか、無効です**。
{: tsSymptoms}

このメッセージは、入力ペイロードのスコアリングが、モデルのスコアリングに必要な予期される入力タイプと一致しない場合に表示されます。具体的には、以下の理由が考えられます。

- 入力ペイロードが空である。
- 入力ペイロード・スキーマが無効である。
- 入力データ・タイプと予期されるデータ・タイプが一致しない。
{: tsCauses}

入力ペイロードを修正してください。ペイロードの構文が正しく、スキーマが有効であり、データ・タイプが適切であることを確認してください。修正後、オンラインでのスコアリングを再試行してください。構文の問題の場合、`jsonlint` コマンドを使用して JSON ファイルを検証してください。
{: tsResolve}

## 許可トークンの有効期限が切れています: 401
{: #os_expired_authorization_token}

オンラインでのスコアリングを試行中に次のメッセージが表示されます: **許可は失敗しました**。
{: tsSymptoms}

このメッセージは、スコアリングに使用されるトークンの有効期限が切れている場合に表示されます。
{: tsCauses}

この {{site.data.keyword.pm_full}} インスタンス用のトークンを再生成し再試行してください。それでもこの問題が起こる場合は、IBM サポートにお問い合わせください。
{: tsResolve}

## 不明デプロイメント ID: 404
{: #os_unkown_depid}

オンラインでのスコアリングを試行中に次のメッセージが表示されます: **不明デプロイメント ID**。
{: osSymptoms}

このメッセージは、スコアリングに使用されるデプロイメント ID が存在しない場合に表示されます。
{: tsCauses}

正しいデプロイメント ID を指定していることを確認してください。そうでない場合、そのデプロイメント ID のモデルをデプロイし、その後でスコアリングを再試行してください。
{: tsResolve}

## 内部サーバー・エラー: 500
{: #os_internal_error}

オンラインでのスコアリングを試行中に次のメッセージが表示されます: **内部サーバー・エラー**
{: osSymptoms}

このメッセージは、オンラインでのスコアリングが依存している下流データ・フローが失敗した場合に表示されます。
{: tsCauses}

しばらく待ってから、オンラインでのスコアリングを再試行してください。再び失敗する場合は、IBM サポートにお問い合わせください。
{: tsResolve}
              
