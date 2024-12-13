---
title: iOS-verificatiefout - adobepass.ios.app is niet gevonden
description: iOS-verificatiefout - adobepass.ios.app is niet gevonden
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# (Verouderd) iOS-verificatiefout - adobepass.ios.app kan niet worden gevonden {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Probleem {#issue}

De gebruiker doorloopt de verificatiestroom en nadat hij of zij zijn of haar verificatiegegevens heeft ingevoerd bij de provider, wordt hij of zij teruggeleid naar een foutpagina, een zoekpagina of een andere aangepaste pagina met de melding dat `adobepass.ios.app` niet is gevonden/opgelost.

## Toelichting {#explanation}

In iOS wordt `adobepass.ios.app` gebruikt als de laatste omleidings-URL om aan te geven dat de AuthN-flow is voltooid. Op dit punt moet app een verzoek aan AccessEnabler indienen om de Symbolische Token van AuthN te krijgen en de stroom af te ronden AuthN.

Het probleem is dat `adobepass.ios.app` niet bestaat en een foutbericht in `webView` activeert. Oudere versies van iOS DemoApp veronderstelden dat deze fout altijd aan het eind van de stroom AuthN zou teweeggebracht worden, en opstelling om het dienovereenkomstig te behandelen (`indidFailLoadWithError`).

**Nota:** Deze kwestie is bevestigd in recentere versies van DemoApp (inbegrepen met de download van iOS SDK).

Helaas is deze veronderstelling NIET juist. Er zijn sommige zogenaamde &quot;slimme&quot;DNS of servers van de Volmacht die niet eenvoudig overgaan-op de opgeheven fout, maar één van het volgende zullen doen:

- Een aangepaste foutpagina maken
- Door:sturen aan een onderzoekspagina, of één of ander ander ander type van klantenpagina of portaal.

In die gevallen zal de reactie die terugkomt op iOS webView een volkomen geldig antwoord zijn wat webView betreft, en zal NIET de fout teweegbrengen die de oude DemoApp afhankelijk van was.

## Oplossing {#solution}

Maak NIET de zelfde veronderstelling die DemoApp maakt. In plaats daarvan onderbreekt u de aanvraag voordat deze wordt uitgevoerd (in `shouldStartLoadWithRequest` ) en verwerkt u deze op de juiste wijze.

Voorbeeld van hoe u de aanvraag onderschept voordat deze wordt uitgevoerd:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Een paar dingen:

- U kunt `adobepass.ios.app` NOOIT overal in de code gebruiken. Gebruik in plaats daarvan de constante `ADOBEPASS_REDIRECT_URL`
- De instructie `return NO;` voorkomt dat de pagina wordt geladen
- Zorg er absoluut voor dat de aanroep van `getAuthenticationToken` eenmaal en slechts eenmaal in de code wordt aangeroepen. Meerdere aanroepen van `getAuthenticationToken` resulteren in ongedefinieerde resultaten.
