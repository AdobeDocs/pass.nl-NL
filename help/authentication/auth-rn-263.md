---
title: Opmerkingen bij de release Adobe Pass Authentication 2.63
description: Opmerkingen bij de release Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-263}

* [Buildnummer](#build-number)
* [Nieuwe functies](#new-features)

### Buildnummer {#build-number-263}

Adobe Pass-verificatie: adobe-pass-**2,63**
Releasedatum: **20-09-2022 - 22-09-2022**

### Nieuwe functies {#new-features-263}

#### Platformidentificatiemechanisme verbeteren {#pf-identification-mech}

* Vanaf deze release is het mechanisme verbeterd waarmee een apparaat wordt geïdentificeerd. Het is niet langer afhankelijk van de implementatie aan de clientzijde. Dit zal een nauwkeurigere granulariteit wanneer het toepassen van bedrijfsregels op platformniveau, en een beter inzicht in verkeerswaarden in ESM- rapporten verstrekken.

* Binnenkort volgt een nieuwe ESM-release met nieuwe en verbeterde rapporten waarin de platformgerelateerde velden worden weergegeven.

* Voor meer informatie over de geplande wijzigingen, gelieve uw TAM te bereiken.

#### Zelfafbraak van MVPD {#mvpd-self-degradation}

Deze eigenschap voorziet MVPDs van het vermogen om hun eigen authentificatie en vergunningseindpunten voor hoge verkeersscenario&#39;s tijdelijk over te slaan wanneer de lading op die respectieve eindpunten te hoog wordt.


#### Proxied ID toevoegen in de koptekst van de aanroepen voor machtigingen {#add-proxied-id}

Deze eigenschap voegt identiteitskaart van een Synacor proxied MVPD in de kopbal van de vergunningsvraag toe. Dat laat Synacor aan opstellings bedrijfsregels voor elk individueel proxied (bijvoorbeeld) toe. het verpletteren aan verschillende domeinen per proxy (MVPD).


#### TVE-dashboard {#tve-dashboard}

In deze versie hebben we een probleem opgelost waarbij authN of authZ TTL op MVPD-niveau niet correct werd berekend in de configuratierapporten.


#### JavaScript SDK 4.6.0 {#js-sdk}

* Het gebruik van `eval` functie, waardoor de SDK compatibel wordt gemaakt met het inhoudsbeveiligingsbeleid.
* Probleem verholpen waardoor de verificatiestroom niet met succes kon worden voltooid wanneer de lokale opslag van de browser expliciet door een Partner-toepassing werd gewist.
