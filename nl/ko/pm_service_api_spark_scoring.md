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



# 온라인 모델 스코어링

{{site.data.keyword.pm_full}} 서비스를 사용하여, 모델을 배치하고 배치된 온라인 모델에 대해 스코어링을 요청함으로써 예측 분석을 생성할 수 있습니다.
{: shortdesc}

**시나리오 이름**: 제품 라인 예측

**시나리오 설명**: 아웃도어 장비를 판매하는 회사가 자사 제품 라인에 대한 고객 관심도를 예측하는 모델을 빌드하여 배치하고자 합니다. 사용자의 태스크는 배치된 모델에 대한 스코어링을 요청하는 것입니다. 

자세한 정보는 [온라인 모델 배치](pm_service_api_spark_online.html)를 참조하십시오. 

다음 예는 온라인 배치 세부사항의 `scoring_url` 값을 사용합니다. 

스코어링을 위해 다음 샘플 레코드를 사용하십시오. 

```
{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}
```

## 런타임 예

[Bash](#bash) |  [Python](#python) | [Scala](#scala) | [Java](#java) | [JavaScript](#javascript)


### Bash

{{site.data.keyword.pm_short}} 서비스 신임 정보에서 `username` 및 `password` 값을 검색해야 합니다. 

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

출력:

```
{"token":"**********"}
```
{: codeblock}

환경 변수 토큰에 토큰 값을 지정하려면 다음 터미널
명령을 사용하십시오.

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

## 자세히 보기

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을 바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[IBM® SPSS® 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오. 

API에 대한 자세한 정보는 [Spark 및 Python 모델용 서비스 API](pm_service_api_spark.html) 또는 [IBM® SPSS® 모델용 서비스 API](pm_service_api_spss.html)를 참조하십시오. 

IBM® SPSS® Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 자세한 정보는 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
