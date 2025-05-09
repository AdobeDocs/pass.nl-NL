---
title: MVPD's toestaan in het dialoogvenster Selectie
description: MVPD's toestaan in het dialoogvenster Selectie
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# (Verouderd) MVPD&#39;s toestaan in het dialoogvenster Selectie {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Probleem {#issue}

De programmeur wil de gebruikerservaring van nieuwe MVPD-integraties mogelijk testen of controleren voordat hij naar eindgebruikers gaat.

## Oplossing {#solution}

In `displayProviderDialog()` callback, keert de Authentificatie van Adobe Pass alle MVPDs terug die met geselecteerde Programmer (identiteitskaart van de Aanvrager) wordt geïntegreerd. Maar de programmeur kan een filter op de terugkeerserie van MVPDs toepassen en slechts die tonen die in beide lijsten zijn.

## Voorbeeld {#example}

Dit voorbeeld toont aan hoe te om slechts CableCompany_1 en CableCompany_2 binnen de de selecteurdialoog van MVPD te tonen, en niet CableCompany_NewIntegration te tonen.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```
