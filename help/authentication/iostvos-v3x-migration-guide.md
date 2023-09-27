---
title: Migratiehandleiding voor iOS/tvOS v3.x
description: Migratiehandleiding voor iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Migratiehandleiding voor iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!TIP]
> 
> **Opmerkingen:**
>
> - Vanaf iOS SDK versie 3.1 kunnen implementors nu WKWebView of UIWebView onderling verwisselbaar gebruiken. Aangezien UIWebView is afgekeurd, dienen apps te migreren naar WKWebView om problemen met toekomstige iOS-versies te voorkomen.
> - Merk op dat de migratie eenvoudig zou impliceren omschakelend de klasse UIWebView met WKWebView, is er geen specifiek werk dat met betrekking tot Adobe AccessEnabler moet worden gedaan.

</br>

## Buildinstellingen bijwerken {#update}

Deze versie bevat functies die in de SWIFT-taal zijn geschreven. Als uw app geheel onder doelstelling C valt, moet u het selectievakje &quot;Altijd SWIFT-standaardbibliotheken insluiten&quot; in de ontwikkelinstellingen van uw doel instellen op &quot;Ja&quot;. Als deze optie is ingesteld, scant Xcode de gebundelde frameworks in uw app en worden de desbetreffende bibliotheken naar de bundel van uw app gekopieerd als een van de frameworks Swift-code bevat. Als u de ontwikkelinstellingen niet bijwerkt, kan de toepassing vastlopen met fouten die aangeven dat AccessEnabler.framework of diverse componenten niet kunnen worden geladen `ibswift*` bibliotheken.

</br>

## Software-instructies toevoegen {#add}

> Ga voor meer informatie over hoe u de softwareinstructie kunt verkrijgen naar deze
> pagina:
> [Registratie van toepassingen](/help/authentication/iostvos-application-registration.md)

Zodra u uw softwareverklaring hebt adviseren wij het ontvangen op een verre server zodat kunt u het gemakkelijk intrekken of veranderen zonder een nieuwe versie van de toepassing in de App Store op te stellen. Wanneer de toepassing begint, verkrijg uw softwareverklaring van de verre plaats en ga het in de aannemer AccessEnabler over:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API-info hier: [iOS / tvOS API-naslaggids](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Het aangepaste URL-schema toevoegen {#add-custom}

> Ga naar deze pagina voor informatie over het verkrijgen van een aangepast URL-schema: [Een URL-schema voor klanten aanvragen](/help/authentication/iostvos-application-registration.md)

Nadat u het aangepaste URL-schema hebt gekregen, moet u dit toevoegen aan het bestand info.plist van de toepassing. Het aangepaste schema heeft de volgende indeling: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. U moet de dubbele punt en de schuine streep weglaten wanneer u deze aan het bestand toevoegt. Het bovenstaande voorbeeld wordt `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

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

Dit is alleen van toepassing voor het geval dat uw toepassing de handmatige verwerking van de Safari View Controller (SVC) via de [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) vraag en voor specifieke MVPDs die Controlemechanisme van de Mening Safari (SVC) vereisen, daarom ladend URLs van de authentificatie en logout eindpunten door een controlemechanisme SFSafariViewController in plaats van een controlemechanisme UIWebView/WKWebView.

Tijdens authentificatie en logout stromen moet uw toepassing de activiteit van controleren `SFSafariViewController `het controlemechanisme aangezien het door verscheidene omleidingen gaat. Uw toepassing moet het moment detecteren waarop een specifieke aangepaste URL wordt geladen die door uw toepassing wordt gedefinieerd `application's custom URL scheme` (bijvoorbeeld`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Wanneer de controller deze specifieke aangepaste URL laadt, moet de toepassing het dialoogvenster `SFSafariViewController` en roepen AccessEnabler `handleExternalURL:url `API-methode.

In uw `AppDelegate` Voeg de volgende methode toe:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API-info hier: [Externe URL verwerken](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## De handtekening van de methode setRequestor bijwerken {#update-setreq}

Omdat de nieuwe SDK een nieuw verificatiemechanisme gebruikt, is er geen behoefte aan de parameter signedRequestId of de openbare sleutel en het geheim (voor tvOS). De `setRequestor` wordt vereenvoudigd en heeft alleen de requestID nodig.

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

> API-info hier: [Aanvrager instellen](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Vervang de methode getAuthenticationToken door de methode handleExternalURL {#replace}

`getAuthentication` De methode werd in het verleden gebruikt voor de voltooiing van de authentificatiestroom. Omdat de naam misleidend was, is de naam gewijzigd in `handleExternalURL` en neemt de url als parameter.

Wijzig alle instanties van dit:

```swift
    accessEnabler.getAuthenticationToken()
```

in dit verband:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API-info hier: [Externe URL verwerken](/help/authentication/iostvos-sdk-api-reference.md)
