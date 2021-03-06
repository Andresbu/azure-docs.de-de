---
title: 'Schnellstartanleitung: Manuelle Installation von SAP HANA (Einzelinstanz) auf Azure-VMs | Microsoft-Dokumentation'
description: "Eine Schnellstartanleitung für die manuelle Installation von SAP HANA (Einzelinstanz) auf Azure-VMs"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: e95d3a57594d55b7d7176d873cde8abfa6e12ecd
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="quick-start-guide-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Schnellstartanleitung: Manuelle Installation von SAP HANA (Einzelinstanz) auf Azure-VMs
## <a name="introduction"></a>Einführung
Diese Schnellstartanleitung dient Ihnen als Hilfe beim Einrichten eines SAP HANA-Einzelinstanzprototyps oder Demosystems unter Azure Virtual Machines (VMs), wenn Sie SAP NetWeaver 7.5 und SAP HANA 1.0 SP12 manuell installieren. Diese Anleitung soll nicht als Ersatz für die SAP-Dokumentation dienen, sondern legt den Schwerpunkt mehr auf die Bereitstellung unter Azure.

>[!Note]
>Beachten Sie, dass sich diese Anleitung auf Bereitstellungen von SAP HANA auf Azure-VMs bezieht. Alle Aspekte der Bereitstellung von SAP HANA für große HANA-Instanzen sind in einer separaten Dokumentation enthalten, die Sie auch unter „https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started“ finden.
 

Die Anleitung setzt voraus, dass Sie mit solchen Azure IaaS-Grundlagen vertraut sind:
 * Bereitstellen virtueller Computer oder virtueller Netzwerke über das Azure-Portal oder PowerShell.
 * Das plattformübergreifende Azure-Befehlszeilentool (CLI), inklusive der Möglichkeit, Vorlagen für JavaScript Object Notification (JSON) zu verwenden.

Die Anleitung setzt auch voraus, dass Sie mit SAP HANA und SAP NetWeaver und deren lokaler Installation vertraut sind.

Weitere Informationen, die Sie kennen und verinnerlicht haben sollten, was die Installation und den Betrieb von SAP HANA- und SAP-Anwendungsinstanzen in Azure betrifft, sind:

Informationen zur Planung der SAP-Bereitstellung in Azure, z.B. VNet-Planung und Nutzung von Azure Storage, finden Sie unter [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).

Bereitstellungsprinzipien und Möglichkeiten zur Bereitstellung von VMs in Azure finden Sie unter [Bereitstellung von Azure Virtual Machines für SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).

Aspekte zur hohen Verfügbarkeit von SAP NetWeaver ASCS, SCS und ERS in Azure werden unter [Hohe Verfügbarkeit von SAP NetWeaver auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide) beschrieben.

Der Leitfaden [Erstellen einer SAP NetWeaver Multi-SID-Konfiguration](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid) enthält Details zur Verbesserung der Effizienz beim Nutzen einer Multi-SID-Installation von ASCS/SCS in Azure.

Prinzipien der Ausführung von SAP NetWeaver basierend auf Linux-VMs in Azure sind unter [Ausführen von SAP NetWeaver auf Microsoft Azure SUSE-Linux-VMs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart) beschrieben. Lesen Sie sich diesen Leitfaden durch, da er Details zu einigen spezifischen Einstellungen für Linux auf Azure-VMs sowie Details zum richtigen Anfügen von Azure-Speicherdatenträgern an Linux-VMs enthält.

Bisher sind Azure-VMs von SAP nur für SAP HANA-Konfigurationen mit zentraler Hochskalierung zertifiziert. Konfigurationen mit horizontaler Hochskalierung mit SAP HANA-Workload werden noch nicht unterstützt. Informationen zur hohen Verfügbarkeit von SAP HANA bei Konfigurationen mit zentraler Hochskalierung finden Sie unter [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).


Empfehlungen und Möglichkeiten zur Sicherung von SAP HANA-Datenbanken auf Azure-VMs sind in diesen Leitfäden aufgeführt.

* [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA-Sicherung mit Azure Backup auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA-Sicherung auf der Grundlage von Speichermomentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

Die Verwendung der SAP Cloud Appliance Library zum Bereitstellen von S/4HANA oder BW/4HANA ist unter [Bereitstellen von SAP S/4HANA oder BW/4HANA in Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) beschrieben.

Von SAP sind die von SAP HANA unterstützten Betriebssysteme unter [SAP Support Note #2235581 – SAP HANA: Supported Operating Systems](https://launchpad.support.sap.com/#/notes/2235581/E) (SAP-Supporthinweis #2235581 – SAP HANA: Unterstützte Betriebssysteme) dokumentiert. Auf Azure-VMs wird nur ein Teil dieser Betriebssysteme unterstützt. Für die Bereitstellung von SAP HANA in Azure werden die folgenden Betriebssysteme unterstützt: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Weitere Dokumentation von SAP in Bezug auf SAP HANA und die verschiedenen Linux-Betriebssysteme:

* [SAP Support Note #171356 – SAP Software on Linux: General Information](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP Support Note #1944799 – SAP HANA Guidelines for SLES Operating System Installation](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP Support Note #2205917 – SAP HANA DB Recommended OS Settings for SLES 12 for SAP Applications](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP Support Note #1984787 – SUSE Linux Enterprise Server 12: Installation Notes](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP Support Note #2009879 – SAP HANA Guidelines for Red Hat Enterprise Linux (RHEL) Operating System](https://launchpad.support.sap.com/#/notes/2009879) (SAP-Supporthinweis #2009879 – SAP HANA-Richtlinien für das RHEL-Betriebssystem (Red Hat Enterprise Linux))
* [2292690 – SAP HANA DB: Recommended OS Settings for RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E) (2292690 – SAP HANA DB: Empfohlene Betriebssystemeinstellungen für RHEL 7)

Beachten Sie auch diese beiden SAP-Hinweise zur SAP-Überwachung in Azure:

* SAP-Hinweis zur „erweiterten Überwachung“ von SAP mit Linux-VMs unter Azure: [SAP-Hinweis 2191498](https://launchpad.support.sap.com/#/notes/2191498/E).
* SAP-Hinweis mit Informationen zu SAPOSCOL unter Linux: [SAP-Hinweis 1102124](https://launchpad.support.sap.com/#/notes/1102124/E).
* Wichtige Überwachungsmetriken für SAP in Microsoft Azure: [SAP-Hinweis 2178632](https://launchpad.support.sap.com/#/notes/2178632/E).


Azure-VM-Typen und von SAP unterstützte Workloadszenarien für SAP HANA sind unter [SAP certified IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html) (IaaS-Plattformen mit SAP-Zertifizierung) dokumentiert. Azure-VM-Typen, die verwendet werden können und von SAP für SAP NetWeaver oder die S/4HANA-Anwendungsschicht zertifiziert sind, sind von SAP unter [SAP Note 1928533 – SAP Applications on Azure: Supported Products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533/E) (SAP-Hinweis 1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen) dokumentiert.

>[!Note]
>SAP-Linux-Azure wird nur für Azure Resource Manager (ARM) und nicht für das klassische Bereitstellungsmodell (ASM) unterstützt. 


In dieser Anleitung werden zwei unterschiedliche Möglichkeiten zum manuellen Installieren von SAP HANA auf Azure-VMs beschrieben:

* Mit dem SAP Software Provisioning Manager (SWPM) im Rahmen einer verteilten NetWeaver-Installation im Schritt „Datenbankinstanzinstallation“.
* Mit dem HANA Lifecycle Management Tool „hdblcm“ und anschließendem Installieren von NetWeaver.

Sie können auch SWPM verwenden und alle Komponenten (SAP HANA, SAP-Anwendungsserver, ASCS-Instanz) auf einer einzelnen VM installieren. Dies ist [hier](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/) beschrieben. Diese Option wird in dieser Anleitung zwar nicht beschrieben, aber die zu berücksichtigenden Elemente sind identisch.

Bevor Sie eine Installation beginnen, lesen Sie unbedingt „Vorbereiten von Azure-VMs auf eine manuelle Installation von SAP HANA“, unmittelbar nach den beiden Checklisten für die SAP HANA-Installation. Auf diese Weise können mehrere grundlegende Fehler vermieden werden, die auftreten können, wenn Sie nur eine standardmäßige Azure-VM-Konfiguration verwenden.

## <a name="checklist-for-sap-hana-installation-using-sap-swpm"></a>Checkliste für SAP HANA-Installation per SAP SWPM
Dieser Abschnitt enthält die wichtigsten Schritte für eine manuelle SAP HANA-Einzelinstanzinstallation, wenn Sie SAP SWPM verwenden, um eine verteilte SAP NetWeaver 7.5-Installation durchzuführen. Die einzelnen Elemente werden im Verlauf der Anleitung in Form von Screenshots ausführlicher veranschaulicht.

* Erstellen Sie ein virtuelles Azure-Netzwerk, das die beiden Test-VMs enthält.
* Stellen Sie zwei Azure-VMs mit Betriebssystem (in unserem Beispiel: SLES bzw. SLES für SAP Applications 12 SP1) gemäß Azure Resource Manager-Modell bereit.
* Fügen Sie zwei Azure Standard- oder Premium-Speicherdatenträger an die Anwendungsserver-VM an (z.B. Datenträger mit 75 GB oder 500 GB).
* Fügen Sie Storage Premium-Datenträger an die HANA DB-Server-VM an. Details hierzu finden Sie im Abschnitt „Datenträgereinrichtung“.
* Fügen Sie je nach Größe- oder Durchsatzanforderungen mehrere Datenträger an, und erstellen Sie Stripesetvolumes, entweder mithilfe logischer Volumeverwaltung (LVM) oder eines Hilfsprogramms zur Verwaltung mehrerer Geräte (mdadm) auf der Betriebssystemebene in der VM.
* Erstellen Sie XFS-Dateisysteme auf den angefügten Datenträgern/logischen Volumes.
* Stellen Sie die neuen XFS-Dateisysteme auf Betriebssystemebene bereit. Verwenden Sie ein Dateisystem, um die gesamte SAP-Software vorzuhalten, und das andere beispielsweise für das sapmnt-Verzeichnis und ggf. Sicherungen. Stellen Sie auf dem SAP HANA-DB-Server XFS-Dateisysteme auf den Datenträgern für Storage Premium als „/hana“ und „/usr/sap“ bereit. Dieser Prozess ist erforderlich, um zu verhindern, dass sich das Stammdateisystem füllt, das auf virtuellen Azure-Linux-Computern nicht groß ist.
* Geben Sie die lokalen IP-Adressen der Test-VMs in „/etc/hosts“ ein.
* Geben Sie den Parameter „nofail“ in „/etc/fstab“ ein.
* Legen Sie die Linux-Kernelparameter gemäß den SAP-Hinweisen zu HANA und zur von Ihnen verwendeten Version des Linux-Betriebssystems fest. Weitere Informationen finden Sie im Abschnitt „Kernel-Parameter“.
* Fügen Sie den Auslagerungsbereich hinzu.
* Installieren Sie optional einen grafischen Desktop auf den Test-VMs. Verwenden Sie andernfalls eine SAPinst-Remoteinstallation.
* Laden Sie die SAP-Software vom SAP Service Marketplace herunter.
* Installieren Sie die SAP ASCS-Instanz auf der App-Server-VM.
* Geben Sie das Verzeichnis „/sapmnt“ zwischen den Test-VMs mithilfe von NFS frei. Die Anwendungsserver-VM ist der NFS-Server.
* Installieren Sie die Datenbankinstanz, einschließlich HANA, mit SWPM auf der DB-Server-VM.
* Installieren Sie den primären Anwendungsserver (PAS) auf der Anwendungsserver-VM.
* Starten Sie die SAP Management Console (MC), und stellen Sie z. B. eine Verbindung mit SAP GUI/HANA Studio her.

## <a name="checklist-for-sap-hana-installation-using-hdblcm"></a>Checkliste für SAP HANA-Installation per hdblcm
Dieser Abschnitt enthält die wichtigsten Schritte für eine manuelle Einzelinstanz-SAP HANA-Installation für Demo- oder Prototypzwecke, wenn Sie SAP hdblcm verwenden, um eine verteilte SAP NetWeaver 7.5-Installation auszuführen. Die einzelnen Elemente werden im Verlauf der Anleitung in Form von Screenshots ausführlicher veranschaulicht.

* Erstellen Sie ein virtuelles Azure-Netzwerk, das die beiden Test-VMs enthält.
* Stellen Sie zwei Azure-VMs mit Betriebssystem (in unserem Beispiel: SLES bzw. SLES für SAP Applications 12 SP1) gemäß Azure Resource Manager-Modell bereit.
* Fügen Sie zwei Azure Standard- oder Premium-Speicherdatenträger an die Anwendungsserver-VM an (z.B. Datenträger mit 75 GB oder 500 GB).
* Fügen Sie Storage Premium-Datenträger an die HANA DB-Server-VM an. Details hierzu finden Sie im Abschnitt „Datenträgereinrichtung“.
* Fügen Sie je nach Größe- oder Durchsatzanforderungen mehrere Datenträger an, und erstellen Sie Stripesetvolumes, entweder mithilfe logischer Volumeverwaltung (LVM) oder eines Hilfsprogramms zur Verwaltung mehrerer Geräte (mdadm) auf der Betriebssystemebene in der VM.
* Erstellen Sie XFS-Dateisysteme auf den angefügten Datenträgern/logischen Volumes.
* Stellen Sie die neuen XFS-Dateisysteme auf Betriebssystemebene bereit. Verwenden Sie ein Dateisystem, um die gesamte SAP-Software vorzuhalten, und das andere beispielsweise für das sapmnt-Verzeichnis und ggf. Sicherungen. Stellen Sie auf dem SAP HANA-DB-Server XFS-Dateisysteme auf den Datenträgern für Storage Premium als „/hana“ und „/usr/sap“ bereit. Dieser Prozess ist erforderlich, um zu verhindern, dass sich das Stammdateisystem füllt, das auf virtuellen Azure-Linux-Computern nicht groß ist.
* Geben Sie die lokalen IP-Adressen der Test-VMs in „/etc/hosts“ ein.
* Geben Sie den Parameter „nofail“ in „/etc/fstab“ ein.
* Legen Sie die Kernelparameter gemäß den SAP-Hinweisen zu HANA und zur von Ihnen verwendeten Version des Linux-Betriebssystems fest. Weitere Informationen finden Sie im Abschnitt „Kernel-Parameter“.
* Fügen Sie den Auslagerungsbereich hinzu.
* Installieren Sie optional einen grafischen Desktop auf den Test-VMs. Verwenden Sie andernfalls eine SAPinst-Remoteinstallation.
* Laden Sie die SAP-Software vom SAP Service Marketplace herunter.
* Erstellen Sie eine Gruppe „sapsys“ mit der Gruppen-ID 1001 auf der HANA DB-Server-VM.
* Installieren Sie SAP HANA auf der DB-Server-VM mithilfe des HANA-Datenbanklebenszyklus-Managers (HDBLCM).
* Installieren Sie die SAP ASCS-Instanz auf der App-Server-VM.
* Geben Sie das Verzeichnis „/sapmnt“ zwischen den Test-VMs mithilfe von NFS frei. Die Anwendungsserver-VM ist der NFS-Server.
* Installieren Sie die Datenbankinstanz, einschließlich HANA, mit SWPM auf der HANA-DB-Server-VM.
* Installieren Sie den primären Anwendungsserver (PAS) auf der Anwendungsserver-VM.
* Starten Sie SAP MC, und stellen Sie eine Verbindung über SAP GUI/HANA Studio her.

## <a name="prepare-azure-vms-for-a-manual-installation-of-sap-hana"></a>Vorbereiten von Azure-VMs auf eine manuelle Installation von SAP HANA
Dieser Abschnitt enthält die folgenden Themen:

* Betriebssystemupdates
* Datenträgereinrichtung
* Kernelparameter
* Dateisysteme
* /etc/hosts
* /etc/fstab

### <a name="os-updates"></a>Betriebssystemupdates
Da von den Distributoren des Linux-Betriebssystems Updates und Fixes für das Betriebssystem bereitgestellt werden, um die Sicherheit und den reibungslosen Betrieb sicherzustellen, empfiehlt es sich, vor dem Installieren zusätzlicher Software die Verfügbarkeit von Updates zu prüfen.
In vielen Fällen lassen sich Supportanfragen vermeiden, wenn sich das System auf dem aktuellen Patchlevel befindet. Stellen Sie außerdem sicher, dass Sie Folgendes verwenden:
* SUSE Linux Enterprise Server für SAP-Anwendung
* Red Hat Enterprise Linux für SAP-Anwendung oder für SAP HANA 

Registrieren Sie die Betriebssystembereitstellung unter Ihrem Linux-Abonnement, das Sie vom Linux-Anbieter erhalten haben. Beachten Sie bei SUSE, dass auch SUSE-Betriebssystemimages für SAP-Anwendungen vorhanden sind, die bereits Dienste enthalten und automatisch registriert werden.

Beispiel: Überprüfen Sie für SUSE Linux einfach wie folgt die verfügbaren Patches:

 `sudo zypper list-patches`

Je nach Art des Fehlers sind die Patches nach Kategorie und Schweregrad klassifiziert.

Für die Kategorie werden häufig folgende Werte verwendet: Sicherheit, Empfohlen, Optional, Feature, Dokument oder YaST.

Für den Schweregrad werden häufig folgende Werte verwendet: Kritisch, Wichtig, Mittel, Niedrig oder Nicht angegeben.

Zypper sucht nur nach erforderlichen Updates für Ihre installierten Pakete.

Ein Befehl könnte beispielsweise folgendermaßen aussehen:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Wenn Sie den Parameter *--dry-run* hinzufügen, können Sie das Update testen, ohne dass das System tatsächlich aktualisiert wird.


### <a name="disk-setup"></a>Datenträgereinrichtung
Das Stammdateisystem einer Linux-VM unter Azure hat eine begrenzte Größe. Daher ist es erforderlich, an eine Azure-VM zur Ausführung von SAP zusätzlichen Datenträgerspeicherplatz anzufügen. Für die SAP-Anwendungsserver-Azure-VMs kann die Nutzung von Azure Standard Storage-Datenträgern zulässig sein. Für die SAP HANA-DBMS-Azure-VMs ist die Nutzung von Azure Storage Premium-Datenträgern für die Implementierung zu Produktions- und anderen Zwecken aber obligatorisch.

Basierend auf den SAP HANA TDI-Speicheranforderungen ([SAP HANA TDI Storage Requirements](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)) empfehlen wir die folgende Konfiguration von Azure Storage Premium: 

| VM-SKU | RAM |  „/hana/data“ und „/hana/log“ <br /> Striping mit LVM oder MDAADM | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

Bei der vorgeschlagenen Datenträgerkonfiguration werden das HANA-Datenvolume und -Protokollvolume auf der gleichen Gruppe von Azure Storage Premium-Datenträgern angeordnet, für die das Striping per LVM oder MDADM durchgeführt wird. Es ist nicht erforderlich, eine RAID-Redundanzebene zu definieren, da bei Azure Storage Premium aus Redundanzgründen drei Images der Datenträger aufbewahrt werden. Sie können sich für unterschiedliche Datenträgerkonfigurationen entscheiden. Lesen Sie sich aber bitte die Artikel [SAP HANA TDI Storage Requirements](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (SAP HANA TDI-Speicheranforderungen) und [SAP HANA Server Installation and Update Guide](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) (SAP HANA-Server – Installations- und Updateleitfaden) durch, um sicherzustellen, dass Sie genügend Speicher konfiguriert haben. Informieren Sie sich auch über die verschiedenen VHD-Durchsatzvolumes der unterschiedlichen Azure Storage Premium-Datenträger, die unter [Storage Premium-Hochleistungsspeicher und verwaltete Datenträger für VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage) dokumentiert sind. 

Je nach Bedarf können Sie den HANA DBMS-VMs weitere Storage Premium-Datenträger zum Speichern von Datenbank- oder Transaktionsprotokollsicherungen hinzufügen.

Weitere Informationen zu den zwei wichtigsten Tools zum Konfigurieren von Striping finden Sie in den folgenden Artikeln:

* [Konfigurieren von Software-RAID unter Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Weitere Informationen zum Anfügen von Datenträgern an Azure-VMs, auf denen Linux als Gast-BS ausgeführt wird, finden Sie unter [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Storage Premium ermöglicht die Definition von Zwischenspeicherungsmodi für Datenträger. Für das Stripeset mit „/hana/data“ und „/hana/log“ sollte die Zwischenspeicherung von Datenträgern deaktiviert sein. Für die anderen Volumes (Datenträger) sollte der Zwischenspeicherungsmodus auf READONLY festgelegt werden.

Weitere Informationen finden Sie unter [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer](../../../storage/storage-premium-storage.md).

Um JSON-Beispielvorlagen für die Erstellung von virtuellen Computern zu suchen, wechseln Sie zu [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) (Azure-Schnellstartvorlagen).
Die Vorlage „vm-simple-sles“ ist eine einfache Vorlage, die einen Speicherabschnitt mit einem zusätzlichen 100-GB-Datenträger enthält. Diese Vorlage kann als Basis verwendet werden. Es wird erwartet, dass Sie die Vorlage an Ihre jeweilige Konfiguration anpassen.

>[!Note]
>Beachten Sie, dass es wichtig ist, den Azure Storage-Datenträger per UUID anzufügen. Dies ist unter [Ausführen von SAP NetWeaver auf Microsoft Azure SUSE-Linux-VMs](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dokumentiert.


In der Testumgebung wurden zwei Azure-Standarddatenträger für die Speicherung der SAP-App-Server-VM angefügt, wie im folgenden Screenshot gezeigt. Ein Datenträger speicherte die gesamte SAP-Software für die Installation (z.B. NetWeaver 7.5, SAP GUI, SAP HANA usw.). Der zweite Datenträger stellte sicher, dass ausreichend freier Speicherplatz für zusätzliche Anforderungen (z.B. Sicherung und Testdaten) verfügbar sein würde und das Verzeichnis „/sapmnt“ (d.h. SAP-Profile) für alle virtuellen Computer freigegeben wurde, die zur gleichen SAP Landscape gehören.

![SAP HANA-App-Server-Datenträgerfenster, in dem zwei Datenträger und ihre Größen angezeigt werden](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Kernelparameter
Für SAP HANA sind spezifische Linux-Kerneleinstellungen erforderlich, die nicht Teil des Standardimage des Azure-Katalogs sind und manuell festgelegt werden müssen. Die Parameter können sich für SUSE und Red Hat unterscheiden. Die oben aufgeführten SAP-Hinweise enthalten Informationen zu diesen Parametern. In den hier gezeigten Screenshots wurde SUSE Linux 12 SP1 verwendet. 

SLES für SAP Applications 12 GA und SP1 verfügen über ein neues Tool, das das bisherige Hilfsprogramm „sapconf“ ersetzt. Das Tool heißt „tuned-adm“, und ein spezielles SAP HANA-Profil ist dafür verfügbar. Um das System für SAP HANA zu optimieren, geben Sie einfach folgenden Befehl als Root-Benutzer ein: „tuned-adm profile sap-hana“.

Weitere Informationen zu „tuned-adm“ finden Sie in der SUSE-Dokumentation unter:

* [SLES-for-SAP Applications 12 SP1 documentation about tuned-adm profile sap-hana](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) (SLES für SAP Applications 12 SP1-Dokumentation zu „tuned-adm profile sap-hana“).

Der folgende Screenshot zeigt, wie mit „tuned-adm“ das Element „transparent_hugepage“ und die Werte für „numa_balancing“ gemäß den erforderlichen SAP HANA-Einstellungen geändert wurden.

![Das „tuned-adm“-Tool ändert Werte gemäß der erforderlichen SAP HANA-Einstellungen](./media/hana-get-started/image005.jpg)

Um die SAP HANA-Kerneleinstellungen permanent zu machen, muss „grub2“ unter SLES 12 verwendet werden. Weitere Informationen zu „grub2“ finden Sie im [Abschnitt „Configuration File Structure“ (Struktur der Konfigurationsdatei) der SUSE-Dokumentation](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

Der folgende Screenshot zeigt, wie die Kerneleinstellungen in der config-Datei geändert und dann mit „grub2-mkconfig“ kompiliert wurden.

![In der Konfigurationsdatei geänderte und mit grub2-mkconfig kompilierte Kerneleinstellungen](./media/hana-get-started/image006.jpg)

Eine andere Möglichkeit ist die Änderung der Einstellungen über YaST und die **Boot Loader** > **Kernel Parameters**-Einstellungen.

![Die Registerkarte "Kernel Parameters" im YaST Boot Loader](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Dateisysteme
Der folgende Screenshot zeigt zwei Dateisysteme, die auf der SAP-App-Server-VM zusätzlich zu den beiden angefügten Azure-Standarddatenträgern für die Speicherung erstellt wurden. Beide Dateisysteme haben den Typ XFS und werden unter „/sapdata“ und „/sapsoftware“ bereitgestellt.

Es ist nicht erforderlich, dass Sie Ihre Dateisysteme auf diese Weise strukturieren. Sie haben andere Optionen für die Strukturierung des Datenträger-Speicherplatzes. Der wichtigste Aspekt ist, zu verhindern, dass dem Stammdateisystem kein freier Speicherplatz mehr zur Verfügung steht.

![Zwei auf der SAP-App-Server-VM erstellte Dateisysteme](./media/hana-get-started/image008.jpg)

In Bezug auf die SAP HANA DB-VM ist der Hinweis wichtig, dass bei einer Datenbankinstallation mit SAPinst (SWPM) und der einfachen Installationsoption „Typisch“ alles unter „/hana“ und „/usr/sap“ installiert wird. Die Standardeinstellung für die Sicherung von SAP HANA-Protokollen befindet sich unter „/usr/sap“. Noch einmal: Da es wichtig ist, zu verhindern, dass der Speicherplatz für das Stammdateisystem ausgeht, stellen Sie sicher, dass genügend freier Speicherplatz unter „/hana“ und „/usr/sap“ vorhanden ist, bevor Sie SAP HANA mit SWPM installieren.

Eine Beschreibung des standardmäßigen Dateisystemlayouts von SAP HANA finden Sie im [SAP HANA Server Installation and Update Guide](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) (Handbuch zu Installation und Update von SAP HANA-Servern).
![Zusätzliche auf der SAP-App-Server-VM erstellte Dateisysteme](./media/hana-get-started/image009.jpg)

Beim Installieren von SAP NetWeaver auf einem Standardimage aus dem SLES/SLES für SAP Applications 12-Azure-Katalog wird eine Meldung angezeigt, dass kein Auslagerungsbereich vorhanden ist. Um diese Meldung zu schließen, können Sie manuell eine Auslagerungsdatei mithilfe von „dd“, „mkswap“ und „swapon“ hinzufügen. Um zu erfahren, wie Sie dabei vorgehen, suchen Sie im [Abschnitt „Using the YaST Partitioner“ (Verwenden des YaST-Partitionierers) der SUSE-Dokumentation](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) nach „Adding a Swap File Manually“ (Manuelles Hinzufügen einer Auslagerungsdatei).

Eine weitere Möglichkeit ist das Konfigurieren des Auslagerungsbereichs über den Linux-VM-Agent. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](../../linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![Popupmeldung, die darüber informiert, dass der Auslagerungsbereich nicht ausreicht](./media/hana-get-started/image010.jpg)


### <a name="etchosts"></a>/etc/hosts
Ein weiterer wichtiger Aspekt, den Sie vor Beginn der Installation von SAP berücksichtigen müssen, ist das Einbeziehen der Hostnamen und IP-Adressen von SAP-VMs in die Datei „/etc/hosts“. Stellen Sie alle SAP-VMs in einem virtuellen Azure-Netzwerk bereit, und verwenden Sie dann die internen IP-Adressen.

![In der /etc/hosts-Datei aufgelistete Hostnamen und IP-Adressen der SAP-VMs](./media/hana-get-started/image011.jpg)

### <a name="etcfstab"></a>/etc/fstab

Es hat sich als sinnvoll herausgestellt, während der Testphase „fstab“ den Parameter „nofail“ hinzuzufügen. Falls mit den Datenträgern etwas schiefgeht, wird die VM trotzdem gestartet und bleibt nicht im Startprozess hängen. Aber achten Sie genau darauf, da, wie in diesem Szenario, der zusätzliche Datenträger-Speicherplatz möglicherweise nicht zur Verfügung steht, und Prozesse vielleicht das Stammdateisystem auffüllen. Wenn „/hana“ nicht vorhanden ist, wird SAP HANA nicht gestartet.

![Hinzufügen des Parameters „nofail“ zu „fstab“](./media/hana-get-started/image000c.jpg)

## <a name="install-graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Installieren des grafischen Gnome-Desktops unter SLES 12/SLES für SAP Applications 12
Dieser Abschnitt enthält die folgenden Themen:

* Installieren von Gnome-Desktop und xrdp unter SLES 12/SLES für SAP Applications 12
* Ausführen der Java-basierten SAP MC mit Firefox unter SLES 12/SLES für SAP Applications 12

Sie können auch Alternativen wie Xterminal oder VNC verwenden, jedoch wird ab September 2016 nur „xrdp“ im Handbuch beschrieben.

### <a name="installing-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Installieren von Gnome-Desktop und xrdp unter SLES 12/SLES für SAP Applications 12
Wenn Sie einen Microsoft Windows-Hintergrund haben, können Sie mühelos direkt einen grafischen Desktop in den SAP-Linux-VMs verwenden, um Firefox, Sapinst, SAP GUI, SAP MC oder HANA Studio auszuführen und mit dem virtuellen Computer von einem Windows-Computer aus eine Verbindung über RDP herzustellen. Diese Vorgehensweise ist unter Umständen für einen Produktionsdatenbankserver nicht geeignet, aber für eine reine Prototyp-/Demoumgebung ist dies kein Problem. So installieren Sie den Gnome-Desktop auf einem virtuellen Azure-Computer unter SLES 12/SLES für SAP Applications 12:

Installieren Sie den Gnome-Desktop mit dem folgenden Befehl (z.B. in einem PuTTY-Fenster):

`zypper in -t pattern gnome-basic`

Installieren Sie „xrdp“, um eine Verbindung mit der VM per RDP zu ermöglichen:

`zypper in xrdp`

Bearbeiten Sie „/etc/sysconfig/windowmanager“, und legen Sie den Standard-Manager für Fenster auf „Gnome“ fest:

`DEFAULT_WM="gnome"`

Stellen Sie durch Ausführen von „chkconfig“ sicher, dass „xrdp“ nach einem Neustart automatisch gestartet wird:

`chkconfig -level 3 xrdp on`

Falls ein Problem mit der RDP-Verbindung auftritt, können Sie es mit einem Neustart probieren (ggf. über ein PuTTY-Fenster):

`/etc/xrdp/xrdp.sh restart`

Falls der xrdp-Neustart nicht wie oben beschrieben funktioniert, sollten Sie das Vorhandensein einer PID-Datei überprüfen und diese ggf. entfernen:

`check /var/run` und suchen Sie nach `xrdp.pid`

Entfernen Sie die Datei, und wiederholen Sie den Neustart.

### <a name="sap-mc"></a>SAP MC
Nach der Installation des Gnome-Desktops wird beim Start der Java-basierten grafischen SAP MC aus einer Firefox-Instanz, die auf einem virtuellen Azure-Computer unter Azure SLES 12/SLES für SAP Applications 12 ausgeführt wird, aufgrund des fehlenden Java-Browser-Plug-Ins möglicherweise ein Fehler angezeigt.

Die URL zum Starten der SAP MC lautet „<server>:5<Instanznummer>13“.

Weitere Informationen finden Sie unter [Starting the Web-Based SAP Management Console](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm) (Starten der webbasierten SAP Management Console).

Der folgende Screenshot zeigt die Fehlermeldung, die angezeigt wird, wenn das Java-Browser-Plug-In nicht vorhanden ist.

![Fehlermeldung über fehlendes Java-Browser-Plug-In](./media/hana-get-started/image013.jpg)

Eine Möglichkeit zur Behebung des Problems besteht darin, das fehlende-Plug-In einfach mithilfe von YaST zu installieren, wie im folgenden Screenshot gezeigt.

![Verwenden von YaST zum Installieren des fehlenden Plug-Ins](./media/hana-get-started/image014.jpg)

Bei wiederholtem Aufruf der URL der SAP Management Console wird ein Dialogfeld angezeigt, in dem Sie gefragt werden, ob Sie das Plug-In aktivieren möchten.

![Dialogfeld zum Anfordern der Plug-In-Aktivierung](./media/hana-get-started/image015.jpg)

Ein zusätzliches potenzielles Problem ist eine Fehlermeldung betreffs einer fehlenden Datei, „javafx.properties“. Dies hängt damit zusammen, dass Oracle Java 1.8 for SAP GUI 7.4 erforderlich ist [siehe SAP-Hinweis 2059429](https://launchpad.support.sap.com/#/notes/2059424). Weder die IBM Java-Version noch das mit SLES/SLES für SAP Applications 12 gelieferte openjdk-Paket enthalten das benötigte JavaFX-Framework. Die Lösung besteht darin, Java SE8 von Oracle herunterzuladen und zu installieren.

Einen Artikel, der ein ähnliches Problem mit openjdk unter openSUSE beschreibt, finden Sie unter [SAPGui 7.4 Java for openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="install-sap-hana-manually-by-using-swpm-as-part-of-a-netweaver-75-installation"></a>Manuelle SAP HANA-Installation per SWPM im Rahmen einer NetWeaver 7.5-Installation
Die Reihe von Screenshots in diesem Abschnitt zeigt die wichtigsten Schritte zum Installieren von SAP NetWeaver 7.5 und SAP HANA SP12 bei der Verwendung von SWPM (SAPinst). Als Teil einer NetWeaver 7.5-Installation kann SWPM auch die HANA-Datenbank als Einzelinstanz installieren.

In der Beispieltestumgebung installierten wir nur einen einzigen Advanced Business Application Programming-App-Server (ABAP). Wie im folgenden Screenshot gezeigt, wurde die „Distributed System“-Option verwendet, um ASCS- und primäre Anwendungsserverinstanzen in einer Azure-VM und SAP HANA als Datenbanksystem in einer anderen Azure-VM zu installieren.

![ASCS- und primäre Anwendungsserverinstanzen, die mithilfe der „Distributed System“-Option installiert werden](./media/hana-get-started/image012.jpg)

Nachdem die ASCS-Instanz auf der App-Server-VM installiert und in der SAP Management Console auf „grün“ festgelegt wurde, muss das Verzeichnis „/sapmnt“ (in dem das SAP-Profilverzeichnis enthalten ist) für die SAP HANA DB-Server-VM freigegeben werden. Für den DB-Installationsschritt muss Zugriff auf diese Informationen bestehen. Die beste Möglichkeit, Zugriff zu gewähren, ist die Verwendung von NFS, das für die Nutzung von YaST konfiguriert werden kann.

![SAP Management Console mit der auf der App-Server-VM installierten und auf „grün“ festgelegten ASCS-Instanz](./media/hana-get-started/image016.jpg)

Auf der App-Server-VM sollte das Verzeichnis „/sapmnt“ über NFS freigegeben werden, indem die Optionen **rw** und **no_root_squash** verwendet werden. Die Standardeinstellungen sind „ro“ und „root_squash“. Dies kann beim Installieren der Datenbankinstanz zu Problemen führen.

![Freigabe des Verzeichnisses „/sapmnt“ über NFS mit den Optionen „rw“ und „no_root_squash“](./media/hana-get-started/image017b.jpg)

Wie der nächste Screenshot zeigt, muss die /sapmnt-Freigabe von der App-Server-VM über **NFS Client** (mithilfe von YaST) auf der SAP HANA DB-Server-VM konfiguriert werden.

![Die mithilfe von „NFS Client“ konfigurierte /sapmnt-Freigabe](./media/hana-get-started/image018b.jpg)

Zum Ausführen einer verteilten NetWeaver 7.5-Installation, **Datenbankinstanz**, wie im folgenden Screenshot gezeigt, melden Sie sich bei der SAP HANA-DB-Server-VM an, und starten Sie SWPM.

![Installieren einer Datenbankinstanz durch Anmelden bei der SAP HANA-DB-Server-VM und Starten von SWPM](./media/hana-get-started/image019.jpg)

Geben Sie nach der Auswahl einer „typischen“ Installation und des Pfads zu den Installationsmedien eine DB-SID, den Hostnamen, die Instanznummer und das DB-Systemadministratorkennwort ein.

![Die Anmeldeseite des SAP HANA-Datenbank-Systemadministrators](./media/hana-get-started/image035b.jpg)

Geben Sie das Kennwort für das DBACOCKPIT-Schema ein.

![Das Kennwort-Eingabefeld für das DBACOCKPIT-Schema](./media/hana-get-started/image036b.jpg)

Geben Sie eine Frage für das SAPABAP1-Schemakennwort ein.

![Eingeben einer Frage für das SAPABAP1-Schemakennwort](./media/hana-get-started/image037b.jpg)

Nachdem der Vorgang abgeschlossen ist, wird neben jeder Phase des DB-Installationsprozesses ein grünes Häkchen angezeigt. In einem Meldungsfeldfenster sollte folgende Meldung angezeigt werden: „Execution of ... Database Instance has completed“ (Ausführung von ... Datenbankinstanz wurde abgeschlossen).

![Fenster mit Bestätigungsmeldung zum Abschluss der Aufgabe](./media/hana-get-started/image023.jpg)

Nach der erfolgreichen Installation sollte die DB-Instanz in der SAP Management Console auch als „grün“ angezeigt werden, und die vollständige Liste mit den SAP HANA-Prozessen sollte zu sehen sein (hdbindexserver, hdbcompileserver usw.).

![SAP Management Console-Fenster mit Liste der SAP HANA-Prozesse](./media/hana-get-started/image024.jpg)

Der folgende Screenshot zeigt die Teile der Dateistruktur unter „/hana/shared“, die von SWPM während der HANA-Installation erstellt wurden. Da es keine Option gab, um einen anderen Pfad anzugeben, ist es wichtig, vor der SAP HANA-Installation mit SWPM zusätzlichen Datenträger-Speicherplatz unter „/hana“ bereitzustellen, um zu verhindern, dass dem Stammdateisystem der Speicherplatz ausgeht.

![Die während der HANA-Installation erstellte /hana/shared-Dateistruktur](./media/hana-get-started/image025.jpg)

Dieser Screenshot zeigt die Dateistruktur des Verzeichnisses „/usr/sap“.

![Die Dateistruktur des Verzeichnisses „/usr/sap“](./media/hana-get-started/image026.jpg)

Der letzte Schritt der verteilten ABAP-Installation ist die „Instanz des primären Anwendungsservers“.

![ABAP-Installation mit der Instanz des primären Anwendungsservers als abschließendem Schritt](./media/hana-get-started/image027b.jpg)

Nach der Installation der primären Instanz des Anwendungsservers und der SAP GUI verwenden Sie die Transaktion „dbacockpit“, um zu bestätigen, dass die SAP HANA-Installation ordnungsgemäß abgeschlossen wurde.

![DBA Cockpit-Fenster mit Bestätigung der erfolgreichen Installation](./media/hana-get-started/image028b.jpg)

Als abschließenden Schritt können Sie zuerst SAP HANA Studio auf der SAP-App-Server-VM installieren und dann eine Verbindung mit der SAP HANA-Instanz herstellen, die auf der DB-Server-VM ausgeführt wird.

![Installieren von SAP HANA Studio auf der SAP-App-Server-VM](./media/hana-get-started/image038b.jpg)

## <a name="install-sap-hana-manually-by-using-the-hana-lifecycle-management-tool-hdblcm"></a>Manuelle Installation von SAP HANA mithilfe des HANA Lifecycle Management Tools (hdblcm)
Außer der Installation von SAP HANA als Teil einer verteilten Installation mit SWPM können Sie HANA zuerst mithilfe von „hdblcm“ eigenständig installieren. Anschließend können Sie z.B. SAP NetWeaver 7.5 installieren. Die folgenden Screenshots zeigen, wie dies funktioniert.

Hier sind drei Quellen mit Informationen zum HANA-Tool „hdblcm“ aufgeführt:

* [Choosing the Correct SAP HANA HDBLCM for Your Task (Auswählen des richtigen SAP HANA-HDBLCM-Tools für Ihre Aufgabe)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA Lifecycle Management Tools (SAP HANA-Tools für die Lebenszyklusverwaltung)](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [SAP HANA Server Installation and Update Guide (SAP HANA-Serverinstallation und Aktualisierungshandbuch)](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

Um Probleme mit einer Gruppen-ID-Standardeinstellung für den (mit dem Tool „hdblcm“ erstellten) Benutzer „\<HANA SID\>adm“ zu vermeiden, sollten Sie eine neue Gruppe mit dem Namen „sapsys“ und der Gruppen-ID 1001 definieren, bevor Sie SAP HANA mit „hdblcm“ installieren.

![Neue mit der Gruppen-ID 1001 definierte Gruppe „sapsys“](./media/hana-get-started/image030.jpg)

Wenn Sie „hdblcm“ zum ersten Mal starten, wird ein einfaches Startmenü angezeigt. Wählen Sie Element 1, **Install new system** (Neues System installieren), wie im folgenden Screenshot gezeigt.

![Option „Install new system“ im hdblcm-Startfenster](./media/hana-get-started/image031.jpg)

Der folgende Screenshot zeigt die wichtigsten Optionen, die Sie zuvor ausgewählt haben.

> [!IMPORTANT]
> Verzeichnisse, die für HANA-Protokoll- und Datenvolumes benannt sind, sowie der Installationspfad („/hana/shared“ in diesem Beispiel) und „/usr/sap“ sollten nicht Teil des Stammdateisystems sein. Die Verzeichnisse gehören zu den Azure-Datenträgern, die der VM angefügt wurden, wie im Azure-VM-Einrichtungsabschnitt beschrieben. Dieser Ansatz vermeidet das Risiko, dass dem Stammdateisystem der Speicherplatz ausgeht. Sie sehen auch, dass der HANA-Administrator über die Benutzer-ID 1005 verfügt und Teil der Gruppe „sapsys“ (ID 1001) ist, die vor der Installation definiert wurde.

![Liste aller zuvor ausgewählten wesentlichen SAP HANA-Komponenten](./media/hana-get-started/image032.jpg)

Sie können die HANA-Benutzerdetails für „\<HANA SID\>adm“ (im folgenden Screenshot„azdadm“) unter „/etc/passwd“ überprüfen.

![Unter „/etc/passwd“ aufgelistete HANA-Benutzerdetails für „\<HANA SID\>adm“](./media/hana-get-started/image033.jpg)

Nach der Installation von SAP HANA mit „hdblcm“ sehen Sie die Dateistruktur in SAP HANA Studio. Das Schema SAPABAP1, in dem alle SAP NetWeaver-Tabellen enthalten sind, ist noch nicht verfügbar.

![SAP HANA-Dateistruktur in SAP HANA Studio](./media/hana-get-started/image034.jpg)

Nach der Installation von SAP HANA können Sie zusätzlich SAP NetWeaver installieren. Wie in diesem Screenshot gezeigt, wurde die Installation mithilfe von SWPM (wie im vorherigen Abschnitt beschrieben) als „verteilte Installation“ vorgenommen. Bei der Installation der Datenbankinstanz mit SWPM geben Sie die gleichen Daten mithilfe von „hdblcm“ ein (z.B. Hostname, HANA SID und Instanznummer). SWPM verwendet anschließend die vorhandene Installation von HANA und fügt weitere Schemas hinzu.

![Eine mithilfe von SWPM ausgeführte verteilte Installation](./media/hana-get-started/image035b.jpg)

Der folgende Screenshot zeigt den SWPM-Installationsschritt, in dem Sie Daten über das DBACOCKPIT-Schema eingeben.

![Der SWPM-Installationsschritt, wo DBACOCKPIT-Schemadaten eingegeben werden](./media/hana-get-started/image036b.jpg)

Geben Sie Daten zum SAPABAP1-Schema ein.

![Eingeben von Daten zum SAPABAP1-Schema](./media/hana-get-started/image037b.jpg)

Nach Abschluss der Installation der SWPM-Datenbankinstanz sehen Sie das SAPABAP1-Schema in SAP HANA Studio.

![Das SAPABAP1-Schema in SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Nach Abschluss der Installation des SAP-App-Servers und der SAP GUI können Sie die HANA DB-Instanz mit der Transaktion „dbacockpit“ überprüfen.

![Die mit der dbacockpit-Transaktion überprüfte HANA DB-Instanz](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP-Softwaredownloads
Sie können die Software vom SAP Service Marketplace herunterladen, wie in den folgenden Screenshots dargestellt.

* NetWeaver 7.5 für Linux/HANA herunterladen:

 ![Installations- und Upgradefenster des SAP Service für das Herunterladen von NetWeaver 7.5](./media/hana-get-started/image001.jpg)
* HANA SP12 Platform Edition herunterladen:

 ![Installations- und Upgradefenster des SAP Service für das Herunterladen von HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)


