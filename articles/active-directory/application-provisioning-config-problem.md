---
title: "Problem beim Konfigurieren der Benutzerbereitstellung für eine Azure AD-Kataloganwendung | Microsoft-Dokumentation"
description: "Problembehandlung für häufige Probleme beim Konfigurieren der Benutzerbereitstellung für eine bereits im Azure AD-Anwendungskatalog aufgeführte Anwendung"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: e15784a662b1691774eb3d9cc8b3fbe66ac67385
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application" class="xliff"></a>

# Problem beim Konfigurieren der Benutzerbereitstellung für eine Azure AD-Kataloganwendung

Das Konfigurieren der [automatischen Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) für eine App (sofern unterstützt) erfordert, dass bestimmte Anweisungen befolgt werden, um die Anwendung auf die automatische Bereitstellung vorzubereiten. Dann können Sie das Azure-Portal dazu verwenden, um den Bereitstellungsdienst für die Synchronisierung von Benutzerkonten mit der Anwendung zu konfigurieren.

Sie sollten immer damit beginnen, das zum Einrichten der Bereitstellung für Ihre Anwendung spezifische Setuptutorial zu suchen. Befolgen Sie dann die Schritte zum Konfigurieren der App und von Azure AD, um die Verbindung für die Bereitstellung zu erstellen. Eine Liste mit App-Tutorials finden Sie unter [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<a id="how-to-see-if-provisioning-is-working" class="xliff"></a>

## Ermitteln, ob die Bereitstellung funktioniert 

Sobald der Dienst konfiguriert ist, erhalten Sie die meisten Einblicke in die Ausführung des Diensts an zwei Stellen:

-   **Überwachungsprotokolle**: Die Überwachungsprotokolle für die Bereitstellung zeichnen alle vom Bereitstellungsdienst durchgeführten Vorgänge auf, einschließlich der Abfragen von Azure AD für zugewiesene Benutzer, die sich im Bereitstellungsbereich befinden. Fragen Sie die Ziel-App hinsichtlich dem Vorkommen dieser Benutzer ab, wobei die Benutzerobjekte zwischen dem System verglichen werden. Fügen Sie dann auf Basis des Vergleichs das Benutzerkonto zum Zielsystem hinzu oder aktualisieren bzw. deaktivieren Sie es. Sie können auf die Überwachungsprotokolle für die Bereitstellung im Azure-Portal auf der Registerkarte **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt; Überwachungsprotokolle** zugreifen. Filtern Sie die Protokolle nach der Kategorie **Kontobereitstellung**, um nur die Bereitstellungsereignisse für die jeweilige App anzuzeigen.

-   **Bereitstellungsstatus**: Eine Zusammenfassung des letzten Bereitstellungsdurchlaufs für eine bestimmte App wird im Abschnitt **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt;Bereitstellung** am unteren Bildschirmrand unter den Diensteinstellungen angezeigt. In diesem Abschnitt wird zusammengefasst, wie viele Benutzer (und/oder Gruppen) aktuell zwischen den beiden Systemen synchronisiert werden und ob dabei Fehler aufgetreten sind. Die Fehlerdetails werden in den Überwachungsprotokollen aufgeführt. Beachten Sie, dass der Bereitstellungsstatus nicht ausgefüllt wird, bis eine vollständige anfängliche Synchronisierung zwischen Azure AD und der App abgeschlossen wurde.

<a id="general-problem-areas-with-provisioning-to-consider" class="xliff"></a>

## Zu berücksichtigende allgemeine Problembereiche bei der Bereitstellung

Nachfolgend finden Sie eine Liste der allgemeinen Problembereiche, für die Sie Detailinformationen anzeigen können, wenn Sie wissen, wo Sie starten müssen.

* [Bereitstellungsdienst wird anscheinend nicht gestartet](#provisioning-service-does-not-appear-to-start)
* [Konfiguration kann aufgrund nicht funktionierender Anmeldeinformationen für die App nicht gespeichert werden](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Überwachungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

<a id="provisioning-service-does-not-appear-to-start" class="xliff"></a>

## Bereitstellungsdienst wird anscheinend nicht gestartet

Wenn Sie für **Bereitstellungsstatus** im Abschnitt **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt;Bereitstellung** des Azure-Portals die Option **Ein** festlegen. Auf dieser Seite werden jedoch nach anschließendem erneuten Laden keine anderen Statusdetails angezeigt. Es ist wahrscheinlich, dass der Dienst ausgeführt wird, aber noch keine anfängliche Synchronisierung abgeschlossen hat. Überprüfen Sie die oben beschriebenen **Überwachungsprotokolle**, um zu ermitteln, welche Vorgänge der Dienst durchführt und ob Fehler aufgetreten sind.

>[!NOTE]
>Eine anfängliche Synchronisierung kann zwischen 20 Minuten und mehreren Stunden dauern. Dies hängt von der Größe des Azure AD-Verzeichnisses und der Anzahl der Benutzer im Bereitstellungsbereich ab. Nachfolgende Synchronisierungen nach der anfänglichen Synchronisierung werden schneller durchgeführt, da der Bereitstellungsdienst Wasserzeichen speichert, die den Status beider Systeme nach der Erstsynchronisierung darstellen, wodurch die Leistung nachfolgender Synchronisierungen verbessert wird.
>
>

<a id="cant-save-configuration-due-to-app-credentials-not-working" class="xliff"></a>

## Konfiguration kann aufgrund nicht funktionierender Anmeldeinformationen für die App nicht gespeichert werden

Damit die Bereitstellung funktioniert, erfordert Azure gültige Anmeldeinformationen, mit denen eine Verbindung zu einer Benutzerverwaltungs-API hergestellt werden kann, die von dieser App bereitgestellt wird. Wenn diese Anmeldeinformationen nicht funktionieren oder Sie sie nicht kennen, lesen Sie das Tutorial zum Einrichten dieser App, die zuvor beschrieben wurde.

<a id="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned" class="xliff"></a>

## Überwachungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind

Wenn ein Benutzer in den Überwachungsprotokollen als „übersprungen“ angezeigt wird, ist es sehr wichtig, die erweiterten Details in der Protokollmeldung zu lesen, um die Ursache zu ermitteln. Nachfolgend sind häufige Ursachen und Lösungen aufgeführt:

-   **Es wurde ein Bereichsfilter konfiguriert****, der einen Benutzer basierend auf einem Attributwert herausfiltert**. Weitere Informationen zu Bereichsfiltern finden Sie unter <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **Der Benutzer ist „nicht wirklich berechtigt“.** Wenn diese bestimmte Fehlermeldung angezeigt wird, liegt ein Problem mit dem Benutzerzuweisungseintrag vor, der in Azure AD gespeichert wird. Heben Sie die Zuweisung des Benutzers (oder der Gruppe) zur App auf, und weisen Sie ihn (oder sie) erneut zu, um dieses Problem zu beheben. Weitere Informationen zu Zuweisungen finden Sie unter <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Ein erforderliches Attribut fehlt oder wurde für einen Benutzer nicht ausgefüllt.** Es ist beim Einrichten der Bereitstellung wichtig zu beachten, dass die Attributzuordnungen und Workflows überprüft und konfiguriert werden, die festlegen, welche Benutzereigenschaften (oder Gruppeneigenschaften) von Azure AD zur Anwendung gelangen. Dazu zählt das Festlegen der „übereinstimmenden Eigenschaft“, mit der Benutzer/Gruppen zwischen den beiden Systemen eindeutig identifiziert und abgeglichen werden. Weitere Informationen zu diesem wichtigen Vorgang finden Sie unter <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Attributzuordnungen für Gruppen:** Die Bereitstellung von Gruppennamen und Gruppendetails zusätzlich zu den Mitgliedern, falls dies für einige Anwendungen unterstützt wird. Sie können diese Funktionen aktivieren oder deaktivieren, indem Sie die **Zuordnung** für Gruppenobjekte aktivieren oder deaktivieren, die auf der Registerkarte **Bereitstellung** angezeigt wird. Wenn die Bereitstellung von Gruppen aktiviert ist, überprüfen Sie auf jeden Fall die Attributzuordnungen, um sicherzustellen, dass ein entsprechendes Feld für die „übereinstimmende ID“ verwendet wird. Dies kann der Anzeigename oder der E-Mail-Alias sein, da die Gruppe und ihre Mitglieder nicht bereitgestellt werden, wenn die entsprechende Eigenschaft leer ist oder nicht für eine Gruppe in Azure AD gefüllt wurde.

<a id="next-steps" class="xliff"></a>

#Nächste Schritte
[Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md)

