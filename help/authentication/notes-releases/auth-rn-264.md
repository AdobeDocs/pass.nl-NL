---
title: Opmerkingen bij de release Adobe Pass Authentication 2.64
description: Opmerkingen bij de release Adobe Pass Authentication 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.64 {#authn-264-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-264}

* [Buildnummer](#build-number-264)
* [Overzicht van release](#release-overview-264)

### Buildnummer {#build-number-264}

De Authentificatie van Adobe Pass: adobe-pas **2.64**

Datum van de versie: **11/08/2022 - 11/10/2022**

### Overzicht van release {#release-overview-264}

* Infrastructuurupdates, gericht op het verminderen van de reactietijden van de server, die algemene prestaties van het systeem verbeteren.
* Verbeteringen van het nieuwe platform-identificatiemechanisme.
* Mogelijkheid om ongevraagde authentificatiereacties van MVPDs te blokkeren waar de &quot;in_response_to&quot;parameter van de bewering van SAML mist.

#### Opgeloste problemen

* Oplossing voor onjuiste opmaak van sommige verouderde TempPass-tokens.
* Minder belangrijke kwesties op de tweede stroom van de het schermauthentificatie werden gericht.
