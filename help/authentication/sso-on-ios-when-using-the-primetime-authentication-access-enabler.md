---
title: SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler
description: SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Overzicht

Single Sign-On (SSO) tussen apps met Adobe Pass-verificatie werkt op verschillende manieren, afhankelijk van het onderliggende besturingssysteem.

Dit documentadres **SSO op iOS**, bij gebruik van de Adobe Pass-verificatie **Toegangsfunctie**.

**Toegangsfunctie** **1,10** is de nieuwste versie van de native SDK van Adobe Pass Authentication iOS. Adobe raadt u ten zeerste aan naar deze versie te gaan in plaats van bij een oudere versie te blijven. Als u een oudere versie van Toegangsbeheer gebruikt, kunt u de recentste versie downloaden [hier](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

De SSO voor iOS wordt gedicteerd door de volgende omstandigheden:

- Toepassingen moeten hetzelfde gebruiken **tokenopslag** (in de vorm van een aangepast plakbord dat door Toegangsbeheer wordt gecreeerd).
- Toepassingen moeten hetzelfde genereren **Apparaat-id** (Toegangsfunctie berekent de apparaat-id op basis van het MAC-adres of de IDFV, afhankelijk van de OS-versie).

## Gedrag

Het gedrag van SSO is als volgt:

- **iOS 6 en lager**: SSO werkt automatisch tussen apps die door hetzelfde team of door verschillende teams zijn ontwikkeld. De apparaat-id wordt berekend op basis van het MAC-adres (dezelfde waarde wordt in alle apps geproduceerd) en het opslaggebied wordt door alle apps gedeeld (aangepast plakbord kan op iOS 6 en lager worden gedeeld door alle apps).
   - **Belangrijk:** Houd er rekening mee dat de iOS SDK 1.9.4-versie [de minimale iOS-implementatiedoelstelling verhoogd tot iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 en hoger**: SSO werkt onder de volgende omstandigheden:

1. Toepassingen worden gepubliceerd met hetzelfde Apple-distributieprofiel of profielen die tot hetzelfde team behoren. Dit is de enige manier waarop apps aangepaste plakborden kunnen delen op iOS 7 en hoger. In alle andere scenario&#39;s wordt het plakbord in sandbox per toepassing geplaatst. Van [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] en +\[`UIPasteboard pasteboardWithUniqueName`\] heeft nu een unieke naam, zodat alleen die toepassingen in dezelfde toepassingsgroep toegang hebben tot het plakbord. Als de ontwikkelaar een plakbord probeert te maken met een naam die al bestaat en geen deel uitmaakt van dezelfde app-suite, krijgt hij of zij een eigen unieke en persoonlijke plakbord. Merk op dat dit niet het systeem verstrekt plakborden, algemeen, en vinden beïnvloedt.

1. Toepassingen hebben hetzelfde voorvoegsel voor de bundel-id (alle componenten behalve de laatste). Alleen toepassingen die hetzelfde voorvoegsel voor de bundel-id hebben, berekenen dezelfde IDFV. Van [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): In IOS 7 worden alle componenten van de bundel, behalve de laatste component, gebruikt om de leverancier-id te genereren. Als de bundel-id slechts één component bevat, wordt de volledige bundel-id gebruikt.

Laten we ons nu richten op de **&#39;iOS 7 en hoger&#39;** scenario, aangezien het voor echte gebruikers het meest frequent is:

Beide voorwaarden (het delen van een profiel van het zelfde ontwikkelingsteam en het hebben van een gemeenschappelijke bundel herkenningsteken prefix) zijn verplichte voorwaarden voor SSO.

Hier volgen de mogelijke combinaties en de resultaten ervan:

- **Profielen van hetzelfde team en hetzelfde voorvoegsel voor de bundel-id**: Toepassingen delen dezelfde plakbordopslag en hebben dezelfde apparaat-id (IDFV). Een gebruiker hoeft slechts eenmaal te verifiëren (in de eerste gebruikte app) en de verificatiestatus wordt ook voor alle andere apps gebruikt. Voorbeeld:

1. Gebruiker opent app A (met bundel-id) *com.x.y.AppA*) en is niet geverifieerd
1. Gebruiker voert verificatie uit in app A
1. Gebruiker opent app B (met bundel-id) *com.x.y.AppB*) en wordt automatisch geverifieerd door de machtigingsgegevens te delen vanuit app A (vanaf stap 2)
1. Gebruiker opent app A en is nog steeds geverifieerd (vanaf stap 2)



- **Profielen van hetzelfde team maar andere voorvoegsels voor de bundel-id**: Toepassingen delen dezelfde plakbordopslag, maar hebben verschillende apparaat-id&#39;s (IDFV&#39;s). Een gebruiker moet één keer per app verifiëren. Voorbeeld:

1. Gebruiker opent app A (met bundel-id) *com.x.y.AppA*) en is niet geverifieerd
1. Gebruiker voert verificatie uit in app A
1. Gebruiker opent app B (met bundel-id) *com.z.AppB*) en de Access Enabler detecteert het token dat door de eerste app is verkregen (omdat de opslag wordt gedeeld), maar zal niet proberen het te gebruiken via SSO vanwege de verschillende apparaat-id&#39;s
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en is nog steeds geverifieerd (vanaf stap 2)



- **Profielen van verschillende teams (bundel-id is in dit scenario irrelevant)**: Toepassingen hebben verschillende pasteboard-opslagruimten en SSO&#39;s worden uitgeschakeld. Een gebruiker moet één keer per app verifiëren en de verificatiesessies blijven blijvend wanneer wordt geschakeld tussen apps. Voorbeeld:


1. Gebruiker opent app A en is niet geverifieerd
1. Gebruiker voert verificatie uit in app A
1. Gebruiker opent app B en is niet geverifieerd
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en is geverifieerd (vanuit stap 2)
1. Gebruiker opent app B en is geverifieerd (vanuit stap 4)

**Opmerking:** Houd er rekening mee dat de bovenstaande voorwaarden voor SSO&#39;s van toepassing zijn wanneer u toepassingen installeert via de **Apple App Store**. Als de toepassingen worden geïmplementeerd op een simulator (waar het ondertekenen van apps niet van toepassing is), worden ze geïnstalleerd met Xcode of worden ze gedistribueerd via een ad-hocprofiel, wat verschillende resultaten oplevert.

**Belangrijk:** Notitie (**betreffende AccessEnabler v1.8**): Het tweede hierboven beschreven scenario (profielen van hetzelfde team maar verschillende voorvoegsels van de bundel-id) zal een zeer onaangename gebruikerservaring creëren voor gebruikers van het **AccessEnabler v1.8** voor alle toepassingen die door hetzelfde team (mediabedrijf) zijn ontwikkeld. De gebruiker wordt automatisch afgemeld bij het overschakelen tussen apps van hetzelfde mediabedrijf. Daarom moeten ontwikkelaars van toepassingen zorgvuldig beslissen over de bundel-id en het distributieprofiel. Het exacte scenario in dit geval wordt hieronder weergegeven:

Toepassingen hebben dezelfde opslagruimte op het plakbord, maar hebben verschillende apparaat-id&#39;s (IDFV&#39;s). Een gebruiker moet één keer per app verifiëren, **maar de verificatiesessies worden gewist bij het schakelen tussen apps**. Voorbeeld:

1. Gebruiker opent app A (met bundel-id) *com.x.y.AppA*) en is niet geverifieerd
1. Gebruiker voert verificatie uit in app A
1. Gebruiker opent app B (met bundel-id) *com.z.AppB*) en de machtigingsgegevens die zijn gemaakt door app A worden automatisch gewist door Access Enabler (beveiligingsmechanisme dat een conflict detecteert tussen de momenteel berekende apparaat-id in app B en de id die is opgeslagen in de machtigingstokens die zijn gemaakt door app A)
1. Gebruiker voert verificatie uit in app B
1. Gebruiker opent app A en de machtigingsgegevens die door app B zijn gemaakt, worden automatisch gewist door Access Enabler (beveiligingsmechanisme dat een conflict detecteert tussen de momenteel berekende apparaat-id in app A en de id die is opgeslagen in de machtigingstokens die door app B zijn gemaakt)
