---
title: Koptekst - X-Apparaat-Info
description: REST API V2 - Koptekst - X-apparaatinfo
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 1%

---

# Koptekst - X-Apparaat-Info {#header-x-device-info}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> x-apparaat-Info </b> verzoekkopbal bevat de cliëntinformatie (apparaat, verbinding en toepassing) met betrekking tot het daadwerkelijke het stromen apparaat.

## Syntaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> x-apparaat-Info </b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>Type koptekst</td>
      <td>Koptekst aanvragen</td>
   </tr>
   <tr>
      <td>Standaard</td>
      <td>Nee</td>
   </tr>
</table>

## Richtlijnen {#directives}

<b> &lt;device_information></b>

De `Base64-encoded` -waarde van het JSON-element dat ten minste de kenmerken bevat die zijn gemarkeerd als vereist door de volgende tabel.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Aanwezigheid</th>
        <th style="background-color: #EFF2F7; width: 15%;">Sleutel</th>
        <th style="background-color: #EFF2F7;">Beschrijving</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Beperkt</th>
        <th style="background-color: #EFF2F7;">Mogelijke waarden</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>Het primaire hardwaretype van het apparaat.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Camera</li>
                <li>DataCollectionTerminal</li>
                <li>Desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Bril</li>
                <li>MediaPlayer</li>
                <li>Mobiele telefoon</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>WirelessHotspot</li>
                <li>polshorloge</li>
                <li>Onbekend</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>vereist</i></td>
        <td>model</td>
        <td>De modelnaam van het apparaat.</td>
        <td></td>
        <td>bv. iPhone, SM-G930V, AppleTV, enz.</td>
    </tr>
    <tr>
        <td><i>vereist</i></td>
        <td>versie</td>
        <td>De apparaatversie.</td>
        <td></td>
        <td>bv. 2.0.1, enz.</td>
    </tr>
    <tr>
        <td></td>
        <td>fabrikant</td>
        <td>Het productiebedrijf/de organisatie van het apparaat.</td>
        <td></td>
        <td>bv. Samsung, LG, ZTE, Huawei, Motorola, Apple, enz.</td>
    </tr>
    <tr>
        <td></td>
        <td>leverancier</td>
        <td>De verkoopmaatschappij/organisatie van het apparaat.</td>
        <td></td>
        <td>bijvoorbeeld Apple, Samsung, LG, Google, enz.</td>
    </tr>
    <tr>
        <td><i>vereist</i></td>
        <td>osName</td>
        <td>De naam van het besturingssysteem van het apparaat.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>De groepsnaam van het besturingssysteem van het apparaat.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation-besturingssysteem</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>De leverancier van het besturingssysteem van het apparaat.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Tizen Project</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>vereist</i></td>
        <td>osVersion</td>
        <td>De versie van het besturingssysteem van het apparaat.</td>
        <td></td>
        <td>bv. 10.2, 9.0.1, enz.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>De browsernaam.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Android Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Symbian Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>De bouwonderneming/organisatie van de browser.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>De browserversie van het apparaat.</td>
        <td></td>
        <td>bijv. 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>De gebruikersagent van het apparaat.</td>
        <td></td>
        <td>Bijvoorbeeld Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>De fysieke schermbreedte van het apparaat.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>De fysieke schermhoogte van het apparaat.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>De fysieke pixeldichtheid van het scherm van het apparaat.</td>
        <td></td>
        <td>bijv. 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>De fysieke schermdiagonale afmeting van het apparaat in inches.</td>
        <td></td>
        <td>bv. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>IP van het apparaat gebruikt voor het verzenden van HTTP- verzoeken.</td>
        <td></td>
        <td>bijv. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>De poort van het apparaat die wordt gebruikt voor het verzenden van HTTP-aanvragen.</td>
        <td></td>
        <td>bijv. 53124</td>
    </tr>
    <tr>
        <td><i>vereist</i></td>
        <td>connectionType</td>
        <td>Het type netwerkverbinding.</td>
        <td></td>
        <td>bijv. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>De beveiligingsstatus van de netwerkverbinding.</td>
        <td>&amp;check;</td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>true - in het geval van een beveiligd netwerk</li>
                <li>false - in het geval van een openbare hotspot</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>De unieke id van de toepassing.</td>
        <td></td>
        <td>bijv. REF30</td>
    </tr>
</table>


## Voorbeelden {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
> 
> De codefragmenten en documentatiemiddelen worden verstrekt voor verwijzende doeleinden.
> 
> De codefragmenten zijn niet uitputtend en kunnen extra wijzigingen vereisen om in uw project te werken.
>
> Ongeacht uw daadwerkelijke implementatie, moet de `X-Device-Info` kopbal een waarde bevatten die zoals in de [ wordt beschreven Richtlijnen ](#directives) sectie.

### Browsers {#browsers}

Voor clienttoepassingen die in een browser worden uitgevoerd, kan de header `X-Device-Info` worden weggelaten omdat de browser automatisch een minimale set vereiste informatie in de header van `User-Agent` verzendt.

U kunt de header van `X-Device-Info` nog steeds gebruiken om aanvullende informatie over het apparaat, de verbinding en de toepassing op te geven, voor het geval dat uw clienttoepassing een bibliotheek of service integreert die een apparaat-identificatiemechanisme biedt.

### Mobiele apparaten {#mobile-devices}

#### iOS en iPadOS {#ios-ipados}

Om de `X-Device-Info` kopbal voor apparaten te bouwen die [ iOS of iPadOS ](https://developer.apple.com/documentation/ios-ipados-release-notes) in werking stellen, kunt u naar de volgende documenten en onder codefragment verwijzen:

* Apple ontwikkelaarsdocumentatie voor [ UIDevice ](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple ontwikkelaarsdocumentatie voor [ Reachability ](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* De handdocumentatie van Linux voor [ uname ](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|------------------------|-----------------|
| model | uname.machine | iPhone |
| leverancier | hardgecodeerd | Apple |
| fabrikant | hardgecodeerd | Apple |
| versie | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|------------------|------------------------------------------|-----------------|
| connectionType | [ Reachability currentReachabilityStatus ] |                 |
| connectionSecure |                                          |                 |


De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------|-----------------|
| applicationId | hardgecodeerd | REF30 |

#### Android {#android}

Om de `X-Device-Info` kopbal voor apparaten te bouwen die [ Android ](https://developer.android.com/about/versions) in werking stellen, kunt u naar de volgende documenten en onder codefragment verwijzen:

* De ontwikkelaarsdocumentatie van Android voor [ bouwt ](https://developer.android.com/reference/android/os/Build.html) klasse.

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | GT-I9505 |
| leverancier | Build.BRAND | samsung |
| fabrikant | Build.MANUFACTURER | samsung |
| versie | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | hardgecodeerd | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1. |

De verbindingsgegevens kunnen als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------|-----------------|
| applicationId | hardgecodeerd | REF30 |

### Op tv aangesloten apparaten {#tv-connected-devices}

#### tvOS {#tvos}

Om de `X-Device-Info` kopbal voor apparaten te bouwen die [ tvOS ](https://developer.apple.com/documentation/tvos-release-notes) in werking stellen, kunt u naar de volgende documenten en onder codefragment verwijzen:

* Apple ontwikkelaarsdocumentatie voor [ UIDevice ](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple ontwikkelaarsdocumentatie voor [ Reachability ](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* De handdocumentatie van Linux voor [ uname ](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|------------------------|-----------------|
| model | uname.machine | AppleTV |
| leverancier | hardgecodeerd | Apple |
| fabrikant | hardgecodeerd | Apple |
| versie | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|------------------|------------------------------------------|-----------------|
| connectionType | [ Reachability currentReachabilityStatus ] |                 |
| connectionSecure |                                          |                 |

De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------|-----------------|
| applicationId | hardgecodeerd | REF30 |

#### Fire OS {#fireos}

Om de `X-Device-Info` kopbal voor apparaten te bouwen die [ Vuur OS ](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) in werking stellen, kunt u naar de volgende documenten verwijzen:

* De ontwikkelaarsdocumentatie van Android voor [ bouwt ](https://developer.android.com/reference/android/os/Build.html) klasse.
* De ontwikkelaarsdocumentatie van Amazon voor [ het identificeren van de Apparaten van TV van het Vuur ](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | AFTM |
| leverancier | Build.BRAND | Amazon |
| fabrikant | Build.MANUFACTURER | Amazon |
| versie | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | hardgecodeerd | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1. |

De verbindingsgegevens kunnen als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------|-----------------|
| applicationId | hardgecodeerd | REF30 |

#### Roku OS {#rokuos}

Om de `X-Device-Info` kopbal voor apparaten te bouwen die [ Roku OS ](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) in werking stellen, kunt u naar de volgende documenten verwijzen:

* De ontwikkelaarsdocumentatie van Roku voor [ ifDeviceInfo ](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

De informatie over de voorziening kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|--------------------------------------------|-----------------|
| model | hardgecodeerd | &quot;Roku&quot; |
| leverancier | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| fabrikant | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| versie | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | hardgecodeerd | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

De verbindingsgegevens kunnen als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | hardgecodeerd | true als de verbinding is bekabeld |

De toepassingsinformatie kan als volgt worden samengesteld:

| Sleutel | Source | Waarde (voorbeeld) |
|---------------|-----------|-----------------|
| applicationId | hardgecodeerd | REF30 |

### Overige {#others}

Voor apparaatplatforms die niet in de documentatie worden besproken, moet de clientinformatie (apparaat, verbinding en toepassing) worden gekoppeld aan alle beschikbare hardware- en besturingssysteemkenmerken (OS), die doorgaans in de hardware- en besturingssysteemhandleidingen van het apparaat worden vermeld.
