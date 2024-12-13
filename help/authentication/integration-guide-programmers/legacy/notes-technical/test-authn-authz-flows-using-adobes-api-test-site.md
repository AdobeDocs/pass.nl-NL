---
title: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
description: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# (Verouderd) Procedure voor het testen van verificatie- en autorisatiestromen met de API-testsite van de Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Om de stromen te testen AuthN en AuthZ, hebben wij een **API testplaats** voorbereid die tot uw beschikking staat. Ons ondersteuningsteam zal u met alle plezier van uw aanmeldingsgegevens voorzien. U kunt ons in **support@tve.zendesk.com** contacteren.


## Deel I {#part-I}

Voor het testen tegen de RELEASE-omgeving gaat u rechtstreeks naar deel II.  Om in het pre-kwalificatiemilieu te testen, zie [ Vestiging Uw Milieu en het Testen in pre-Qual ](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Deel II

Voer de volgende stappen uit nadat u deel I hebt voltooid:


1. Open webpage: [ het Staging API test ](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Toegangsfunctie laden vanaf de volgende URL:
   * [ toegang laat javascript voor het opvoeren ](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js) toe.
   * OF
   * [ toegang toelaat javascript voor productie ](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * VERVOLGENS klik op &quot;**Toegelaten van de Toegang van de Lading**&quot;knoop.
1. Plaats nu de aanvrager identiteitskaart waarde aan &quot;**requestID**&quot;en klik op de &quot;setRequestor&quot;knoop.
1. Vervolgens drukt u op de knop &quot;getAuthentication&quot; en wacht u tot de weergavekiezer verschijnt.
1. Selecteer &quot;**MVPD**&quot;van de plukker.
1. Ga uw geloofsbrieven op de &quot;**MVPD**&quot;login pagina in.
1. Voer de stappen 1 tot en met 3 opnieuw uit nadat u de omleiding hebt uitgevoerd
1. Na het opnieuw uitvoeren van stap 3 op &quot;setAuthenticationStatus&quot;zou u de waarde &quot;1&quot;moeten zien. Als de verificatie niet werkt, wordt het dialoogvenster MVPD weergegeven.
1. Om vergunning te testen, op het gebied van de middelinput &quot;**requestID**&quot;ingaan en op de &quot;getAuthorization&quot;knoop klikken.
1. Dientengevolge, in &quot;setToken&quot; - \>&quot;middel identiteitskaart&quot;textbox zal de middel worden getoond en in &quot;setToken&quot;- \>&quot;symbolische&quot;textbox shortAuthorizationToken zal worden getoond betekenend dat authZ succesvol was.
1. Nu kunt u op de knop &quot;logout&quot; klikken om de tokens te verwijderen.
