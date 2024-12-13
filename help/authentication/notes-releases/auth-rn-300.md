---
title: Opmerkingen bij de release Adobe Pass Authentication 3.0
description: Opmerkingen bij de release Adobe Pass Authentication 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-300}

* [Buildnummer](#build-number-300)
* [Overzicht van release](#release-overview-300)

### Buildnummer {#build-number-2651}

De Authentificatie van Adobe Pass: adobe-pas **3.0**
Datum van de versie: **09/10/2024 - 09/12/2024**

### Nieuwe functies {#new-features-300}

#### REST API v2 {#rest-apis}

##### Code

* Beginnend met adobe-pas **3.0** versie, kunnen de nieuwe en bestaande cliënttoepassingen integreren of aan nieuwe REST API v2 migreren om van de recentste functies van Adobe Pass te profiteren.

##### Documentatie

* Raadpleeg de volgende documenten om te beginnen met de nieuwe REST API v2:
   * [REST API v2 - API&#39;s - Overzicht](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Stromen - Overzicht](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* De URL&#39;s voor openbare documenten van REST API v1 zijn gewijzigd. Raadpleeg de volgende documenten:
   * [REST API v1 - API&#39;s - Overzicht](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - API&#39;s - Referentie](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Gereedschappen

* Om nieuwe REST API v2 te proberen, verwijs naar de nieuwe pagina van de Authentificatie van Adobe Pass van [ Adobe Developer ](https://developer.adobe.com/adobe-pass) website.

### Opgeloste problemen {#bug-fixes-300}

* Probleem verholpen waarbij URL-parameter omleiden niet werd gebruikt wanneer deze aanwezig was in de logout-aanvraag.
