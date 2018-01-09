---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolução de Problemas

Aqui estão as respostas para perguntas comuns sobre resolução de problemas no uso do {{site.data.keyword.pm_full}}.
{: shortdesc}

## Obtendo ajuda e suporte para aprendizado de máquina
{: #gettinghelp}

Se você tiver problemas ou perguntas ao usar o
{{site.data.keyword.pm_short}}, poderá obter ajuda procurando informações ou
fazendo perguntas em fórum. Também é possível abrir um chamado de suporte.

Ao usar os fóruns para fazer uma pergunta, marque a sua pergunta
para que ela possa ser vista pelas equipes de desenvolvimento do {{site.data.keyword.pm_short}}.

Se você tiver questões técnicas sobre {{site.data.keyword.pm_short}}, poste sua pergunta em
<a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Estouro
de pilha <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> e
identifique sua pergunta com "ibm-bluemix" e "machine-learning".

Para perguntas sobre o serviço e sobre as instruções de introdução, use o fórum <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">Respostas do IBM developerWorks dW <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Inclua as tags "machine-learning" e "bluemix".
Veja [Obtendo ajuda](https://console.bluemix.net/docs/support/index.html#getting-help) para obter mais detalhes sobre o uso dos fóruns.

Para mais informações sobre como abrir um chamado de suporte IBM ou sobre níveis de suporte e severidades de chamados, consulte [Entrando em contato com o suporte](https://console.bluemix.net/docs/support/index.html#contacting-support).

## O token de autorização não foi fornecido.
{: #ts_missing_authorization_token}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O token de autorização não foi fornecido no cabeçalho `Authorization`.
{: tsCauses}
 
Transmita o token de autorização no cabeçalho `Authorization`.
{: tsResolve}
       
## Token de autorização inválido.
{: #ts_invalid_authorization_token}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O token de autorização que foi fornecido não pode ser decodificado ou analisado.
{: tsCauses}
 
Transmita o token de autorização correto no cabeçalho `Authorization`.
{: tsResolve}
       
## O token de autorização e o instance_id que foi usado na solicitação não são os mesmos.
{: #ts_not_matching_authorization_token}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O token de autorização que foi usado não é gerado para a instância de serviço com a qual ele foi usado.
{: tsCauses}
 
Transmita o token de autorização no cabeçalho `Authorization` que corresponde à instância de serviço que está sendo usada.
{: tsResolve}
       
## O token de autorização está expirado.
{: #ts_expired_authorization_token}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O token de autorização está expirado.
{: tsCauses}
 
Transmita um token de autorização não expirado no cabeçalho `Authorization`.
{: tsResolve}
       
## A chave pública necessária para autenticação não está disponível.
{: #ts_missing_public_key}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Este é um problema de serviço interno.
{: tsCauses}
 
O problema precisa ser corrigido pela equipe de suporte.
{: tsResolve}
       
## A operação atingiu o tempo limite após {{timeout}}
{: #ts_operation_timeout}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O tempo limite ocorreu durante a execução da operação solicitada.
{: tsCauses}
 
Tente chamar a operação desejada novamente.
{: tsResolve}
       
## Exceção não manipulada do tipo {{type}} com {{status}}
{: #ts_unhandled_exception_with_status}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Este é um problema de serviço interno.
{: tsCauses}
 
Tente chamar a operação desejada novamente. Se esse problema ocorrer mais vezes, então, ele deverá ser corrigido pela equipe de suporte.
{: tsResolve}
       
## Exceção não manipulada do tipo {{type}} com {{response}}
{: #ts_unhandled_exception_with_response}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Este é um problema de serviço interno.
{: tsCauses}
 
Tente chamar a operação desejada novamente. Se esse problema ocorrer mais vezes, então, ele deverá ser corrigido pela equipe de suporte.
{: tsResolve}
       
## Exceção não manipulada do tipo {{type}} com {{json}}
{: #ts_unhandled_exception_with_json}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Este é um problema de serviço interno.
{: tsCauses}
 
Tente chamar a operação desejada novamente. Se esse problema ocorrer mais vezes, então, ele deverá ser corrigido pela equipe de suporte.
{: tsResolve}
       
## Exceção não manipulada do tipo {{type}} com {{message}}
{: #ts_unhandled_exception_with_message}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Este é um problema de serviço interno.
{: tsCauses}
 
Tente chamar a operação desejada novamente. Se esse problema ocorrer mais vezes, então, ele deverá ser corrigido pela equipe de suporte.
{: tsResolve}
       
## O objeto solicitado não pôde ser localizado.
{: #ts_not_found}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O recurso da solicitação não pôde ser localizado.
{: tsCauses}
 
Certifique-se de que você está se referindo ao recurso existente.
{: tsResolve}
       
## O banco de dados subjacente relatou muitas solicitações.
{: #ts_too_many_cloudant_requests}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
O usuário enviou muitas solicitações em um determinado período de tempo.
{: tsCauses}
 
Tente chamar a operação desejada novamente.
{: tsResolve}
       
 ## A definição da avaliação não é especificada nem na artifactModelVersion, nem na implementação. Ela precisa ser especificada \" +\n      \"em pelo menos em um dos lugares.
{: #ts_missing_evaluation_definition}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
A configuração de aprendizado não contém todas as informações necessárias
{: tsCauses}
 
Forneça `definition` na `learning configuration`
{: tsResolve}
       
## A avaliação requer configuração de aprendizado especificada para o modelo.
{: #ts_missing_learning_configuration}
 
Não há possibilidade de criar uma `learning iteration`.
{: tsSymptoms}
 
Não há nenhuma `learning configuration` definida para o modelo.
{: tsCauses}
 
Crie uma `learning configuration` e tente criar uma `learning iteration` novamente.
{: tsResolve}
       
## A avaliação requer que a instância spark seja fornecida no cabeçalho `X-Spark-Service-Instance`
{: #ts_missing_spark_definition_for_evaluation}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Nenhuma informação necessária existe na `learning configuration`
{: tsCauses}
 
Forneça um `spark_service` na configuração de aprendizado ou no cabeçalho `X-Spark-Service-Instance`
{: tsResolve}
       
## O modelo não contém nenhuma versão.
{: #ts_missing_latest_model_version}
 
Não há possibilidade de criar nenhuma implementação nem de definir a configuração de aprendizado.
{: tsSymptoms}
 
Há uma inconsistência relacionada à persistência do modelo.
{: tsCauses}
 
Tente persistir o modelo e executar a ação novamente.
{: tsResolve}
       
## A operação de correção pode modificar somente a configuração de aprendizado existente.
{: #ts_patch_non_existing_learning_configuration}
 
Não há possibilidade de chamar o método de correção da API de REST para corrigir a configuração de aprendizado.
{: tsSymptoms}
 
Não há nenhuma `learning configuration` definida para este modelo ou o modelo não existe.
{: tsCauses}
 
Assegure-se de que esse modelo exista e que a configuração de aprendizado já esteja definida.
{: tsResolve}
       
## A operação de correção espera exatamente uma operação de substituição.
{: #ts_patch_multiple_ops}
 
A implementação não pode ser corrigida.
{: tsSymptoms}
 
A carga útil da correção contém mais de uma operação ou a operação de correção é diferente de `replace`.
{: tsCauses}
 
Use somente uma operação na carga útil da correção que seja a operação `replace`
{: tsResolve}
       
## A carga útil fornecida não possui campos obrigatórios: FIELD ou os valores dos campos estão corrompidos.
{: #ts_invalid_request_payload}
 
Não há possibilidade de processar a ação que está relacionada ao acesso ao conjunto de dados subjacente.
{: tsSymptoms}
 
O acesso ao conjunto de dados não está definido corretamente.
{: tsCauses}
 
Corrija a definição de acesso para o conjunto de dados.
{: tsResolve}
       
## O método de avaliação fornecido: METHOD não é suportado. Valores suportados: VALUE.
{: #ts_evaluation_method_not_supported}
 
Não há possibilidade de criar a configuração de aprendizado.
{: tsSymptoms}
 
Um método de avaliação errado foi usado para criar a configuração de aprendizado.
{: tsCauses}
 
Use um método de avaliação suportado que seja um de: `regression`, `binary`, `multiclass`.
{: tsResolve}
       
## Pode haver somente uma avaliação ativa por modelo. A solicitação não pôde ser concluída devido a uma avaliação ativa existente: {{url}}
{: #ts_active_evaluation_conflict}
 
Não há possibilidade de criar outra iteração de aprendizado
{: tsSymptoms}
 
Pode haver somente uma avaliação em execução para o modelo.
{: tsCauses}
 
Veja a avaliação já em execução ou aguarde até que ela termine e inicie uma nova.
{: tsResolve}
       
## O tipo de implementação {{type}} não é suportado.
{: #ts_not_supported_deployment_type}
 
Não há possibilidade de criar a implementação.
{: tsSymptoms}
 
Um tipo de implementação não suportado foi usado.
{: tsCauses}
 
Um tipo de implementação suportado deve ser usado.
{: tsResolve}
       
## Entrada incorreta: ({{message}})
{: #ts_deserialization_error}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Há um problema com a análise de json.
{: tsCauses}
 
Certifique-se de que o json correto seja transmitido na solicitação.
{: tsResolve}
       
## Dados insuficientes - a métrica {{name}} não pôde ser calculada
{: #ts_missing_metric}
 
A iteração de aprendizado falhou
{: tsSymptoms}
 
O valor para a métrica com o limite definido não pôde ser calculado devido a dados de feedback insuficientes
{: tsCauses}
 
Revise e melhore os dados na origem de dados `feedback_data_ref` na `learning configuration`
{: tsResolve}
       
## Para o tipo {{type}}, uma instância spark deve ser fornecida no cabeçalho `X-Spark-Service-Instance`
{: #ts_missing_prediction_spark_definition}
 
A implementação não pode ser criada
{: tsSymptoms}
 
As implementações `batch` e `streaming` requerem que uma instância spark seja fornecida
{: tsCauses}
 
Forneça uma instância spark no cabeçalho `X-Spark-Service-Instance`
{: tsResolve}
       
## A ação {{action}} falhou com a mensagem {{message}}
{: #ts_http_client_error}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Houve um problema com a chamada de serviço subjacente.
{: tsCauses}
 
Se houver uma sugestão sobre como corrigir o problema, então, siga-a. Entre em contato com a equipe de suporte se não houver sugestão na mensagem ou se a sugestão não resolver o problema.
{: tsResolve}
       
## O caminho `{{path}}` não é permitido. O único caminho permitido para o fluxo de correção é `/status`
{: #ts_wrong_stream_patch_path}
 
Não há possibilidade de corrigir a implementação do fluxo.
{: tsSymptoms}
 
Um caminho errado foi usado para corrigir a implementação `stream`.
{: tsCauses}
 
Corrija a implementação do `stream` com a opção de caminho suportada que seja `/status` (que permite iniciar/parar o processamento do fluxo).
{: tsResolve}
       
## A operação de correção não é permitida para instância do tipo `{{$type}}`
{: #ts_patch_not_supported}
 
Não há possibilidade de corrigir a implementação.
{: tsSymptoms}
 
Um tipo de implementação errado está sendo corrigido.
{: tsCauses}
 
Corrija o tipo de implementação `stream`.
{: tsResolve}
       
## A conexão de dados `{{data}}` é inválida para feedback_data_ref
{: #ts_invalid_feedback_data_connection}
 
Não há possibilidade de criar uma `learning configuration` para o modelo.
{: tsSymptoms}
 
Uma origem de dados não suportada foi usada ao definir feedback_data_ref.
{: tsCauses}
 
Use somente um tipo de origem de dados suportado, que seja `dashdb`
{: tsResolve}
       
## O caminho {{path}} não é permitido. O único caminho permitido para o modelo de correção é `/deployed_version/url` ou `/deployed_version/href` para V2
{: #ts_patch_model_path_not_allowed}
 
Não há nenhuma opção para o modelo de correção.
{: tsSymptoms}
 
Um caminho errado foi usado durante a correção do modelo.
{: tsCauses}
 
Modelo de correção com caminho suportado que permite atualizar a versão do modelo implementado.
{: tsResolve}
       
## Analisando a falha: {{msg}}
{: #ts_parsing_error}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
A carga útil solicitada não pôde ser analisada com sucesso.
{: tsCauses}
 
Assegure-se de que a carga útil de sua solicitação esteja correta e que possa ser analisada corretamente.
{: tsResolve}
       
## Ambiente de tempo de execução para o modelo selecionado: {{env}} não é suportado para `learning configuration`. Ambientes suportados: [{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
Não há a opção para criar `learning configuration`
{: tsSymptoms}
 
O modelo para o qual a criação do `learning_configuration` foi tentada não é suportado.
{: tsCauses}
 
Crie uma `learning configuration` para modelo que possua um tempo de execução suportado.
{: tsResolve}
       
## O plano atual \'{{plan}}\' permite somente implementações {{limit}}
{: #ts_deployments_plan_limit_reached}
 
Não há possibilidade de criar a implementação.
{: tsSymptoms}
 
O limite para o número de implementações foi atingido para o plano atual.
{: tsCauses}
 
Faça upgrade para um plano que não tenha essa limitação.
{: tsResolve}
       
## A definição de conexão com o banco de dados não é válida ({{code}})
{: #ts_sql_error}
 
Não há possibilidade de utilizar a funcionalidade da `learning configuration`.
{: tsSymptoms}
 
A definição de conexão com o banco de dados não é válida.
{: tsCauses}
 
Tente corrigir o problema descrito pelo `code` retornado pelo banco de dados subjacente.
{: tsResolve}
       
## Houve problemas durante a conexão subjacente {{system}}
{: #ts_stream_tcp_error}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Houve um problema durante a conexão com o sistema subjacente. Isso pode ser um problema de rede temporário.
{: tsCauses}
 
Tente chamar a operação desejada novamente. Se isso ocorrer mais vezes, entre em contato com a equipe de suporte.
{: tsResolve}
       
## Erro ao extrair o cabeçalho X-Spark-Service-Instance: ({{message}})
{: #ts_spark_header_deserialization_error}
 
Não há possibilidade de chamar a API de REST que requer credenciais do Spark
{: tsSymptoms}
 
Há um problema com a decodificação base-64 ou com a análise de credenciais do Spark.
{: tsCauses}
 
Assegure-se de que as credenciais corretas do Spark foram codificadas corretamente com base-64. Para obter mais informações, consulte a documentação.
{: tsResolve}
       
## Esta funcionalidade é proibida para usuários não beta.
{: #ts_not_beta_user}
 
A API de REST desejada não pode ser chamada com sucesso.
{: tsSymptoms}
 
A API de REST que foi chamada está atualmente em beta.
{: tsCauses}
 
Se estiver interessado em participar, inclua-se na lista de espera. Os detalhes podem ser localizados na documentação.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
A API de REST não pode ser chamada com sucesso.
{: tsSymptoms}
 
Houve um problema com a chamada de serviço subjacente.
{: tsCauses}
 
Se houver uma sugestão sobre como corrigir o problema, então, siga-a. Entre em contato com a equipe de suporte se não houver sugestão na mensagem ou se a sugestão não resolver o problema.
{: tsResolve}
       
## Limite de taxa excedido.
{: #ts_rate_limit_exceeded}
 
Limite de taxa excedido.
{: tsSymptoms}
 
O limite de taxa para o plano atual foi excedido.
{: tsCauses}
 
Para resolver esse problema, adquira outro plano com um limite de taxa maior
{: tsResolve}
       
## Valor de parâmetro de consulta inválido `{{paramName}}`: {{value}}
{: #ts_invalid_query_parameter_value}
 
Erro de validação como valor incorreto transmitido para o parâmetro de consulta.
{: tsSymptoms}
 
Erro ao obter resultados para a consulta.
{: tsCauses}
 
Corrija o valor do parâmetro de consulta. Os detalhes podem ser localizados na documentação.
{: tsResolve}
       
## Tipo de token inválido: {{type}}
{: #ts_invalid_token_type}
 
Erro referente ao tipo de token.
{: tsSymptoms}
 
Erro na autorização.
{: tsCauses}
 
O token deve ser iniciado com o prefixo `Bearer`
{: tsResolve}
       
## Formato de token inválido. Um formato do token de acesso deve ser utilizado.
{: #ts_invalid_token_format}
 
Erro referente ao formato do token.
{: tsSymptoms}
 
Erro na autorização.
{: tsCauses}
 
O token deve ser um token de acesso e iniciar com o prefixo `Bearer`
{: tsResolve}

## O arquivo JSON de entrada está ausente ou é inválido: 400
{: #os_invalid_input}

A mensagem a seguir é exibida ao tentar efetuar uma pontuação on-line: **um arquivo JSON de entrada está ausente ou é inválido**.
{: tsSymptoms}

Esta mensagem é exibida quando a carga útil de entrada de pontuação não corresponde ao tipo de entrada esperado que é necessário para pontuar o modelo. Especificamente, as razões a seguir podem aplicar-se:

- A carga útil de entrada está vazia.
- O esquema de carga útil de entrada não é válido.
- Os tipos de dados de entrada não correspondem aos tipos de dados esperados.
{: tsCauses}

Corrija a carga útil de entrada. Certifique-se de que a carga útil possua sintaxe correta, um esquema válido e tipos de dados apropriados. Depois de fazer as correções, tente efetuar uma pontuação on-line novamente. Para problemas de sintaxe, verifique o arquivo JSON usando o comando `jsonlint`.
{: tsResolve}

## O token de autorização expirou: 401
{: #os_expired_authorization_token}

A mensagem a seguir é exibida ao tentar efetuar uma pontuação on-line: **Falha de autorização**.
{: tsSymptoms}

Esta mensagem é exibida quando o token que é usado para pontuação expira.
{: tsCauses}

Gere novamente o token para essa instância do {{site.data.keyword.pm_full}} e tente novamente. Se este problema ainda ocorrer, entre em contato com o Suporte IBM.
{: tsResolve}

## Identificação de implementação desconhecida: 404
{: #os_unkown_depid}

A mensagem a seguir é exibida ao tentar efetuar uma pontuação on-line: **Identificação de implementação desconhecida**. {: osSymptoms}

Esta mensagem é exibida quando o ID de implementação que é usado para pontuação não existe.
{: tsCauses}

Certifique-se de que você está fornecendo o ID de implementação correto. Caso contrário, implemente o modelo com o ID de implementação e, em seguida, tente novamente efetuar a pontuação.
{: tsResolve}

## Erro do servidor interno: 500
{: #os_internal_error}

A mensagem a seguir é exibida ao tentar efetuar uma pontuação on-line: **Erro de servidor interno**.  {: osSymptoms}

Esta mensagem será exibida se o fluxo de recebimento de dados do qual a pontuação on-line depende falhar.
{: tsCauses}

Depois de aguardar por um período de tempo, tente efetuar pontuação on-line novamente. Se falhar novamente, entre em contato com o Suporte IBM.
{: tsResolve}
              
