---
title: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.0.0
description: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.0.0
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.0.0 {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Buildnummer {#build-no-javascript-sdk-400}

Adobe Pass-verificatie: JavaScript 4.0.0

Datum van de versie: **07/05/2018**


## Overzicht van release {#overview-javascript-sdk-400}

* Geïmplementeerde ondersteuning voor dynamisch clientregistratiemechanisme, een uniform beveiligingsmechanisme voor alle platforms. Het beheren van beveiligingstoegang op verschillende platforms kan via het TVE-dashboard, door de programmeur zelf, worden uitgevoerd.
* Elimineert het gebruik van alle cookies van derden en bijna alle cookies van de eerste partij voor alle functies (behalve individualisatie waarbij een cookie van de eerste partij nog steeds wordt gebruikt - in de toekomst wordt verwijderd), waardoor deze beter compatibel en toekomstbestendiger wordt met het opkomende browserbeleid voor cookies van andere leveranciers, of cookies in het algemeen (bijvoorbeeld Safari&#39;s ITP). Merk op dat de vorige 3.x versie zoals verwacht voor de gelukkige stromen door slechts 1st partijkoekjes te gebruiken werkt, maar dit zou met de versie van nieuwe browser versies kunnen veranderen.
* Ondersteuning voor het uitschakelen van caching voor Preflight-autorisatiecontroles.
* Deze versie is NIET compatibel met oudere versies en wordt gehost via een nieuw pad: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Om van deze nieuwe JS SDK te kunnen profiteren, is een migratieproces nodig van programmeurs om hun webtoepassingen te registreren met behulp van het nieuwe dynamische mechanisme voor clientregistratie en om naar de nieuwe JS SDK url te verwijzen.


## Geen pakket {#rel-pkg-javascript-sdk-400}

De productie-URL is: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

De staging-URL is: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
