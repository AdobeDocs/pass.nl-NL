---
title: Overzicht van dynamische clientregistratie
description: Overzicht van dynamische clientregistratie
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# Overzicht van dynamische clientregistratie {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

De dynamische cliëntregistratie vertegenwoordigt een vergunningsmechanisme dat door [&#x200B; wordt bepaald RFC 7591 &#x200B;](https://datatracker.ietf.org/doc/html/rfc7591), en het is gebaseerd op het OAuth 2.0 vergunningskader dat door [&#x200B; RFC 6749 &#x200B;](https://datatracker.ietf.org/doc/html/rfc6749) wordt beschreven.

Adobe Pass biedt een dynamische service voor clientregistratie die toegang biedt tot de volgende beveiligde API&#39;s:

* Adobe Pass Authentication Management API&#39;s:
   * [Tijdcontrole-API opnieuw instellen](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [Verslechterings-API](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
   * [Proxy MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [Entitlement Service Monitoring API](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)
* Adobe Pass-verificatie REST-API&#39;s:
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(Verouderd) REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* SDK&#39;s voor Adobe Pass-verificatie:
   * [(Verouderd) JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [(Verouderd) iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [(Verouderd) Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [(Verouderd) FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Het dynamische mechanisme voor clientregistratie vervangt de oudere Adobe Pass-verificatieoplossingen die moeten worden stopgezet:
>
> * Het mechanisme voor de ondertekende aanvraag-id.
> * Het mechanisme voor domeinlijsten.
> * Het API-sleutelmechanisme.

Met de invoering van dynamische clientregistratie zijn de belangrijkste voordelen:

* Uitgebreide beveiliging.
* Verenigd model op verschillende platforms.
* Fijne controle over de levenscyclus van uw toepassing.

Raadpleeg de volgende secties voor meer informatie over het beheren en gebruiken van dynamische clientregistratie.

## Dynamisch clientregistratiebeheer {#dynamic-client-registration-management}

Het dynamische proces van het cliëntregistratiebeheer staat cliënttoepassingen toe die op specifieke platforms lopen en toegang tot specifieke de Authentificatie APIs van Adobe Pass moeten registreren door het [&#x200B; Dashboard van Adobe Pass TVE &#x200B;](https://experience.adobe.com/#/pass/authentication).

Het Adobe Pass TVE-dashboard is een hulpmiddel voor Adobe Pass Authentication-klanten (Programmers) om hun configuratie en gegevens te beheren. Dit zelfbedienings dashboard laat een waaier van functionaliteit toe die in de [&#x200B; documentatie van de Gids van de Gebruiker van het Dashboard van Adobe Pass TVE &#x200B;](../../../user-guide-tve-dashboard/tve-dashboard-overview.md) wordt beschreven.

Voor het geval u toegang tot het [&#x200B; Adobe Pass TVE Dashboard &#x200B;](https://experience.adobe.com/#/pass/authentication) hebt, volg de stappen in de hieronder secties om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

### Geregistreerde toepassingen beheren {#manage-registered-applications}

>[!IMPORTANT]
>
> Voor het geval u geen toegang tot het Dashboard van Adobe Pass TVE hebt, creeer een kaartje door onze [&#x200B; Zendesk &#x200B;](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing tot stand te brengen en de softwareverklaring met u te delen.

Er zijn twee manieren waarop u een geregistreerde toepassing kunt maken:

* **het niveau van de Programmer**

  Met het registratieproces op programmeerniveau kunt u een geregistreerde toepassing maken die is gekoppeld aan alle beschikbare kanalen of een geselecteerde subset kanalen. Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van het Dashboard van TVE voor de documentatie van Programmers &#x200B;](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).


* **niveau van het Kanaal**

  Met het registratieproces op kanaalniveau kunt u een geregistreerde toepassing maken die alleen aan het geselecteerde kanaal is gekoppeld. Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van het Dashboard van TVE voor de documentatie van Kanalen &#x200B;](../../../user-guide-tve-dashboard/tve-dashboard-channels.md).

>[!IMPORTANT]
>
> Het wordt aanbevolen geregistreerde toepassingen te maken met specifiekere en beperkte machtigingen om de beveiliging te verbeteren en ongeoorloofde toegang te voorkomen. Wanneer u geregistreerde toepassingen maakt, kunt u daarom overwegen om striktere opties te gebruiken voor de toegewezen `channels` , `platforms` en `scopes` .
>
> Het wordt aanbevolen een nieuwe geregistreerde toepassing te maken voor elke belangrijke update van uw clienttoepassing om de levenscyclus en het gebruik ervan te beheren. Indien noodzakelijk, creeer een kaartje door onze [&#x200B; Zendesk &#x200B;](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing te herroepen om de functionaliteit van een specifieke versie van de cliënttoepassing te blokkeren.

### Softwareinstructies beheren {#manage-software-statements}

>[!IMPORTANT]
>
> Voor het geval u geen toegang tot het Dashboard van Adobe Pass TVE hebt, creeer een kaartje door onze [&#x200B; Zendesk &#x200B;](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing tot stand te brengen en de softwareverklaring met u te delen.

Alvorens een softwareverklaring te downloaden, zorg ervoor u een geregistreerde toepassing hebt die zoals in [&#x200B; wordt beschreven leidt geregistreerde toepassingen &#x200B;](#manage-registered-applications) sectie die aan uw vereisten van de cliënttoepassing voldoet.

Er zijn twee beschikbare manieren u een softwareverklaring kunt downloaden die op het niveau wordt gebaseerd waar de geregistreerde toepassing werd gecreeerd:

* **het niveau van de Programmer**

  Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van het Dashboard van TVE voor de documentatie van Programmers &#x200B;](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).

* **niveau van het Kanaal**

  Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van het Dashboard van TVE voor de documentatie van Kanalen &#x200B;](../../../user-guide-tve-dashboard/tve-dashboard-channels.md).

De softwareverklaring is een Token van het Web JSON (`JWT`) dat informatie over uw software van de cliënttoepassing als bundel bevat. Wanneer voorgesteld aan [&#x200B; wint cliëntgeloofsbrieven &#x200B;](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API terug, wordt de softwareverklaring digitaal ondertekend gebruikend de Handtekening van het Web JSON (`JWS`).

Voor meer gedetailleerde verklaring over welke softwareverklaringen zijn en hoe zij werken, verwijs naar [&#x200B; RFC 7591 &#x200B;](https://tools.ietf.org/html/rfc7591) documentatie.

## Dynamic Client Registration Flow {#dynamic-client-registration-flow}

Samengevat omvat het dynamische mechanisme voor clientregistratie verschillende stappen:

**Beheer**

* Een cliëntvertegenwoordiger moet een geregistreerde toepassing tot stand brengen zoals die in [&#x200B; wordt beschreven leidt geregistreerde toepassingen &#x200B;](#manage-registered-applications) sectie.
* Een cliëntvertegenwoordiger moet een softwareverklaring downloaden en inbedden zoals die in [&#x200B; wordt beschreven leidt softwareverklaringen &#x200B;](#manage-software-statements) sectie.

**Stroom**

* De cliënttoepassing moet de cliëntgeloofsbrieven verkrijgen zoals die in [&#x200B; worden beschreven terugwinnen cliëntgeloofsbrieven &#x200B;](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
* De cliënttoepassing moet het toegangstoken zoals die in [&#x200B; wordt beschreven verkrijgen toegangstoken &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.

Verwijs naar de [&#x200B; Dynamische documentatie van de Stroom van de Registratie van de Cliënt &#x200B;](flows/dynamic-client-registration-flow.md) om beter te begrijpen hoe te om tot Adobe Pass beschermde APIs toegang te hebben. Voorts kunt u deze [&#x200B; webinar &#x200B;](https://my.adobeconnect.com/pzkp8ujrigg1/) opname ook letten, die meer context verstrekt en een demo omvat.
