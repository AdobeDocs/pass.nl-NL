---
title: Apple SSO Cookbook (iOS/tvOS SDK)
description: Apple SSO Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# (Verouderd) Apple SSO Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

De Adobe Pass Authentication AccessEnabler iOS/tvOS SDK biedt ondersteuning voor Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan de bestaande documentatie van iOS/tvOS SDK AccessEnabler, die [&#x200B; hier &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) kan worden gevonden.

## Cookbook {#apple-sso-cookbook-iostvos-sdk-cookbook}

Om te kunnen profiteren van de Apple SSO-gebruikerservaring, moet de toepassing de AccessEnabler iOS/tvOS SDK integreren en de onderstaande reeks stappen volgen.

### Vereisten {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Machtiging {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>ProUiteinde:</u>** de het stromen toepassing moet toegang tot de het abonnementinformatie van de gebruiker verzoeken die op apparatenniveau wordt bewaard, waarvoor de gebruiker de toepassingstoestemming moet geven te werk te gaan, gelijkend op het verlenen van toegang tot de camera of microfoon van het apparaat. Deze toestemming moet per toepassing worden gevraagd gebruikend het Kader van de Rekening van de Abonnee van Apple [&#x200B; Video &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) en het apparaat zal de selectie van de gebruiker bewaren.

>[!TIP]
>
> **<u>ProUiteinde:</u>** wij adviseren gebruikers te stimuleren die weigeren om toestemming te geven om tot abonnementinformatie toegang te hebben door de voordelen van de enige sign-on gebruikerservaring van Apple uit te leggen, maar zich ervan bewust te zijn dat de gebruiker hun besluit kan veranderen door naar de toepassingsmontages (de toestemmingstoegang van TV Provider) of naar *`Settings -> TV Provider`* op iOS en iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS te gaan.

>[!TIP]
>
> **<u>ProUiteinde:</u>** de het stromen toepassing kan om de toestemming van de gebruiker verzoeken wanneer de toepassing de voorgrondstaat ingaat, omdat de toepassing [&#x200B; toestemming kan controleren om tot &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementsinformatie van de gebruiker op om het even welk punt toegang te hebben alvorens gebruikersauthentificatie te vereisen.

>[!TIP]
>
> **<u>ProTip:</u>** als de gebruiker geen toegang tot zijn abonnementsinformatie verleent of in het geval dat de communicatie met het Video Kader van de Rekening van de Abonnee ontbreekt, dan zal AccessEnabler iOS/tvOS SDK terug naar de regelmatige authentificatiestroom vallen.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### Callbacks {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>ProUiteinde:</u>** voer de volgende lijst van [&#x200B; callbacks &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) uit die voor het werkschema van Apple SSO specifiek zijn.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback teweeggebracht wanneer de plukker van Apple MVPD gaat openen.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback teweeggebracht wanneer de plukker van Apple MVPD gaat sluiten.

#### Fout bij rapporteren {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>ProUiteinde:</u>** voer de volgende lijst van [&#x200B; geavanceerde foutencodes &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) uit die voor het werkschema van Apple SSO specifiek zijn.

* ***N003*** - de gebruiker selecteerde de &quot;Andere optie van de Leverancier van TV&quot;van de plukker van MVPD van Apple.
* ***N004*** - de gebruiker selecteerde een Leverancier van TV van de plukker van Apple MVPD, die niet (integratie of Enige Sign-On gehandicapt) door de huidige aanvrager wordt gesteund.
* ***N005*** - de gebruiker besloot om de regelmatige plukker van MVPD of van Apple MVPD te annuleren.
* ***VSA403*** - de toestemming van TV van de gebruiker wordt van de Leverancier ontkend voor de toepassing.
* ***VSA404*** - de toestemming van de leverancier van TV van de gebruiker is onbepaald voor de toepassing.
* ***VSA503*** - het Video Ontbroken de meta-gegevensverzoek van de Rekening van de Abonnee, meer context wordt verstrekt in het *bericht* gebied.
* ***AAPL / APPL_ERROR*** - het Video Ontbroken de meta-gegevensverzoek van de Rekening van de Abonnee, meer context wordt verstrekt op het *details* gebied.

### Verificatie {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Tip:</u>** volg de stappen hieronder voor iOS/iPadOS/tvOS implementatie/s.

1. De toepassing zou [&#128279;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) moeten initialiseren AccessEnabler iOS/tvOS SDK.


1. De toepassing zou [&#x200B; het huidige vraagherkenningsteken &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3) moeten plaatsen.

   **Belangrijk:** Deze tweede stap kon een [&#x200B; geavanceerde foutencode &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) teweegbrengen die voor het werkschema van Apple SSO specifiek is, in het geval **één van het volgende waar is**:

   * ***VSA403*** - de toestemming van TV van de gebruiker wordt van de Leverancier ontkend voor de toepassing.
   * ***VSA404*** - de toestemming van de leverancier van TV van de gebruiker is onbepaald voor de toepassing.
   * ***APPL*** - de communicatie tussen AccessEnabler iOS/tvOS SDK en het Video Kader van de Rekening van de Abonnee ontmoette een fout.

   Deze tweede stap zou proberen om het profiel van Apple SSO voor een teken van de Adobe authentificatie stil te ruilen, voor het geval **alle hierboven vals** zijn en **elk van het volgende is waar**:

   * De machtiging van de tv-provider van de gebruiker wordt verleend voor de toepassing.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau.
   * De AccessEnabler iOS/tvOS SDK heeft de tv-provider-id van de gebruiker ontvangen via het Video Subscriber Account Framework.
   * De integratie van de tv-provider van de gebruiker met de toepassing wordt mogelijk gemaakt via het Adobe Primetime TVE-dashboard.
   * De eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker wordt ingeschakeld via het Adobe Primetime TVE-dashboard.
   * De tv-provider van de gebruiker wordt niet gedegradeerd via het Adobe Primetime TVE-dashboard.
   * De AccessEnabler iOS/tvOS SDK heeft de SAML-reactie van de tv-provider van de gebruiker ontvangen van het Video Subscriber Account Framework.

   **<u>ProUiteinde:</u>** Deze tweede stap zal geen andere callbacks, behalve [&#x200B; setRequestorComplete &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) callback teweegbrengen, aangezien de authentificatie niet uitdrukkelijk door de toepassing in werking werd gesteld.


1. De toepassing zou [&#x200B; de authentificatiestatus &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn) moeten controleren.

   **Belangrijk:** Deze derde stap kon een [&#x200B; geavanceerde foutencode &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) teweegbrengen die voor het werkschema van Apple SSO specifiek is, in het geval **één van het volgende waar is**:

   * ***VSA403** - de gebruiker wordt binnen ondertekend aan zijn rekening van TV Provider bij
het systeemniveau van het apparaat, maar de machtiging van de tv-provider van de gebruiker is
geweigerd voor de toepassing.
   * ***VSA404** - de gebruiker wordt binnen ondertekend aan zijn rekening van TV Provider bij
het systeemniveau van het apparaat, maar de machtiging van de tv-provider van de gebruiker
is onbepaald voor de toepassing.
   * ***APPL \_ERROR** - de gebruiker wordt binnen ondertekend aan zijn Dienstverlener van TV
account op apparaatsysteemniveau, maar de communicatie tussen
De AccessEnabler iOS/tvOS SDK en de Video Subscriber Account
Er is een fout opgetreden.

   **Belangrijk:** Deze derde stap zal [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 0 teweegbrengen, voor het geval **één van het volgende waar is**:

   * De gebruiker wordt niet aangemeld bij zijn tv-provider-account op het systeemniveau van het apparaat of via een regelmatige verificatiestroom.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau of via een regelmatige verificatiestroom, maar het verificatietoken van de tv-provider van de gebruiker is geslaagd.
   * De gebruiker wordt aangemeld bij zijn tv-provider-account op apparaatsysteemniveau of via een regelmatige verificatiestroom, maar de integratie van de tv-provider van de gebruiker met de toepassing wordt uitgeschakeld via het Adobe Primetime TVE-dashboard.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker is uitgeschakeld via het Adobe Primetime TVE-dashboard.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
   * De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar er is een fout opgetreden tijdens de communicatie tussen de AccessEnabler iOS/tvOS SDK en het Video Subscriber Account Framework.

   **Belangrijk:** Deze derde stap zal [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 1 teweegbrengen, voor het geval **alle hierboven vals zijn.**


1. De toepassing zou de authentificatie [&#128279;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) moeten initialiseren in het geval dat de vorige controle van de authentificatiestatus [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 0 teweegbracht.

   **<u>ProUiteinde:</u>** voer één van volgende AccessEnabler iOS/tvOS SDK API [&#x200B; getAuthentication &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) of [&#x200B; getAuthentication uit:filter &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Belangrijk:** Deze vierde stap kon een [&#x200B; geavanceerde foutencode &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) teweegbrengen die voor het werkschema van Apple SSO specifiek is, in het geval **één van het volgende waar is**:

   * ***VSA403*** - de toestemming van TV van de gebruiker wordt van de Leverancier ontkend voor de toepassing.
   * ***VSA404*** - de toestemming van de leverancier van TV van de gebruiker is onbepaald voor de toepassing.
   * ***VSA503*** - de communicatie tussen AccessEnabler iOS/tvOS SDK en het Video Kader van de Rekening van de Abonnee ontmoette een fout.
   * ***N003*** - de gebruiker selecteerde de &quot;Andere optie van de Leverancier van TV&quot;van de plukker van MVPD van Apple.
   * ***N004*** - de gebruiker selecteerde een Leverancier van TV van de plukker van Apple MVPD, die niet (integratie of Enige Sign-On gehandicapt) door de huidige aanvrager wordt gesteund.
   * ***N005*** - de gebruiker besloot om de regelmatige plukker van MVPD of van Apple MVPD te annuleren.

   **Belangrijk:** Deze vierde stap zou terug naar de regelmatige authentificatiestroom vallen, door [&#x200B; displayProviderDialog &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) callback en **één** van de bovengenoemde [&#x200B; geavanceerde foutencodes &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) in werking te stellen, voor het geval **één van het bovengenoemde is waar**.

   **Belangrijk:** Deze vierde stap zou terug naar de regelmatige authentificatiestroom vallen, door [&#x200B; navigateToUrl &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) of [&#x200B; navigateToUrl:useSVC &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) callback en **niets** van de bovengenoemde [&#x200B; geavanceerde foutencodes &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) te teweegbrengen, in het geval de gebruiker een leverancier van TV selecteerde, die geen Apple SSO steunt, maar aanwezig is in de Apple MVPD-kiezer.

   **<u>ProUiteinde:</u>** AccessEnabler iOS/tvOS SDK roept stil [&#x200B; setSelectedProvider &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) API, voor het geval de gebruiker een leverancier van TV selecteerde, die geen Apple SSO steunt, maar in de plukker van Apple MVPD aanwezig is.

   **Belangrijk:** Deze vierde stap zou proberen om het profiel van Apple SSO voor een teken van de Adobe authentificatie stil te ruilen, voor het geval **alle hierboven vals** zijn en **elk van het volgende is waar**:

   * De machtiging van de tv-provider van de gebruiker wordt verleend voor de toepassing.
   * De gebruiker is aangemeld / momenteel aangemeld bij de TV Provider-account op apparaatsysteemniveau.
   * De AccessEnabler iOS/tvOS SDK heeft de tv-provider-id van de gebruiker ontvangen via het Video Subscriber Account Framework.
   * De integratie van de tv-provider van de gebruiker met de toepassing wordt mogelijk gemaakt via het Adobe Primetime TVE-dashboard.
   * De eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker wordt ingeschakeld via het Adobe Primetime TVE-dashboard.
   * De tv-provider van de gebruiker wordt niet gedegradeerd via het Adobe Primetime TVE-dashboard.
   * De AccessEnabler iOS/tvOS SDK heeft de SAML-reactie van de tv-provider van de gebruiker ontvangen van het Video Subscriber Account Framework.

   **<u>ProUiteinde:</u>** Deze vierde stap zal [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus) callback, ongeacht *status* resultaat teweegbrengen, aangezien de authentificatie uitdrukkelijk door de toepassing in werking werd gesteld.

### Metagegevens {#apple-sso-cookbook-iostvos-sdk-metadata}

De toepassing heeft de optie om te bepalen als de authentificatie als resultaat van aanmelding door de Partner SSO of niet is gebeurd, gebruikend &quot;*tokenSource&quot;* [&#x200B; gebruikersmeta-gegevens &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) API van AccessEnabler iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Afmelden {#apple-sso-cookbook-iostvos-sdk-logout}

Het [&#x200B; Video Kader van de Rekening van de Abonnee van de Abonnee &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) verstrekt geen API aan programmatically logout mensen die binnen aan hun de leveranciersrekening van TV op het niveau van het apparatensysteem hebben ondertekend. Daarom moet de eindgebruiker zich expliciet afmelden bij *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS, anders wordt de aanmelding niet volledig van kracht. De andere optie die de gebruiker zou hebben, is het intrekken van de machtiging om de abonnementsgegevens van de gebruiker te openen in het gedeelte met specifieke toepassingsinstellingen (toegang tot tv-provider-machtiging).

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van AccessEnabler iOS/tvOS SDK [&#x200B; logout &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) API uit.

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg de stappen hieronder voor de implementatie/s tvOS.

* De toepassing zou de logout [&#128279;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) van AccessEnabler iOS/tvOS SDK moeten in werking stellen . Dit zou het opschonen van sessies aan MVPD-zijde niet vergemakkelijken.
* De toepassing zou de gebruiker moeten instrueren/ertoe aanzetten om zich uitdrukkelijk uit *`Settings -> Accounts -> TV Provider`* op tvOS slechts in te tekenen voor het geval [*VSA203* statuscode &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) wordt teweeggebracht.

>[!TIP]
>
> **<u>ProTip:</u>** volg de stappen hieronder voor de implementatie/s iOS/iPadOS.

* De toepassing zou de logout [&#128279;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) van AccessEnabler iOS/tvOS SDK moeten in werking stellen . Dit zou het opschonen van sessies aan MVPD-zijde vergemakkelijken.
* De toepassing zou de gebruiker moeten instrueren/ertoe aanzetten om zich uitdrukkelijk uit *`Settings -> TV Provider`* op iOS/iPadOS slechts in het geval [*VSA203* statuscode wordt teweeggebracht &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) af te melden.
