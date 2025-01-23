---
title: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
description: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 65475d6da7a1b25cb2d8ebd6229a7cb360c7ab4a
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# (Verouderd) Procedure voor het testen van verificatie- en autorisatiestromen met de API-testsite van de Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Om de stromen te testen AuthN en AuthZ, hebben wij een **API testplaats** voorbereid die tot uw beschikking staat. Ons ondersteuningsteam zal u met alle plezier van uw aanmeldingsgegevens voorzien. U kunt ons in **tve-support@adobe.com** contacteren.


## Deel I {#part-I}

Voor het testen tegen de RELEASE-omgeving gaat u rechtstreeks naar deel II.  Om in het pre-kwalificatiemilieu te testen, zie [ Vestiging Uw Milieu en het Testen in pre-Qual ](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Deel II

Voer de volgende stappen uit nadat u deel I hebt voltooid:


1. Open webpage: [ het Staging API test ](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Toegangsfunctie laden via:
   * In het vervolgkeuzemenu kiezen waar u het wilt openen (opmaken of maken) en in de foutopsporingsmodus kiezen
   * Het ingaan van de softwareverklaring waarmee u wilt testen
   * Dan klikkend op &quot;**Toegelaten van de Toegang van de Lading**&quot;knoop.
1. Plaats nu de aanvrager identiteitskaart waarde aan &quot;**requestID**&quot;en klik op de &quot;setRequestor&quot;knoop.
1. Vervolgens drukt u op de knop &quot;getAuthentication&quot; en wacht u tot de weergavekiezer verschijnt.
1. Selecteer &quot;**MVPD**&quot;van de plukker.
1. Ga uw geloofsbrieven op de &quot;**MVPD**&quot;login pagina in.
1. Voer de stappen 1 tot en met 3 opnieuw uit nadat u de omleiding hebt uitgevoerd
1. Na het opnieuw uitvoeren van stap 3 op &quot;setAuthenticationStatus&quot;zou u de waarde &quot;1&quot;moeten zien. Als de verificatie niet werkt, wordt het dialoogvenster MVPD weergegeven.
1. Om vergunning, op het inputgebied rechts van de knopen te testen die als &quot;checkAuthorization&quot;en &quot;getAuthorization&quot;worden geëtiketteerd ga het **middel** in u waarvoor u wenst om te machtigen en op de &quot;getAuthorization&quot;knoop te klikken.
1. Dientengevolge, in &quot;setToken&quot; - \>&quot;middel identiteitskaart&quot;textbox zal de middel worden getoond en in &quot;setToken&quot;- \>&quot;symbolische&quot;textbox shortAuthorizationToken zal worden getoond betekenend dat authZ succesvol was.
1. Nu kunt u op de knop &quot;logout&quot; klikken om de tokens te verwijderen.
