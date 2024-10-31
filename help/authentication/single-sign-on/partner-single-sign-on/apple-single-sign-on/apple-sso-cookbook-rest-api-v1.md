---
title: Apple SSO Cookbook (REST API V1)
description: Apple SSO Cookbook (REST API V1)
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1473'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De Adobe Pass Authentication REST API V1 biedt ondersteuning voor Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan de bestaande REST API V1 documentatie, die [ hier ](/help/authentication/rest-api-reference.md) kan worden gevonden.

## Cookbook {#apple-sso-cookbook-rest-api-v1-cookbook}

Om van de gebruikerservaring van Apple te profiteren SSO, moet de toepassing het [ Video Kader van de Rekening van de Abonnee van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) integreren dat door Apple wordt ontwikkeld, terwijl voor de Communicatie van de Authentificatie van Adobe Pass REST API V1, het de opeenvolging van hieronder voorgestelde stappen moet volgen.

### Machtiging {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>ProUiteinde:</u>** de het stromen toepassing moet toegang tot de het abonnementinformatie van de gebruiker verzoeken die op apparatenniveau wordt bewaard, waarvoor de gebruiker de toepassingstoestemming moet geven te werk te gaan, gelijkend op het verlenen van toegang tot de camera of microfoon van het apparaat. Deze toestemming moet per toepassing worden gevraagd gebruikend het Kader van de Rekening van de Abonnee van Apple [ Video ](https://developer.apple.com/documentation/videosubscriberaccount) en het apparaat zal de selectie van de gebruiker bewaren.

>[!TIP]
>
> **<u>ProUiteinde:</u>** wij adviseren gebruikers te stimuleren die weigeren om toestemming te geven om tot abonnementinformatie toegang te hebben door de voordelen van de enige sign-on gebruikerservaring van Apple uit te leggen, maar zich ervan bewust te zijn dat de gebruiker hun besluit kan veranderen door naar de toepassingsmontages (de toestemmingstoegang van TV Provider) of naar *`Settings -> TV Provider`* op iOS en iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS te gaan.

>[!TIP]
>
> **<u>ProUiteinde:</u>** wij adviseren het verzoeken van de toestemming van de gebruiker wanneer de toepassing de voorgrondstaat ingaat, omdat de toepassing [ toestemming kan controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementsinformatie van de gebruiker op om het even welk punt toegang te hebben alvorens gebruikersauthentificatie te vereisen.

### Verificatie {#apple-sso-cookbook-rest-api-v1-authentication}

* [Is er een geldig teken van de Adobe authentificatie?](#step1)
* [Is de gebruiker het programma geopend via Partner SSO?](#step2)
* [Configuratie Adobe ophalen](#step3)
* [De werkstroom van SSO van de Partner met Adobe config in werking stellen](#step4)
* [Is gebruikersaanmelding geslaagd?](#step5)
* [Vraag een profielverzoek van Adobe voor geselecteerde MVPD](#step6)
* [Door:sturen het verzoek van de Adobe aan Partner SSO om het profiel te verkrijgen](#step7)
* [Uitwisseling het profiel van Partner SSO voor een teken van de Adobe authentificatie](#step8)
* [Is het genereren van een Adobe token gelukt?](#step9)
* [Regelmatige verificatieworkflow starten](#step10)
* [Doorgaan met autorisatiestromen](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Stap: &quot;Is er een geldig token voor Adobe-verificatie?&quot; {#step1}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de 2} Symbolische dienst van de Authentificatie van de Controle van Adobe Pass uit ](/help/authentication/check-authentication-token.md) API.[

#### Stap: &quot;Is de gebruiker het programma geopend via Partner SSO?&quot; {#step2}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) uit.

* De toepassing zou [ toestemming moeten controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
* De toepassing zou a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening moeten voorleggen.
* De toepassing zou de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie moeten wachten en verwerken.

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg het codefragment en let extra aandacht op de commentaren.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Stap: &quot;Configuratie Adobe ophalen&quot; {#step3}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de Authentificatie van Adobe Pass [ uit verleent de dienst van MVPD Lijst ](/help/authentication/provide-mvpd-list.md) API.

>[!TIP]
>
> **<u>ProTip:</u>** gelieve zich van de eigenschappen MVPD bewust te zijn: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* en extra aandacht te besteden aan de commentaren die in codefragmenten van andere stappen worden voorgesteld.

#### Stap &quot;De werkschema van SSO van de Partner met Adobe config&quot;initialiseren {#step4}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) uit.

* De toepassing zou [ toestemming moeten controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
* De toepassing zou a [ afgevaardigde ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor VSAccountManager moeten verstrekken.
* De toepassing zou a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening moeten voorleggen.
* De toepassing zou de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie moeten wachten en verwerken.

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg het codefragment en let extra aandacht op de commentaren.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Stap: &quot;Is gebruikersaanmelding geslaagd?&quot; {#step5}

>[!TIP]
>
> **<u>ProUiteinde:</u>** gelieve zich van het codefragment van [ bewust te zijn &quot;de werkschema&#39;s van SSO van de Partner met Adobe config&quot;beginnen ](#step4) stap. De gebruikersaanmelding is geslaagd als de waarde van *`vsaMetadata!.accountProviderIdentifier`* geldig is en de huidige datum niet de waarde van *`vsaMetadata!.authenticationExpirationDate`* heeft bereikt.

#### Stap &quot;verkrijg een profielverzoek van Adobe voor geselecteerde MVPD&quot; {#step6}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de 2} het Verzoek van het Profiel van Adobe Pass Authentificatie ](/help/authentication/retrieve-profilerequest.md) API dienst uit.[

>[!TIP]
>
> **<u>ProTip:</u>** gelieve zich ervan bewust te zijn dat het leveranciersidentificatie die van het Video Kader van de Rekening van de Abonnee wordt verkregen *`platformMappingId`* in termen van de configuratie van de Authentificatie van Adobe Pass vertegenwoordigt. Daarom moet de toepassing de MVPD identiteitskaart bezitswaarde bepalen, gebruikend de *`platformMappingId`* waarde, door het middel van de Authentificatie van Adobe Pass [ verstrekt de dienst MVPD Lijst ](/help/authentication/provide-mvpd-list.md) API.

#### Stap: &quot;Door:sturen het verzoek van de Adobe aan Partner SSO om het profiel&quot;te verkrijgen {#step7}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) uit.


* De toepassing zou [ toestemming moeten controleren om tot ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de het abonnementinformatie van de gebruiker toegang te hebben en slechts te werk te gaan als de gebruiker het toeliet.
* De toepassing zou a [ verzoek ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening moeten voorleggen.
* De toepassing zou de [ meta-gegevens ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie moeten wachten en verwerken.

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg het codefragment en let extra aandacht op de commentaren.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Stap: &quot;Uitwisseling het profiel van Partner SSO voor een teken van de Adobe authentificatie&quot; {#step8}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de 2} Symbolische dienst van de Uitwisseling van de Authentificatie van Adobe Pass ](/help/authentication/token-exchange.md) API uit.[

>[!TIP]
>
> **<u>ProUiteinde:</u>** gelieve zich van het codefragment van [ bewust te zijn &quot;door:sturen het verzoek van de Adobe aan Partner SSO om het profiel&quot;te verkrijgen ](#step7) stap. Dit *`vsaMetadata!.samlAttributeQueryResponse!`* vertegenwoordigt *`SAMLResponse`*, die op [ Symbolische Uitwisseling ](/help/authentication/token-exchange.md) moet worden overgegaan en koordmanipulatie en het coderen vereist (*Base64* gecodeerd en *URL* daarna gecodeerd) alvorens de vraag te maken.

#### Stap: &quot;Wordt het teken van de Adobe met succes geproduceerd?&quot; {#step9}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de Authentificatie van Adobe Pass [ Symbolische Uitwisseling ](/help/authentication/token-exchange.md) succesvolle reactie uit, die a *`204 No Content`* zal zijn, erop wijzend dat het teken met succes werd gecreeerd en klaar is om voor de vergunningsstromen te worden gebruikt.

#### Stap: &quot;Reguliere verificatieworkflow starten&quot; {#step10}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van het 2} Verzoek van de Code van de Authentificatie van Adobe Pass ](/help/authentication/registration-code-request.md) uit, [ initieer Authentificatie ](/help/authentication/initiate-authentication.md) en [ wint het Symbolische van de Authentificatie ](/help/authentication/retrieve-authentication-token.md) of [ Symbolische van de Authentificatie van de Controle ](/help/authentication/check-authentication-token.md) API diensten terug.[

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg de stappen hieronder voor de implementatie/s tvOS.

* De toepassing zou [ een registratiecode ](/help/authentication/registration-code-request.md) moeten verkrijgen en het aan het eind voorleggen - gebruiker op het 1ste apparaat (scherm).
* De toepassing zou [ opiniepeiling moeten beginnen om de authentificatiestatus ](/help/authentication/retrieve-authentication-token.md) op het 1ste apparaat (scherm) te erkennen nadat de registratiecode wordt verkregen.
* Een andere toepassing zou authentificatie ](/help/authentication/initiate-authentication.md) op een 2de apparaat (scherm) moeten in werking stellen wanneer de registratiecode wordt gebruikt.[
* De toepassing zou [ opiniepeiling ](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) moeten tegenhouden wanneer het authentificatietoken wordt geproduceerd.

>[!TIP]
>
> **<u>ProTip:</u>** volg de stappen hieronder voor de implementatie/s iOS/iPadOS.

* De toepassing zou een registratiecode ](/help/authentication/registration-code-request.md) moeten verkrijgen die niet aan het eind - gebruiker op het 1ste apparaat (scherm) zou moeten worden voorgesteld.[
* De toepassing zou authentificatie ](/help/authentication/initiate-authentication.md) op het eerste apparaat (scherm) in werking moeten stellen gebruikend de registratiecode en a [ WKWebView ](https://developer.apple.com/documentation/webkit/wkwebview) of a [ SFSafariViewController ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) component.[
* De toepassing zou [ het opiniepeilen moeten beginnen om de authentificatiestatus ](/help/authentication/retrieve-authentication-token.md) op het 1ste apparaat (scherm) na [ WKWebView ](https://developer.apple.com/documentation/webkit/wkwebview) of [ SFSafariViewController ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) componentensluiten te kennen.
* De toepassing zou [ opiniepeiling ](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) moeten tegenhouden wanneer het authentificatietoken wordt geproduceerd.

#### Stap: &quot;Doorgaan met vergunningsstromen&quot; {#step11}

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de Authentificatie van Adobe Pass [ in werking stelt Vergunning ](/help/authentication/initiate-authorization.md) en [ verkrijgt de Korte Token van Media ](/help/authentication/obtain-short-media-token.md) API diensten.

### Afmelden {#apple-sso-cookbook-rest-api-v1-logout}

Het [ Video Kader van de Rekening van de Abonnee van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) verstrekt geen API aan programmatically logout mensen die binnen aan hun de leveranciersrekening van TV op het niveau van het apparatensysteem hebben ondertekend. Daarom moet de eindgebruiker zich expliciet afmelden bij *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS, anders wordt de aanmelding niet volledig van kracht. De andere mogelijkheid die de gebruiker zou hebben, is het intrekken van de machtiging om de abonnementsgegevens van de gebruiker te openen in het gedeelte met specifieke toepassingsinstellingen (toegang tot tv-provider).

>[!TIP]
>
> **<u>Uiteinde:</u>** voer dit door het middel van de Authentificatie van Adobe Pass [ de Vraag van de Metagegevens van de Gebruiker ](/help/authentication/user-metadata.md) en [ Logout ](/help/authentication/initiate-logout.md) API diensten uit.

>[!TIP]
>
> **<u>ProUiteinde:</u>** volg de stappen hieronder voor de implementatie/s tvOS.

* De toepassing zou moeten bepalen als de authentificatie als resultaat van aanmelding door partner SSO of niet is gebeurd, gebruikend &quot;*tokenSource&quot;* [ gebruikersmeta-gegevens ](/help/authentication/user-metadata.md) van de dienst van de Authentificatie van Adobe Pass.
* De toepassing zou de gebruiker moeten instrueren/ertoe aanzetten om uit *`Settings -> Accounts -> TV Provider`* op tvOS **slechts** uitdrukkelijk te ondertekenen in het geval dat de *&quot;tokenSource&quot;* waarde aan &quot;*Apple&quot; gelijk is.*
* De toepassing zou de logout ](/help/authentication/initiate-logout.md) van de dienst van de Authentificatie van Adobe Pass moeten in werking stellen gebruikend een directe vraag van HTTP. [ Dit zou sessieopruiming aan de MVPD-zijde niet vergemakkelijken.

>[!TIP]
>
> **<u>ProTip:</u>** volg de stappen hieronder voor de implementatie/s iOS/iPadOS.

* De toepassing zou moeten bepalen als de authentificatie als resultaat van aanmelding door partner SSO of niet is gebeurd, gebruikend &quot;*tokenSource&quot;* [ gebruikersmeta-gegevens ](/help/authentication/user-metadata.md) van de dienst van de Authentificatie van Adobe Pass.
* De toepassing zou de gebruiker moeten instrueren/ertoe aanzetten om uitdrukkelijk uit *`Settings -> TV Provider`* op iOS/iPadOS **slechts** te ondertekenen in het geval dat de *&quot;tokenSource&quot;* waarde aan *&quot;Apple&quot;* gelijk is.
* De toepassing zou logout ](/help/authentication/initiate-logout.md) van de dienst van de Authentificatie van Adobe Pass moeten in werking stellen gebruikend a [ WKWebView ](https://developer.apple.com/documentation/webkit/wkwebview) of a [ SFSafariViewController ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) component. [ Dit zou zittingsschoonmaak aan de kant van MVPD vergemakkelijken.
