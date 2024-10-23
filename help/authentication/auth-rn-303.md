---
title: Opmerkingen bij de release Adobe Pass Authentication 3.0.3
description: Opmerkingen bij de release Adobe Pass Authentication 3.0.3
source-git-commit: 2f5e511f774e1a2d8b8b60084844edfe27be6c76
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-303}

* [Buildnummer](#build-number-303)
* [Overzicht van release](#release-overview-303)

### Buildnummer {#build-number-2651}

De Authentificatie van Adobe Pass: adobe-pas **3.0.3**
Datum van de versie: **10/29/2024 - 10/31/2024**

### Nieuwe functies {#new-features-303}

#### REST API v2 {#rest-apis}

##### Code

* REST API V2 verhogingen (aangezien Adobe Pass 3.0 belangrijke versie leverde [ REST API V2 ](./rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* `notBefore` en `notAfter` velden op `/sessions/{code}` toegevoegd om informatie over de geldigheid van verificatiecode te retourneren.
* Verbeterde platformidentificatie.

##### Documentatie

* Om met nieuw REST API v2 te beginnen, verwijs naar het [ REST API v2 Overzicht ](./rest-api-v2/rest-api-v2-overview.md) document.

##### Gereedschappen

* Om nieuwe REST API v2 te proberen, verwijs naar de nieuwe pagina van de Authentificatie van Adobe Pass van [ Adobe Developer ](https://developer.adobe.com/adobe-pass) website.
