---
title: Voorkomen dat MVPD's worden weergegeven in het dialoogvenster Selectie
description: Voorkomen dat MVPD's worden weergegeven in het dialoogvenster Selectie
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# Voorkomen dat MVPD&#39;s worden weergegeven in het dialoogvenster Selectie

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Probleem {#issue-prevent-mvpd-sel-dialog}

U moet (&quot;blok-lijst&quot;) specifieke MVPDs verhinderen te verschijnen in de selecteur MVPD.


## Oplossing {#solution-prevent-mvpd-sel-dialog}

De oplossing is om een blokvermelding uit te voeren wanneer `displayProviderDialog()` wordt aangeroepen.

Bijvoorbeeld, als u CableCompany_1 en CableCompany_2 niet binnen de selecteur zou willen tonen MVPD, zou u iets doen als wat in het volgende voorbeeld wordt getoond.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
