---
title: "Containerlösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Die Containerlösung in Log Analytics unterstützt Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem Ort."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: banders
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 05dfdc3491e6c7f838f5e7e2c16951bc1328e32b
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="containers-preview-solution-log-analytics"></a>Log Analytics-Containerlösung (Vorschau)
In diesem Artikel wird das Einrichten und Verwenden der Containerlösung in Log Analytics beschrieben, die Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem zentralen Ort unterstützt. Docker ist ein Softwarevirtualisierungssystem zum Erstellen von Containern, durch die das Bereitstellen von Software in einer IT-Infrastruktur automatisiert werden kann.

Mit der Lösung können Sie erkennen, welche Container auf Ihren Containerhosts ausgeführt und welche Images in den Containern ausgeführt werden. Sie können ausführliche Überwachungsinformationen anzeigen, die auch die mit Containern verwendeten Befehle enthalten. Außerdem können Sie die Probleme mit Containern beheben, indem Sie zentralisierte Protokolle anzeigen und durchsuchen, ohne eine Remoteanzeige der Docker- und Windows-Hosts zu benötigen. Sie können Container suchen, die Störungen verursachen und übermäßig viele Ressourcen auf einem Host verbrauchen. Darüber hinaus können Sie an einem zentralen Ort Informationen zur Leistung und Auslastung von CPU, Arbeitsspeicher, Speicher und Netzwerk zu Containern anzeigen. Auf Windows-Computern können Sie die Protokolle von Windows Server-, Hyper-V- und Docker-Containern zentralisieren und vergleichen.

Das folgende Diagramm zeigt die Beziehungen zwischen verschiedenen Containerhosts und Agents mit OMS.

![Containerdiagramm](./media/log-analytics-containers/containers-diagram.png)

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Containerlösung hinzu.

Es gibt ein paar Methoden für das Installieren und Verwenden von Docker mit OMS:

* Auf unterstützten Linux-Betriebssystemen installieren Sie zunächst Docker und führen es aus. Anschließend installieren und konfigurieren Sie den OMS-Agent für Linux.
* Unter CoreOS kann der OMS-Agent für Linux nicht ausgeführt werden. Stattdessen führen Sie eine Containerversion des OMS-Agents für Linux aus.
* Installieren Sie unter Windows Server 2016 und Windows 10 das Docker-Modul und den Client, und stellen Sie dann eine Verbindung mit einem Agent her, um Informationen zu sammeln und sie an Log Analytics zu senden.


Sie können die unterstützten Versionen von Docker und Linux-Betriebssystemen für Ihren Containerhost auf [GitHub](https://github.com/Microsoft/OMS-docker) überprüfen.

- Wenn Sie über einen Kubernetes-Cluster verfügen, der den Azure Container Service nutzt, finden Sie unter [Überwachen eines Azure Container Service-Clusters mit Microsoft Operations Management Suite (OMS)](../container-service/container-service-kubernetes-oms.md) weitere Informationen.
- Wenn Sie über einen DC/OS-Cluster in Azure Container Service verfügen, finden Sie unter [Überwachen eines Azure Container Service-DC/OS-Clusters mit Operations Management Suite](../container-service/container-service-monitoring-oms.md) weitere Informationen.
- Wenn Sie Container mit Service Fabric verwenden, finden Sie weitere Informationen unter [Übersicht über Azure Service Fabric ](../service-fabric/service-fabric-overview.md).
- Weitere Informationen zum Installieren und Konfigurieren von Docker-Modulen auf Windows-Computern finden Sie im Artikel [Docker-Modul unter Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).

> [!IMPORTANT]
> Docker muss ausgeführt werden, **bevor** Sie den [OMS-Agent für Linux](log-analytics-linux-agents.md) auf Ihren Containerhosts installieren. Wenn Sie vor der Installation von Docker bereits den Agent installiert haben, müssen Sie den OMS-Agent für Linux erneut installieren. Weitere Informationen zu Docker finden Sie auf der [Docker-Website](https://www.docker.com).
>
>

Sie müssen die folgenden Einstellungen auf den Containerhosts konfigurieren, bevor Sie Container überwachen können.

## <a name="configure-settings-for-a-linux-container-host"></a>Konfigurieren von Einstellungen für einen Linux-Containerhost

Unterstützte Linux-Versionen:

- Docker 1.11 bis 1.13
- Docker CE und EE v17.03


Die folgenden x64-Linux-Distributionen werden als Containerhosts unterstützt:

- Ubuntu 14.04 LTS, 16.04 LTS
- CoreOS (Stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2, 7.3
- SLES 12
- RHEL 7.2, 7.3


Verwenden Sie nach dem Installieren von Docker die folgenden Einstellungen für den Containerhost, um den Agent für die Verwendung mit Docker zu konfigurieren. Sie benötigen Ihre [OMS-Arbeitsbereichs-ID und den Schlüssel](log-analytics-linux-agents.md).


### <a name="for-all-linux-container-hosts-except-coreos"></a>Für alle Linux-Containerhosts mit Ausnahme von CoreOS

- Führen Sie die Anweisungen unter [Steps to install the OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) (Schritte zum Installieren des OMS-Agents für Linux).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Für alle Linux-Containerhosts inklusive CoreOS

Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an.

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a>Wechseln von der Verwendung eines installierten Linux-Agents zu einem in einem Container enthaltenen
Wenn Sie zuvor den direkt installierten Agent verwendet haben und stattdessen einen Agent verwenden möchten, der in einem Container ausgeführt wird, müssen Sie zunächst OMSAgent entfernen. Lesen Sie unter [Schritte zum Installieren des OMS-Agents für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) nach.

## <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional oder Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Unter Windows unterstützte Docker-Versionen

- Docker 1.12 – 1.13
- Docker 17.03.0 [stabil]

### <a name="preparation-before-installing-agents"></a>Vorbereitung vor der Installation von Agents

Vor der Installation von Agents auf Windows-Computern müssen Sie den Docker-Dienst konfigurieren. Die Konfiguration ermöglicht dem Windows-Agent oder der Log Analytics-Erweiterung für virtuelle Computer, den Docker-TCP-Socket zu verwenden, sodass die Agents remote auf den Docker-Daemon zugreifen und Daten für die Überwachung erfassen können.

#### <a name="to-start-docker-and-verify-its-configuration"></a>So starten Sie Docker und prüfen seine Konfiguration

Zum Einrichten einer TCP-Named Pipe für Windows Server müssen mehrere Schritte durchgeführt werden:

1. Aktivieren Sie in Windows PowerShell TCP Pipe und Named Pipe.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Konfigurieren Sie Docker mit der Konfigurationsdatei für die TCP Pipe und Named Pipe. Die Konfigurationsdatei befindet sich unter C:\ProgramData\docker\config\daemon.json.

    In der Datei „daemon.json“ benötigen Sie Folgendes:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Weitere Informationen zur Docker-Daemon-Konfiguration mit Windows-Containern finden Sie unter [Docker-Modul unter Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Installieren des Windows-Agents

Um die Überwachung von Windows- und Hyper-V-Containern zu aktivieren, installieren Sie Agents auf Windows-Computern, die Containerhosts sind. Informationen zu Windows-Computern in Ihrer lokalen Umgebung finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md). Verbinden Sie virtuelle Computer in Azure unter Verwendung der [VM-Erweiterung](log-analytics-azure-vm-extension.md) mit Log Analytics.

Sie können Windows-Container überwachen, die unter Service Fabric ausgeführt werden. Allerdings werden derzeit nur [unter Azure ausgeführte virtuelle Computer](log-analytics-azure-vm-extension.md) und [Windows-Computer, die in Ihrer lokalen Umgebung ausgeführt werden](log-analytics-windows-agents.md), für Service Fabric unterstützt.

So überprüfen Sie, ob die Container-Lösung ordnungsgemäß festgelegt ist:

- Überprüfen Sie, ob das Management Pack ordnungsgemäß heruntergeladen wurde, suchen Sie nach *ContainerManagement.xxx*.
    - Die Dateien sollten sich im Verzeichnis „C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs“ befinden.
- Überprüfen Sie in **Systemsteuerung** > **System und Sicherheit**, ob die OMS-Arbeitsbereichs-ID richtig ist.
    - Öffnen Sie **Microsoft Monitoring Agent**, und stellen Sie sicher, dass die Arbeitsbereichsinformationen richtig sind.


## <a name="containers-data-collection-details"></a>Details zur Datensammlung in Containern
Die Containerlösung sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern mit Agents, die Sie aktivieren.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Container erfasst werden.

| Plattform | [OMS Agent für Linux](log-analytics-linux-agents.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

| Plattform | [Windows-Agent](log-analytics-windows-agents.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

| Plattform | [Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

Die folgende Tabelle enthält Beispiele für von der Containerlösung gesammelte Datentypen, in Protokollsuchen verwendete Datentypen sowie für Ergebnisse.

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Type=Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `Type=ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | `Type=ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | `Type=ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | `Type=ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Überwachen von Containern
Nachdem Sie die Lösung im OMS-Portal aktiviert haben, werden auf der Kachel **Container** zusammenfassende Informationen zu den Containerhosts und den auf den Hosts ausgeführten Containern angezeigt.

![Kachel „Container“](./media/log-analytics-containers/containers-title.png)

Die Kachel zeigt eine Übersicht über die Anzahl der Container in der Umgebung an. Außerdem erfahren Sie, ob Container fehlerhaft sind, ausgeführt werden oder beendet wurden.

### <a name="using-the-containers-dashboard"></a>Verwenden des Containerdashboards
Klicken Sie auf die Kachel **Container**. Dort sind die Ansichten wie folgt angeordnet:

* Containerereignisse
* Fehler
* Containerstatus
* Containerimageinhalt
* Leistung von CPU und Arbeitsspeicher

Jeder Bereich im Dashboard ist eine visuelle Darstellung einer Suche, die über die gesammelten Daten ausgeführt wird.

![Containerdashboard](./media/log-analytics-containers/containers-dash01.png)

![Containerdashboard](./media/log-analytics-containers/containers-dash02.png)

Klicken Sie auf dem Blatt **Container Status** (Containerstatus) auf den oberen Bereich, wie unten dargestellt.

![Containerstatus](./media/log-analytics-containers/containers-status.png)

Die Protokollsuche wird geöffnet. Darin werden Informationen zu den Hosts und den Containern, die darauf ausgeführt werden, angezeigt.

![Protokollsuche für Container](./media/log-analytics-containers/containers-log-search.png)

Hier können Sie die Suchabfrage bearbeiten, um die für Sie interessanten spezifischen Informationen zu suchen. Weitere Informationen zur Verwendung der Protokollsuche finden Sie unter [Protokollsuchvorgänge in Log Analytics](log-analytics-log-searches.md).

Sie können z.B. die Suchabfrage so ändern, dass alle beendeten Container anstelle der ausgeführten Container angezeigt werden. Dazu ändern Sie **Running** in der Abfrage in **Stopped**.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Beheben von Problemen durch das Suchen fehlerhafter Container
OMS kennzeichnet einen Container als **Failed**, wenn dieser mit einem anderen Exitcode als null beendet wurde. Auf dem Blatt **Container mit Fehlern** wird eine Übersicht über die Fehler und Ausfälle in der Umgebung angezeigt.

### <a name="to-find-failed-containers"></a>So suchen Sie nach Containern mit Fehlern
1. Klicken Sie auf das Blatt **Container Events** (Containerereignisse).  
   ![Containerereignisse](./media/log-analytics-containers/containers-events.png)
2. Die Protokollsuche wird geöffnet. Darin wird der Status von Containern wie folgt angezeigt.  
   ![Containerzustand](./media/log-analytics-containers/containers-container-state.png)
3. Klicken Sie anschließend auf den Wert für Container mit Fehlern, um zusätzliche Informationen wie Imagegröße und Anzahl der beendeten und fehlerhaften Images anzuzeigen. Erweitern Sie **Mehr anzeigen**, um die Image-ID anzuzeigen.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-state-failed.png)
4. Suchen Sie als Nächstes den Container, in dem dieses Image ausgeführt wird. Geben Sie Folgendes in die Suchabfrage ein.
   `Type=ContainerInventory <ImageID>` Dadurch werden die Protokolle angezeigt. Sie können scrollen, um fehlerhafte Container zu finden.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Suchprotokolle für Containerdaten
Wenn Sie Probleme mit einen bestimmten Fehler beheben möchten, kann es hilfreich sein, herauszufinden, wo dieser in Ihrer Umgebung auftritt. Mit den folgenden Protokolltypen können Sie Abfragen erstellen, die die gewünschten Informationen zurückgeben.

* **ContainerInventory:** Verwenden Sie diesen Typ, wenn Sie Informationen zum Containerspeicherort und -namen sowie zu den darin ausgeführten Images benötigen.
* **ContainerImageInventory:** Verwenden Sie diesen Typ, wenn Sie die Informationen organisiert nach Image suchen und Imageinformationen wie Image-IDs oder -Größen anzeigen möchten.
* **ContainerLog:** Verwenden Sie diesen Typ, wenn Sie bestimmte Informationen und Einträge im Fehlerprotokoll suchen möchten.
* **ContainerServiceLog:** Verwenden Sie diesen Typ, wenn Sie Audit-Trail-Informationen zum Docker-Daemon suchen, z.B. zu den Befehlen „start“, „stop“, „delete“ oder „pull“.

### <a name="to-search-logs-for-container-data"></a>So durchsuchen Sie Protokolle nach Containerdaten
* Wählen Sie ein Image aus, bei dem kürzlich ein Fehler aufgetreten ist, und suchen Sie die zugehörigen Fehlerprotokolle. Suchen Sie zunächst mit **ContainerInventory** nach dem Namen eines Containers, in dem das Image ausgeführt wird. Suchen Sie beispielsweise nach `Type=ContainerInventory ubuntu Failed`.  
    ![Durchsuchen von Ubuntu-Containern](./media/log-analytics-containers/search-ubuntu.png)

  Notieren Sie sich den Namen des Containers neben **Namen**, und suchen Sie nach den zugehörigen Protokollen. In diesem Beispiel lautet er `Type=ContainerLog adoring_meitner`.

**Anzeigen von Leistungsinformationen**

Wenn Sie mit dem Erstellen von Abfragen beginnen, ist es möglicherweise hilfreich, zunächst zu erfahren, was alles möglich ist. Wenn Sie beispielsweise alle Leistungsdaten anzeigen möchten, können Sie die folgende allgemeine Abfrage eingeben.

```
Type=Perf
```

![Containerleistung](./media/log-analytics-containers/containers-perf01.png)

Sie können dies auch grafisch darstellen, indem Sie in den Ergebnissen auf das Wort **Metriken** klicken.

![Containerleistung](./media/log-analytics-containers/containers-perf02.png)

Sie können die angezeigten Leistungsdaten auf einen bestimmten Container einschränken, indem Sie den Namen rechts von Ihrer Abfrage eingeben.

```
Type=Perf <containerName>
```

Damit zeigen Sie die Liste der Leistungsmetriken an, die für einen einzelnen Container gesammelt wurden.

![Containerleistung](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Erstellung von Abfragen ausgehend von einem oder zwei Beispielen zu beginnen und diese dann an die eigene Umgebung anzupassen. Als Ausgangspunkt können Sie auf dem Blatt **Relevante Abfragen** experimentieren, um komplexere Abfragen zu erstellen.

![Containerabfragen](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Speichern von Abfragen für die Protokollsuche
Das Speichern von Abfragen ist ein Standardfeature in Log Analytics. Durch die Speicherung können Sie schnell auf die Abfragen zurückgreifen, die Sie als besonders hilfreich empfinden.

Nachdem Sie eine Abfrage erstellt haben, die für Sie nützlich ist, speichern Sie sie, indem Sie oben auf der Seite „Protokollsuche“ auf **Favoriten** klicken. Sie können später auf der Seite **Mein Dashboard** auf einfache Weise darauf zugreifen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [Protokollsuchen](log-analytics-log-searches.md) für die Anzeige ausführlicher Containerdatensätze.

