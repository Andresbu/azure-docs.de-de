---
title: "Simuliertes Gerät und Azure IoT Gateway – Erste Schritte | Microsoft-Dokumentation"
description: "Enthält die ersten Schritte mit dem IoT Gateway Starter Kit und Informationen zum Erstellen des Azure IoT Hub und Herstellen der Verbindung des Gateways mit dem IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Hub, IoT Gateway, Erste Schritte mit dem Internet der Dinge, IoT Toolkit
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 916fa40d9ac857dfa72197b40c232834593d3891
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---

# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a>Erste Schritte mit dem IoT Gateway Starter Kit mit einem simulierten Gerät

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simuliertes Gerät](iot-hub-gateway-kit-c-sim-get-started.md)

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen des [IoT Gateway Starter Kit](https://aka.ms/gateway-kit) vertraut. Sie verwenden ein Intel NUC-Gerät mit Ausführung von Wind River Linux. Es wird beschrieben, wie Sie Ihre Geräte mithilfe von Azure IoT Hub nahtlos mit der Cloud verbinden.

***
**Wenn Sie noch nicht über das Kit verfügen:** Klicken Sie [hier](https://aka.ms/gateway-kit).
***

## <a name="lesson-1-configure-your-nuc"></a>Lektion 1: Konfigurieren Ihres NUC-Geräts
![Lektion 1: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

In dieser Lektion richten Sie Intel NUC (Next Unit of Computing) im Kit als Azure IoT Gateway ein, installieren das Azure IoT Edge-Paket auf dem NUC und führen eine Beispiel-App aus, um die Gatewayfunktionalität zu überprüfen.

*Geschätzter Zeitaufwand*: 15 Minuten.

[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md) (Einrichten von Intel NUC als IoT Gateway)

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen des IoT Hub
![Lektion 2: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

In dieser Lektion installieren Sie die Tools und Software auf dem Hostcomputer. Anschließend erstellen Sie ein kostenloses Azure-Konto, stellen Ihren Azure IoT Hub bereit und erstellen Ihr erstes Gerät im IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-tools"></a>Tools herunterladen
Installieren Sie die Tools und Software auf dem Hostcomputer.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Get the tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md) (Herunterladen der Tools).

### <a name="create-an-iot-hub-and-register-your-device"></a>Erstellen eines IoT Hub und Registrieren Ihres Geräts
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihren ersten Azure IoT Hub bereit, und fügen Sie dem IoT Hub mithilfe der Azure-CLI Ihr erstes Gerät hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen eines IoT Hub und Registrieren Ihres Geräts](iot-hub-gateway-kit-c-sim-lesson2-register-device.md).

## <a name="lesson-3-receive-messages-from-the-simulated-device-and-read-messages-from-your-iot-hub"></a>Lektion 3: Empfangen von Nachrichten vom simulierten Gerät und Lesen von Nachrichten vom IoT Hub
In dieser Lektion verwenden Sie Skripts zum Automatisieren der Konfiguration und Ausführung einer simulierten Geräte-App in Ihrem Gateway. Die simulierte Geräte-App generiert Temperaturbeispieldaten und sendet sie an ein IoT Hub-Modul. Das IoT Hub-Modul packt die empfangenen Daten und sendet sie über das Gatewayframework, das über Azure IoT Edge bereitgestellt wird, an Ihren IoT Hub.

![Lektion 3: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a>Konfigurieren und Ausführen eines simulierten Geräts
Bereiten Sie die Beispielcodes vor. Konfigurieren Sie anschließend die Beispielanwendung für das simulierte Gerät, und führen Sie sie aus.

*Geschätzter Zeitaufwand*: 15 Minuten.

Wechseln Sie zu [Konfigurieren und Ausführen eines simulierten Geräts](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

### <a name="read-messages-from-your-iot-hub"></a>Lesen von Nachrichten von Ihrem IoT Hub
Führen Sie Beispielcode auf Ihrem Hostcomputer aus, um die Nachrichten vom IoT Hub zu lesen.

*Geschätzter Zeitaufwand*: 15 Minuten.

Wechseln Sie zu [Lesen von Nachrichten von Ihrem IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>Lektion 4: Speichern von Nachrichten in Azure Table Storage
Erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub erhält und sie in Azure Table Storage (Tabellenspeicher) schreibt.

![Lektion 4: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).

### <a name="read-messages-persisted-in-azure-table-storage"></a>Lesen von Nachrichten in Azure Table Storage
Überwachen Sie die Nachrichten vom Gateway an die Cloud, wenn sie in den Azure-Tabellenspeicher geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Lesen von Nachrichten in Azure Table Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Problembehandlung
Falls Sie bei den Lektionen Probleme haben, helfen Ihnen die Lösungen im Artikel zur [Problembehandlung](iot-hub-gateway-kit-c-sim-troubleshooting.md) weiter.

## <a name="explore-more"></a>Mehr erfahren
Weitere Informationen finden Sie unter [Intel IoT Gateway Kit Developer Zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit).
