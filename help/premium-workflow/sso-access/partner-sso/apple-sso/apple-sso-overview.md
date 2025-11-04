---
title: Apple SSO - Overzicht
description: Apple SSO - Overzicht
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Apple SSO - Overzicht {#apple-sso-overview}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Apple biedt gebruikers de mogelijkheid om zich aan te melden bij hun tv-provider op apparaatsysteemniveau, zodat ze zich niet meer per app hoeven te verifiëren.

Adobe Pass Authentication ging samen met Apple om de Single Sign-On (SSO)-gebruikerservaring van de Partner te creëren in het tv-systeem Overal voor eigenaars van iPhone, iPad en Apple TV.

Om te kunnen profiteren van de Single Sign-On (SSO) gebruikerservaring op een Apple-apparaat, is er een lijst met voorwaarden die hieronder worden beschreven en die moeten worden voltooid.

Het eindresultaat zou een ervaring in lijn met de volgende gebruikersstromen moeten tot stand brengen, die wij u adviseren alvorens u begint uw toepassing te ontwikkelen:

* Enige Sign-On (SSO) [ gebruikersstromen voor iPhone en iPad ](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) apparaten.
* Enige Sign-On (SSO) [ gebruikersstromen voor Apple TV ](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) apparaten.

## Vereisten {#apple-sso-prerequisites}

Voorwaarden bij instapweigering kunnen van toepassing zijn op een of meer entiteiten die betrokken zijn bij de TVE-activiteiten, zoals programmeurs, MVPD&#39;s, Adobe Pass Authentication of Apple.

### Programmeur {#apple-sso-prerequisites-programmer}

Om te profiteren van de Single Sign-On (SSO) gebruikerservaring, moet één programmeur:

* Contact Apple om het [ Video Kader van de Rekening van de Abonnee van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) als deel van uw identiteitskaart van het Team van Apple toe te laten en de [ Video Abonnee Enige Sign-On Entitlement ](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) als deel van uw Rekening van de Ontwikkelaar van Apple te vormen.

   * Gebruik Xcode versie 8 of hoger en iOS/tvOS versie 10 of hoger.

* Laat Enige Sign-On (SSO) voor elke gewenste integratie en platform (iOS/tvOS) door het [ Dashboard van Adobe Pass TVE ](https://experience.adobe.com/#/pass/authentication) toe door het `Enable Single Sign On` bezit aan `Yes` te plaatsen.

| Adobe Single Sign On inschakelen | Apple **Op-boted (Gesteund)** MVPDs | Apple **Plukker** MVPDs | Apple **niet Op-board (niet gesteund)** MVPDs |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Ja (ingeschakeld) | De verificatie- en uitlogstromen hebben zowel betrekking op Apple- als op Adobe Pass-verificatieoplossingen, terwijl alle andere stromen (autorisatie, voorafgaande toestemming, metagegevens, enz.) uitsluitend door Adobe Pass Authentication worden beheerd. | De verificatie- en logout-stromen vallen terug op de reguliere stromen die uitsluitend door Adobe Pass Authentication worden beheerd. | De verificatie- en logout-stromen vallen terug op de reguliere stromen die uitsluitend door Adobe Pass Authentication worden beheerd. |
| Nee (uitgeschakeld) | De verificatie- en logout-stromen vallen terug op de reguliere stromen die uitsluitend door Adobe Pass Authentication worden beheerd. | De verificatie- en logout-stromen vallen terug op de reguliere stromen die uitsluitend door Adobe Pass Authentication worden beheerd. | De verificatie- en logout-stromen vallen terug op de reguliere stromen die uitsluitend door Adobe Pass Authentication worden beheerd. |

* Integreer de Single Sign-On (SSO) gebruikersstromen met behulp van een van de volgende oplossingen die door Adobe Pass Authentication worden aangeboden voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd.

   * De Adobe Pass Authentication REST API V2 biedt ondersteuning voor Single Sign-On (SSO) voor partners.

     Verwijs naar [ Apple SSO Cookbook (REST API V2) ](apple-sso-cookbook-rest-api-v2.md) documentatie.

   * De oudere Adobe Pass Authentication REST API V1 biedt ondersteuning voor Single Sign-On (SSO) voor partners.

     Raadpleeg de [ (Verouderd) Apple SSO Cookbook (REST API V1) ](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md) -documentatie.

   * De verouderde Adobe Pass Authentication AccessEnabler iOS/tvOS SDK biedt ondersteuning voor Single Sign-On (SSO) voor partners.

     Raadpleeg de [ (Verouderd) Apple SSO Cookbook (iOS/tvOS SDK) ](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md) -documentatie.

### MVPD {#apple-sso-prerequisites-mvpd}

Om te profiteren van de Single Sign-On (SSO) gebruikerservaring, moet één MVPD:

* Neem contact op met Apple om het instapproces aan Apple-zijde te starten.

   * Vraag de technische documentatie aan over het integreren en ontwikkelen van een JavaScript TVML-toepassing waarmee het aanmeldingsformulier voor de gebruiker kan worden verwerkt.

* Neem contact op met Adobe Pass Authentication om het instapproces aan Adobe zijde te starten.

   * Geef de tekenreekswaarde op die de tv-provider-id vertegenwoordigt die Apple tijdens het instapproces heeft toegewezen.

## Veelgestelde vragen {#FAQ}

* Als er iets mis gaat met de Apple SSO-workflow, kan de toepassing die gebruikmaakt van de Adobe Pass Authentication AccessEnabler iOS/tvOS SDK dan terugvallen op de normale verificatiestroom?

  Dit is mogelijk maar vereist een configuratieverandering die door het [ Dashboard van Adobe Pass TVE ](https://experience.adobe.com/#/pass/authentication) wordt uitgevoerd om **toe te laten Enige Sign-On** op **** voor de gewenste integratie en het platform (iOS/tvOS). Ben zich ervan bewust dat de cliënttoepassing de configuratieverandering slechts na het roepen van [ setRequestor ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API zal erkennen.


* Weet de toepassing wanneer er een verificatie heeft plaatsgevonden als gevolg van een aanmelding via Apple SSO?

  Deze informatie is beschikbaar als deel van de sleutel van gebruikersmeta-gegevens: *tokenSource*, die de koordwaarde zou moeten terugkeren: &quot;Apple&quot;in dit geval.


* Weet de toepassing wanneer een verificatie is uitgevoerd als gevolg van een aanmelding via Apple SSO bij een andere toepassing?

  Deze informatie is niet beschikbaar.


* Wat gebeurt er als een gebruiker zich aanmeldt door naar de sectie *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS te gaan met een MVPD die niet is geïntegreerd met de toepassing?

  Wanneer de gebruiker de toepassing start, wordt de gebruiker niet geverifieerd via de Apple SSO-workflow. Daarom moet de toepassing terugvallen op de normale verificatiestroom en een eigen MVPD-kiezer presenteren.


* Wat gebeurt als een gebruiker binnen ondertekent door naar *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS sectie te gaan die een MVPD heeft die **toelaten Enig Teken** geplaatst op **** door [ Adobe Pass TVE Dashboard ](https://experience.adobe.com/#/pass/authentication) voor iOS/tvOS platform heeft?

  Wanneer de gebruiker de toepassing start, wordt de gebruiker niet geverifieerd via de Apple SSO-workflow. Daarom moet de toepassing terugvallen op de normale verificatiestroom en een eigen MVPD-kiezer presenteren.


* Wat gebeurt er als een gebruiker een MVPD heeft die niet door Apple wordt geregistreerd (niet wordt ondersteund), maar die wel aanwezig is in de Apple-kiezer?

  Wanneer de gebruiker de toepassing start, zal de gebruiker de MVPD alleen selecteren via de SSO-workflow van Apple zonder de verificatiestroom te voltooien. Daarom moet de toepassing terugvallen op de normale verificatiestroom, maar kan de reeds geselecteerde MVPD worden gebruikt.


* Wat gebeurt er als een gebruiker een MVPD heeft die niet door Apple wordt geactiveerd (niet wordt ondersteund)?

  Wanneer de gebruiker de toepassing start, selecteert de gebruiker de optie Andere tv-providers via de Apple SSO-workflow. Daarom moet de toepassing terugvallen op de normale verificatiestroom en een eigen MVPD-kiezer presenteren.


* Wat gebeurt als een gebruiker een MVPD heeft die door het middel van [ het Dashboard van Adobe Pass TVE ](https://experience.adobe.com/#/pass/authentication) wordt gedegradeerd?

  Wanneer de gebruiker de toepassing start, wordt de gebruiker geverifieerd via het afbraakmechanisme en niet via de Apple SSO-workflow. De ervaring zou naadloos voor de gebruiker moeten zijn, terwijl de toepassing door *N010* waarschuwingscode zal worden geïnformeerd voor het geval het Adobe Pass Authentication AccessEnabler iOS/tvOS SDK gebruikt.


* Zal de MVPD gebruiker - identiteitskaart veranderen tussen de SSO van Apple en niet-Apple SSO authentificatiestromen?

  De verwachting is dat de gebruikers-id niet verandert, maar dat deze voor elke geselecteerde provider moet worden geverifieerd.


* Zal er om het even welke verandering in authentificatie TTLs zijn?

  Adobe Pass Authentication zal de TTL&#39;s blijven respecteren die de programmeurs nodig hebben voor hun integratie met elke MVPD. Wanneer het navigeren van één toepassing van de Programmer aan een andere toepassing van de Programmer door Apple SSO, zal de tweede toepassing TTL van zijn overeenkomstige integratie van Programmer x MVPD hebben (het zal niet TTL van de eerste toepassing delen die voor authentiek verklaart)

|                                      | Adobe Pass Authentication TTL verlopen | Adobe Pass Authentication TTL geldig |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **het apparatentekenTTL van Apple verlopen** | gebruiker is NIET geverifieerd (MVPD-kiezer moet worden weergegeven) | gebruiker is geverifieerd en TTL is de resterende tijd van zijn Adobe Pass-verificatietoken/profiel |
| **geldig het apparatenteken van Apple teken TTL** | De gebruiker is ongemerkt geverifieerd en verkrijgt een ander Adobe Pass-verificatietoken/profiel met de TTL die is opgegeven in het TVE-dashboard | gebruiker is geverifieerd en TTL is de resterende tijd van zijn Adobe Pass-verificatietoken/profiel |
