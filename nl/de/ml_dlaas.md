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

# Einführung

<!-- ![deep learning process flow](images/ml_dlaas_api_calls.png) -->

Als Data-Scientist müssen Sie Hunderte von Modellen trainieren, um die richtige Kombination von Daten und Hyperparametern zu ermitteln, die die Leistung Ihrer neuronalen Netze optimieren. Sie möchten mehr experimentieren ... und schneller. Sie möchten tiefere Netzwerke trainieren und breitere Hyperparameter-Bereiche erkunden. {{site.data.keyword.pm_full}} beschleunigt diesen Experimentzyklus, indem der Prozess vereinfacht wird, sodass Modelle parallel in einem elastischen GPU-Berechnungscluster trainiert werden.
{: shortdesc}

Hier erfahren Sie mehr über den Einstieg:
1. [Einrichten Ihrer Umgebung für {{site.data.keyword.pm_full}}](ml_getting_access.html)
2. [Installieren der WML-Befehlszeilenschnittstelle (CLI)](ml_dlaas_environment.html)
3. Erfahren, wie man einen eigenen Trainingslauf konfiguriert
4. Hochladen von Trainingsdaten in die Cloud
5. Starten des Trainings
6. Überwachen und evaluieren

## Konfigurieren jedes Trainingslaufs

{{site.data.keyword.pm_full}} ermöglicht Ihnen die schnelle Durchführung von Deep-Learning-Experimenten durch die Übergabe von 10 bis 100 Trainingsläufen, die in die Warteschlange für das Training eingereiht werden können. Ein Trainingslauf besteht aus den folgenden Teilen: 

* Ihr Modell für ein neuronales Netz, das im [unterstützten Deep-Learning-Framework](ml_dlaas_supported_framework.html) definiert ist 
* Die Konfiguration für die Vorgehensweise zum Ausführen des Trainings; dies umfasst auch die Anzahl der GPUs und die Position des [Objektspeichers, der Ihre Datensätze enthält](ml_dlaas_object_store.html)

<p align="center"><img src="images/experiment_to_training_runs_text.svg" alt="Relation von Experimenten zu Trainingsläufen"></p>

[Es werden Beispiel-Trainingsläufe bereitgestellt](ml_dlaas_working_with_sample_models.html), die Daten enthalten, die sich auf einem von IBM bereitgestellten Objektspeicher befinden. Lesen Sie diese Beispiele, um zu verstehen, wie Arbeits-Manifest-yml-Dateien konfiguriert sind. Anschließend rufen Sie diese Seite wieder auf, um zu [erfahren, wie Sie Ihre eigenen Trainingsläufe definieren](ml_dlaas_working_with_new_models.html).  

## Hochladen von Trainingsdaten in die Cloud

Bevor Sie mit dem Training Ihrer neuronalen Netze beginnen können, müssen Sie zunächst Ihre Daten in die IBM Cloud verschieben. Dazu [laden Sie Ihre Trainingsdaten in eine Objektspeicher-Serviceinstanz hoch](ml_dlaas_object_store.html). Wenn Sie das Training abgeschlossen haben, wird die Ausgabe Ihrer Trainingsläufe in Ihren Objektspeicher geschrieben, so dass Sie Dateien auf Ihren Desktop ziehen können.

## Starten des Trainings

Nachdem Sie Ihre Trainingsdefinitionen erstellt haben, verwenden Sie die [CLI (Befehlszeilenschnittstelle)](ml_dlaas_environment.html), um Ihre Jobs an {{site.data.keyword.pm_full}} zu übergeben. {{site.data.keyword.pm_full}} packt jeden Ihrer Trainingsläufe und ordnet ihn einem Kubernetes-Container mit den angeforderten Ressourcen und Deep-Learnin-Framework zu. Trainingsläufe werden parallel ausgeführt, abhängig von den GPU-Ressourcen, die Ihrer Kontoebene zur Verfügung stehen. Bei einem freien Konto sind Sie auf 1 GPU eingeschränkt, sodass alle weiteren Läufe in eine Warteschlange gestellt werden.

<p align="center"><img src="images/ml_dlaas_markitecture.svg" alt="Prozessablauf für Deep Learning"></p>

Wie im vorhergehenden Diagramm angegeben, sind 4 Trainingsläufe zu 4 Containern zugeordnet. In jedem dieser Container befindet sich das vom Trainingslauf benötigte Deep-Learning-Framework und jeder Container hat Zugriff auf eine einzelne K40-GPU (in dieser Instanz). Alle Ressourcen werden elastisch zugeordnet, so dass Sie nur ab dem Zeitpunkt, zu dem Ihrem Trainingslauf ein GPU zugeordnet wird, belastet werden, bis das Training abgeschlossen ist und die Ausgabedaten an Ihre Object Storage-Instanz übertragen wurden.

## Nächste Schritte

Verwenden Sie diese [Beispiel-Trainingsläufe](ml_dlaas_working_with_sample_models.html) oder erstellen Sie Ihre eigenen [neuen Trainingsläufe](ml_dlaas_working_with_new_models.html).
