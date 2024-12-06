---
title: Migratiehandleiding voor iOS/tvOS v3.x
description: Migratiehandleiding voor iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Migratiehandleiding voor iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!TIP]
> 
> **Nota&#39;s:**
>
> - Vanaf iOS SDK versie 3.1 kunnen implementors nu WKWebView of UIWebView onderling verwisselbaar gebruiken. Aangezien UIWebView is afgekeurd, dienen apps te migreren naar WKWebView om problemen met toekomstige iOS-versies te voorkomen.
> - Merk op dat de migratie eenvoudig zou impliceren omschakelend de klasse UIWebView met WKWebView, is er geen specifiek werk dat met betrekking tot Adobe AccessEnabler moet worden gedaan.

</br>

## Buildinstellingen bijwerken {#update}

Deze versie bevat functies die in de SWIFT-taal zijn geschreven. Als uw app geheel onder doelstelling C valt, moet u het selectievakje &quot;Altijd SWIFT-standaardbibliotheken insluiten&quot; in de ontwikkelinstellingen van uw doel instellen op &quot;Ja&quot;. Als deze optie is ingesteld, scant Xcode de gebundelde frameworks in uw app en worden de desbetreffende bibliotheken naar de bundel van uw app gekopieerd als een van de frameworks Swift-code bevat. Als u de ontwikkelinstellingen niet bijwerkt, loopt uw app mogelijk vast met fouten die aangeven dat AccessEnabler.framework of verschillende `ibswift*` -bibliotheken niet kunnen worden geladen.

</br>

## Software-instructies toevoegen {#add}

> Ga voor meer informatie over hoe u de softwareinstructie kunt verkrijgen naar deze
> pagina:
> [Toepassingsregistratie ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Zodra u uw softwareverklaring hebt adviseren wij het ontvangen op een verre server zodat kunt u het gemakkelijk intrekken of veranderen zonder een nieuwe versie van de toepassing in de App Store op te stellen. Wanneer de toepassing begint, verkrijg uw softwareverklaring van de verre plaats en ga het in de aannemer AccessEnabler over:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API info hier: [ iOS / tvOS API Verwijzing ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Het aangepaste URL-schema toevoegen {#add-custom}

> Voor informatie over hoe te om een schema van douane URL te verkrijgen gaat naar deze pagina: [ verkrijg een regeling van klant URL ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Nadat u het aangepaste URL-schema hebt gekregen, moet u dit toevoegen aan het bestand info.plist van de toepassing. Het aangepaste schema heeft de volgende indeling: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. U moet de dubbele punt en de schuine streep weglaten wanneer u deze aan het bestand toevoegt. Het bovenstaande voorbeeld wordt `adbe.u-XFXJeTSDuJiIQs0HVRAg` .

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Aanroepen onderscheppen op het aangepaste URL-schema {#intercept}

Dit is slechts van toepassing op het geval dat uw toepassing eerder handmatige behandeling van het Controlemechanisme van de Mening Safari (SVC) via [ setOptions (\[ &quot;handleSVC&quot;:true&quot;\]) ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) en voor specifieke MVPDs die Controlemechanisme van de Mening Safari (SVC) vereisen, daarom vereist ladend URLs van de authentificatie en logout eindpunten door een controlemechanisme SFSafariViewController in plaats van UIWeb View/WKWebView-controller.

Tijdens authentificatie en logout stromen moet uw toepassing de activiteit van het `SFSafariViewController ` controlemechanisme controleren aangezien het door verscheidene redirects gaat. Uw toepassing moet het moment detecteren waarop een specifieke aangepaste URL wordt geladen die door uw `application's custom URL scheme` wordt gedefinieerd (bijvoorbeeld `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)` . Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing `SFSafariViewController` sluiten en de 1} API methode van AccessEnabler roepen {.`handleExternalURL:url `

Voeg in de `AppDelegate` de volgende methode toe:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API info hier: [ Handel Externe URL ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## De handtekening van de methode setRequestor bijwerken {#update-setreq}

Omdat de nieuwe SDK een nieuw verificatiemechanisme gebruikt, is er geen behoefte aan de parameter signedRequestId of de openbare sleutel en het geheim (voor tvOS). De methode `setRequestor` is vereenvoudigd en heeft alleen de aanvragerID nodig.

### iOS

Deze code:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

wordt:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Deze code:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

wordt:

```swift
    accessEnabler.setRequestor(requestorId)
```

> API info hier: [ vastgestelde Aanvrager ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Vervang de methode getAuthenticationToken door de methode handleExternalURL {#replace}

De methode `getAuthentication` is in het verleden gebruikt voor het voltooien van de verificatiestroom. Omdat de naam misleidend was, is de naam gewijzigd in `handleExternalURL` en wordt de URL als parameter gebruikt.

Wijzig alle instanties van dit:

```swift
    accessEnabler.getAuthenticationToken()
```

in dit verband:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API info hier: [ Handel Externe URL ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
