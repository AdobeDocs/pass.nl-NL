---
title: Clientgegevens doorgeven (apparaat, verbinding en toepassing)
description: Clientgegevens doorgeven (apparaat, verbinding en toepassing)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# (Verouderd) clientgegevens doorgeven (apparaat, verbinding en toepassing) {#pass-client-info}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Toepassingsgebied {#pass-client-info-scope}

Dit document bevat gegevens en cookies waarmee clientgegevens (apparaat, verbinding en toepassing) kunnen worden doorgegeven van een programmeertoepassing naar Adobe Pass Authentication REST API&#39;s of SDK&#39;s.

De voordelen van het verstrekken van cliëntinformatie zijn:

* De mogelijkheid om de Home Base Authentication (HBA) correct in te schakelen voor bepaalde apparaattypen en MVPD&#39;s die HBA kunnen ondersteunen.
* De capaciteit om TTLs in het geval van sommige apparatentypes behoorlijk toe te passen (bijvoorbeeld, vorm langere TTLs voor authentificatiesessies op TV aangesloten apparaten).
* De capaciteit om bedrijfsmetriek in uitgesplitste naar apparatentypen behoorlijk samen te voegen gebruikend de Controle van de Dienst van de Entitlement (ESM).
* Maakt de mogelijkheid ongedaan om verschillende bedrijfsregels correct toe te passen (bijvoorbeeld afbraak) op specifieke apparaattypen.

## Overzicht {#pass-client-info-overview}

De clientinformatie bestaat uit:

* **Apparaat** informatie over de hardware en softwareattributen van het apparaat van waar de gebruiker probeert om de inhoud van de Programmer te verbruiken.
* **de informatie van de Verbinding** over de verbindingsattributen van het apparaat van waar de gebruiker met de diensten van de Authentificatie van Adobe Pass en/of de diensten van de Programmer (b.v. server-aan-server implementaties) verbindt.
* **de informatie van de Toepassing** over de geregistreerde toepassing van waar de gebruiker probeert om de inhoud van de Programmer te verbruiken.

De clientinformatie is een JSON-object dat is gebouwd met sleutels die in de volgende tabel worden weergegeven.

>[!NOTE]
>
>De volgende **sleutels** zijn **verplicht** om in de cliëntinformatie JSON voorwerp worden verzonden: **model**, **osName**.
>
>De volgende sleutels hebben **beperkte** waarden: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Sleutel | Beperkt | Beschrijving | Mogelijke waarden |
|---|---|---|---|---|
|            | primaryHardwareType | # Ja | Het primaire hardwaretype van het apparaat. | # De waarden zijn beperkt:                                                                     Camera                                                      DataCollectionTerminal                                                      Desktop                                                      EmbeddedNetworkModule                                                      eReader                                                      GamesConsole                                                      GeolocationTracker                                                      Bril                                                      MediaPlayer                                                      Mobiele telefoon                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tablet                                                      WirelessHotspot                                                      polshorloge                                                      Onbekend |
| #mandatory | model | Nee | De modelnaam van het apparaat. | bv. iPhone, SM-G930V, AppleTV, enz. |
|            | versie | Nee | De apparaatversie. | bv. 2.0.1, enz. |
|            | fabrikant | Nee | Het productiebedrijf/de organisatie van het apparaat. | bv. Samsung, LG, ZTE, Huawei, Motorola, Apple, enz. |
|            | leverancier | Nee | Het verkoopbedrijf/de organisatie van het apparaat. | bijvoorbeeld Apple, Samsung, LG, Google, enz. |
| #mandatory | osName | # Ja | De naam van het besturingssysteem van het apparaat. | # De waarden zijn beperkt:                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Ja | De groepsnaam van het besturingssysteem van het apparaat. | # De waarden zijn beperkt:                                                   Android                   BSD                   Linux                   PlayStation-besturingssysteem                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | Nee | De leverancier van het besturingssysteem van het apparaat. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Tizen Project |
|            | osVersion | Nee | De versie van het besturingssysteem van het apparaat. | bv. 10.2, 9.0.1, enz. |
|            | browserName | # Ja | De browsernaam. | # De waarden zijn beperkt:                                                   Android Browser                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Symbian Browser |
|            | browserVendor | # Ja | De bouwonderneming/organisatie van de browser. | # De waarden zijn beperkt:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Nee | De browserversie van het apparaat. | bijv. 60.0.3112 |
|            | userAgent | Nee | De gebruikersagent van het apparaat. | Bijvoorbeeld Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | Nee | De fysieke schermbreedte van het apparaat. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Nee | De fysieke schermhoogte van het apparaat. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Nee | De fysieke pixeldichtheid van het scherm van het apparaat. | bijv. 294 |
|            | diagonalScreenSize | Nee | De fysieke schermdiagonale afmeting van het apparaat in inches. | bv. 5.5, 10.1 |
|            | connectionIp | Nee | IP van het apparaat wordt gebruikt voor het verzenden van HTTP- verzoeken die. | bijv. 8.8.4.4 |
|            | connectionPort | Nee | De poort van het apparaat die wordt gebruikt voor het verzenden van HTTP-aanvragen. | bijv. 53124 |
|            | connectionType | Nee | Het type netwerkverbinding. | bijv. WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Ja | De beveiligingsstatus van de netwerkverbinding. | # De waarden zijn beperkt:                                                   true - in het geval van een beveiligd netwerk                   false - in het geval van een openbare hotspot |
|            | applicationId | Nee | De unieke id van de toepassing. | bv. CNN |

## API-verwijzingen {#api-ref}

In deze sectie wordt de API weergegeven die verantwoordelijk is voor de verwerking van clientgegevens bij het gebruik van Adobe Pass Authentication REST API&#39;s of SDK&#39;s.

### REST API {#rest-api}

Adobe Pass-verificatieservices bieden op de volgende manieren ondersteuning voor het ontvangen van de clientgegevens:

* Als a **kopbal: &quot;x-Apparaat-Info&quot;**
* Als a **vraagparameter: &quot;device_info&quot;**
* Als a **postparameter: &quot;device_info&quot;**

>[!IMPORTANT]
>
>In alle drie scenario&#39;s, moet de lading van de kopbal of de parameter **Base64 gecodeerd en URL gecodeerd** zijn.

**SDK**

#### JavaScript SDK {#js-sdk}

De AccessEnabler JavaScript SDK bouwt standaard een JSON-object met clientinformatie dat wordt doorgegeven aan de Adobe Pass Authentication-services, tenzij dit wordt overschreven.

JavaScript AccessEnabler SDK steunt **het met voeten treden slechts** de &quot;applicationId&quot;sleutel van de cliëntinformatie JSON voorwerp door [&#x200B; setRequestor &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)) *applicationId* optiesparameter.

>[!CAUTION]
>
>De `applicationId` -parameterwaarde moet een tekenreeks zonder opmaak zijn.
>Als de toepassing van de Programmer besluit om applicationId over te gaan, dan zullen de rest sleutels van cliëntinformatie nog door AccessEnabler JavaScript SDK worden verwerkt.

#### iOS/tvOS SDK {#ios-tvos-sdk}

De AccessEnabler iOS/tvOS SDK bouwt standaard een JSON-object met clientinformatie dat wordt doorgegeven aan de Adobe Pass Authentication-services, tenzij dit wordt overschreven.

AccessEnabler iOS/tvOS SDK steunt **het met voeten treden van de volledige** cliëntinformatie JSON voorwerp door de [&#x200B; setOptions &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions) apparaat_info parameter.

>[!CAUTION]
>
>De *device_info* parameterwaarde moet a **Base64 gecodeerde** ** waarde zijn NSString.
>
>In het geval dat de toepassing van de Programmer besluit om *device_info* over te gaan, dan zullen alle sleutels van cliëntinformatie die door AccessEnabler iOS/tvOS SDK worden gegevens verwerkt worden met voeten getreden. Daarom is het zeer belangrijk om de waarden voor zoveel mogelijk sleutels te berekenen en over te gaan. Voor meer details betreffende de implementatie, zie de [&#x200B; lijst van het 0&rbrace; Overzicht &lbrace;en &#x200B;](#pass-client-info-overview) het koekjesboek van iOS/tvOS [.](#ios-tvos)

#### Android/FireOS SDK {#and-fire-os-sdk}

De `AccessEnabler` Android/FireOS SDK bouwt standaard een JSON-object met clientinformatie dat wordt doorgegeven aan de Adobe Pass-verificatieservices, tenzij dit wordt overschreven.

De `AccessEnabler` Android/FireOS SDK steunt **met voeten tredend de volledige** cliëntinformatie JSON voorwerp door de [&#x200B; setOptions &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions) [&#x200B; setOptions &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption) parameter `device_info`.

>[!NOTE]
>
>De `device_info` parameterwaarde moet a **Base64 gecodeerde** waarde van het Koord zijn.

>[!IMPORTANT]
>
>Als de programmeertoepassing besluit om `device_info` door te geven, worden alle sleutels voor clientinformatie die door de `AccessEnabler` Android/FireOS SDK zijn berekend, overschreven. Daarom is het zeer belangrijk om de waarden voor zoveel mogelijk sleutels te berekenen en over te gaan. Voor meer details betreffende de implementatie, zie de [&#x200B; lijst van het 0&rbrace; Overzicht &lbrace;en &#x200B;](#pass-client-info-overview) Android [&#x200B; en &#x200B;](#android) FireOS [&#x200B; kookboek.](#fire-tv)

## Cookbooks {#cookbooks}

Deze sectie presenteert een cookbook voor het samenstellen van de client-informatie JSON-object in het geval van verschillende apparaattypen.

>[!IMPORTANT]
>
>De toetsen die zijn gemarkeerd met **!** zijn verplicht te worden verzonden.

### Android {#android}

De informatie over de voorziening kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|-----------------------------|---------------|
| ! | model | Build.MODEL | GT-I9505 |
|   | leverancier | Build.BRAND | samsung |
|   | fabrikant | Build.MANUFACTURER | samsung |
| ! | versie | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | hardgecodeerd | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1. |

De verbindingsgegevens kunnen als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

De toepassingsinformatie kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|-----------|--------------|
|   | applicationId | hardgecodeerd | CNN |

>[!IMPORTANT]
>
>Het apparaat, de verbinding en de toepassingsinformatie moeten aan hetzelfde JSON-object worden toegevoegd. Daarna, moet het resulterende voorwerp **Base64 gecodeerd** zijn. Ook, in het geval van de VERTONING APIs van de Authentificatie van Adobe Pass, moet de waarde **gecodeerde URL** zijn.

**code van de Steekproef**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**Middelen:**
>* de openbare klasse [&#x200B; bouwt &#x200B;](https://developer.android.com/reference/android/os/Build.html){target=_blank} in de documentatie van ontwikkelaars van Java.

### FireTV {#fire-tv}

De informatie over de voorziening kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (bijvoorbeeld) |
|---|---------------|-----------------------------|--------------|
| ! | model | Build.MODEL | AFTM |
|   | leverancier | Build.BRAND | Amazon |
|   | fabrikant | Build.MANUFACTURER | Amazon |
| ! | versie | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | hardgecodeerd | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1. |

De verbindingsgegevens kunnen als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

De toepassingsinformatie kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|-----------|--------------|
|   | applicationId | hardgecodeerd | CNN |

>[!IMPORTANT]
>
>Het apparaat, de verbinding en de toepassingsinformatie moeten aan hetzelfde JSON-object worden toegevoegd. Daarna, moet het resulterende voorwerp **Base64 gecodeerd** zijn. Ook, in het geval van de VERTONING APIs van de Authentificatie van Adobe Pass, moet de waarde **gecodeerde URL** zijn.

>[!NOTE]
>
>**Middelen:**
>* openbare klasse [&#x200B; bouwt &#x200B;](https://developer.android.com/reference/android/os/Build.html){target=_blank} in de documentatie van ontwikkelaars van Android.
>* [&#x200B; identificerend apparaten FireTV &#x200B;](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

De informatie over de voorziening kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|------------------------|--------------|
| ! | model | uname.machine | iPhone |
|   | leverancier | hardgecodeerd | Apple |
|   | fabrikant | hardgecodeerd | Apple |
| ! | versie | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [ Reachability currentReachabilityStatus ] |              |
|   | connectionSecure |                                           |              |


De toepassingsinformatie kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|-----------|--------------|
|   | applicationId | hardgecodeerd | CNN |

>[!IMPORTANT]
>
>Het apparaat, de verbinding en de toepassingsinformatie moeten aan hetzelfde JSON-object worden toegevoegd. Daarna, moet het resulterende voorwerp Base64 gecodeerd zijn. In het geval van Adobe Pass Authentication REST API&#39;s moet de waarde ook URL-gecodeerd zijn.

**code van de Steekproef**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**Middelen:**
>* [&#x200B; UIDevice &#x200B;](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [&#x200B; uname &#x200B;](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [&#x200B; Ongeveer Reachability &#x200B;](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | model | hardgecodeerd | &quot;Roku&quot; |
|     | leverancier | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | fabrikant | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | versie | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | hardgecodeerd | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | hardgecodeerd | true als de verbinding is bekabeld |

De toepassingsinformatie kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---------------|-----------|--------------|
|   | applicationId | hardgecodeerd | CNN |

>[!IMPORTANT]
>
>Het apparaat, de verbinding en de toepassingsinformatie moeten aan hetzelfde JSON-object worden toegevoegd. Daarna, moet het resulterende voorwerp **Base64 gecodeerd** zijn. In het geval van Adobe Pass Authentication REST API&#39;s moet de waarde ook URL-gecodeerd zijn.

>[!NOTE]
>
>Voor meer informatie, zie [&#x200B; ifDeviceInfo &#x200B;](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

De informatie over de voorziening kan als volgt worden samengesteld:

|   | Sleutel | Source | Waarde (voorbeeld) |
|---|---|---|---|
| ! | model | EasClientDeviceInformation.SystemProductName |                 |
|   | leverancier | hardgecodeerd | Microsoft |
|   | fabrikant | hardgecodeerd | Microsoft |
| ! | versie | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

|   | Sleutel | Source | Voorbeeld |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;None&quot;, &quot;Wpa&quot; enz. |

De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---|---|---|
| applicationId | hardgecodeerd | CNN |

>[!IMPORTANT]
>
>Het apparaat, de verbinding en de toepassingsinformatie moeten aan hetzelfde JSON-object worden toegevoegd. Daarna, moet het resulterende voorwerp **Base64 gecodeerd** zijn. Ook, in het geval van de VERTONING APIs van de Authentificatie van Adobe Pass, moet de waarde **gecodeerde URL** zijn.

**Middelen**

* [&#x200B; Klasse EasClientDeviceInformation &#x200B;](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [&#x200B; Klasse DisplayInformation &#x200B;](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
