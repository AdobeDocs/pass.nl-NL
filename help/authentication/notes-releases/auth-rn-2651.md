---
title: Opmerkingen bij de release Adobe Pass Authentication 2.65.1
description: Opmerkingen bij de release Adobe Pass Authentication 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.65.1 {#authn-2651-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-2651}

* [Buildnummer](#build-number-2651)
* [Overzicht van release](#release-overview-2651)

### Buildnummer {#build-number-2651}

De Authentificatie van Adobe Pass: adobe-pas **2.65.1**
Datum van de versie: **06/20/2023 - 06/22/2023**

### Overzicht van release {#release-overview-2651}

Deze release voegt het volgende toe:

Vanaf deze release introduceren we technische ondersteuning voor het toevoegen van regels voor snelheidsbeperking voor al onze API&#39;s om het ecosysteem in zijn geheel te beschermen tegen onjuist gebruik.

Het eerste doel is het toezicht op het gedrag van toepassingen om de juiste grenswaarden vast te stellen en er zullen geen limieten worden toegepast als onderdeel van deze release. Als het verkeer dat door een specifieke toepassing wordt gegenereerd bepaalde limieten overschrijdt, werken we met elke klant samen om de implementatie te valideren.

De het beperken van het tarief regels zullen later dit jaar worden toegepast, nadat wij verkeer bevestigen dat van alle toepassingen van de klant wordt geproduceerd.
