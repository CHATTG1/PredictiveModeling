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

# Onlinemodelle bereitstellen

Zum Bereitstellen eines Modelles und Generieren einer Vorhersageanalyse durch Erstellen von Scoring-Anforderungen für das bereitgestellte Modell verwenden Sie den {{site.data.keyword.pm_full}}-Service. Das folgende Szenario veranschaulicht die Vorgehensweise hierfür.
{: shortdesc}

**Szenarioname:** Produktlinienvorhersage.

**Szenariobeschreibung:** Eine Firma für Outdoorausrüstung möchte
ein Modell erstellen, das das Kundeninteresse an ihren Produktlinien
vorhersagt. Ein Data-Scientist hat ein Vorhersagemodell vorbereitet und
teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, das Modell bereitzustellen und eine Vorhersage zu generieren, indem Sie
Scoring-Anforderungen für das bereitgestellte Modell erstellen.

## Beispielmodell verwenden

1. Wechseln Sie zur Registerkarte **Beispiele** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Suchen Sie im Abschnitt **Beispielmodelle** die Kachel **Produktlinienvorhersage**
   und klicken Sie auf das Symbol **Modell hinzufügen** (+).

Das Beispielmodell **Produktlinienvorhersage** wird in der
List der verfügbaren Modelle in der Registerkarte **Modelle** angezeigt.


## Onlinebereitstellung erstellen

1. Wechseln Sie zur Registerkarte **Modelle** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Klicken Sie im Menü **Aktionen** auf **Bereitstellung erstellen**.
3. Geben Sie im Formular **Bereitstellung erstellen** die Felder **Name**, **Beschreibung** und **Onlinetyp** ein.
4. Klicken Sie auf **Speichern**.

Die Onlinebereitstellung wird in der Liste der verfügbaren Bereitstellungen auf der Registerkarte **Bereitstellungen** angezeigt.

## Bereitstellungsdetails abrufen

Sie können den Status, die Scoring-Endpunktadresse (`Scoring-Endpunkt`) und die Parameter des
Bereitstellungsmodells prüfen.

1. Wechseln Sie zur Registerkarte **Bereitstellungen** des {{site.data.keyword.pm_full}}-
   Dashboards.
2. Klicken Sie im Menü **Aktionen** auf **Details anzeigen**.

Beachten Sie den Wert für `Scoring-Endpunkt`, der erforderlich ist, um Scoring-Anforderungen zu erstellen.


## Scoring-Anforderungen stellen

Nachdem Sie einen Scoring-Endpunkt erstellt haben, können Sie Vorhersagen generieren, indem Sie Scoring-Anforderungen erstellen. Im folgenden Szenario werden Kundendatensätze an das Vorhersagemodell übergeben und eine Vorhersage für ein Sportprodukt zurückgegeben.

Beispieldatensatzheader:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Beispieldatensatz:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Anforderungsbeispiel:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Bearer  $token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Ausgabebeispiel:

```
{
   "fields": ["GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

Wir können beispielsweise erkennen, dass ein 55 Jahre alter Angestellter sich
für Bergsteigerausrüstung interessiert, während ein 23 Jahre alter Student sich für
Persönliches Zubehör interessiert.

## Weitere Informationen

Weitere Informationen zur API finden Sie unter [Service-API für Spark- und Python-Modelle](pm_service_api_spark.html) oder [Service-
API für IBM® SPSS® Modelle] (pm_service_api_spss.html).

Weitere Informationen zu IBM® SPSS® Modeler und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie im [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Weitere Informationen zu IBM® Data Science Experience und den von ihm bereitgestellten Modellierungsalgorithmen
finden Sie unter [https://datascience.ibm.com](https://datascience.ibm.com).
