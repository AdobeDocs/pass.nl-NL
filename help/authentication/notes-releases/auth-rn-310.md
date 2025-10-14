---
title: Opmerkingen bij de release Adobe Pass Authentication 3.1.0
description: Opmerkingen bij de release Adobe Pass Authentication 3.1.0
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-310}

* [Buildnummer](#build-number-310)
* [Overzicht van release](#release-overview-310)

### Buildnummer {#build-number-310}

De Authentificatie van Adobe Pass: adobe-pas **3.1.0**

Datum van de versie: **02/25/2025 - 27-02-2025**

### Overzicht van release {#release-overview-310}

#### REST API v2

* Nieuwe `partner_logout` actienaam en `partner_interactive` actietype om tussen regelmatige logout en partner enige sign-on logout in REST API v2 [&#x200B; te onderscheiden Logout API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) reactie.
* Nieuw `reason` gebied om meer inzichten in de actienaam in REST API v2 [&#x200B; Sessies API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) en [&#x200B; Sessies SSO API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) reacties te verstrekken.

#### Opgeloste problemen

* Oplossing een kwestie die Spectrumabonnees verhinderde om door REST API v2 [&#x200B; voor authentiek te verklaren API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* Vaste een kwestie die gebeurtenissen die via REST API V2 [&#x200B; worden geproduceerd voor authentiek verklaart API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) werden behoorlijk samengevoegd in ESM.
* Oplossing van een kwestie die verkeerde berekening van `notBefore` timestamp voor de gebruikersprofielen in REST API v2 [&#x200B; Profiles API &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) reactie veroorzaakte.

#### JavaScript SDK

* Verwijderd versie 3.5.0 van [&#x200B; AccessEnabler JavaScript SDK &#x200B;](authn-rn-javascript-471.md).