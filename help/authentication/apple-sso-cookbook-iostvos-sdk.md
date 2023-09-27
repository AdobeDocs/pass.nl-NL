---
title: Apple SSO Cookbook (iOS/tvOS SDK)
description: Apple SSO Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#Introduction}

De Adobe Primetime Authentication AccessEnabler iOS/tvOS SDK kan platform Single Sign-On (SSO)-verificatie ondersteunen voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd via wat we Apple SSO-workflow noemen.

Dit document fungeert als een uitbreiding op de bestaande AccessEnabler iOS/tvOS SDK-documentatie, die u kunt vinden [hier](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Cookbook {#Cookbook}

Om te kunnen profiteren van de Apple SSO-gebruikerservaring, moet één toepassing de AccessEnabler iOS/tvOS SDK integreren en de reeks tips volgen die hieronder wordt weergegeven.

</br>

### Vereisten {#Prerequisites}

</br>

#### Machtiging

>[!TIP]
>
> **<u>Pro Tip:</u>** Om toegang te hebben tot de abonnementsinformatie van de gebruiker, moet de gebruiker de toepassingstoestemming geven te werk te gaan, gelijkend op het verlenen van toegang tot de camera of microfoon van het apparaat. Deze machtiging moet per toepassing worden aangevraagd en het apparaat slaat de selectie van de gebruiker op. Houd er rekening mee dat de gebruiker zijn beslissing kan wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of naar de sectie te gaan vanuit *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS.

>[!TIP]
>
> **<u>Pro Tip:</u>** We raden u aan de gebruiker om toestemming te vragen wanneer de toepassing de voorgrondstatus activeert, maar dit is slechts een suggestie, omdat de toepassing kan controleren op [toegangsrechten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsgegevens van de gebruiker op om het even welk punt alvorens gebruikersauthentificatie te vereisen. De AccessEnabler iOS/tvOS SDK-API&#39;s vragen automatisch om toestemming van de gebruiker wanneer deze deze nodig heeft.

>[!TIP]
>
> **<u>Pro Tip:</u>** Als de gebruiker geen toegang verleent tot zijn abonnementsinformatie of als de communicatie met het VideoKader van de Rekening van de Abonnee, dan zal AccessEnabler iOS/tvOS SDK aan de regelmatige authentificatiestroom terugvallen.

>[!TIP]
>
> **<u>Pro Tip:</u>** We raden gebruikers aan die geen toestemming geven om abonnementsgegevens te openen, te stimuleren door de voordelen van Single Sign-On (SSO)-gebruikerservaring uit te leggen. Houd er rekening mee dat de gebruiker zijn beslissing kan wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of naar de sectie te gaan vanuit *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS.


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

</br>

#### Callbacks

>[!TIP]
>
> **<u>Pro Tip:</u>** Voer de volgende lijst van uit [callbacks](/help/authentication/iostvos-sdk-api-reference.md) die specifiek zijn voor de Apple SSO-workflow.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback geactiveerd wanneer de Apple MVPD-kiezer wordt geopend.
- [*TVProviderDialog negeren*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback die wordt geactiveerd wanneer de Apple MVPD-kiezer wordt gesloten.

</br>

#### Fout bij rapporteren

>[!TIP]
>
> **<u>Pro Tip:</u>** Voer de volgende lijst van uit [geavanceerde foutcodes](/help/authentication/error-reporting.md) die specifiek zijn voor de Apple SSO-workflow.

- ***N003*** - De gebruiker heeft de optie Andere tv-provider geselecteerd in de Apple MVPD-kiezer.
- ***N004*** - De gebruiker heeft een tv-provider geselecteerd in de Apple MVPD-kiezer. Deze wordt niet ondersteund (integratie of Single Sign-On uitgeschakeld) door de huidige aanvrager.
- ***N005*** - De gebruiker heeft besloten de normale MVPD-kiezer of de Apple MVPD-kiezer te annuleren.
- ***VSA403*** - De machtiging van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
- ***VSA404*** - De toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
- ***VSA503*** - De metagegevensaanvraag van de account van de videoabonnee is mislukt. Meer context is te vinden in het dialoogvenster *message* veld.
- ***AAPL / APPL_ERROR*** - De metagegevensaanvraag van de account van de videoabonnee is mislukt. Meer context is te vinden in het dialoogvenster *details* veld.

</br>

### Verificatie {#Authentication}

>[!TIP]
>
> **<u>Tip:</u>** Voer de onderstaande stappen uit voor de iOS/iPadOS/tvOS-implementatie(s).

1. De aanvraag moet [initialiseren](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) de AccessEnabler iOS/tvOS SDK.
1. De aanvraag moet [de huidige aanvrager-id instellen](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Belangrijk:** Deze tweede stap kan een [geavanceerde foutcode](/help/authentication/error-reporting.md) die specifiek is voor de Apple SSO-workflow, voor het geval dat **is een van de volgende uitspraken waar**:

   - ***VSA403*** - De machtiging van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
   - ***VSA404*** - De toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
   - ***APPL*** - Er is een fout opgetreden tijdens de communicatie tussen de AccessEnabler iOS/tvOS SDK en het Video Subscriber Account-framework.

   In deze tweede stap wordt gepoogd het Apple SSO-profiel zonder toezicht uit te wisselen voor een Adobe-verificatietoken, voor het geval **alle bovenstaande uitspraken zijn onwaar** en **alle volgende uitspraken zijn waar**:

   - De machtiging van de tv-provider van de gebruiker wordt verleend voor de toepassing.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau.
   - De AccessEnabler iOS/tvOS SDK heeft de tv-provider-id van de gebruiker ontvangen via het accountframework voor videoabonnees.
   - De integratie van de tv-provider van de gebruiker met de toepassing wordt mogelijk gemaakt via het Adobe Primetime TVE-dashboard.
   - De eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker wordt ingeschakeld via het Adobe Primetime TVE-dashboard.
   - De tv-provider van de gebruiker wordt niet gedegradeerd via het Adobe Primetime TVE-dashboard.
   - De AccessEnabler iOS/tvOS SDK heeft de SAML-reactie van de tv-provider van de gebruiker ontvangen van het accountframework van de videoabonnee.

   **<u>Pro Tip:</u>** Deze tweede stap zal geen andere callbacks teweegbrengen naast [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) callback, aangezien de authentificatie niet uitdrukkelijk door de toepassing in werking werd gesteld.

1. De aanvraag moet [de verificatiestatus controleren](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Belangrijk:** Deze derde stap kan een [geavanceerde foutcode](/help/authentication/error-reporting.md) die specifiek is voor de Apple SSO-workflow, voor het geval dat **is een van de volgende uitspraken waar**:

   - ***VSA403** - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
   - ***VSA404** - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
   - ***APPL\_ERROR** - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar er is een fout opgetreden tijdens de communicatie tussen de AccessEnabler iOS/tvOS SDK en het Video Subscriber Account-framework.

   **Belangrijk:** Deze derde stap activeert de [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 0, in geval van **is een van de volgende uitspraken waar**:

   - De gebruiker wordt niet aangemeld bij zijn tv-provider-account op het systeemniveau van het apparaat of via een regelmatige verificatiestroom.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau of via een regelmatige verificatiestroom, maar het verificatietoken van de tv-provider van de gebruiker is geslaagd.
   - De gebruiker wordt aangemeld bij zijn tv-provider-account op apparaatsysteemniveau of via een regelmatige verificatiestroom, maar de integratie van de tv-provider van de gebruiker met de toepassing wordt uitgeschakeld via het Adobe Primetime TVE-dashboard.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker is uitgeschakeld via het Adobe Primetime TVE-dashboard.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar de toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
   - De gebruiker is aangemeld bij zijn tv-provider-account op apparaatsysteemniveau, maar er is een fout opgetreden tijdens de communicatie tussen de AccessEnabler iOS/tvOS SDK en het Video Subscriber Account-framework.

   **Belangrijk:** Deze derde stap activeert de [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 1, in geval van **al deze elementen zijn onjuist.**


1. De aanvraag moet [de verificatie initialiseren](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) als de vorige verificatie-statuscontrole de [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback met *status* gelijk aan 0.

   **<u>Pro Tip:</u>** Implementeer een van de volgende AccessEnabler iOS/tvOS SDK-API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) of [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Belangrijk:** Deze vierde stap kan een [geavanceerde foutcode](/help/authentication/error-reporting.md) die specifiek is voor de Apple SSO-workflow, voor het geval dat **is een van de volgende uitspraken waar**:

   - ***VSA403*** - De machtiging van de tv-provider van de gebruiker wordt geweigerd voor de toepassing.
   - ***VSA404*** - De toestemming van de tv-provider van de gebruiker is onbepaald voor de toepassing.
   - ***VSA503*** - Er is een fout opgetreden tijdens de communicatie tussen de AccessEnabler iOS/tvOS SDK en het Video Subscriber Account-framework.
   - ***N003*** - De gebruiker heeft de optie Andere tv-provider geselecteerd in de Apple MVPD-kiezer.
   - ***N004*** - De gebruiker heeft een tv-provider geselecteerd in de Apple MVPD-kiezer. Deze wordt niet ondersteund (integratie of Single Sign-On uitgeschakeld) door de huidige aanvrager.
   - ***N005*** - De gebruiker heeft besloten de normale MVPD-kiezer of de Apple MVPD-kiezer te annuleren.

   **Belangrijk:** Deze vierde stap zou terug naar de regelmatige authentificatiestroom, door het teweegbrengen van [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) callback en **één** van het bovenstaande [geavanceerde foutcodes](/help/authentication/error-reporting.md), in geval van **een van de bovenstaande punten is waar**.

   **Belangrijk:** Deze vierde stap zou terug naar de regelmatige authentificatiestroom, door het teweegbrengen van [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) of [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback en **none** van het bovenstaande [geavanceerde foutcodes](/help/authentication/error-reporting.md), als de gebruiker een tv-provider heeft geselecteerd die geen ondersteuning biedt voor Apple SSO, maar wel aanwezig is in de Apple MVPD-kiezer.

   **<u>Pro Tip:</u>** De AccessEnabler iOS/tvOS SDK roept de [setSelectedProvder](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, voor het geval de gebruiker een tv-provider heeft geselecteerd die geen ondersteuning biedt voor Apple SSO, maar wel aanwezig is in de Apple MVPD-kiezer.

   **Belangrijk:** In deze vierde stap wordt geprobeerd stilzwijgend het Apple SSO-profiel uit te wisselen voor een Adobe-verificatietoken, voor het geval dat **alle bovenstaande uitspraken zijn onwaar** en **alle volgende uitspraken zijn waar**:

   - De machtiging van de tv-provider van de gebruiker wordt verleend voor de toepassing.
   - De gebruiker is aangemeld / momenteel aangemeld bij de TV Provider-account op apparaatsysteemniveau.
   - De AccessEnabler iOS/tvOS SDK heeft de tv-provider-id van de gebruiker ontvangen via het accountframework voor videoabonnees.
   - De integratie van de tv-provider van de gebruiker met de toepassing wordt mogelijk gemaakt via het Adobe Primetime TVE-dashboard.
   - De eenmalige aanmelding bij de toepassing door de tv-provider van de gebruiker wordt ingeschakeld via het Adobe Primetime TVE-dashboard.
   - De tv-provider van de gebruiker wordt niet gedegradeerd via het Adobe Primetime TVE-dashboard.
   - De AccessEnabler iOS/tvOS SDK heeft de SAML-reactie van de tv-provider van de gebruiker ontvangen van het accountframework van de videoabonnee.



>**<u>Pro Tip:</u>** Deze vierde stap activeert de [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) callback, ongeacht *status* resultaat, aangezien de authentificatie uitdrukkelijk door de toepassing in werking werd gesteld.


</br>

### Metagegevens {#Metadata}

De toepassing heeft de optie om te bepalen of de authentificatie als resultaat van login door platform SSO of niet is gebeurd, gebruikend &quot;*tokenSource&quot;* [gebruikersmetagegevens](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API van de AccessEnabled iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Afmelden {#Logout}

De [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) framework biedt geen API om personen die zich op het niveau van het apparaatsysteem hebben aangemeld bij hun tv-provider, via programmacode af te melden. Om de logout volledig van kracht te laten worden, moet de eindgebruiker zich daarom expliciet afmelden *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS. De andere optie die de gebruiker zou hebben, is het intrekken van de machtiging om de abonnementsgegevens van de gebruiker te openen in het gedeelte met specifieke toepassingsinstellingen (toegang tot tv-provider-machtiging).

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via de AccessEnabler iOS/tvOS SDK [afmelden](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Pro Tip:</u>** Volg de onderstaande stappen voor de implementatie(s) van tvOS.

- De aanvraag moet [start de logout](/help/authentication/iostvos-sdk-api-reference.md#logout) van de AccessEnabler iOS/tvOS SDK. Dit zou sessieopruiming aan de MVPD-zijde niet vergemakkelijken.
- De toepassing moet de gebruiker de instructie geven/vragen zich expliciet af te melden *`Settings -> Accounts -> TV Provider`* alleen op tvOS in geval van [*VSA203* statuscode wordt geactiveerd](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro Tip:</u>** Voer de onderstaande stappen uit voor de iOS/iPadOS-implementatie(s).

- De aanvraag moet [start de logout](/help/authentication/iostvos-sdk-api-reference.md#logout) van de AccessEnabler iOS/tvOS SDK. Dit zou zittingsschoonmaak aan de kant van MVPD vergemakkelijken.
- De toepassing moet de gebruiker de instructie geven/vragen zich expliciet af te melden *`Settings -> TV Provider`* alleen in geval op iOS/iPadOS [*VSA203* statuscode wordt geactiveerd](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
