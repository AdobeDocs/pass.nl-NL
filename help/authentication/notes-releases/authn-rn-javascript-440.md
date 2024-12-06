---
title: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0
description: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0 {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Buildnummer {#build-no-javascript-sdk-440}

Adobe Pass-verificatie: JavaScript 4.4.0

Datum van de versie: **06/22/2021**


## Overzicht van release {#overview-javascript-sdk-440}

### Nieuwe functies {#new-features-javascript-sdk-440}

Preflight-autorisatie

* Nieuwe API voor voorafgaande autorisatie - dit is de nieuwe API-aanroep die moet worden gebruikt voor onze Preflight-autorisatiefunctie. Hiermee wordt ook een verbeterde foutmelding geïntroduceerd voor Preflight-flow.
* Deze functie is op verzoek beschikbaar omdat deze moet worden ingeschakeld in de configuratie voor primetime-verificatie. Neem contact op met uw TAM voor meer informatie over het inschakelen van deze functie.
* Hiermee wordt de checkPreauthorisedResources-API afgekeurd.

Platformidentificatie

* Voegt AP-SDK-Identifier kopbal op alle vraag van SDK toe om het type en de versie van SDK beter te identificeren.

Overige

* Interne architectonische verbeteringen.


### Opgeloste problemen {#bug-fixes-javascript-sdk-440}

* Verbeter rassenvoorwaarde die wordt geproduceerd wanneer setRequestor en getAuthentication gelijktijdig worden geroepen.
* Probleem verholpen waarbij rechten niet correct konden worden geladen in testomgevingen.
* Oplossing voor een probleem dat ervoor zorgde dat de logout op de achtergrond in Safari-browsers niet kon worden voltooid. De gebruiker leek dus nog steeds geverifieerd totdat een pagina vernieuwde. Een onderbreking werd geïntroduceerd, momenteel geplaatst bij 30 seconden, zodat als er geen reactie van de server van de Authentificatie Primetime tijdens deze periode is, zal SDK setAuthenticationStatus callback aanhalen.

## Geen pakket {#rel-pkg-javascript-sdk-440}

De productie-URL is: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

De staging-URL is: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
