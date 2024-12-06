---
title: Opmerkingen bij de release Adobe Pass Authentication 2.66
description: Opmerkingen bij de release Adobe Pass Authentication 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '154'
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

De Authentificatie van Adobe Pass: adobe-pas **2.66.0.1**
Datum van de versie: **07/11/2023 - 07/13/2023**

### Overzicht van release {#release-overview-266}

Met deze release zijn we doorgegaan met interne updates voor de nieuwe REST API.

#### bugs {#release-overview-bugfixes-266}

* Vaste de logout stroom voor SAML gebaseerde MVPDs, waar de parameter RelayState van het logout verzoek mist. Wij zullen configuratieupdates na de versie richten om de logout stroom voor beïnvloede MVPDs te herstellen.
* De mogelijkheid toegevoegd om SSL-certificaten in onze configuratie bij te werken voor SOAP eindpunten van machtigingen.
* Probleem verholpen waarbij in sommige ESM-rapporten onjuiste gegevens waren aangemeld in het veld Programmer.
