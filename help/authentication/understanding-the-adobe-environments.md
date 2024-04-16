---
title: De Adobe omgevingen begrijpen
description: De Adobe omgevingen begrijpen
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# De Adobe omgevingen begrijpen {#understanding-the-adobe-environments}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De officiële documentatie die de omgeving van de Adobe beschrijft, is beschikbaar [Uw omgeving instellen en testen op basis van pre-kwaliteit](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

De omgeving van de Adobe, die in een paar woorden wordt samengevat:

Adobe heeft twee omgevingen: **Voorkwalificatie** en **Geen**.

* In de pre-kwalificatieomgeving bereiden we de nieuwe build voor die vrijkomt.

* De huidige versie bouwt voort op de milieu van de Versie.

Elke omgeving heeft twee profielen: **fasering** en **productie**.

* Het het opvoeren profiel verbindt met de MVPD het opvoeren server
* Het productieprofiel maakt verbinding met het MVPD-productieprofiel.

De reden voor het hebben van de twee profielen is dat op het het opvoeren profiel wij nieuwe partners voorbereiden om te leven, en zij zouden het systeem met of de volgende bouwstijl (pre-Kwalificatie) of met versie willen testen (stabieler).

Als een partner de nieuwe versie wil testen, zijn er een paar extra stappen die moeten worden gedaan. Zie, [Uw omgeving instellen en testen op basis van pre-kwaliteit](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

Door de bovenstaande stappen te volgen, is gegarandeerd dat de komende release zal worden getest in de pre-kwalificatieomgeving.
