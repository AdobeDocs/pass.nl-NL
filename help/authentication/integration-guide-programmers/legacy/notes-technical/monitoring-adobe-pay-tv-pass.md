---
title: Adobe Pass-verificatie controleren
description: Adobe Pass-verificatie controleren
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# (Verouderd) Adobe Pass-verificatie controleren {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#intro}

De klanten kunnen [ Nagios ](http://www.nagios.org) of andere hulpmiddelen gebruiken om te controleren of de Authentificatie van Adobe Pass omhoog of neer is.

## Eindpunten controleren {#monitoring-endpoints}

### Eindpunten die u kunt controleren {#endpoints-to-monitor}

* Het configuratieeindpunt voor alle platforms: `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]` - Deze is beschikbaar via HTTP of HTTPS (afhankelijk van de keuze die door de ontwikkelaar van de inhoudprovider is gemaakt). Als dit eindpunt mist betekent dat dat uw inhoud niet beschikbaar zal zijn over alle platforms en alle MVPDs. Voor de client REST API hebben we ook het volgende eindpunt: `https://api.auth.adobe.com/adobe-services/config your-config-ID]` .

* De volgende eindpunten maken deel uit van het Adobe Pass Authentication-web SDK.  Als deze ontbreekt, betekent dit dat pay-TV-pass voor alle programmeurs en alle wegeigenschappen is ingesteld:

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Eindpunten die u niet zou moeten controleren {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  U zult altijd een fout 503 krijgen, omdat dit eindpunt een reactie van MVPD SAML op het vereist.

* Overige eindpunten van Entitlement - `adobe-services/1.0/authenticate/` , `adobe-services/1.0/deviceShortAuthorize` , `adobe-services/1.0/authorize`

U kunt deze eindpunten niet controleren omdat zij een lading voor een relevant antwoord nodig hebben.
