---
title: Apple SSO Cookbook (REST API)
description: Apple SSO Cookbook (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#Introduction}

De Adobe Pass Authentication REST API kan platform Single Sign-On (SSO)-verificatie ondersteunen voor eindgebruikers van clienttoepassingen die op iOS, iPadOS of tvOS worden uitgevoerd via wat we de Apple SSO-workflow noemen.

Dit document fungeert als een uitbreiding op de bestaande REST API-documentatie, die u kunt vinden [hier](/help/authentication/rest-api-reference.md).

</br>

## Cookbooks {#Cookbooks}

Om van de Apple SSO-gebruikerservaring te kunnen profiteren, moet één toepassing de [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) door Apple ontwikkeld raamwerk, maar wat de communicatie van de Adobe Pass Authentication REST API betreft, moet het de reeks tips volgen die hieronder wordt weergegeven.

</br>

### Verificatie {#Authentication}

- [Is er een geldig teken van de Adobe authentificatie?](#Is_there_a_valid_Adobe_authentication_token)
- [Is de gebruiker aangemeld via Platform SSO?](#Is_the_user_logged_in_via_Platform_SSO)
- [Configuratie Adobe ophalen](#Fetch_Adobe_configuration)
- [Platform SSO-workflow starten met Adobe config](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [Is gebruikersaanmelding geslaagd?](#Is_user_login_successful)
- [Vraag een profielverzoek van Adobe voor geselecteerde MVPD](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Het verzoek van de Adobe doorsturen naar Platform SSO om het profiel te verkrijgen](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Exchange the Platform SSO profile for an Adobe authentication token](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Is het genereren van een Adobe token gelukt?](#Is_Adobe_token_generated_successfully)
- [Tweede workflow voor schermverificatie starten](#Initiate_second_screen_authentication_workflow)
- [Doorgaan met autorisatiestromen](#Proceed_with_authorization_flows)



![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)

</br>

#### Stap: &quot;Is er een geldig token voor Adobe-verificatie?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tip:</u>** Deze implementeren via het medium [Adobe Pass-verificatie](/help/authentication/check-authentication-token.md) service.

</br>

#### Stap: &quot;Is de gebruiker aangemeld via Platform SSO?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Tip:</u>** Deze implementeren via het medium [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) kader.

- De toepassing moet controleren op [toegangsrechten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsgegevens van de gebruiker en ga alleen verder als de gebruiker dit heeft toegestaan.
- De aanvraag moet een [verzoek](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening.
- De toepassing moet wachten en de [metagegevens](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie.



>[!TIP]
>
> **<u>Pro Tip:</u>** Volg het codefragment en let extra op de commentaren.

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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

</br>

#### Stap: &quot;Configuratie Adobe ophalen&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Tip:</u>** Deze implementeren via het medium [Adobe Pass-verificatie](/help/authentication/provide-mvpd-list.md) service.


>[!TIP]
>
> **<u>Pro Tip:</u>** Houd rekening met de MVPD-eigenschappen: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* en extra aandacht besteden aan de opmerkingen in codefragmenten uit andere stappen.

</br>

#### Stap &quot;De werkschema van SSO van het Platform met Adobe config&quot;initialiseren {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Tip:</u>** Deze implementeren via het medium [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) kader.

- De toepassing moet controleren op [toegangsrechten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsgegevens van de gebruiker en ga alleen verder als de gebruiker dit heeft toegestaan.
- De toepassing moet een [gedelegeerde](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) voor VSAccountManager.
- De aanvraag moet een [verzoek](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening.
- De toepassing moet wachten en de [metagegevens](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie.



>[!TIP]
>
> **<u>Pro Tip:</u>** Volg het codefragment en let extra op de commentaren.


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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Stap: &quot;Is gebruikersaanmelding geslaagd?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Pro Tip:</u>** Houd rekening met het codefragment van het dialoogvenster [&quot;Platform SSO-workflow starten met Adobe config&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) stap. De gebruikersaanmelding is geslaagd voor het geval dat de *`vsaMetadata!.accountProviderIdentifier`* bevat een geldige waarde en de huidige datum is nog niet verstreken *`vsaMetadata!.authenticationExpirationDate`* waarde.

</br>

#### Stap &quot;verkrijg een profielverzoek van Adobe voor geselecteerde MVPD&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via het medium Adobe Pass Authentication [Profielaanvraag](/help/authentication/retrieve-profilerequest.md) service.

>[!TIP]
>
> **<u>Pro Tip:</u>** Houd er rekening mee dat de provider-id die wordt verkregen via het framework Video Subscriber Account de *`platformMappingId`* in termen van Adobe Pass-verificatieconfiguratie. Daarom moet de toepassing de MVPD identiteitskaart bezitswaarde bepalen, gebruikend *`platformMappingId`* waarde, via het medium Adobe Pass Authentication [MVPD-lijst opgeven](/help/authentication/provide-mvpd-list.md) service.

</br>

#### Stap: &quot;Verstuur het verzoek van de Adobe naar Platform SSO om het profiel te verkrijgen&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Tip:</u>** Deze implementeren via het medium [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) kader.


- De toepassing moet controleren op [toegangsrechten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) de abonnementsgegevens van de gebruiker en ga alleen verder als de gebruiker dit heeft toegestaan.
- De aanvraag moet een [verzoek](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) voor de informatie van de abonneerekening.
- De toepassing moet wachten en de [metagegevens](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informatie.



>[!TIP]
>
> **<u>Pro Tip:</u>** Volg het codefragment en let extra op de commentaren.

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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Stap: &quot;Exchange the Platform SSO profile for an Adobe authentication token&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via het medium Adobe Pass Authentication [Tokenuitwisseling](/help/authentication/token-exchange.md) service.


>[!TIP]
>
> **<u>Pro Tip:</u>** Houd rekening met het codefragment van het dialoogvenster [&quot;Het verzoek van de Adobe aan Platform SSO doorsturen om het profiel te verkrijgen&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) stap. Dit *`vsaMetadata!.samlAttributeQueryResponse!`* vertegenwoordigt de *`SAMLResponse`*, die moet worden doorgegeven [Tokenuitwisseling](/help/authentication/token-exchange.md) en vereist tekenreeksmanipulatie en -codering (*Base64* gecodeerd en *URL* gecodeerd nadien) alvorens de vraag te maken.

</br>

#### Stap: &quot;Wordt het teken van de Adobe met succes geproduceerd?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via de mediale Adobe Pass-verificatie [Tokenuitwisseling](/help/authentication/token-exchange.md) een succesvol antwoord , dat *`204 No Content`*, die aangeeft dat de token is gemaakt en klaar is om te worden gebruikt voor de machtigingsstromen.

</br>

#### Stap: &quot;De tweede workflow voor schermverificatie starten&quot; {#Initiate_second_screen_authentication_workflow}

**Belangrijk:** De terminologie van de &quot;Tweede workflow voor schermverificatie&quot; is geschikt voor AppleTV&#39;s, terwijl de terminologie &quot;Eerste workflow voor schermverificatie&quot; / &quot;Reguliere workflow voor verificatie&quot; geschikter zou zijn voor iPhones en iPads.


>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via het medium Adobe Pass Authentication

[Registratiecode-aanvraag](/help/authentication/registration-code-request.md), [Verificatie starten](/help/authentication/initiate-authentication.md) en [REST API-verificatietoken ophalen](/help/authentication/retrieve-authentication-token.md) of [Verificatietoken controleren](/help/authentication/check-authentication-token.md) diensten.


>[!TIP]
>
> **<u>Pro Tip:</u>** Volg de onderstaande stappen voor de implementatie(s) van tvOS.

- De aanvraag moet [een registratiecode verkrijgen](/help/authentication/registration-code-request.md) en deze aan de eindgebruiker presenteren op het eerste apparaat (scherm).
- De toepassing moet worden gestart [opiniepeiling ter erkenning van de verificatiestatus](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) nadat de registratiecode is verkregen.
- Een andere toepassing moet [verificatie starten](/help/authentication/initiate-authentication.md) op een tweede apparaat (scherm) wanneer de registratiecode wordt gebruikt.
- De toepassing moet stoppen [pollen](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) wanneer het authentificatietoken wordt geproduceerd.



>[!TIP]
>
> **<u>Pro Tip:</u>** Voer de onderstaande stappen uit voor de iOS/iPadOS-implementatie(s).

- De aanvraag moet [een registratiecode verkrijgen](/help/authentication/registration-code-request.md) die niet aan het eind - gebruiker op het 1ste apparaat (scherm) zou moeten worden voorgesteld.
- De aanvraag moet [verificatie starten](/help/authentication/initiate-authentication.md) op het eerste apparaat (scherm) met behulp van de registratiecode en een [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) of [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) component.
- De toepassing moet worden gestart [opiniepeiling om de verificatiestatus te kennen](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) na de [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) of de [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) wordt gesloten.
- De toepassing moet stoppen [pollen](/help/authentication/retrieve-authentication-token.md) op het eerste apparaat (scherm) wanneer het authentificatietoken wordt geproduceerd.

</br>

#### Stap: &quot;Doorgaan met vergunningsstromen&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via het medium Adobe Pass Authentication [Autorisatie starten](/help/authentication/initiate-authorization.md) en [Token voor korte media verkrijgen](/help/authentication/obtain-short-media-token.md) diensten.

</br>

### Afmelden {#Logout}

De [Video-abonneeaccount](https://developer.apple.com/documentation/videosubscriberaccount) framework biedt geen API om personen die zich op het niveau van het apparaatsysteem hebben aangemeld bij hun tv-provider, via programmacode af te melden. Om de logout volledig van kracht te laten worden, moet de eindgebruiker zich daarom expliciet afmelden *`Settings -> TV Provider`* op iOS/iPadOS of *`Settings -> Accounts -> TV Provider`* op tvOS. De andere mogelijkheid die de gebruiker zou hebben, is het intrekken van de machtiging om de abonnementsgegevens van de gebruiker te openen in het gedeelte met specifieke toepassingsinstellingen (toegang tot tv-provider).

>[!TIP]
>
> **<u>Tip:</u>** Implementeer dit via het medium Adobe Pass Authentication [Gebruikersmetagegevensaanroep](/help/authentication/user-metadata.md) en [Afmelden](/help/authentication/initiate-logout.md) diensten.


>[!TIP]
>
> **<u>Pro Tip:</u>** Volg de onderstaande stappen voor de implementatie(s) van tvOS.


- De toepassing zou moeten bepalen als de authentificatie als resultaat van een login door platform SSO of niet, gebruikend &quot;*tokenSource&quot;* [gebruikersmetagegevens](/help/authentication/user-metadata.md) van de Adobe Pass Authentication-service.
- De toepassing moet de gebruiker de instructie geven/vragen zich expliciet af te melden *`Settings -> Accounts -> TV Provider`* op tvOS **alleen** in geval van *&quot;tokenSource&quot;* waarde is gelijk aan &quot;*Apple&quot;.*
- De aanvraag moet [start de logout](/help/authentication/initiate-logout.md) van de Adobe Pass-verificatieservice via een directe HTTP-aanroep. Dit zou sessieopruiming aan de MVPD-zijde niet vergemakkelijken.



>[!TIP]
>
> **<u>Pro Tip:</u>** Voer de onderstaande stappen uit voor de iOS/iPadOS-implementatie(s).

- De toepassing zou moeten bepalen als de authentificatie als resultaat van een login door platform SSO of niet, gebruikend &quot;*tokenSource&quot;* [gebruikersmetagegevens](/help/authentication/user-metadata.md) van de Adobe Pass Authentication-service.
- De toepassing moet de gebruiker de instructie geven/vragen zich expliciet af te melden *`Settings -> TV Provider`* op iOS/iPadOS **alleen** in geval van *&quot;tokenSource&quot;* waarde is gelijk aan *&quot;Apple&quot;*.
- De aanvraag moet [start de logout](/help/authentication/initiate-logout.md) van de Adobe Pass-verificatieservice via een [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) of [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) component. Dit zou zittingsschoonmaak aan de kant van MVPD vergemakkelijken.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
