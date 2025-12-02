---
title: Opmerkingen bij de release Adobe Pass Authentication 3.5.0
description: Meer informatie over de nieuwe functies, wijzigingen en bekende problemen met deze release.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Opmerkingen bij de release Adobe Pass Authentication 3.5.0

Laatste update: Tue Dec 09 2025 00 :00: 00 GMT+0000 (Coordinated Universal Time)

* Onderwerpen:
* Verificatie

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-350}

* [Buildnummer](#build-number-350)
* [Overzicht van release](#release-overview-350)

### Buildnummer {#build-number-350}

De Authentificatie van Adobe Pass: adobe-pas **3.5.0**\
Datum van de versie: **12/09/2025 - 12/11/2025**

### Overzicht van release {#release-overview-350}

#### REST API v2

* Toegevoegd een nieuwe dimensie in ESM (&quot;api&quot;) om klanten te helpen rapporten bouwen die gebeurtenissen zouden onderscheiden die op het implementatietype worden gebaseerd: SDK, REST V1, of REST V2.

#### Opgeloste problemen

* Probleem verholpen waarbij aangepaste afbraakberichten die waren ingesteld in het TVE-dashboard, niet werden weergegeven in de foutdetails van de REST API V2.
* Probleem verholpen met REST API V2 waarbij `authenticated_profile_expired` -foutcode niet werd geretourneerd toen geverifieerde profielen waren verlopen.
* Probleem verholpen waarbij latentieberekeningen voor autorisatie en Preflight-TTL-waarden onjuist waren in REST API V2.
* Probleem verholpen waarbij de inconsistente antwoordindeling werd geretourneerd wanneer het DCR-token vervalt.
