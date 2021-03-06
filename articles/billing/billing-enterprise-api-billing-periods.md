---
title: "Azure-Abrechnungs-APIs für Unternehmen – Abrechnungszeiträume | Microsoft-Dokumentation"
description: "Erhalten Sie Informationen zu den Berichterstellungs-APIs, mit denen Azure-Unternehmenskunden Verbrauchsdaten programmgesteuert abrufen können."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: ab8d7fefb64b1358bb1b9667d280cb53fc2f636c
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="reporting-apis-for-enterprise-customers---billing-periods-preview"></a>Berichterstellungs-APIs für Unternehmenskunden – Abrechnungszeiträume (Vorschau)

Die API für Abrechnungszeiträume gibt eine Liste von Abrechnungszeiträumen zurück, die Verbrauchsdaten für die angegebene Registrierung in umgekehrter chronologischer Reihenfolge enthalten. Jeder Zeitraum enthält eine Eigenschaft, die auf die API-Route für die vier Sätze von Daten verweist: BalanceSummary, UsageDetails, Marketplace Charges und PriceSheet. Wenn der Zeitraum keine Daten enthält, ist die entsprechende Eigenschaft „null“. 


##<a name="request"></a>Anforderung 
Allgemeine Headereigenschaften, die hinzugefügt werden müssen, werden [hier](billing-enterprise-api.md) angegeben. 

|Methode | Anforderungs-URI|
|-|-|
|GET| https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/billingperiods|


## <a name="response"></a>Antwort
 
    
    
        [
            {
                  "billingPeriodId": "201704",
                  "billingStart": "2017-04-01T00:00:00Z",
                  "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                  "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                  "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                  "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },            
            ....
        ]
    

**Definitionen der Antworteigenschaft**

|Eigenschaftenname| Typ| Beschreibung
|-|-|-|
|billingPeriodId| string| Eindeutige ID, die einen bestimmten Abrechnungszeitraum darstellt|
|billingStart| datetime| ISO 8601-Zeichenfolge, die das Startdatum des Zeitraums darstellt|
|billingEnd| datetime| ISO 8601-Zeichenfolge, die das Enddatum des Zeitraums darstellt|
|balanceSummary| string| URL-Pfad, der zu den Bilanzzusammenfassungsdaten für diesen Zeitraum weiterleitet|
|usageDetails| string| URL-Pfad, der zu den Verwendungsdetaildaten für diesen Zeitraum weiterleitet|
|marketplaceCharges| string| URL-Pfad, der zu den Daten für die Marketplace-Gebühren für diesen Zeitraum weiterleitet|
|priceSheet| string| URL-Pfad, der zu den PriceSheet-Daten für diesen Zeitraum weiterleitet|

<br/>
## <a name="see-also"></a>Weitere Informationen
* [API für Bilanz und Zusammenfassung](billing-enterprise-api-balance-summary.md)

* [API für Verwendungsdetails](billing-enterprise-api-usage-detail.md) 

* [API für Marketplace Store-Gebühren](billing-enterprise-api-marketplace-storecharge.md) 

* [Preisblatt-API](billing-enterprise-api-pricesheet.md)
