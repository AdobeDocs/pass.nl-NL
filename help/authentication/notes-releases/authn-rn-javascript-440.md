---
title: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0
description: Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0 {#javascript-sdk-440-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Buildnummer {#build-number-440}

Adobe Pass-verificatie: JavaScript 4.4.0

Datum van de versie: **06/22/2021**

## Overzicht van release {#release-overview-440}

### Nieuwe functies

Preflight-autorisatie

* Nieuwe API voor voorafgaande autorisatie - dit is de nieuwe API-aanroep die moet worden gebruikt voor onze Preflight-autorisatiefunctie. Hiermee wordt ook een verbeterde foutmelding geïntroduceerd voor Preflight-flow.
* Deze functie is op verzoek beschikbaar omdat deze moet worden ingeschakeld in de configuratie voor primetime-verificatie. Neem contact op met uw TAM voor meer informatie over het inschakelen van deze functie.
* Hiermee wordt de checkPreauthorisedResources-API afgekeurd.

Platformidentificatie

* Voegt AP-SDK-Identifier kopbal op alle vraag van SDK toe om SDK type en versie beter te identificeren.

Overige

* Interne architectonische verbeteringen.

### Opgeloste problemen

* Verbeter rassenvoorwaarde die wordt geproduceerd wanneer setRequestor en getAuthentication gelijktijdig worden geroepen.
* Probleem verholpen waarbij rechten niet correct konden worden geladen in testomgevingen.
* Oplossing voor een probleem dat ervoor zorgde dat de logout op de achtergrond in Safari-browsers niet kon worden voltooid. De gebruiker leek dus nog steeds geverifieerd totdat een pagina vernieuwde. Er is een time-out geïntroduceerd, die momenteel is ingesteld op 30 seconden. Als er tijdens deze periode geen reactie is van de Primetime-verificatieserver, roept de SDK de callback setAuthenticationStatus aan.

## Geen pakket {#release-package-440}

De productie-URL is: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

De staging-URL is: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
