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

# Objektspeicher für Deep Learning einrichten

Um mit dem {{site.data.keyword.pm_full}}-Service und den Deep-Learning-Experimenten, die Teil dieses Service sind, arbeiten zu können, müssen Sie Zugriff zu einer Instanz von Cloud Object Storage (control.softlayer.com) oder Object Storage OpenStack Swift (console.bluemix.net) haben. Sie müssen Cloud Object Storage-Buckets definieren, um die Trainingsdaten bereitzustellen und das trainierte Modell und die Protokolle zu speichern, und Sie können dieselben oder separate Cloud Object Storage-Instanzen zu diesem Zweck verwenden.
{: shortdesc}

## Cloud Object Storage-Instanz erstellen

1. Wechseln Sie zur Seite [{{site.data.keyword.Bluemix_short}} Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) und wählen Sie eine der folgenden Optionen aus:

   - Nicht verschlüsselte, aber freie (Lite-) Speicherung: [Object Storage OpenStack Swift](https://console.bluemix.net/catalog/services/object-storage) (console.bluemix.net)
   - Verschlüsselter, aber kostenpflichtiger Service: [Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage) (control.softlayer.com)
   
2. Konfigurieren Sie Ihre Object Storage-Instanz und klicken Sie auf **Erstellen**.
3. Nach dem Erstellen des Service klicken Sie im Seitenmenü auf **Serviceberechtigungsnachweise**, um die Berechtigungsnachweise abzurufen, z. B. API-URL, Benutzername und Kennwort, um auf die Object Storage-Instanz zugreifen zu können.

## Zugriff auf die Cloud Object Storage (IaaS)-Instanz

Der Cloud Object Storage- (IaaS-) Service bietet S3-kompatible APIs und ist über jede kompatible Clientanwendung zugänglich.

## Befehlszeilenschnittstelle für Amazon Web Services (AWS)

1. Installieren Sie [AWS CLI](https://aws.amazon.com/cli/) mithilfe von `pip install awscli`
2. Konfigurieren Sie die Berechtigungsnachweise für Cloud Object Storage (IaaS) in Ihrer AWS-Berechtigungsnachweisdatei (für Linux in ~/.aws/credentials).

```
...

[ibm_s3_cos]
aws_access_key_id=<USER NAME>
aws_secret_access_key=<PASSWORD>

```
{: codeblock}

Sie können die AWS-CLI verwenden, um Buckets und Dateien aufzulisten und zu aktualisieren, indem Sie das obige Profil und den Cloud Object Storage-Endpunkt aus Ihren Berechtigungsnachweisen angeben. Wenn Sie z. B. die Buckets in der Instanz auflisten möchten, führen Sie den folgenden Befehl aus:

```
aws --endpoint-url=<URL-FOR-YOUR-IBM-OBJECT-STORE> --profile ibm_s3_cos s3 ls
```
{: codeblock}

## Zugriff auf die Object Storage OpenStack Swift for Bluemix-Instanz

Sie können die Swift-CLI oder Clientbibliotheken verwenden (z. B. die Swift-Python-Clientbibliothek für den Zugriff auf diesen Objektspeicher). Weitere Informationen finden Sie in der [Dokumentation](https://console.bluemix.net/docs/services/ObjectStorage/index.html)

## Datenformatierung

Unterschiedliche Frameworks erfordern Trainings-und Testdatensätze in unterschiedlichen Formaten. Zum Beispiel erfordert Caffe Datensätze im LevelDB- oder LMDB-Format, während Torch Datensätze in Torch-proprietärem Format benötigt. Wir gehen davon aus, dass die Daten bereits in dem für das spezifische Framework benötigten Format vorhanden sind. Details zur Vorgehensweise zum Konvertieren von Rohdaten in ein Framework-spezifisches Format liegen außerhalb des Geltungsbereichs für dieses Dokument. Weitere Informationen finden Sie in der Dokumentation zum Deep-Learning-Framework, das Sie verwenden möchten.
