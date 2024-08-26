---
title: Overzicht van dynamische clientregistratie
description: Overzicht van dynamische clientregistratie
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 0%

---


# Overzicht van dynamische clientregistratie {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De dynamische cliëntregistratie vertegenwoordigt een vergunningsmechanisme dat door [ wordt bepaald RFC 7591 ](https://datatracker.ietf.org/doc/html/rfc7591), en het is gebaseerd op het OAuth 2.0 vergunningskader dat door [ RFC 6749 ](https://datatracker.ietf.org/doc/html/rfc6749) wordt beschreven.

Adobe Pass biedt een dynamische service voor clientregistratie die toegang biedt tot de volgende beveiligde API&#39;s:

* Adobe Pass Authentication Management API&#39;s:
   * [Tijdcontrole-API opnieuw instellen](../reset-temp-pass.md)
   * [Verslechterings-API](../degradation-api-overview.md)
   * [Proxy MVPD API](../proxy-mvpd-webserv.md)
   * [Entitlement Service Monitoring API](../entitlement-service-monitoring-api.md)
* Adobe Pass-verificatie REST-API&#39;s:
   * [REST API V1](../rest-api-reference.md)
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* SDK&#39;s voor Adobe Pass-verificatie:
   * [JAVASCRIPT SDK](../javascript-sdk-api-reference.md)
   * [iOS/tvOS SDK](../iostvos-sdk-api-reference.md)
   * [ANDROID SDK](../android-sdk-api-reference.md)
   * [FireOS SDK](../amazon-fireos-native-client-api-reference.md)

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

Het dynamische proces van het cliëntregistratiebeheer staat cliënttoepassingen toe die op specifieke platforms lopen en toegang tot specifieke de Authentificatie APIs van Adobe Pass moeten registreren door het [ Dashboard van Adobe Pass TVE ](https://console.auth.adobe.com/).

Het Adobe Pass TVE-dashboard is een hulpmiddel voor Adobe Pass Authentication-klanten (Programmers) om hun configuratie en gegevens te beheren. Dit zelfbedienings dashboard laat een waaier van functionaliteit toe die in de [ documentatie van de Gids van de Gebruiker van het Dashboard van Adobe Pass TVE ](../tve-dashboard-user-guide.md) wordt beschreven.

Voor het geval u toegang tot het [ Adobe Pass TVE Dashboard ](https://console.auth.adobe.com/) hebt, volg de stappen in de hieronder secties om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

### Geregistreerde toepassingen beheren {#manage-registered-applications}

>[!IMPORTANT]
>
> Voor het geval u geen toegang tot het Dashboard van Adobe Pass TVE hebt, creeer een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing tot stand te brengen en de softwareverklaring met u te delen.

Er zijn twee manieren waarop u een geregistreerde toepassing kunt maken:

* **het niveau van de Programmer**

  Met het registratieproces op programmeerniveau kunt u een geregistreerde toepassing maken die is gekoppeld aan alle beschikbare kanalen of een geselecteerde subset kanalen. Voor meer details, verwijs naar [ creeer een geregistreerde toepassing op programmeerniveau ](../tve-dashboard-user-guide.md#create-registered-application-programmer-level) sectie van de [ Gids van de Gebruiker van het Dashboard van de TVE ](../tve-dashboard-user-guide.md) documentatie.


* **niveau van het Kanaal**

  Met het registratieproces op kanaalniveau kunt u een geregistreerde toepassing maken die alleen aan het geselecteerde kanaal is gekoppeld. Voor meer details, verwijs naar [ creeer een geregistreerde toepassing op kanaalniveau ](../tve-dashboard-user-guide.md#create-registered-application-channel-level) sectie van de [ Gids van de Gebruiker van het Dashboard van TVE ](../tve-dashboard-user-guide.md) documentatie.

>[!IMPORTANT]
>
> Het wordt aanbevolen geregistreerde toepassingen te maken met specifiekere en beperkte machtigingen om de beveiliging te verbeteren en ongeoorloofde toegang te voorkomen. Wanneer u geregistreerde toepassingen maakt, kunt u daarom overwegen om striktere opties te gebruiken voor de toegewezen `channels` , `platforms` en `scopes` .
>
> Het wordt aanbevolen een nieuwe geregistreerde toepassing te maken voor elke belangrijke update van uw clienttoepassing om de levenscyclus en het gebruik ervan te beheren. Indien nodig, creeer een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing te herroepen om de functionaliteit van een specifieke versie van de cliënttoepassing te blokkeren.

### Softwareinstructies beheren {#manage-software-statements}

>[!IMPORTANT]
>
> Voor het geval u geen toegang tot het Dashboard van Adobe Pass TVE hebt, creeer een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om een geregistreerde toepassing tot stand te brengen en de softwareverklaring met u te delen.

Alvorens een softwareverklaring te downloaden, zorg ervoor u een geregistreerde toepassing hebt die zoals in [ wordt beschreven leidt geregistreerde toepassingen ](#manage-registered-applications) sectie die aan uw vereisten van de cliënttoepassing voldoet.

Er zijn twee beschikbare manieren u een softwareverklaring kunt downloaden die op het niveau wordt gebaseerd waar de geregistreerde toepassing werd gecreeerd:

* **het niveau van de Programmer**

  Voor meer details, verwijs naar [ Download een softwareverklaring op programmeerniveau ](../tve-dashboard-user-guide.md#download-software-statement-programmer-level) sectie van de [ Gids van de Gebruiker van het Dashboard van de TVE ](../tve-dashboard-user-guide.md) documentatie.

* **niveau van het Kanaal**

  Voor meer details, verwijs naar [ Download een softwareverklaring op kanaalniveau ](../tve-dashboard-user-guide.md#download-software-statement-channel-level) sectie van de [ Gids van de Gebruiker van het Dashboard van de TVE ](../tve-dashboard-user-guide.md) documentatie.

De softwareverklaring is een Token van het Web JSON (`JWT`) dat informatie over uw software van de cliënttoepassing als bundel bevat. Wanneer voorgesteld aan [ wint cliëntgeloofsbrieven ](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API terug, wordt de softwareverklaring digitaal ondertekend gebruikend de Handtekening van het Web JSON (`JWS`).

Voor meer gedetailleerde verklaring over welke softwareverklaringen zijn en hoe zij werken, verwijs naar [ RFC 7591 ](https://tools.ietf.org/html/rfc7591) documentatie.

## Dynamic Client Registration Flow  {#dynamic-client-registration-flow}

Samengevat omvat het dynamische mechanisme voor clientregistratie verschillende stappen:

**Beheer**

* Een cliëntvertegenwoordiger moet een geregistreerde toepassing tot stand brengen zoals die in [ wordt beschreven leidt geregistreerde toepassingen ](#manage-registered-applications) sectie.
* Een cliëntvertegenwoordiger moet een softwareverklaring downloaden en inbedden zoals die in [ wordt beschreven leidt softwareverklaringen ](#manage-software-statements) sectie.

**Stroom**

* De cliënttoepassing moet de cliëntgeloofsbrieven verkrijgen zoals die in [ worden beschreven terugwinnen cliëntgeloofsbrieven ](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
* De cliënttoepassing moet het toegangstoken zoals die in [ wordt beschreven verkrijgen toegangstoken ](./apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.

Verwijs naar de [ Dynamische documentatie van de Stroom van de Registratie van de Cliënt ](./flows/dynamic-client-registration-flow.md) om beter te begrijpen hoe te om tot Adobe Pass beschermde APIs toegang te hebben. Voorts kunt u deze [ webinar ](https://my.adobeconnect.com/pzkp8ujrigg1/) opname ook letten, die meer context verstrekt en een demo omvat.
