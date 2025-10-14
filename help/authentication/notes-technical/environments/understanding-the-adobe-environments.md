---
title: De Adobe omgevingen begrijpen
description: De Adobe omgevingen begrijpen
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# De Adobe omgevingen begrijpen {#understanding-the-adobe-environments}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De officiële documentatie die de milieu&#39;s van de Adobe beschrijft is beschikbaar [&#x200B; Vestiging uw milieu en het testen in Prequal &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md):

De omgeving van de Adobe, die in een paar woorden wordt samengevat:

Adobe heeft twee milieu&#39;s: **pre-Kwalificatie** en **Versie**.

* In de pre-kwalificatieomgeving bereiden we de nieuwe build voor die vrijkomt.

* De huidige versie bouwt voort op de milieu van de Versie.

Elk milieu heeft twee profielen: **het opvoeren** en **productie**.

* Het het opvoeren profiel verbindt met de MVPD het opvoeren server
* Het productieprofiel maakt verbinding met het MVPD-productieprofiel.

De reden voor het hebben van de twee profielen is dat op het het opvoeren profiel wij nieuwe partners voorbereiden om te leven, en zij zouden het systeem met of de volgende bouwstijl (pre-Kwalificatie) of met versie willen testen (stabieler).

Als een partner de nieuwe versie wil testen, zijn er een paar extra stappen die moeten worden gedaan. Zie, [&#x200B; Vestiging uw milieu en het testen in Pre-qual &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

Door de bovenstaande stappen te volgen, is gegarandeerd dat de komende release zal worden getest in de pre-kwalificatieomgeving.
