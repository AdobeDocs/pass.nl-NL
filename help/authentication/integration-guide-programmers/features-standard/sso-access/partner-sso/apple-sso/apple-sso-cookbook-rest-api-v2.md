---
title: Apple SSO Cookbook (REST API V2)
description: Apple SSO Cookbook (REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '3443'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De Adobe Pass Authentication REST API V2 biedt ondersteuning voor Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan het bestaande [ REST API V2 Overzicht ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) dat een mening op hoog niveau en het document verstrekt dat beschrijft hoe te om [ Enige sign-on uit te voeren gebruikend partnerstromen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Apple Single Sign-On met gebruik van partnerstromen {#cookbook}

### Vereisten {#prerequisites}

Voordat u verdergaat met de Apple Single Sign-On met behulp van partnerstromen, moet u ervoor zorgen dat aan de volgende voorwaarden wordt voldaan:

* De streamingtoepassing moet alle vereiste gegevens voor de headers `X-Device-Info` en/of `User-Agent` verzamelen, zodat de Adobe Pass-verificatieachterkant het apparaatplatform en de mogelijkheden ervan kan identificeren. Voor meer details over `X-Device-Info` kopbal, verwijs naar [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) documentatie.

* De streamingtoepassing moet toegang aanvragen tot de abonnementsgegevens van de gebruiker die op apparaatniveau zijn opgeslagen, waarvoor de gebruiker de toepassing toestemming moet geven om door te gaan, net als het verlenen van toegang tot de camera of microfoon van het apparaat. Deze toestemming moet per toepassing worden gevraagd gebruikend het Kader van de Rekening van de Abonnee van Apple [ Video ](https://developer.apple.com/documentation/videosubscriberaccount) en het apparaat zal de selectie van de gebruiker bewaren.

  We raden gebruikers aan die geen toestemming geven om abonnementsgegevens te openen, te stimuleren door de voordelen uit te leggen van de Apple Single Sign-On gebruikerservaring, maar we raden gebruikers aan hun beslissing te wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of *`Settings -> TV Provider`* op iOS en iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS te gaan.

  De het stromen toepassing kan om de toestemming van de gebruiker verzoeken wanneer de toepassing de voorgrondstaat ingaat, omdat de toepassing [ toestemming kan controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsinformatie van de gebruiker op om het even welk punt toegang te hebben alvorens gebruikersauthentificatie te vereisen.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
>
> * De het stromen toepassing heeft [ op het instappen eerste vereisten ](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) voltooid die op een Programmer van toepassing zijn en worden vereist om Apple enige sign-on gebruikerservaring toe te laten.

### Workflow {#workflow}

Voer de bepaalde stappen uit om Apple enig teken-op uit te voeren gebruikend partnerstromen zoals aangetoond in het volgende diagram.

![ Apple enig teken-op gebruikend partnerstromen ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Apple enig teken-op gebruikend partnerstromen*

+++A. Registratiefase

1. **wint cliëntgeloofsbrieven terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om cliëntgeloofsbrieven terug te winnen door het eindpunt van het Register van de Cliënt te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen cliëntgeloofsbrieven ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `software_statement`
   > * Alle _vereiste_ kopballen, als `Content-Type`, `X-Device-Info`
   > * Alle _facultatieve_ parameters en kopballen

1. **de cliëntgeloofsbrieven van de Terugkeer:** De het eindpuntreactie van het Register van de Cliënt bevat informatie over de cliëntgeloofsbrieven verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen cliëntgeloofsbrieven ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API documentatie voor details over de informatie die in een reactie van de cliëntgeloofsbrieven wordt verstrekt.
   >
   > <br/>
   >
   > In het clientregister worden de aanvraaggegevens gevalideerd om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, die extra informatie verstrekken die aan [ voldoet wint cliëntgeloofsbrieven ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API documentatie terug.

   >[!TIP]
   >
   > Suggestie: de clientgegevens moeten in de cache worden opgeslagen en kunnen voor onbepaalde tijd worden gebruikt.

1. **wint toegangstoken terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om toegangstoken terug te winnen door het Symbolische eindpunt van de Cliënt te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `client_id`, `client_secret`, en `grant_type`
   > * Alle _vereiste_ kopballen, als `Content-Type`, `X-Device-Info`
   > * Alle _facultatieve_ parameters en kopballen

1. **het toegangstoken van de Terugkeer:** De het eindpuntreactie van het Symbolische van de Cliënt bevat informatie over het toegangstoken verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API documentatie voor details over de informatie die in een reactie van het toegangstoken wordt verstrekt.
   >
   > <br/>
   >
   > De Clienttoken valideert de aanvraaggegevens om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, die extra informatie verstrekken die aan [ voldoet verkrijgt toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API documentatie.

   >[!TIP]
   >
   > Suggestie: het toegangstoken moet in de cache worden opgeslagen en alleen binnen de opgegeven tijdsduur worden gebruikt (bijvoorbeeld 24-uurs time-to-live). Nadat deze is verlopen, moet de streamingtoepassing een nieuw toegangstoken aanvragen.

+++

+++B. Verificatiefase controleren

1. **wint de status van het partnerkader terug:** de het stromen toepassing roept het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) dat door Apple wordt ontwikkeld, om gebruikerstoestemming en leveranciersinformatie te verkrijgen.

   >[!IMPORTANT]
   >
   > Verwijs naar de ](https://developer.apple.com/documentation/videosubscriberaccount) documentatie van het Kader van de Rekening van de Abonnee van 0} Video voor details op:[
   >
   > <br/>
   >
   > * De het stromen toepassing moet [ toestemming controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
   > * De het stromen toepassing moet a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor `VSAccountManager` verstrekken.
   > * De het stromen toepassing moet a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening voorleggen.
   > * De het stromen toepassing moet de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie wachten en verwerken.
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een Booleaanse waarde opgeeft die gelijk is aan `false` voor de eigenschap [`isInterruptionAllowed` ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) in het `VSAccountMetadataRequest` -object, om aan te geven dat de gebruiker in deze fase niet kan worden onderbroken.

1. **de statusinformatie van het partnerkader van de Terugkeer:** De het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   * De toegangsstatus van de gebruikerstoestemming wordt verleend.
   * De toewijzingsaanduiding van de gebruikersprovider is aanwezig en geldig.
   * De vervaldatum (indien beschikbaar) van het gebruikersprovider-profiel is geldig.

1. **wint profielen terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om alle profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profielen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`, en `AP-Partner-Framework-Status`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde bevat voor de status van het partnerframework, zodat de opgehaalde reactie een &quot;appleSSO&quot;-typeprofiel kan bevatten.
   >
   > <br/>
   >
   > Voor meer details over `AP-Partner-Framework-Status` kopbal, verwijs naar [ AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) documentatie.

1. **de informatie van de Terugkeer over gevonden profielen:** de het eindpuntreactie van Profielen bevat informatie over de gevonden profielen verbonden aan de ontvangen parameters en kopballen.

1. **kies een profiel en ga met besluitvormingsstromen te werk:** als de het eindpuntreactie van Profielen profielen bevat, gebruikt de het stromen toepassing zijn interne logica (uiteindelijk door met de eindgebruiker in wisselwerking te staan) om één van de beschikbare profielen te kiezen om met verdere besluitvormingsstromen verder te gaan.

1. **ga met de stroom van de partnerauthentificatie te werk:** als de het eindpuntreactie van Profielen geen profiel bevat, gaat de het stromen toepassing met de stroom van de partnerauthentificatie verder.

+++

+++C. Verificatiefase voor partners

1. **wint configuratie terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om de lijst van MVPDs terug te winnen die een actieve integratie hebben door een verzoek naar het eindpunt van de Configuratie te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen configuratie voor specifieke dienstverlener ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`, en `X-Device-Info`
   > * Alle _facultatieve_ parameters en kopballen

1. **de configuratie van de Terugkeer:** De het eindpuntreactie van de Configuratie bevat informatie over MVPDs die een actieve integratie met de dienstverlener hebben.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen configuratie voor specifieke dienstverlener ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) API documentatie voor details op de informatie die in een configuratiereactie wordt verstrekt.
   >
   > <br/>
   >
   > Het eindpunt van de Configuratie bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   >
   > De streamingtoepassing moet ervoor zorgen dat deze de volgende gegevens verwerkt die voor elke MVPD worden verstrekt wanneer verder wordt gegaan:
   >
   > * `enablePlatformServices`: Geeft aan of de MVPD momenteel ondersteuning biedt voor de eenmalige aanmelding voor Apple.
   > * `displayInPlatformPicker`: geeft aan of de MVPD kan worden weergegeven in de Apple-kiezer.
   > * `boardingStatus`: geeft aan of de MVPD is geactiveerd in het eenmalige aanmelding voor Apple.

1. **wint de status van het partnerkader terug:** de het stromen toepassing roept het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) dat door Apple wordt ontwikkeld, om gebruikerstoestemming en leveranciersinformatie te verkrijgen.

   >[!IMPORTANT]
   >
   > Verwijs naar de ](https://developer.apple.com/documentation/videosubscriberaccount) documentatie van het Kader van de Rekening van de Abonnee van 0} Video voor details op:[
   >
   > <br/>
   >
   > * De het stromen toepassing moet [ toestemming controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
   > * De het stromen toepassing moet a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor `VSAccountManager` verstrekken.
   > * De het stromen toepassing moet a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening voorleggen.
   > * De het stromen toepassing moet de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie wachten en verwerken.
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een Booleaanse waarde opgeeft die gelijk is aan `true` voor de eigenschap [`isInterruptionAllowed` ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) in het `VSAccountMetadataRequest` -object, om aan te geven dat de gebruiker kan worden onderbroken om in deze fase een tv-provider te selecteren.

1. **de statusinformatie van het partnerkader van de Terugkeer:** De het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   * De toegangsstatus van de gebruikerstoestemming wordt verleend.
   * De toewijzingsaanduiding van de gebruikersprovider is aanwezig en geldig.
   * De vervaldatum (indien beschikbaar) van het gebruikersprovider-profiel is geldig.

1. **wint het verzoek van de partnerauthentificatie terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om een authentificatiesessie in werking te stellen door het eindpunt van de Partner van Sessies te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen de verzoek van de partnerauthentificatie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider` en `partner`
   > * Alle _vereiste_ kopballen zoals `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info`, en `AP-Partner-Framework-Status`
   > * Alle _facultatieve_ kopballen en parameters
   >
   > <br/>
   >
   > De het stromen toepassing moet ervoor zorgen het een geldige waarde voor de status van het partnerkader omvat dusdanig dat de teruggewonnen reactie een verzoek van de partnerauthentificatie (het verzoek van SAML) kan omvatten.
   >
   > <br/>
   >
   > Voor meer details over `AP-Partner-Framework-Status` kopbal, verwijs naar [ AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) documentatie.

1. **wijs op de volgende actie:** De het eindpuntreactie van de Partner van Sessies bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen de verzoek van de partnerauthentificatie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API documentatie voor details over de informatie die in een zittingsreactie wordt verstrekt.
   >
   > <br/>
   >
   > Het eindpunt van de Partner van Sessies bevestigt de verzoekgegevens om ervoor te zorgen dat aan basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   >
   > Als de basisbevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.
   >
   > <br/>
   >
   > Het eindpunt van de Partner van Sessies bevestigt de verzoekgegevens om ervoor te zorgen dat de partner enige sign-on voorwaarden wordt voldaan aan:
   >
   >  * De partner enige sign-on configuratie in de server van Adobe Pass moet geldig en toegelaten zijn.
   >  * De de statuslading van het partnerkader die via [ wordt ontvangen AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) kopbal moet geldig zijn.
   >
   > <br/>
   >
   > Als de partner enige sign-on bevestiging ontbreekt, zal de reactie aan de basisauthentificatiestroom in gebreke blijven.

1. **ga met besluitvormingsstromen te werk:** De het eindpuntreactie van de Partner van Sessies bevat de volgende gegevens:
   * Het attribuut `actionName` wordt ingesteld op &quot;authorize&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.

   Als de Adobe Pass-backend een geldig profiel identificeert, hoeft de streamingtoepassing niet opnieuw te worden geverifieerd met de geselecteerde MVPD, omdat er al een profiel is dat kan worden gebruikt voor volgende beslissingsstromen.

1. **ga met basisauthentificatiestroom te werk:** De het eindpuntreactie van de Partner van Sessies bevat de volgende gegevens:
   * Het kenmerk `actionName` wordt ingesteld op &quot;authenticate&quot; of &quot;resume&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot; of &quot;direct&quot;.

   Als de Adobe Pass backend geen geldig profiel identificeert en de partner enige sign-on bevestiging ontbreekt, valt de server van Adobe Pass terug naar de basisauthentificatiestroom.

   Raadpleeg de volgende documenten voor meer informatie over de basisverificatiestroom:
   * [Verificatie uitvoeren binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Verificatie uitvoeren binnen secundaire toepassing met vooraf geselecteerde mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Verificatie uitvoeren binnen secundaire toepassing zonder vooraf geselecteerde mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **ga met terug profiel gebruikend de stroom van de partnerauthentificatiereactie:** De het eindpuntreactie van de Partner van Sessies bevat de volgende gegevens:
   * Het attribuut `actionName` wordt ingesteld op &quot;partner_profile&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.
   * Het attribuut `authenticationRequest - type` omvat het veiligheidsprotocol dat door het partnerkader voor login van MVPD wordt gebruikt (momenteel geplaatst aan SAML slechts).
   * Het attribuut `authenticationRequest - request` omvat het SAML- verzoek dat tot het partnerkader wordt overgegaan.
   * Het attribuut `authenticationRequest - attributesNames` omvat de attributen SAML die tot het partnerkader worden overgegaan.

   Als de Adobe Pass backend geen geldig profiel identificeert en de partner enige sign-on bevestiging overgaat, ontvangt de het stromen toepassing een reactie met acties en gegevens om tot het partnerkader over te gaan voor het beginnen van de authentificatiestroom met MVPD.

1. **Volledige authentificatie van MVPD met partnerkader:** door:sturen het verzoek van de partnerauthentificatie (SAML verzoek) in vorige stap aan het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) wordt verkregen. Als de authentificatiestroom succesvol is, veroorzaakt de ](https://developer.apple.com/documentation/videosubscriberaccount) interactie van het Kader van de Rekening van de Abonnee 0} Video met MVPD een reactie van de partnerauthentificatie (reactie van SAML) die samen met de informatie van de de statusstatus van het partnerkader is teruggekeerd.[

   >[!IMPORTANT]
   >
   > Verwijs naar de ](https://developer.apple.com/documentation/videosubscriberaccount) documentatie van het Kader van de Rekening van de Abonnee van 0} Video voor details op:[
   >
   > <br/>
   >
   > * De het stromen toepassing moet [ toestemming controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
   > * De het stromen toepassing moet a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor `VSAccountManager` verstrekken.
   > * De het stromen toepassing moet a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening voorleggen en moet het verzoek van de partnerauthentificatie (SAML verzoek) omvatten die in vorige stap wordt verkregen.
   > * De het stromen toepassing moet de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie wachten en verwerken.
   >
   > <br/>
   >
   > De het stromen toepassing moet ervoor zorgen het een waarde Van Boole gelijk aan `true` voor het [`isInterruptionAllowed` ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) bezit in het `VSAccountMetadataRequest` voorwerp specificeert, om erop te wijzen dat de gebruiker kan worden onderbroken om met de geselecteerde leverancier van TV in deze fase voor authentiek te verklaren.

1. **de reactie van de partnerauthentificatie van de Terugkeer:** De het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   * De toegangsstatus van de gebruikerstoestemming wordt verleend.
   * De toewijzingsaanduiding van de gebruikersprovider is aanwezig en geldig.
   * De vervaldatum (indien beschikbaar) van het gebruikersprovider-profiel is geldig.
   * De reactie van de partnerauthentificatie (de reactie van SAML) is aanwezig en geldig.

1. **wint profiel terug gebruikend de reactie van de partnerauthentificatie:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om een profiel tot stand te brengen en terug te winnen door het eindpunt van de Partner van Profielen te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terug wint profiel gebruikend de reactie van de partnerauthentificatie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `partner`, en `SAMLResponse`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info`, en `AP-Partner-Framework-Status`
   > * Alle _facultatieve_ kopballen en parameters
   >
   > <br/>
   >
   > De het stromen toepassing moet ervoor zorgen het geldige waarden voor de status van het partnerkader en de reactie van de partnerauthentificatie (de reactie van SAML) omvat zodat de teruggewonnen reactie een &quot;appleSSO&quot;typeprofiel kan omvatten.
   >
   > <br/>
   >
   > Voor meer details over `AP-Partner-Framework-Status` kopbal, verwijs naar [ AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) documentatie.

1. **de informatie van de Terugkeer over partnerprofiel:** de het eindpuntreactie van Profielen bevat informatie over het partnerprofiel, met inbegrip van de attributen `type` die aan &quot;appleSSO&quot;worden geplaatst.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel gebruikend de reactie van de partnerauthentificatie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) API documentatie voor details op de informatie die in een profielreactie wordt verstrekt.
   >
   > <br/>
   >
   > Het eindpunt van de Partner van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.
   >
   > <br/>
   >
   > Het eindpunt van de Partner van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de partner enige sign-on voorwaarden wordt voldaan aan:
   >
   >  * De partner enige sign-on configuratie in de server van Adobe Pass moet geldig en toegelaten zijn.
   >  * De de statuslading van het partnerkader die via [ wordt ontvangen AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) kopbal moet geldig zijn.
   >
   > <br/>
   >
   > Als de partner enige sign-on bevestiging ontbreekt, zal de reactie aan de basisstroom van de profielherwinning in gebreke blijven.

1. **ga met besluitvormingsstromen te werk:** De het stromen toepassing kan met verdere besluitvormingsstromen verdergaan.

+++

+++ D. Besluiten fase

1. **wint de status van het partnerkader terug:** de het stromen toepassing roept het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) dat door Apple wordt ontwikkeld, om gebruikerstoestemming en leveranciersinformatie te verkrijgen.

   >[!IMPORTANT]
   >
   > Verwijs naar de ](https://developer.apple.com/documentation/videosubscriberaccount) documentatie van het Kader van de Rekening van de Abonnee van 0} Video voor details op:[
   >
   > <br/>
   >
   > * De het stromen toepassing moet [ toestemming controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
   > * De het stromen toepassing moet a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor `VSAccountManager` verstrekken.
   > * De het stromen toepassing moet a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening voorleggen.
   > * De het stromen toepassing moet de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie wachten en verwerken.
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een Booleaanse waarde opgeeft die gelijk is aan `false` voor de eigenschap [`isInterruptionAllowed` ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) in het `VSAccountMetadataRequest` -object, om aan te geven dat de gebruiker in deze fase niet kan worden onderbroken.

   >[!TIP]
   > 
   > Suggestie: de streamingtoepassing kan een in cache opgeslagen waarde gebruiken voor de statusinformatie van het partnerframework. We raden u aan deze waarde te vernieuwen wanneer de toepassing van achtergrond naar voorgrondstatus overgaat.

1. **de statusinformatie van het partnerkader van de Terugkeer:** De het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   * De toegangsstatus van de gebruikerstoestemming wordt verleend.
   * De toewijzingsaanduiding van de gebruikersprovider is aanwezig en geldig.
   * De vervaldatum (indien beschikbaar) van het gebruikersprovider-profiel is geldig.

1. **wint pre-vergunningsbesluiten terug:** De het stromen toepassing verzamelt alle noodzakelijke gegevens om pre-vergunningsbesluiten voor een lijst van middelen te verkrijgen door de Besluiten te roepen preauthorize eindpunt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen pre-vergunningsbesluiten gebruikend specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `resources`
   > * Alle _vereiste_ kopballen, als `Authorization` en `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de partnerframestatus bevat voordat u een aanvraag verder indient, wanneer het gekozen profiel een &quot;appleSSO&quot;-typeprofiel is.
   >
   > <br/>
   >
   > Voor meer details over `AP-Partner-Framework-Status` kopbal, verwijs naar [ AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) documentatie.

1. **de besluiten van de Terugkeer voorafgaand aan autorisatie:** De Besluiten pre-autoriseren eindpuntreactie bevat a `Permit` of `Deny` besluit voor elk middel:
   * Een `Permit` -beslissing betekent dat de bron kan worden afgespeeld. De reactie omvat geen media teken, aangezien de pre-vergunningsstroom niet moet worden gebruikt om middelen te spelen.
   * Een `Deny` -beslissing betekent dat de bron niet kan worden afgespeeld. De reactie omvat een foutenlading die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen pre-vergunningsbesluiten gebruikend specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
   >
   > <br/>
   >
   > Het eindpunt van Besluiten pre-autoriseert bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **wint de status van het partnerkader terug:** de het stromen toepassing roept het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) dat door Apple wordt ontwikkeld, om gebruikerstoestemming en leveranciersinformatie te verkrijgen.

   >[!IMPORTANT]
   >
   > Verwijs naar de ](https://developer.apple.com/documentation/videosubscriberaccount) documentatie van het Kader van de Rekening van de Abonnee van 0} Video voor details op:[
   >
   > <br/>
   >
   > * De het stromen toepassing moet [ toestemming controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
   > * De het stromen toepassing moet a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor `VSAccountManager` verstrekken.
   > * De het stromen toepassing moet a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening voorleggen.
   > * De het stromen toepassing moet de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie wachten en verwerken.
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een Booleaanse waarde opgeeft die gelijk is aan `false` voor de eigenschap [`isInterruptionAllowed` ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) in het `VSAccountMetadataRequest` -object, om aan te geven dat de gebruiker in deze fase niet kan worden onderbroken.

   >[!TIP]
   >
   > Suggestie: de streamingtoepassing kan een in cache opgeslagen waarde gebruiken voor de statusinformatie van het partnerframework. We raden u aan deze waarde te vernieuwen wanneer de toepassing van achtergrond naar voorgrondstatus overgaat.

1. **de statusinformatie van het partnerkader van de Terugkeer:** De het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan aan:
   * De toegangsstatus van de gebruikerstoestemming wordt verleend.
   * De toewijzingsaanduiding van de gebruikersprovider is aanwezig en geldig.
   * De vervaldatum (indien beschikbaar) van het gebruikersprovider-profiel is geldig.

1. **wint vergunningsbesluit terug:** De het stromen toepassing verzamelt alle noodzakelijke gegevens om een vergunningsbesluit voor een specifiek middel te verkrijgen door Besluiten te roepen machtigt eindpunt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen vergunningsbesluiten gebruikend specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `resources`
   > * Alle _vereiste_ kopballen, als `Authorization` en `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de partnerframestatus bevat voordat u een aanvraag verder indient, wanneer het gekozen profiel een &quot;appleSSO&quot;-typeprofiel is.
   >
   > <br/>
   >
   > Voor meer details over `AP-Partner-Framework-Status` kopbal, verwijs naar [ AP-partner-kader-status ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) documentatie.

1. **de vergunningsbesluit van de Terugkeer:** De Besluiten machtigen eindpuntreactie bevat a `Permit` of `Deny` besluit voor het specifieke middel:
   * Een `Permit` -beslissing betekent dat de bron kan worden afgespeeld. De reactie bevat een media-token.
   * Een `Deny` -beslissing betekent dat de bron niet kan worden afgespeeld. De reactie omvat een foutenlading die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen vergunningsbesluiten gebruikend specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
   >
   > <br/>
   >
   > De Besluiten machtigen eindpunt bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

+++

+++ D. Afmeldingsfase

1. **stelt Adobe Pass logout in werking:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om de logout stroom in werking te stellen door het de Logout van Adobe Pass eindpunt te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ Logout van het Begin voor specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `redirectUrl`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

1. **wijs op de volgende actie:** De het eindpuntreactie van het Logout van Adobe Pass bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden:
   * Het attribuut `url` ontbreekt aangezien de gebruiker met het partner (systeem) niveau moet in wisselwerking staan om de logout stroom te voltooien.
   * Het attribuut `actionName` wordt geplaatst aan &quot;partner_logout&quot;.
   * Het kenmerk `actionType` wordt ingesteld op &quot;partner_interactive&quot;.

   >[!IMPORTANT]
   >
   > Verwijs naar [ Logout van het Begin voor specifieke mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API documentatie voor details op de informatie die in een logout reactie wordt verstrekt.
   >
   > <br/>
   >
   > Het Adobe Pass Logout eindpunt valideert de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   > 
   > De het stromen toepassing moet ervoor zorgen het op de gebruiker wijst om zich van partnerniveau verder uit te melden.

+++
