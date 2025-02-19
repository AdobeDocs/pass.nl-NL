---
title: Opmerkingen bij de release Adobe Pass Authentication 2.63
description: Opmerkingen bij de release Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.63 {#authn-263-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-263}

* [Buildnummer](#build-number-263)
* [Overzicht van release](#release-overview-263)

### Buildnummer {#build-number-263}

De Authentificatie van Adobe Pass: adobe-pas **2.63**

Datum van de versie: **09/20/2022 - 09/22/2022**

### Overzicht van release {#release-overview-263}

#### Platformidentificatiemechanisme verbeteren

* Vanaf deze release is het mechanisme verbeterd waarmee een apparaat wordt geïdentificeerd. Het is niet langer afhankelijk van de implementatie aan de clientzijde. Dit zal een nauwkeurigere granulariteit wanneer het toepassen van bedrijfsregels op platformniveau, en een beter inzicht in verkeerswaarden in ESM- rapporten verstrekken.

* Binnenkort volgt een nieuwe ESM-release met nieuwe en verbeterde rapporten waarin de platformgerelateerde velden worden weergegeven.

* Voor meer informatie over de geplande wijzigingen, gelieve uw TAM te bereiken.

#### MVPD-zelfdegradatie

Deze eigenschap voorziet MVPDs van het vermogen om hun eigen authentificatie en vergunningseindpunten voor hoge verkeersscenario&#39;s tijdelijk over te slaan wanneer de lading op die respectieve eindpunten te hoog wordt.

#### Proxied ID toevoegen in de koptekst van de aanroepen voor machtigingen

Deze functie voegt de id van een Synacor proxied MVPD toe aan de header van de aanroepende machtiging. Dat laat Synacor aan opstellings bedrijfsregels voor elk individueel proxied (bijvoorbeeld) toe. routering naar verschillende domeinen per proxy (MVPD).

#### TVE-dashboard

In deze versie hebben we een probleem opgelost waarbij authN of authZ TTL op MVPD-niveau niet correct werd berekend in de configuratierapporten.

#### JavaScript SDK 4.6.0

* Het gebruik van de functie `eval` is verwijderd, waardoor de SDK compatibel wordt gemaakt met Content Security Policy.
* Probleem verholpen waardoor de verificatiestroom niet met succes kon worden voltooid wanneer de lokale opslag van de browser expliciet door een Partner-toepassing werd gewist.
