---
title: Azure IoT Suite und Azure Active Directory | Microsoft Docs
description: Informationen, wie Azure Active Directory von Azure IoT Suite verwendet werden, um Berechtigungen zu verwalten.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 5f2615b3df14c82147ff8a2cd997458756581d01
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Berechtigungen für die Website „azureiotsuite.com“

## <a name="what-happens-when-you-sign-in"></a>Was bei der Anmeldung geschieht

Bei Ihrer ersten Anmeldung bei [azureiotsuite.com][lnk-azureiotsuite] bestimmt die Website basierend auf dem derzeit ausgewählten Azure Active Directory-Mandanten (AAD) und dem Azure-Abonnement Ihre Berechtigungsstufen.

1. Um die Liste der Mandanten aufzufüllen, die neben Ihrem Benutzernamen angezeigt werden, ermittelt die Website zunächst mithilfe von Azure, zu welchen AAD-Mandanten Sie gehören. Derzeit kann die Website nur Benutzertoken für jeweils einen Mandanten abrufen. Daher werden Sie, wenn Sie über das Dropdownfeld rechts oben den Mandanten wechseln, bei diesem Mandanten angemeldet, um die Token für diesen Mandanten zu beziehen.

2. Als Nächstes ermittelt die Website mithilfe von Azure, welche Abonnements Sie dem ausgewählten Mandanten zugeordnet haben. Die verfügbaren Abonnements werden angezeigt, wenn Sie eine neue vorkonfigurierte Lösung erstellen.

3. Abschließend ruft die Website alle Ressourcen in den Abonnements und Ressourcengruppen ab, die als vorkonfigurierte Lösungen gekennzeichnet sind, und füllt die Kacheln auf der Startseite auf.

In den beiden folgenden Abschnitten werden die Rollen beschrieben, die den Zugriff auf die vorkonfigurierten Lösungen steuern.

## <a name="aad-roles"></a>AAD-Rollen

Die AAD-Rollen steuern die Fähigkeit, vorkonfigurierte Lösungen bereitzustellen und Benutzer in einer vorkonfigurierten Lösung zu verwalten.

Weitere Informationen zu Administratorrollen in AAD finden Sie unter [Zuweisen von Administratorrollen in Azure AD][lnk-aad-admin]. Der aktuelle Artikel konzentriert sich auf die Verzeichnisrollen **Globaler Administrator** und **Benutzer**, die von den vorkonfigurierten Lösungen verwendet werden.

**Globaler Administrator:** Es können viele globale Administratoren pro AAD-Mandanten vorhanden sein. Wenn Sie einen AAD-Mandanten erstellen, sind Sie standardmäßig der globale Administrator dieses Mandanten. Der globale Administrator kann eine vorkonfigurierte Lösung bereitstellen und erhält die Rolle **Administrator** für die Anwendung innerhalb des AAD-Mandanten. Wenn jedoch ein anderer Benutzer im selben AAD-Mandanten eine Anwendung erstellt, ist **ReadOnly** die Standardrolle, die dem globalen Administrator zugewiesen wird. Globale Administratoren können Benutzer über das [Azure-Portal][lnk-portal] zu Rollen für Anwendungen zuweisen.

**Benutzer:** Pro AAD-Mandant können zahlreiche Domänenbenutzer vorhanden sein. Domänenbenutzer können eine vorkonfigurierte Lösung über die Website [azureiotsuite.com][lnk-azureiotsuite] bereitstellen. Die Standardrolle, die ihnen für die bereitgestellte Anwendung zugewiesen wird, ist **Administrator**. Sie können mithilfe des Skripts „build.cmd“ eine Anwendung im Repository [azure-iot-remote-monitoring][lnk-rm-github-repo] oder [azure-iot-predictive-maintenance][lnk-pm-github-repo] erstellen. Die Standardrolle, die ihnen erteilt wird, ist jedoch **ReadOnly**, da sie nicht berechtigt sind, Rollen zuzuweisen. Wenn ein anderer Benutzer im AAD-Mandanten eine Anwendung erstellt, wird ihnen standardmäßig die Rolle **ReadOnly** für diese Anwendung zugewiesen. Benutzer können keine Rollen für Anwendungen zuweisen und daher keine Benutzer oder Rollen für Benutzer einer Anwendung hinzufügen, auch wenn sie sie bereitgestellt haben.

**Gastbenutzer:** Pro AAD-Mandanten kann es viele Gastbenutzer geben. Gastbenutzer verfügen im AAD-Mandanten über begrenzte Rechte. Daher können Gastbenutzer keine vorkonfigurierte Lösung im AAD-Mandanten bereitstellen.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Erstellen von Benutzern in Azure AD][lnk-create-edit-users]
* [Zuweisen von Benutzern zu Apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Administratorrollen für Azure-Abonnements

Azure-Administratorrollen steuern die Fähigkeit, einem AD-Mandanten ein Azure-Abonnement zuzuordnen.

Weitere Informationen zu den Azure-Rollen „Co-Administrator“, „Dienstadministrator“ und „Kontoadministrator“ finden Sie im Artikel [Hinzufügen oder Ändern des Co-Administrators, Dienstadministrators und Kontoadministrators in Azure][lnk-admin-roles].

## <a name="application-roles"></a>Anwendungsrollen

Anwendungsrollen steuern den Zugriff auf Geräte in Ihrer vorkonfigurierten Lösung.

Es gibt zwei definierte und eine implizite Rolle in der Anwendung, die erstellt wird, wenn Sie eine vorkonfigurierte Lösung bereitstellen.

* **Administrator:** Hat Vollzugriff zum Hinzufügen, Verwalten und Entfernen von Geräten sowie zum Ändern von Einstellungen.
* **ReadOnly:** Kann Geräte, Regeln, Aktionen, Aufträge und Telemetrie anzeigen.

Sie finden die Berechtigungen, die jeder Rolle zugewiesen sind, in der Quelldatei [RolePermissions.cs][lnk-resource-cs].

### <a name="changing-application-roles-for-a-user"></a>Ändern von Anwendungsrollen eines Benutzers

Mithilfe des folgenden Verfahrens können Sie einen Benutzer in Ihrem Active Directory zum Administratoren Ihrer vorkonfigurierten Lösung ernennen.

Sie müssen globaler AAD-Administrator sein, um Rollen für einen Benutzer zu ändern:

1. Öffnen Sie das [Azure-Portal][lnk-portal].
2. Wählen Sie **Azure Active Directory**.
3. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Sie auf azureiotsuite.com ausgewählt haben, als Sie Ihre Lösung bereitgestellt haben. Wenn Sie mehrere Verzeichnisse haben, die Ihrem Abonnement zugeordnet sind, können Sie zwischen diesen wechseln, wenn Sie auf Ihren Kontonamen in der oberen rechten Ecke des Portals klicken.
4. Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.
4. Zeigen Sie **Alle Anwendungen** mit dem Status **Beliebig**an. Suchen Sie dann nach einer Anwendung mit dem Namen Ihrer vorkonfigurierten Lösung.
5. Klicken Sie auf den Namen der Anwendung, der mit dem Namen der vorkonfigurierten Lösung übereinstimmt.
6. Klicken Sie auf **Benutzer und Gruppen**.
7. Wählen Sie den Benutzer aus, dessen Rolle gewechselt werden soll.
8. Klicken Sie auf **Zuweisen**, wählen Sie die Rolle (beispielsweise **Admin**) aus, die sie dem Benutzer zuweisen möchten, und klicken Sie anschließend auf das Häkchen.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ich bin Dienstadministrator und möchte die Verzeichniszuordnung zwischen meinem Abonnement und einen bestimmten AAD-Mandanten ändern. Wie führe ich diese Aufgabe aus?

1. Wechseln Sie zum [klassischen Azure-Portal][lnk-classic-portal], und klicken Sie links in der Liste der Dienste auf **Einstellungen**.
2. Wählen Sie das Abonnement aus, dessen Verzeichniszuordnung Sie ändern möchten.
3. Klicken Sie auf **Verzeichnis bearbeiten**.
4. Wählen Sie in der Dropdownliste das **Verzeichnis** aus, das Sie verwenden möchten. Klicken Sie auf den Vorwärtspfeil.
5. Bestätigen Sie die Verzeichniszuordnung und die betroffenen Co-Administratoren. Wenn eine Verschiebung aus einem anderen Verzeichnis erfolgt, werden alle Co-Administratoren aus dem ursprünglichen Verzeichnis entfernt.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ich bin Domänenbenutzer/-mitglied des AAD-Mandanten und habe eine vorkonfigurierte Lösung erstellt. Wie wird mir eine Rolle für meine Anwendung zugewiesen?

Bitten Sie einen globalen Administrator, Sie als globalen Administrator für den AAD-Mandanten zuzuweisen, damit Sie selbst Berechtigungen zum Zuweisen von Rollen zu Benutzern erhalten. Oder bitten Sie einen globalen Administrator, Ihnen eine Rolle zuzuweisen. Wenn Sie den AAD-Mandanten ändern möchten, in dem Ihre vorkonfigurierte Lösung bereitgestellt wurde, sehen Sie sich die nächste Frage an.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Wie ändere ich den AAD-Mandanten, dem meine vorkonfigurierte Lösung und Anwendung für die Remoteüberwachung zugewiesen sind?

Sie können unter <https://github.com/Azure/azure-iot-remote-monitoring> eine Cloudbereitstellung ausführen und eine erneute Bereitstellung mit einem neu erstellten AAD-Mandanten durchführen. Da Sie standardmäßig ein globaler Administrator sind, wenn Sie einen AAD-Mandanten erstellen, haben Sie das Recht zum Hinzufügen von Benutzern und Zuweisen von Rollen zu diesen Benutzern.

1. Erstellen Sie ein AAD-Verzeichnis im [Azure-Portal][lnk-portal].
2. Navigieren Sie zu <https://github.com/Azure/azure-iot-remote-monitoring>.
3. Führen Sie `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` aus (z. B. `build.cmd cloud debug myRMSolution`).
4. Legen Sie bei Aufforderung die **tenantid** auf den neu erstellten Mandanten anstatt auf den vorherigen Mandanten fest.

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ich möchte bei Anmeldung über ein Organisationskonto einen Dienstadministrator oder Co-Administrator ändern.

Lesen Sie den Artikel [Ändern des Dienstadministrators und des Co-Administrators bei Anmeldung über ein Organisationskonto][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Warum wird dieser Fehler angezeigt? „Ihr Konto hat nicht die erforderlichen Berechtigungen zum Erstellen einer Lösung. Wenden Sie sich an den Administrator Ihres Kontos, oder versuchen Sie es mit einem anderen Konto.“

Orientieren Sie sich an folgendem Diagramm:

![][img-flowchart]

> [!NOTE]
> Wenn Sie den Fehler auch nach der Überprüfung noch sehen und ein globaler Administrator des AAD-Mandanten und ein Co-Administrator des Abonnements sind, können Sie Ihren Kontoadministrator bitten, den Benutzer zu entfernen und die erforderlichen Berechtigungen in dieser Reihenfolge neu zuzuweisen: Fügen Sie zuerst den Benutzer als globalen Administrator hinzu, und fügen Sie den Benutzer dann als Co-Administrator für das Azure-Abonnement hinzu. Verwenden Sie [Hilfe und Support][lnk-help-support], falls die Probleme weiterhin bestehen.

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Weshalb wird dieser Fehler angezeigt, wenn ich ein Azure-Abonnement habe? „Zum Erstellen vorkonfigurierter Lösungen ist ein Azure-Abonnement erforderlich. In nur wenigen Minuten können Sie ein kostenloses Testkonto erstellen.“

Wenn Sie sicher sind, dass Sie über ein Azure-Abonnement verfügen, überprüfen Sie die Mandantenzuordnung für Ihr Abonnement, und stellen Sie sicher, dass der ordnungsgemäße Mandant in der Dropdownliste ausgewählt ist. Nachdem Sie überprüft haben, ob der gewünschte Mandant ordnungsgemäß ist, überprüfen Sie anhand des obigen Diagramms die Zuordnung Ihres Abonnements und dieses AAD-Mandanten.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie mehr über IoT Suite erfahren möchten, lesen Sie, wie Sie [eine vorkonfigurierte Lösung anpassen][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md

