---
title: Apple SSO - Overzicht
description: Apple SSO - Overzicht
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Apple SSO - Overzicht {#apple-sso-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#Introduction}

Apple biedt een API waarmee mensen zich op apparaatsysteemniveau kunnen aanmelden bij hun tv-provider, zodat ze zich niet meer per app hoeven te verifiëren.

Apple en Adobe Pass Authentication hebben daarom samengewerkt om het platform Single Sign-On (SSO)-gebruikerservaring in het tv-overal-ecosysteem te creëren voor eigenaars van iPhone-, iPad- en Apple-tv.

Om te kunnen profiteren van de Single Sign-On (SSO) gebruikerservaring op een Apple-apparaat, is er een lijst met voorwaarden die moeten worden voltooid.

</br>

## Vereisten {#Prerequisites}

De voorwaarde kan op één of meerdere entiteiten van toepassing zijn die bij de zaken van TVE, zoals Programmers, MVPDs, de Authentificatie van Adobe Pass of Apple betrokken zijn.

</br>

### Programmeur {#Programmer}

Om te profiteren van de Single Sign-On (SSO) gebruikerservaring, moet één programmeur:

1. Gebruik ten minste Xcode versie 8 en iOS/tvOS versie 10.

1. beschikken over [Video Subscriber Single Sign-On Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) geconfigureerd op hun Apple Developer Account. Neem contact op met Apple om deze functie in te schakelen [Videoabonneeaccountframework](https://developer.apple.com/documentation/videosubscriberaccount) voor uw team-id van Apple.

1. Schakel Single Sign-On (YES) in voor elke gewenste integratie (Channel x MVPD) en het gewenste platform (iOS / tvOS) via het [Adobe Primetime TVE-dashboard](https://console.auth.adobe.com/).

1. Integreer de Apple SSO-workflows met een van de volgende twee oplossingen die door het Adobe Pass Authentication-team worden aangeboden:

   - De Adobe Pass Authentication REST API kan platform Single Sign-On (SSO)-verificatie ondersteunen voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd. Zie ook [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - De Adobe Pass Authentication AccessEnabler iOS/tvOS SDK kan platform Single Sign-On (SSO)-verificatie ondersteunen voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd. Zie ook [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Pro Tip:</u>** Om toegang te hebben tot de abonnementsinformatie van de gebruiker, moet de gebruiker de toepassingstoestemming geven te werk te gaan, gelijkend op het verlenen van toegang tot de camera of microfoon van het apparaat. Deze machtiging moet per toepassing worden aangevraagd en het apparaat slaat de selectie van de gebruiker op. Houd er rekening mee dat de gebruiker zijn beslissing kan wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of naar de sectie te gaan vanuit *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS.

   - **<u>Pro Tip:</u>** We raden u aan de gebruiker om toestemming te vragen wanneer de toepassing de voorgrondstatus activeert, maar dit is slechts een suggestie, omdat de toepassing kan controleren op [toegangsrechten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsgegevens van de gebruiker op om het even welk punt alvorens gebruikersauthentificatie te vereisen. De AccessEnabler iOS/tvOS SDK-API&#39;s vragen automatisch om toestemming van de gebruiker wanneer deze deze nodig heeft.

   - **<u>Pro Tip:</u>** We raden gebruikers aan die geen toestemming geven om abonnementsgegevens te openen, te stimuleren door de voordelen van Single Sign-On (SSO)-gebruikerservaring uit te leggen. Houd er rekening mee dat de gebruiker zijn beslissing kan wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of naar de sectie te gaan vanuit *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS.

Het resultaat moet een ervaring opleveren die aansluit bij de volgende gebruikersstromen. U wordt aangeraden dit te raadplegen voordat u begint met het ontwikkelen van uw toepassing(en):

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) gebruikersstromen
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) gebruikersstromen


>[!IMPORTANT]
>
> Wanneer de functie Single Sign-On **enabled** voor iOS/tvOS **en** voor Apple **Ingesloten (ondersteund) of kiezer** MVPDs de authentificatie/logout stromen van de werkschema&#39;s van SSO van Apple zullen zowel de oplossingen van de Authentificatie van Apple als van Adobe Pass omvatten, terwijl alle andere stromen (vergunning, pre-vergunning, meta-gegevens, enz.) wordt alleen door Adobe Pass Authentication onderhouden.


>[!IMPORTANT]
>
> Wanneer de functie Single Sign-On **uitgeschakeld** voor iOS/tvOS **of** voor Apple **niet aan boord genomen (niet ondersteund)** MVPD&#39;s de verificatie-/logout-stromen zullen terugvallen van de Apple SSO-workflows naar de gewone workflows die uitsluitend door Adobe Pass Authentication worden beheerd.


>[!IMPORTANT]
>
> Een belangrijke winst van de Apple SSO-workflow wordt vertegenwoordigd door de gebruikersstroom voor schermverificatie, die ook op Apple TV&#39;s kan worden geleverd wanneer de functie Single Sign-On is **enabled** voor tvOS **en** voor Apple **aan boord genomen (ondersteund)** MVPD&#39;s.


### MVPD {#MVPD}

Om te profiteren van de Single Sign-On (SSO) gebruikerservaring, moet één MVPD:



1. Aan de Apple zijde van de Apple SSO-workflow worden opgenomen. Neem contact op met Apple om het instapproces te vergemakkelijken.
1. Geef een JavaScript-TVML-toepassing op die het aanmeldingsformulier voor de gebruiker kan verwerken. Neem contact op met Apple voor de juiste documentatie.
1. Geef een tekenreekswaarde op die de provider-id vertegenwoordigt die door Apple is toegewezen tijdens het instapproces. Neem contact op met de Adobe Pass-verificatie om configuratiewijzigingen uit te voeren.

</br>

## Veelgestelde vragen {#FAQ}

1. Als er iets mis gaat met de Apple SSO-workflow, kan de toepassing die de AccessEnabler iOS/tvOS SDK gebruikt dan terugvallen op de normale verificatiestroom?
   - Dit is mogelijk maar er moet een configuratiewijziging worden uitgevoerd op de [Adobe Primetime TVE-dashboard](https://console.auth.adobe.com/). De *Single Sign-On inschakelen* moet worden ingesteld op *NEE* voor de gewenste integratie (Channel x MVPD) en het gewenste platform (iOS/tvOS).
   - De toepassing zou de configuratieverandering slechts na het roepen erkennen [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API voor het geval deze de AccessEnabler iOS/tvOS SDK gebruikt.
1. Weet de toepassing wanneer een verificatie is uitgevoerd als gevolg van een aanmelding via de SSO van het platform op een ander apparaat of een andere toepassing?
   - Deze informatie is niet beschikbaar.
1. Weet de toepassing wanneer een verificatie is uitgevoerd als gevolg van een aanmelding via de platform-SSO op hetzelfde apparaat?
   - Deze informatie is beschikbaar als onderdeel van de metagegevenssleutel van de gebruiker: *tokenSource*, die in dit geval de tekenreekswaarde &quot;Apple&quot; moet retourneren.
1. Wat gebeurt er als een gebruiker zich aanmeldt door naar de *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS-sectie met een MVPD die niet is geïntegreerd met de toepassing?
   - Wanneer de gebruiker de toepassing start, wordt de gebruiker niet geverifieerd via de Apple SSO-workflow. Daarom zou de toepassing aan regelmatige authentificatiestroom moeten terugvallen en zijn eigen plukker moeten voorstellen MVPD.
1. Wat gebeurt er als een gebruiker zich aanmeldt door naar de *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS-sectie met een MVPD die de *Single Sign-On inschakelen* ingesteld op *NEE* op de [Adobe Primetime TVE-dashboard](https://console.auth.adobe.com/) voor iOS/tvOS-platform?
   - Wanneer de gebruiker de toepassing start, wordt de gebruiker niet geverifieerd via de Apple SSO-workflow. Daarom zou de toepassing aan regelmatige authentificatiestroom moeten terugvallen en zijn eigen plukker moeten voorstellen MVPD.
1. Wat gebeurt er als een gebruiker een MVPD heeft die niet door Apple wordt geregistreerd (wordt niet ondersteund), maar die wel aanwezig is in de Apple-kiezer?
   - Wanneer de gebruiker de toepassing start, zal de gebruiker de MVPD alleen selecteren via de Apple SSO-workflow zonder de verificatiestroom te voltooien. Daarom zou de toepassing aan regelmatige authentificatiestroom moeten terugvallen, maar kon reeds geselecteerde MVPD gebruiken.
1. Wat gebeurt er als een gebruiker een MVPD heeft die niet wordt geregistreerd (niet wordt gesteund) door Apple?
   - Wanneer de gebruiker de toepassing start, selecteert de gebruiker de optie Andere tv-providers via de Apple SSO-workflow. Daarom zou de toepassing aan regelmatige authentificatiestroom moeten terugvallen en zijn eigen plukker moeten voorstellen MVPD.
1. Wat gebeurt er als een gebruiker een MVPD heeft die door het middel van [Adobe Primetime TVE-dashboard](https://console.auth.adobe.com/)?
   - Wanneer de gebruiker de toepassing start, wordt de gebruiker geverifieerd via het afbraakmechanisme en niet via de Apple SSO-workflow.
   - De ervaring moet naadloos zijn voor de gebruiker, terwijl de toepassing via de *N010* waarschuwingscode als deze de AccessEnabler iOS/tvOS SDK gebruikt.
1. Zal de MVPD gebruiker - identiteitskaart veranderen tussen Apple SSO en niet-Apple SSO authentificatiestroom?
   - De verwachting is dat de gebruikers-id niet verandert, maar dat deze voor elke geselecteerde provider moet worden geverifieerd.
1. Zal er om het even welke verandering in authentificatie TTLs zijn?
   - Adobe Pass Authentication zal de TTL&#39;s blijven respecteren die de programmeurs nodig hebben voor hun integratie met elke MVPD.
   - Wanneer het navigeren van één toepassing van de Programmer aan een andere toepassing van de Programmer door Apple SSO, zal de tweede toepassing TTL van zijn overeenkomstige integratie van Programmer x MVPD hebben (het zal niet TTL van de eerste toepassing delen die voor authentiek verklaart)

|                                      | Adobe Pass Authentication TTL verlopen | Adobe Pass Authentication TTL geldig |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **TTL voor Apple-apparaattoken verlopen** | gebruiker is NIET geverifieerd (MVPD-kiezer moet worden weergegeven) | gebruiker wordt voor authentiek verklaard en TTL is de resterende tijd van zijn teken van de Authentificatie van Adobe Pass |
| **Apple-apparaattoken TTL geldig** | de gebruiker is ongemerkt geverifieerd en verkrijgt een ander Adobe Pass-verificatietoken met de TTL die is opgegeven in het TVE-dashboard | gebruiker wordt voor authentiek verklaard en TTL is de resterende tijd van zijn teken van de Authentificatie van Adobe Pass |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
