---
title: Koptekst - X-Apparaat-Info
description: REST API V2 - Koptekst - X-apparaatinfo
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Koptekst - X-Apparaat-Info {#header-x-device-info}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> x-apparaat-Info </b> verzoekkopbal bevat de cliëntinformatie (apparaat, verbinding en toepassing) met betrekking tot het daadwerkelijke het stromen apparaat.

## Syntaxis {#syntax}

<table>
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

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Sleutel</th>
        <th style="background-color: #EFF2F7;">Beschrijving</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Aanwezigheid</th>
        <th style="background-color: #EFF2F7;">Mogelijke waarden</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>Het primaire hardwaretype van het apparaat.</td>
        <td></td>
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
        <td>model</td>
        <td>De modelnaam van het apparaat.</td>
        <td><i>vereist</i></td>
        <td>bv. iPhone, SM-G930V, AppleTV, enz.</td>
    </tr>
    <tr>
        <td>versie</td>
        <td>De apparaatversie.</td>
        <td></td>
        <td>bv. 2.0.1, enz.</td>
    </tr>
    <tr>
        <td>fabrikant</td>
        <td>Het productiebedrijf/de organisatie van het apparaat.</td>
        <td></td>
        <td>bv. Samsung, LG, ZTE, Huawei, Motorola, Apple, enz.</td>
    </tr>
    <tr>
        <td>leverancier</td>
        <td>De verkoopmaatschappij/organisatie van het apparaat.</td>
        <td></td>
        <td>bijvoorbeeld Apple, Samsung, LG, Google, enz.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>De naam van het besturingssysteem van het apparaat.</td>
        <td><i>vereist</i></td>
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
        <td>osFamily</td>
        <td>De groepsnaam van het besturingssysteem van het apparaat.</td>
        <td></td>
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
        <td>osVendor</td>
        <td>De leverancier van het besturingssysteem van het apparaat.</td>
        <td></td>
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
        <td>osVersion</td>
        <td>De versie van het besturingssysteem van het apparaat.</td>
        <td></td>
        <td>bv. 10.2, 9.0.1, enz.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>De browsernaam.</td>
        <td></td>
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
        <td>browserVendor</td>
        <td>De bouwonderneming/organisatie van de browser.</td>
        <td></td>
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
        <td>browserVersion</td>
        <td>De browserversie van het apparaat.</td>
        <td></td>
        <td>bijv. 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>De gebruikersagent van het apparaat.</td>
        <td></td>
        <td>Bijvoorbeeld Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>De fysieke schermbreedte van het apparaat.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>De fysieke schermhoogte van het apparaat.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>De fysieke pixeldichtheid van het scherm van het apparaat.</td>
        <td></td>
        <td>bijv. 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>De fysieke schermdiagonale afmeting van het apparaat in inches.</td>
        <td></td>
        <td>bv. 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>IP van het apparaat gebruikt voor het verzenden van HTTP- verzoeken.</td>
        <td></td>
        <td>bijv. 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>De poort van het apparaat die wordt gebruikt voor het verzenden van HTTP-aanvragen.</td>
        <td></td>
        <td>bijv. 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Het type netwerkverbinding.</td>
        <td></td>
        <td>bijv. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>De beveiligingsstatus van de netwerkverbinding.</td>
        <td></td>
        <td>
            De waarden zijn beperkt:
            <ul>
                <li>true - in het geval van een beveiligd netwerk</li>
                <li>false - in het geval van een openbare hotspot</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>De unieke id van de toepassing.</td>
        <td></td>
        <td>bv. CNN</td>
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
