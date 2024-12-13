---
title: SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler
description: SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 0%

---

# (Verouderd) SSO op iOS bij gebruik van de Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Overzicht

Single Sign-On (SSO) tussen apps met Adobe Pass-verificatie werkt op verschillende manieren, afhankelijk van het onderliggende besturingssysteem.

Dit document richt **SSO op iOS**, wanneer het gebruiken van de Authentificatie van Adobe Pass **Toegelaten Toegang**.

**** 1.10 **van Toegangsactivering van 0} {is de recentste versie van de Authentificatie iOS van Adobe Pass inheemse SDK.** Adobe raadt u ten zeerste aan naar deze versie te gaan in plaats van bij een oudere versie te blijven. Als u een oudere versie van Toegankelijkheid gebruikt, kunt u de recentste versie [ hier ](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library) downloaden.

De SSO voor iOS wordt gedicteerd door de volgende omstandigheden:

- Apps moeten de zelfde **symbolische opslag** (in de vorm van een douaneplakbord gebruiken dat door Toegankelijkheid wordt gecreeerd).
- Toepassingen moeten zelfde **identiteitskaart van het Apparaat** produceren (Toegang toelaat verwerkt identiteitskaart van het Apparaat die op het adres van MAC of IDFV, afhankelijk van de OS versie wordt gebaseerd).

## Gedrag

Het gedrag van SSO is als volgt:

- **iOS 6 en lager**: SSO werkt automatisch tussen apps die door het zelfde team of verschillende teams worden ontwikkeld. De apparaat-id wordt berekend op basis van het MAC-adres (dezelfde waarde wordt in alle apps geproduceerd) en het opslaggebied wordt door alle apps gedeeld (aangepast plakbord kan op iOS 6 en lager worden gedeeld door alle apps).
   - **Belangrijk:** Gelieve te merken op dat de versie van iOS SDK 1.9.4 [ het minimumplaatsingsdoel van iOS aan iOS 7 heeft verhoogd.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 en hoger**: SSO zal in de volgende voorwaarden werken:

1. Toepassingen worden gepubliceerd met hetzelfde Apple-distributieprofiel of profielen die tot hetzelfde team behoren. Dit is de enige manier waarop apps aangepaste plakborden kunnen delen op iOS 7 en hoger. In alle andere scenario&#39;s wordt het plakbord in sandbox per toepassing geplaatst. Van [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html* ](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+ \ [`UIPasteboard pasteboardWithName:create:\`] en + \ [`UIPasteboard pasteboardWithUniqueName` \] nu uniek de bepaalde naam om slechts die apps in de zelfde toepassingsgroep toe te staan om tot het plakbord toegang te hebben. Als de ontwikkelaar een plakbord probeert te maken met een naam die al bestaat en geen deel uitmaakt van dezelfde app-suite, krijgt hij of zij een eigen unieke en persoonlijke plakbord. Merk op dat dit niet het systeem verstrekt plakborden, algemeen, en vinden beïnvloedt.

1. Toepassingen hebben hetzelfde voorvoegsel voor de bundel-id (alle componenten behalve de laatste). Alleen toepassingen die hetzelfde voorvoegsel voor de bundel-id hebben, berekenen dezelfde IDFV. Van [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice \ _Class/index.html \#//apple\_ref/occ/instp/UIDevice/identifierForVendor* ](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): Op IOS 7, worden alle componenten van de bundel behalve de laatste component gebruikt om verkopersidentiteitskaart te produceren. Als de bundel-id slechts één component bevat, wordt de volledige bundel-id gebruikt.

Laten we nu focussen op het **&#39;iOS 7 en hoger&#39;** scenario, aangezien het voor echte gebruikers het meest frequent is:

Beide voorwaarden (het delen van een profiel van het zelfde ontwikkelingsteam en het hebben van een gemeenschappelijke bundel herkenningsteken prefix) zijn verplichte voorwaarden voor SSO.

Hier volgen de mogelijke combinaties en de resultaten ervan:

- **Profielen van het zelfde team en de zelfde prefix van identiteitskaart van de Bundel**: apps zullen de zelfde plakbordopslag delen en zullen zelfde identiteitskaart van het Apparaat (IDFV) hebben. Een gebruiker hoeft slechts eenmaal te verifiëren (in de eerste gebruikte app) en de verificatiestatus wordt ook voor alle andere apps gebruikt. Voorbeeld:

1. De gebruiker opent app A (met Bundel identiteitskaart *com.x.y.AppA*) en is niet-voor authentiek verklaard
1. Gebruiker voert verificatie uit in app A
1. De gebruiker opent app B (met Bundel identiteitskaart *com.x.y.AppB*) en automatisch door de machtigingsgegevens van app te delen voor authentiek verklaard
A (vanaf stap 2)
1. Gebruiker opent app A en is nog steeds geverifieerd (vanaf stap 2)



- **Profielen van het zelfde team maar de verschillende prefixen van identiteitskaart van de Bundel**: apps zullen de zelfde plakbordopslag delen maar zullen verschillende Apparaat IDs (IDFVs) hebben. Een gebruiker moet één keer per app verifiëren. Voorbeeld:

1. De gebruiker opent app A (met Bundel identiteitskaart *com.x.y.AppA*) en is niet-voor authentiek verklaard
1. Gebruiker voert verificatie uit in app A
1. De gebruiker opent app B (met Bundel identiteitskaart *com.z.AppB*) en Toegangsmanager ontdekt het teken dat door eerste app wordt verkregen (omdat de opslag wordt gedeeld) maar het zal niet proberen om het via SSO wegens verschillende Apparaat IDs te gebruiken
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en is nog steeds geverifieerd (vanaf stap 2)



- **Profielen van verschillende teams (Bundle ID is irrelevant in dit scenario)**: apps zullen verschillende pasteboard opslag hebben en SSO zal tussen hen worden onbruikbaar gemaakt. Een gebruiker moet één keer per app verifiëren en de verificatiesessies blijven blijvend wanneer wordt geschakeld tussen apps. Voorbeeld:


1. Gebruiker opent app A en is niet geverifieerd
1. Gebruiker voert verificatie uit in app A
1. Gebruiker opent app B en is niet geverifieerd
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en is geverifieerd (vanuit stap 2)
1. Gebruiker opent app B en is geverifieerd (vanuit stap 4)

**Nota:** gelieve nota te nemen dat de bovengenoemde voorwaarden voor SSO van toepassing zijn wanneer het installeren van toepassingen door **Apple App Store**. Als de toepassingen worden geïmplementeerd op een simulator (waar het ondertekenen van apps niet van toepassing is), worden ze geïnstalleerd met Xcode of worden ze gedistribueerd via een ad-hocprofiel, wat verschillende resultaten oplevert.

**Belangrijk:** Nota (**betreffende AccessEnabler v1.8**): Het tweede hierboven beschreven scenario (profielen van het zelfde team maar de verschillende prefixen van identiteitskaart van de Bundel) zal tot een zeer onaangename gebruikerservaring voor gebruikers van **AccessEnabler v1.8** over toepassingen leiden die door het zelfde team (media bedrijf) worden ontwikkeld. De gebruiker wordt automatisch afgemeld bij het overschakelen tussen apps van hetzelfde mediabedrijf. Daarom moeten ontwikkelaars van toepassingen zorgvuldig beslissen over de bundel-id en het distributieprofiel. Het exacte scenario in dit geval wordt hieronder weergegeven:

Toepassingen hebben dezelfde opslagruimte op het plakbord, maar hebben verschillende apparaat-id&#39;s (IDFV&#39;s). Een gebruiker zal één keer per elke app voor authentiek moeten verklaren, **maar de authentificatiesessies zullen worden gewist wanneer het schakelen tussen apps**. Voorbeeld:

1. De gebruiker opent app A (met Bundel identiteitskaart *com.x.y.AppA*) en is niet-voor authentiek verklaard
1. Gebruiker voert verificatie uit in app A
1. De gebruiker opent app B (met Bundel identiteitskaart *com.z.AppB*) en het machtigingsgegeven dat door app A werd gecreeerd wordt automatisch gewist door Toegangsmanager (veiligheidsmechanisme dat een botsing tussen momenteel gegevens verwerkt identiteitskaart van het Apparaat in app B en die in machtigingstokens - die door app A wordt gecreeerd ontdekt wordt opgeslagen)
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en de machtigingsgegevens die door app B zijn gemaakt, worden automatisch gewist door Access Enabler (beveiligingsmechanisme dat een conflict detecteert tussen de momenteel berekende apparaat-id in app A en de id die is opgeslagen in de machtigingstokens die door app B zijn gemaakt)
