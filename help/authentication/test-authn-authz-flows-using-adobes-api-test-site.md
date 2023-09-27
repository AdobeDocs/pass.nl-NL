---
title: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
description: Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Om de AuthN en AuthZ stromen te testen, hebben wij voorbereid een **API-testsite** die u ter beschikking staat. Ons ondersteuningsteam zal u met alle plezier van uw aanmeldingsgegevens voorzien. U kunt contact met ons opnemen op **support@tve.zendesk.com**.


## Deel I {#part-I}

Voor het testen tegen de RELEASE-omgeving gaat u rechtstreeks naar deel II.  Voor het testen in de pre-kwalificatieomgeving raadpleegt u [Uw omgeving instellen en testen in een proefversie](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Deel II

Voer de volgende stappen uit nadat u deel I hebt voltooid:


1. Webpagina openen: [Testen van API](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Toegangsfunctie laden vanaf de volgende URL:
   * [JavaScript voor opmaken van bestanden inschakelen](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * OF
   * [Toegangsenabler javascript voor productie](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * Klik vervolgens op &quot;**Toegangsfunctie laden**&quot;.
1. Stel nu de id-waarde van de aanvrager in op &quot;**aanvragerID**&quot; en klik op de knop &quot;setRequest&quot;.
1. Vervolgens drukt u op de knop &quot;getAuthentication&quot; en wacht u tot de weergavekiezer verschijnt.
1. Selecteer &quot;**MVPD**&quot; in de kiezer.
1. Voer uw gegevens in op het tabblad &quot;**MVPD**&quot; aanmeldingspagina.
1. Voer de stappen 1 tot en met 3 opnieuw uit nadat u de omleiding hebt uitgevoerd
1. Na het opnieuw uitvoeren van stap 3 op &quot;setAuthenticationStatus&quot;zou u de waarde &quot;1&quot;moeten zien. Als de authentificatie niet werkte, zal de MVPD dialoogdoos worden getoond.
1. Als u de autorisatie wilt testen, voert u in het veld voor de broninvoer de waarde &quot;**aanvragerID**&quot; en klik op de knop &quot;getAuthorization&quot;.
1. Dientengevolge, in &quot;setToken&quot; - \>&quot;middel identiteitskaart&quot;textbox zal de middel worden getoond en in &quot;setToken&quot;- \>&quot;symbolische&quot;textbox shortAuthorizationToken zal worden getoond betekenend dat authZ succesvol was.
1. Nu kunt u op de knop &quot;logout&quot; klikken om de tokens te verwijderen.
