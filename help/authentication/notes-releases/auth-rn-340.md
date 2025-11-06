---
title: Opmerkingen bij de release Adobe Pass Authentication 3.4.0
description: Opmerkingen bij de release Adobe Pass Authentication 3.4.0
exl-id: ad572617-f607-419d-a085-70c025465080
source-git-commit: c9958a17ad9dfb518bab1d24087c85fdcb6fd057
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-340}

* [Buildnummer](#build-number-340)
* [Overzicht van release](#release-overview-340)

### Buildnummer {#build-number-340}

De Authentificatie van Adobe Pass: adobe-pas **3.4.0**
Datum van de versie: **09/16/2025 - 09/18/2025**

### Overzicht van release {#release-overview-340}

#### REST API v2

* Toegevoegde steun voor [ identiteitskaart van Experience Cloud (ECID) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) om gebruikersidentificatie en het volgen mogelijkheden te verbeteren.
* De veranderde kopballen AP-Apparaat-Herkenningsteken en x-Apparaat-Info aan facultatief voor REST API V2 [ Configuratie API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Opgeloste problemen

* Probleem verholpen waar identiteitskaart van MVPD en identiteitskaart van de Volmacht MVPD in het teken van REST API V2 [ de reactie van Besluiten ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) werd omgekeerd.
* Oplossing voor een kwestie waar verzoekers identiteitskaart niet aanwezig in het teken van REST API V2 [ de reactie van Besluiten ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) was.
* Vaste een kwestie waar het overgaan te veel middelen aan REST API V2 [ vooraf goedkeurde Besluiten ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API in een lege besluitenlijst in de reactie in plaats van de [ Verbeterde Code van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **to_many_resources** resulteerde.

#### Dic

* Problemen met patchbeveiliging.
