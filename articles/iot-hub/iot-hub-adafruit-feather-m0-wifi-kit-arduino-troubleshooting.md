---
title: "Verbinden von Arduino (C) mit Azure IoT – Problembehandlung | Microsoft-Dokumentation"
description: "Seite „Problembehandlung“ zur Arduino-Benutzeroberfläche für den Adafruit Feather M0 WiFi"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Problembehandlung für Arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 448dc0370014ad295ed820d796f7af2eb5fe698e
ms.lasthandoff: 01/24/2017


---
# <a name="troubleshooting"></a>Problembehandlung
## <a name="hardware-issues"></a>Hardwareprobleme
Informationen zum Beheben von gängigen Problemen auf dem Arduino-Board Adafruit Feather M0 WiFi finden Sie auf der [offiziellen Problembehandlungsseite](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).

## <a name="nodejs-package-issues"></a>Probleme mit Node.js-Paketen
### <a name="no-response-during-gulp-tasks"></a>Keine Antwort während Gulp-Tasks
Wenn Sie während der Ausführung von Gulp-Tasks auf Probleme stoßen, können Sie die Option `--verbose` für das Debuggen hinzufügen. Versuchen Sie, laufende Gulp-Tasks mit `Ctrl + C` zu beenden, und führen Sie dann den folgenden Befehl in Ihrem Konsolenfenster aus, um Debugmeldungen anzuzeigen. In der Konsolenausgabe werden unter Umständen detaillierte Fehlermeldungen angezeigt.

```bash
gulp --verbose
```

Sie können auch `--listen` für den offenen seriellen Anschluss hinzufügen, um Geräteprotokollinformationen auszugeben.

```bash
gulp --listen
``` 

### <a name="npm-issues"></a>NPM-Probleme
Führen Sie den folgenden Befehl aus, um das NPM-Paket zu aktualisieren:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, schreiben Sie am Ende dieses Artikels einen Kommentar, oder erstellen Sie in unserem [Beispielrepository][sample-repository] ein GitHub-Problem.

## <a name="azure-cli-issues"></a>Probleme mit der Azure-Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle (Azure CLI) ist ein Vorschaubuild. Sie können im [Installationshandbuch für Vorabversionen](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) nach Lösungen suchen. Aktualisieren Sie die Azure-Befehlszeilenschnittstelle auf die neueste Version, falls die Befehle nicht wie erwartet funktionieren.

Wenn mit dem Tool Fehler auftreten, legen Sie einen [Eintrag](https://github.com/Azure/azure-cli/issues) im Bereich **Probleme** des GitHub Repositorys an.

Für Unterstützung bei der Behebung von gängigen Problemen, lesen Sie [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Wenn in einem Fehler angezeigt wird, dass keine Version gefunden werden konnte, die die Anforderung erfüllt, führen Sie den folgenden Befehl aus, um pip auf die neueste Version zu aktualisieren.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Probleme bei der Installation von Python
### <a name="legacy-installation-issues-macos"></a>Probleme bei der Vorgänger-Installation (MacOS)
Bei der Installation von **pip** tritt ein Berechtigungsfehler auf, wenn frühere Pakete vorhanden sind, die mit **su**-Berechtigungen installiert wurden. Diese Situation tritt auf, wenn eine frühere Installation von Python mithilfe von brew (MacOS) nicht vollständig deinstalliert wurde. Einige **pip**-Pakete aus einer früheren Installation wurden durch root erstellt, was den Berechtigungsfehler verursacht. Die Lösung ist, die Pakete zu entfernen, die durch root erstellt wurden. Führen Sie die folgenden Schritte aus, um dies abzuschließen:

1. Wechseln Sie zu „/usr/local/lib/python2.7/site-packages“.
2. Listen Sie die Pakete auf, die durch root erstellt wurden: `ls -l | grep root`
3. Deinstallieren Sie die Pakete aus Schritt 2: `sudo rm -rf {package name}`
4. Installieren Sie Python neu.

## <a name="azure-iot-hub-issues"></a>Probleme mit Azure IoT Hub
Wenn Sie Azure IoT Hub erfolgreich per `azure-cli` bereitgestellt haben und ein Tool benötigen, um die Geräte zu verwalten, die eine Verbindung mit IoT Hub herstellen, können Sie die unten angegebenen Tools ausprobieren:

### <a name="device-explorer"></a>Geräte-Explorer
[Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) wird auf Ihrem lokalen Windows-Computer ausgeführt und stellt eine Verbindung mit IoT-Hub in Azure her. Er kommuniziert mit den folgenden [IoT Hub-Endpunkten](iot-hub-devguide.md):

* *Geräteidentitätsverwaltung* zum Bereitstellen und Verwalten von Geräten, die bei IoT Hub registriert sind.
* *Empfangen von Gerät-zu-Cloud*, damit Sie von Ihrem Gerät an IoT Hub gesendete Nachrichten überwachen können.
* *Senden von Cloud-zu-Gerät* ermöglicht es Ihnen, von Ihren Geräten Nachrichten an IoT Hub zu senden.

Konfigurieren Sie `IoT hub connection string` innerhalb dieses Tools, um all seine Eigenschaften zu verwenden.

### <a name="iot-hub-explorer"></a>ioT Hub-Explorer
[iothub-explorer](https://github.com/Azure/iothub-explorer) ist ein Beispiel für CLI-Tools für mehrere Plattformen, um Geräteclients zu verwalten. Mit dem Tool können Sie Geräte im Identitätsregister verwalten, Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Befehle senden.


Um die aktuellste Version (Vorabversion) des iothub-explorer-Tools zu installieren, führen Sie den folgenden Befehl in Ihrer Befehlszeilenumgebung aus:

```bash
npm install -g iothub-explorer@latest
```

Sie können den folgenden Befehl verwenden, um weitere Hilfe zu allen iothub-explorer-Befehlen und ihren Parametern zu erhalten:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure-Portal
Sie können eine vollständige CLI-Oberfläche nutzen, um alle Azure-Ressourcen zu erstellen und zu verwalten. Sie können auch das [Azure-Portal](../azure-portal-overview.md) nutzen, um das Bereitstellen, Verwalten, und Debuggen Ihrer Azure-Ressourcen zu unterstützen.

## <a name="azure-storage-issues"></a>Probleme mit Azure Storage
[Microsoft Azure-Speicher-Explorer (Vorschau)](http://storageexplorer.com) ist eine eigenständige App von Microsoft, mit der Sie unter Windows, MacOS und Linux mit Azure Storage-Daten arbeiten können. Mit diesem Tool können Sie eine Verbindung mit Ihrer Tabelle herstellen und die Daten darin anzeigen. Mithilfe dieses Tools können Sie Probleme mit Azure Storage beheben.

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md
