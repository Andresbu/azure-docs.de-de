---
title: Aktivieren der Datensammlung in Azure Security Center | Microsoft Docs
description: " Hier erfahren Sie, wie Sie die Datensammlung in Azure Security Center aktivieren. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 360073c0ed75552e62e69ce72b225ba35a2a3e09
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="enable-data-collection-in-azure-security-center"></a>Aktivieren der Datensammlung in Azure Security Center
Azure Security Center erfasst und verarbeitet Daten zu Ihren virtuellen Azure-Computern (einschließlich Konfigurationsinformationen, Metadaten und Ereignisprotokolle), um Kunden bei der Vermeidung, Erkennung und Behandlung von Bedrohungen zu unterstützen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie deaktiviert werden. (Informationen hierzu finden Sie unter [Deaktivieren der Datensammlung](#disabling-data-collection).) Wenn Sie die Datensammlung deaktivieren, empfiehlt Security Center, die Datensammlung in der Sicherheitsrichtlinie für dieses Abonnement zu aktivieren.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Sammlung von Daten für Abonnements aktivieren** aus.  Das Blatt **Turn on data collection** (Datensammlung aktivieren) wird geöffnet.
   ![Blatt „Empfehlungen“][2]
2. Wählen Sie auf dem Blatt **Turn on data collection** (Datensammlung aktivieren) Ihr Abonnement aus. Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet.
3. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** unter **Datensammlung** die Option **Ein** aus, um automatisch Protokolle zu erfassen. Durch Aktivieren der Datensammlung wird die Überwachungserweiterung auf allen aktuellen und neuen unterstützten VMs im Abonnement bereitgestellt.

   ![Blatt „Sicherheitsrichtlinie“][3]

4. Wählen Sie **Speichern**aus.
5. Wählen Sie **Ein Speicherkonto pro Region auswählen**aus. Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten dieser virtuellen Computer gespeichert werden. Wenn Sie nicht für jede Region ein Speicherkonto auswählen, wird ein Speicherkonto für Sie erstellt und in der securitydata-Ressourcengruppe platziert. In diesem Beispiel wählen wir **newstoracct**. Sie können das Speicherkonto später ändern, indem Sie zur Sicherheitsrichtlinie für Ihr Abonnement zurückkehren und ein anderes Speicherkonto auswählen.
   ![Auswählen eines Speicherkontos][4]
6. Klicken Sie auf **OK**.

> [!NOTE]
> Es wird empfohlen, dass Sie zunächst die Datensammlung aktivieren und ein Speicherkonto auf Abonnementebene auswählen. Sicherheitsrichtlinien können auf der Ebene von Azure-Abonnement und Ressourcengruppe festgelegt werden, die Konfiguration von Datensammlung und Speicherkonten erfolgt jedoch nur auf Abonnementebene.
>
>

## <a name="after-data-collection-is-enabled"></a>Nach Aktivierung der Datensammlung
Datensammlung wird über den Azure-Überwachungs-Agent und die Azure-Erweiterung für Sicherheitsüberwachung aktiviert. Die Azure-Erweiterung für die Sicherheitsüberwachung sucht nach verschiedenen sicherheitsrelevanten Konfigurationen und sendet diese in [Ereignisablaufverfolgungen für Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW, Event Tracing for Windows). Außerdem erstellt das Betriebssystem Einträge im Ereignisprotokoll. Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto. Darüber hinaus kopiert der Überwachungs-Agent Absturzabbilddateien in Ihr Speicherkonto. Dies ist das Speicherkonto, das Sie in der Sicherheitsrichtlinie konfiguriert haben.

## <a name="disabling-data-collection"></a>Deaktivieren der Datensammlung
Die Datensammlung kann jederzeit deaktiviert werden. Dadurch werden automatisch sämtliche Überwachungs-Agents entfernt, die zuvor von Security Center installiert wurden. Sie müssen ein Abonnement auswählen, um die Datensammlung zu deaktivieren.

> [!NOTE]
> Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden. Zum Deaktivieren der Datensammlung muss allerdings ein Abonnement ausgewählt werden.
>
>

1. Kehren Sie zum Blatt **Security Center** zurück, und wählen Sie die Kachel **Richtlinie** aus. Das Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** wird geöffnet.
   ![Auswählen der Kachel „Richtlinie“][5]
2. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** das Abonnement aus, für das Sie die Datensammlung deaktivieren möchten.
   ![Auswählen eines Abonnements zum Deaktivieren der Datensammlung][6]
3. Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet.  Wählen Sie unter „Datensammlung“ die Option **Aus** aus.
4. Wählen Sie im oberen Menüband die Option **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Datensammlung aktivieren“ implementieren. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) : Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md)Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md)Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png

