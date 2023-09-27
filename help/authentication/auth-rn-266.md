---
title: Opmerkingen bij de release Adobe Pass Authentication 2.66
description: Opmerkingen bij de release Adobe Pass Authentication 2.66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.66 {#authn-266-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-266}

* [Buildnummer](#build-number-266)
* [Overzicht van release](#release-overview-266)

### Buildnummer {#build-number-266}

Adobe Pass-verificatie: adobe-pass-**2 66 0,1**
Releasedatum: **11-07-2023 - 13-07-2023**

### Overzicht van release {#release-overview-266}

Met deze release zijn we doorgegaan met interne updates voor de nieuwe REST API.

#### bugs {#release-overview-bugfixes-266}

* Vaste de logout stroom voor SAML gebaseerde MVPDs, waar de parameter RelayState van het logout verzoek mist. Wij zullen configuratieupdates na de versie richten om de logout stroom voor beïnvloede MVPDs te herstellen.
* De mogelijkheid toegevoegd om SSL-certificaten bij te werken in onze configuratie voor SOAP-autorisatieeindpunten.
* Probleem verholpen waarbij in sommige ESM-rapporten onjuiste gegevens waren aangemeld in het veld Programmer.
