---
title: Opmerkingen bij de release Adobe Pass Authentication 2.66
description: Opmerkingen bij de release Adobe Pass Authentication 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.66 {#authn-266-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-266}

* [Buildnummer](#build-number-266)
* [Overzicht van release](#release-overview-266)

### Buildnummer {#build-number-266}

Adobe Pass-verificatie: adobe-pass-**2.66.0.1**

Datum van de versie: **07/11/2023 - 07/13/2023**

### Overzicht van release {#release-overview-266}

Met deze release zijn we doorgegaan met interne updates voor de nieuwe REST API.

#### Opgeloste problemen

* Vaste de logout stroom voor SAML gebaseerde MVPDs, waar de parameter RelayState van het logout verzoek mist. Wij zullen configuratieupdates na de versie richten om de logout stroom voor beïnvloede MVPDs te herstellen.
* De mogelijkheid om SSL-certificaten bij te werken in onze configuratie voor SOAP-autorisatieeindpunten is toegevoegd.
* Probleem verholpen waarbij in sommige ESM-rapporten onjuiste gegevens waren aangemeld in het veld Programmer.
