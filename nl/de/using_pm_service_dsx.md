---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Service verwenden

Beachten Sie die folgenden wichtigen Informationen bezüglich des Machine Learning-Service.

## Schritte zum Binden des Service an eine Bluemix-Anwendung

Sie können Node.js-[Beispielcode](https://github.com/pmservice/product-line-prediction/blob/master/README.md) herunterladen, um den
Machine Learning-Service zu testen. Führen Sie die folgenden Schritte aus, um Ihre Bluemix-Anwendung
zu erstellen und den Machine Learning-Service zu binden. Diese Beispiele verwenden Node.js, da es eine gängige Laufzeit ist. Es können stattdessen auch andere Komponenten verwendet werden, beispielsweise iOS, Ruby, Perl oder Java.

Beachten Sie, dass Sie die Schritte 1 - 3 auch über die grafische Oberfläche von Bluemix anstatt mithilfe des Cloud Foundry-Tools
(cf) ausführen können.

1. Verwenden Sie den Befehl `cf create-service`, um eine Serviceinstanz zu erstellen:

   ```
   cf create-service pm-20 Free {local naming}
   ```
   {: codeblock}

   Beispiel:

   ```
   cf create-service pm-20 Free my_wml_free
   ```
   {: codeblock}

   Dieser Befehl erstellt eine Machine Learning-Serviceinstanz mit dem kostenfreien Plan
(Free) `my_wml_free` in Ihrem Bluemix-Bereich.

2. Verwenden Sie den Befehl `cf
create-service-key`, um Serviceberechtigungsnachweise zu
erstellen:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   Beispiel:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   Dieser Befehl erstellt Machine Learning-Serviceberechtigungsnachweise.

3. Verwenden Sie den Befehl cf bind-service, um die Serviceinstanz
   `my_wml_free` an Ihre Anwendung zu binden. 

   ```
   cf bind-service {AppName} my_wml_free
   ```
   {: codeblock}

   Beispiel:

   ```
   cf bind-service my_app1 my_wml_free
   ```
   {: codeblock}

   Dieser Befehl bindet die Machine Learning-Serviceinstanz
   `my_wml_free` an die Bluemix-Anwendung `my_app1`.



## Machine Learning-Berechtigungsnachweise

Nachdem Sie die Machine Learning-Serviceinstanz an Ihre Bluemix-Anwendung gebunden haben, werden die Machine Learning-Berechtigungsnachweise zur Umgebungsvariablen `VCAP_SERVICES` hinzugefügt:

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "Free",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   Die Umgebungsvariable `VCAP_SERVICES` umfasst die folgenden Informationen:

   * `plan` - Der Machine Learning-Plan, der für die Servicebereitstellung verwendet wird. 
   * `url` - Die Adresse der Machine Learning-Serviceinstanz.
   * `access_key` - Nur für SPSS Modeler-Datenströme.
   * `instance_id` - Eindeutige ID der Machine Learning-Instanz. 
   * `username`, `password` - Grundlegende Berechtigung, die erforderlich ist, um ein
Zugriffstoken (access token) zu generieren, das in allen Anforderungen an diese Serviceinstanz übergeben wird. Sie können z. B. ein Zugriffstoken (access token)
mithilfe eines curl-Befehls wie dem folgenden generieren:

Anforderungsbeispiel:

```
       curl --basic --user {username}:{password} https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Ausgabebeispiel:

       {"token":"**********"}
```
{: codeblock}

   Der folgende Node.js-Code ist ein Beispiel für das Abrufen des
Benutzernamens (username) und Kennworts
(password) aus der Umgebungsvariablen `VCAP_SERVICES`: 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
