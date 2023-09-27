---
title: iOS-verificatiefout - adobepass.ios.app is niet gevonden
description: iOS-verificatiefout - adobepass.ios.app is niet gevonden
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS-verificatiefout - adobepass.ios.app is niet gevonden {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Probleem {#issue}

De gebruiker doorloopt de verificatiestroom en nadat hij of zij zijn of haar verificatiegegevens heeft ingevoerd bij de provider, wordt hij of zij teruggeleid naar een foutpagina, een zoekpagina of een andere aangepaste pagina met de melding dat `adobepass.ios.app` kan niet worden gevonden/opgelost.

## Toelichting {#explanation}

Op iOS `adobepass.ios.app` wordt gebruikt als definitieve redirection URL om erop te wijzen dat de stroom AuthN volledig is. Op dit punt moet app een verzoek aan AccessEnabler indienen om de Symbolische Token van AuthN te krijgen en de stroom af te ronden AuthN.

Het probleem is dat `adobepass.ios.app` bestaat niet en wordt een foutbericht weergegeven in het dialoogvenster `webView`. Oudere versies van de iOS DemoApp gingen ervan uit dat deze fout altijd zou worden geactiveerd aan het einde van de AuthN-flow en dat deze was ingesteld om deze dienovereenkomstig te verwerken (`indidFailLoadWithError`).

**Opmerking:** Dit probleem is opgelost in latere versies van de DemoApp (opgenomen in de SDK-download van iOS).

Helaas is deze veronderstelling NIET juist. Er zijn sommige zogenaamde &quot;slimme&quot;DNS of servers van de Volmacht die niet eenvoudig overgaan-op de opgeheven fout, maar één van het volgende zullen doen:

- Een aangepaste foutpagina maken
- Door:sturen aan een onderzoekspagina, of één of ander ander ander type van klantenpagina of portaal.

In die gevallen zal de reactie die terugkomt op iOS webView een volkomen geldig antwoord zijn wat webView betreft, en zal NIET de fout teweegbrengen die de oude DemoApp afhankelijk van was.

## Oplossing {#solution}

Maak NIET de zelfde veronderstelling die DemoApp maakt. In plaats daarvan onderschept u de aanvraag voordat deze wordt uitgevoerd (in `shouldStartLoadWithRequest`) en op de juiste wijze te verwerken.

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

- NOOIT gebruiken `adobepass.ios.app` rechtstreeks in de code. Gebruik in plaats daarvan de constante `ADOBEPASS_REDIRECT_URL`
- De `return NO;` instructie voorkomt dat de pagina wordt geladen
- Zorg er absoluut voor dat de `getAuthenticationToken` De vraag wordt eens en slechts eenmaal geroepen in uw code. Meerdere aanroepen van `getAuthenticationToken` resulteert in ongedefinieerde resultaten.
