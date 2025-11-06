---
title: Opmerkingen bij de release Adobe Pass Authentication 3.2.0
description: Opmerkingen bij de release Adobe Pass Authentication 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-320}

* [Buildnummer](#build-number-320)
* [Overzicht van release](#release-overview-320)

### Buildnummer {#build-number-320}

De Authentificatie van Adobe Pass: adobe-pas **3.2.0**

Datum van de versie: **06/10/2025 - 06/12/2025**

### Overzicht van release {#release-overview-320}

#### REST API v2

* Een nieuwe reden `missing_parameters_fallback` is toegevoegd voor gevallen wanneer de parameters op [&#x200B; Sessies API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) reactie missen.
* Een nieuw gebied &quot;apparaat&quot;is toegevoegd aan [&#x200B; Sessies API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) reactie.

#### Nieuwe functies

* Breid Afbraak API uit om het vermogen toe te voegen om afbraakregels voor veelvoudige MVPDs in één enkele vraag toe te passen

#### Opgeloste problemen

* Probleem verholpen waarbij de verificatiestroom niet kon worden voltooid in gevallen waarin de verwerking van metagegevens van de gebruiker is mislukt.
* Probleem verholpen waarbij de reden van de autorisatie niet correct werd berekend.
* Probleem verholpen waarbij de upstreamUser-id niet aanwezig was in de profielreactie.
* Probleem verholpen waarbij het verificatieverzoek ertoe leidde dat geen bereikinformatie voor REST API V2 werd opgenomen, leidde tot verificatieverzoeken.
