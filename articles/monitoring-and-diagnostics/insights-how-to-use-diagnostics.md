---
title: "Aktivieren von Überwachung und Diagnose in Microsoft Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie die Diagnose für Ihre Ressourcen in Azure einrichten."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: af1947a9-c211-4aa1-8924-880a86240be4
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: b82bb1ab419831e803689edb2a2a7fe256dde5a2
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="enable-monitoring-and-diagnostics"></a>Aktivieren von Überwachung und Diagnose
Im [Azure-Portal](https://portal.azure.com)können Sie eine umfangreiche, regelmäßig durchgeführte Erfassung von Überwachungs- und Diagnosedaten konfigurieren. Es ist auch möglich, die Diagnose mit [REST-API](https://msdn.microsoft.com/library/azure/dn931932.aspx) oder [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) programmgesteuert zu konfigurieren.

Diagnose-, Überwachungs- und Metrikdaten werden in Azure in einem Speicherkonto Ihrer Wahl gespeichert. Auf diese Weise können Sie zum Lesen der Daten ein beliebiges Tool einsetzen – von einem Speicher-Explorer über Power BI bis hin zu einem Drittanbietertool.

## <a name="when-you-create-a-resource"></a>Beim Erstellen einer Ressource
Die meisten Dienste ermöglichen es Ihnen, die Diagnose bei der ersten Erstellung im [Azure-Portal](https://portal.azure.com)zu aktivieren.

1. Navigieren Sie zu **Neu** , und wählen Sie die gewünschte Ressource.
2. Wählen Sie **Optionale Konfiguration**.
    ![Blatt „Diagnose“](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Wählen Sie **Diagnose** aus, und klicken Sie auf **Ein**. Sie müssen das Speicherkonto auswählen, in dem die Diagnosedaten gespeichert werden sollen. Ihnen werden normale Datenraten für Speicherung und Transaktionen in Rechnung gestellt, wenn Sie Diagnoseinformationen an ein Speicherkonto senden.
4. Klicken Sie auf **OK** , und erstellen Sie die Ressource.

## <a name="change-settings-for-an-existing-resource"></a>Ändern der Einstellungen für eine vorhandene Ressource
Wenn Sie bereits eine Ressource erstellt haben und die Diagnoseeinstellungen für diese Ressource ändern möchten (um beispielsweise den Umfang der Datensammlung zu ändern), können Sie diese Änderung direkt im Azure-Portal durchführen.

1. Wechseln Sie zur Ressource, und klicken Sie auf **Einstellungen** .
2. Wählen Sie **Diagnose**.
3. Auf dem Blatt **Diagnose** werden alle Diagnose- und Überwachungsdatensammlungen angezeigt, die für diese Ressource konfiguriert werden können. Für einige Ressourcen können Sie auch eine Richtlinie zur **Aufbewahrung** der Daten festlegen, um eine Bereinigung Ihres Speicherkontos durchzuführen.
    ![Speicherdiagnose](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. Nachdem Sie die gewünschten Einstellungen ausgewählt haben, klicken Sie auf **Speichern** . Es kann nach dem ersten Aktivieren etwas Zeit in Anspruch nehmen, bis Überwachungsdaten angezeigt werden.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorien von Datensammlungen für virtuelle Computer
Für virtuelle Computer werden alle Metriken und Protokolle in Intervallen von 1 Minute aufgezeichnet, sodass Sie stets über aktuelle Daten zu Ihrem Computer verfügen.

* **Basismetriken:** Metriken zur Integrität des virtuellen Computers, z.B. für Prozessor und Arbeitsspeicher
* **Netzwerk- und Webmetriken** : Metriken zu Ihren Netzwerkverbindungen und Webdiensten.
* **.NET-Metriken** : Metriken zu den .NET- und ASP.NET-Anwendungen, die auf Ihrem virtuellen Computer ausgeführt werden.
* **SQL-Metriken** : Wenn Sie Microsoft SQL Service ausführen, werden die zugehörigen Leistungsmetriken angezeigt.
* **Anwendungsprotokolle zu Windows-Ereignissen** : Windows-Ereignisse, die an den Anwendungskanal gesendet werden.
* **Systemprotokolle zu Windows-Ereignissen** : Windows-Ereignisse, die an den Systemkanal gesendet werden. Dazu gehören auch alle Ereignisse von [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **Sicherheitsprotokolle zu Windows-Ereignissen** : Windows-Ereignisse, die an den Sicherheitskanal gesendet werden.
* **Infrastrukturprotokolle der Diagnose** : Protokolle zur Sammlungsinfrastruktur für die Diagnose.
* **IIS-Protokolle** : Protokolle zu Ihrem IIS-Server.

Beachten Sie, dass derzeit bestimmte Linux-Distributionen nicht unterstützt werden, und der Gast-Agent muss auf dem virtuellen Computer installiert sein.

## <a name="next-steps"></a>Nächste Schritte
* [Empfangen von Warnbenachrichtigungen](insights-receive-alert-notifications.md) , wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten.
* [Überwachen von Dienstmetriken](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Automatisches Skalieren der Instanzenzahl](insights-how-to-scale.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Überwachen der Anwendungsleistung](../application-insights/app-insights-azure-web-apps.md) , um präzise Informationen zur Leistung Ihres Codes in der Cloud zu ermitteln.
* [Anzeigen von Ereignissen und Überwachungsprotokollen](insights-debugging-with-events.md), um sich über sämtliche Aktivitäten Ihres Diensts zu informieren.
* [Nachverfolgen des Dienststatus](insights-service-health.md) , um den Zeitpunkt von Leistungsabfällen oder Dienstunterbrechungen zu ermitteln, die bei Azure aufgetreten sind.


