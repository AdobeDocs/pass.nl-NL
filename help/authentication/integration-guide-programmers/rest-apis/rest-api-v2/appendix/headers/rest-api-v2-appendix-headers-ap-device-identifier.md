---
title: Koptekst - AP-apparaat-id
description: REST API V2 - Koptekst - AP-apparaat-id
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Koptekst - AP-apparaat-id {#header-ap-device-identifier}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AP-Apparaat-Herkenningsteken </b> verzoekkopbal bevat het stromen apparatenherkenningsteken aangezien het door de cliënttoepassing werd gecreeerd.

## Syntaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AP-apparaat-Herkenningsteken </b>: &lt;type&gt; &lt;identifier&gt;</td>
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

<b>&lt;type> </b>

Het type apparaat-id.

Er zijn slechts één ondersteunde typen, zoals hieronder wordt weergegeven.

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Type</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>vingerafdruk</td>
      <td>
            De apparaat-id bestaat uit een stabiele en unieke id die door de clienttoepassing voor elk apparaat wordt gemaakt en beheerd.
            <br/>
            De clienttoepassing moet de apparaat-id in de cache plaatsen in permanente opslag, omdat het verlies of wijzigen van de id de verificatie ongeldig maakt. De clienttoepassing moet waardewijzigingen voorkomen die worden veroorzaakt door gebruikersacties zoals het verwijderen van de toepassing, het opnieuw installeren of upgrades van de toepassing.
      </td>
   </tr>
</table>


<b> &lt;identifier> </b>

De `Base64-encoded` -waarde van de apparaat-id.

## Voorbeeld {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> De documentatiebronnen worden verschaft voor referentiedoeleinden.
>
> De documentatiebronnen zijn niet volledig en vereisen mogelijk aanvullende wijzigingen om in uw project te werken.
> 
> Ongeacht uw daadwerkelijke implementatie, moet de `AP-Device-Identifier` kopbal een waarde bevatten die zoals die in de [&#x200B; wordt beschreven Richtlijnen &#x200B;](#directives) sectie.

### Browsers {#browsers}

Als u de header `AP-Device-Identifier` wilt samenstellen voor apparaten die in een browser worden uitgevoerd, moet uw clienttoepassing een stabiele en unieke id berekenen op basis van beschikbare gegevens zoals browser, apparaat of gebruikersspecifieke gegevens.

_(*) wij adviseren om een bibliotheek of de dienst te integreren die browser of apparaat vingerafdrukken verstrekt._

### Mobiele apparaten {#mobile-devices}

#### iOS en iPadOS {#ios-ipados}

Om de `AP-Device-Identifier` kopbal voor apparaten te bouwen die [&#x200B; iOS of iPadOS &#x200B;](https://developer.apple.com/documentation/ios-ipados-release-notes) in werking stellen, kunt u naar de volgende documenten verwijzen:

* Apple ontwikkelaarsdocumentatie voor [&#x200B; identifierForVendor &#x200B;](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) wij adviseren om een hash functie SHA-256 op OS toe te passen verstrekte waarde._

#### Android {#android}

Om de `AP-Device-Identifier` kopbal voor apparaten te bouwen die [&#x200B; Android &#x200B;](https://developer.android.com/about/versions) in werking stellen, kunt u naar de volgende documenten verwijzen:

* Android ontwikkelaarsdocumentatie voor [&#x200B; ANDROID_ID &#x200B;](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) wij adviseren om een hash functie SHA-256 op OS toe te passen verstrekte waarde._

### Op tv aangesloten apparaten {#tv-connected-devices}

#### tvOS {#tvos}

Om de `AP-Device-Identifier` kopbal voor apparaten te bouwen die [&#x200B; tvOS &#x200B;](https://developer.apple.com/documentation/tvos-release-notes) in werking stellen, kunt u naar de volgende documenten verwijzen:

* Apple ontwikkelaarsdocumentatie voor [&#x200B; identifierForVendor &#x200B;](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) wij adviseren om een hash functie SHA-256 op OS toe te passen verstrekte waarde._

#### Fire OS {#fireos}

Om de `AP-Device-Identifier` kopbal voor apparaten te bouwen die [&#x200B; Vuur OS &#x200B;](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) in werking stellen, kunt u naar de volgende documenten verwijzen:

* Android ontwikkelaarsdocumentatie voor [&#x200B; ANDROID_ID &#x200B;](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) wij adviseren om een hash functie SHA-256 op OS toe te passen verstrekte waarde._

#### Roku OS {#rokuos}

Om de `AP-Device-Identifier` kopbal voor apparaten te bouwen die [&#x200B; Roku OS &#x200B;](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) in werking stellen, kunt u naar de volgende documenten verwijzen:

* De ontwikkelaarsdocumentatie van Roku voor [&#x200B; GetChannelClientId &#x200B;](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) wij adviseren om een hash functie SHA-256 op OS toe te passen verstrekte waarde._

### Overige {#others}

Voor apparaatplatforms die niet in de documentatie worden vermeld, moet de apparaat-id worden gekoppeld aan een beschikbare hardware-identificatie, die doorgaans in de hardwarehandleiding van het apparaat wordt vermeld.

Als er geen hardware-id&#39;s beschikbaar zijn, moet een uniek gegenereerde id op basis van de kenmerken van de clienttoepassing worden gebruikt en in cache worden geplaatst in permanente opslag.
