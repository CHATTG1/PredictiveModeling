---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Modelos de pontuação on-line

Usando o serviço {{site.data.keyword.pm_full}}, é possível implementar um
modelo e gerar análise preditiva fazendo solicitações de escore com relação ao modelo
implementado on-line.
{: shortdesc}

**Nome do cenário**: predição de linha de produto.

**Descrição do cenário**: uma empresa que vende
equipamento de outdoor constrói e implementa um modelo para prever o interesse do cliente
em sua linha de produtos. Sua tarefa é fazer solicitações de escore com relação ao modelo
implementado.

Para obter mais
informações, consulte [Implementando
modelos on-line](pm_service_api_spark_online.html).

Os exemplos a seguir usam o valor `scoring_url` dos detalhes da
implementação on-line.

Use o registro de amostra a seguir para pontuação:

```
{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}
```

## Exemplos de tempo de execução

[Bash](#bash) |  [Python](#python) | [Scala](#scala) | [Java](#java) | [JavaScript](#javascript)


### Bash

Deve-se recuperar os valores `username` e `password` nas credenciais de serviço dos valores de sua {{site.data.keyword.pm_short}}.

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Saída:

```
{"token":"**********"}
```
{: codeblock}

Use o comando do terminal a seguir para designar seu valor do token para o token de variável de ambiente:

```
token="<token_value>"
```
{: codeblock}

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: Bearer  $token" -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' scoring_url
```
{: codeblock}


### Python

```
import urllib3, requests, json

wml_credentials={
  "url": "https://ibm-watson-ml.mybluemix.net",
  "access_key": "***",
  "username": "***",
  "password": "***",
  "instance_id": "***"
}

headers = urllib3.util.make_headers(basic_auth='{username}:{password}'.format(username=wml_credentials['username'], password=wml_credentials['password']))
url = '{}/v3/identity/token'.format(wml_credentials['url'])
response = requests.get(url, headers=headers)
mltoken = json.loads(response.text).get('token')

header = {'Content-Type': 'application/json', 'Authorization': 'Bearer ' + mltoken}

payload_scoring = {"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}

response_scoring = requests.post(scoring_url, json=payload_scoring, headers=header)
```
{: codeblock}

### Scala

```
import scalaj.http.{Http, HttpOptions}
import scala.util.{Success, Failure}
import java.util.Base64
import java.nio.charset.StandardCharsets
import play.api.libs.json._

val wml_service_path = "https://ibm-watson-ml.mybluemix.net"
val wml_instance_id = "***"
val wml_username = "***"
val wml_password = "***"

// Get WML service instance token

val wml_auth_header = "Basic " + Base64.getEncoder.encodeToString((wml_username + ":" + wml_password).getBytes(StandardCharsets.UTF_8))
val wml_url = service_path + "/v3/identity/token"
val wml_response = Http(wml_url).header("Authorization", wml_auth_header).asString
val wmltoken_json: JsValue = Json.parse(wml_response.body)

val wmltoken = (wmltoken_json \ "token").asOpt[String] match {
    case Some(x) => x
    case None => ""
}

val payload_scoring = Json.stringify(Json.toJson(Map("fields" -> Json.toJson(List(Json.toJson("GENDER"), Json.toJson("AGE"), Json.toJson("MARITAL_STATUS"), Json.toJson("PROFESSION"))), "values" -> Json.toJson(List(List(Json.toJson("M"), Json.toJson(23), Json.toJson("Single"), Json.toJson("Student")), List(Json.toJson("M"), Json.toJson(55), Json.toJson("Single"), Json.toJson("Executive")))))))

val response_scoring = Http(scoring_url).postData(payload_scoring).header("Content-Type", "application/json").header("Authorization", "Bearer " + wmltoken).option(HttpOptions.method("POST")).option(HttpOptions.connTimeout(10000)).option(HttpOptions.readTimeout(50000)).asString
```
{: codeblock}

### Java

```
import java.io.*;
import java.net.MalformedURLException;
import java.util.Base64;
import java.util.HashMap;
import java.util.Map;
import java.net.HttpURLConnection;
import java.net.URL;

import java.nio.charset.StandardCharsets;

public class HttpClientTest {
    public static void main(String[] args) throws IOException {
        Map<String, String> wml_credentials = new HashMap<String, String>()
        {{
            put("url", "https://ibm-watson-ml.mybluemix.net");
            put("access_key", "***");
            put("username", "***");
            put("password", "***");
            put("instance_id", "***");
        }};

        String wml_auth_header = "Basic " +
                Base64.getEncoder().encodeToString((wml_credentials.get("username") + ":"
                                                  + wml_credentials.get("password")).getBytes(StandardCharsets.UTF_8));

        String wml_url = wml_credentials.get("url") + "/v3/identity/token";

        HttpURLConnection tokenConnection = null;
        HttpURLConnection scoringConnection = null;

        BufferedReader tokenBuffer = null;
        BufferedReader scoringBuffer = null;

        try {
            // Getting WML token
            URL tokenUrl = new URL(wml_url);
            tokenConnection = (HttpURLConnection) tokenUrl.openConnection();

            tokenConnection.setDoInput(true);
            tokenConnection.setDoOutput(true);
            tokenConnection.setRequestMethod("GET");
            tokenConnection.setRequestProperty("Authorization", wml_auth_header);

            tokenBuffer = new BufferedReader(new InputStreamReader(tokenConnection.getInputStream()));
            StringBuffer jsonString = new StringBuffer();
            String line;
            while ((line = tokenBuffer.readLine()) != null) {
                jsonString.append(line);
            }

            // Scoring request
            URL scoringUrl = new URL(scoring_url);

            String wml_token = "Bearer " +
                    jsonString.toString()
                              .replace("\"","")
                              .replace("}", "")
                              .split(":")[1];

            scoringConnection = (HttpURLConnection) scoringUrl.openConnection();

            scoringConnection.setDoInput(true);
            scoringConnection.setDoOutput(true);
            scoringConnection.setRequestMethod("POST");
            scoringConnection.setRequestProperty("Accept", "application/json");
            scoringConnection.setRequestProperty("Authorization", wml_token);
            scoringConnection.setRequestProperty("Content-Type", "application/json; charset=UTF-8");
            OutputStreamWriter writer = new OutputStreamWriter(scoringConnection.getOutputStream(), "UTF-8");

            String payload = "{\"fields\": [\"GENDER\",\"AGE\",\"MARITAL_STATUS\",\"PROFESSION\"]," +
                              "\"values\": [[\"M\",23,\"Single\",\"Student\"],[\"M\",55,\"Single\",\"Executive\"]]}";

            writer.write(payload);
            writer.close();
            scoringBuffer = new BufferedReader(new InputStreamReader(scoringConnection.getInputStream()));
            StringBuffer jsonStringScoring = new StringBuffer();
            String lineScoring;
            while ((lineScoring = scoringBuffer.readLine()) != null) {
                jsonStringScoring.append(lineScoring);
            }

            System.out.println(jsonStringScoring);

        } catch (IOException e) {
            System.out.println("The URL is not valid.");
            System.out.println(e.getMessage());
        }

        finally {
            if (tokenConnection != null) {
                tokenConnection.disconnect();
            }

            if (tokenBuffer != null) {
                tokenBuffer.close();
            }

            if (scoringConnection != null) {
                scoringConnection.disconnect();
            }

            if (scoringBuffer != null) {
                scoringBuffer.close();
            }
        }
    }
}
```
{: codeblock}

### JavaScript

```
var wml_credentials = new Map();

wml_credentials.set("url", "https://ibm-watson-ml.mybluemix.net");
wml_credentials.set("username", "***");
wml_credentials.set("password", "***");

function apiGet(url, username, password, loadCallback, errorCallback){
    var oReq = new XMLHttpRequest();

    const TOKEN_PATH = '/v3/identity/token';

    tokenHeader = "Basic " + btoa((wml_credentials.get("username") + ":"
                                 + wml_credentials.get("password")));

    tokenUrl = url + TOKEN_PATH;

    oReq.addEventListener("load", loadCallback);
    oReq.addEventListener("error", errorCallback);
    oReq.open("GET", tokenUrl);
    oReq.setRequestHeader("Authorization", tokenHeader);
    oReq.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
    oReq.send();
}

function apiPost(scoring_url, token, payload, loadCallback, errorCallback){
    var oReq = new XMLHttpRequest();
    oReq.addEventListener("load", loadCallback);
    oReq.addEventListener("error", errorCallback);
    oReq.open('POST', url);
    oReq.setRequestHeader("Accept", "application/json");
    oReq.setRequestHeader("Authorization", token);
    oReq.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
    oReq.send(payload);
}

apiGet(wml_credentials.get("url"),
    wml_credentials.get("username"),
    wml_credentials.get("password"),
    function (res) {
        token = JSON.parse(res.srcElement.response).token;
        console.log(token)

        wmlToken = "Bearer " + token;

        payload = "{\"fields\": [\"GENDER\",\"AGE\",\"MARITAL_STATUS\",\"PROFESSION\"]," +
            "\"values\": [[\"M\",23,\"Single\",\"Student\"],[\"M\",55,\"Single\",\"Executive\"]]}";

        apiPost(scoring_url, wmlToken, payload, function (resp) {
            response = JSON.parse(resp.srcElement.response);
            console.log(response);
        }, function (error) {
            console.log(error);
        });

    }, function (err) {
        console.log(err);
    });
```
{: codeblock}

## Saiba mais

Pronto para começar? Para criar uma instância de um serviço ou ligar um aplicativo, consulte [Usando o serviço com modelos Spark e Python](using_pm_service_dsx.html) ou [Usando o serviço com os modelos do IBM® SPSS®](using_pm_service.html).

Para obter mais informações sobre a API, consulte
[API de serviço para modelos Spark e Python](pm_service_api_spark.html)
ou [API de serviço para modelos IBM® SPSS®](pm_service_api_spss.html).

Para obter mais informações sobre o IBM® SPSS® Modeler e os algoritmos de modelagem que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Para obter mais informações sobre o IBM Data Science Experience e os algoritmos de modelagem que ele fornece, consulte [https://datascience.ibm.com](https://datascience.ibm.com).
