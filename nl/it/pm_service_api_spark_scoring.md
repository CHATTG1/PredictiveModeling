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



# Calcolo modelli online

Utilizzando il servizio {{site.data.keyword.pm_full}}, puoi distribuire
un modello e generare l'analisi predittiva effettuando richieste di punteggio sul modello online distribuito. {: shortdesc}

**Nome scenario**: Previsione linea di prodotti.

**Descrizione scenario**: una società che vende attrezzature di outdoor
crea e distribuisce un modello per prevedere l'interesse del cliente nella sua
linea di prodotti. La tua azione è di calcolare le richieste per il modello distribuito.

Per ulteriori informazioni, vedi [Distribuzione di modelli
online](pm_service_api_spark_online.html).

I seguenti esempi utilizzano il valore `scoring_url` dai dettagli della distribuzione online.

Utilizza il seguente record di esempio per il calcolo:

```
{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}
```

## Esempi di runtime

[Bash](#bash) |  [Python](#python) | [Scala](#scala) | [Java](#java) | [JavaScript](#javascript)


### Bash

Devi richiamare i valori `username` e `password` dalle tue credenziali del servizio {{site.data.keyword.pm_short}}.

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Output:

```
{"token":"**********"}
```
{: codeblock}

Utilizza il seguente comando di terminale per assegnare il tuo valore token alla variabile di ambiente token:

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
            // Richiamo del token WML
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

            // Calcolo della richiesta
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

## Ulteriori informazioni

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli IBM® SPSS®](using_pm_service.html).

Per ulteriori informazioni sull'API, vedi [API del servizio per i modelli Spark e Python](pm_service_api_spark.html) o [API del
servizio per i modelli IBM® SPSS®](pm_service_api_spss.html).

Per ulteriori informazioni su IBM® SPSS® Modeler e sugli algoritmi di modellazione che fornisce, consulta
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per ulteriori informazioni su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
