---
title: Tracking Prevention Assessment Google Chrome
description: Tracking Prevention Assessment Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# (Legacy) Tracking Prevention Assessment - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht

In dit document worden nuttige bronnen samengevoegd en worden de wijzigingen beoordeeld die Google Chrome in het vooruitzicht stelt als onderdeel van hun initiatief om cookies van derden af te bouwen.

De evaluatie wordt uitgevoerd voor TVE-toepassingen (TV Anywhere) die worden uitgevoerd in de Google Chrome-browser en die Adobe Pass Access Enabler JavaScript SDK v4 gebruiken om te integreren met de Adobe Pass Authentication-backendservices.

## Openbare middelen

Hieronder vindt u een lijst met bronnen die zijn verzameld op de website van Google Developer en ook op hun officiële blog waarin we onze klanten aanbevelen om te raadplegen:

* [ de volgende stap naar het geleidelijk uit elimineren derdekoekjes in Chrome ](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [ documentatie van de Ontwikkelaar voor Sandbox van de Privacy ](https://developers.google.com/privacy-sandbox)
* [ voorbereidingen voor derdekoekjesbeperkingen ](https://developers.google.com/privacy-sandbox/3pcd)
* [ voorbereidingen voor de dederdekoekjesgeleidelijke eliminatie ](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [ Voorbereidend voor het eind van derdekoekjes ](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [ derdekoekjes die door gebrek voor 1% van de gebruikers van Chrome worden beperkt ](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Tijdlijn

Als kort samenvattend Google Chrome begon [ het Volgen Beveiliging ](https://privacysandbox.com/) te testen, een nieuwe eigenschap die dwars-plaats het volgen beïnvloedt die alle derdekoekjes beïnvloedt.

Aanvankelijk begon dit begin 2024, wat ongeveer 1% van de gebruikers betreft en hun (tentatieve) plan is om dit met ingang van het derde kwartaal van 2024 met maximaal 100% van de gebruikers te verlengen.

## Beoordeling

Google heeft een document gepubliceerd waarin het aanbevolen afspeelboek wordt samengevoegd om de uitfasering van cookies van andere bedrijven voor te bereiden: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

We hebben ons aan dit afspeelboek gehouden voor de beoordeling van TVE-toepassingen (TV Anywhere) die op Google Chrome-browser worden uitgevoerd en die Adobe Pass Access Enabler JavaScript v4 gebruiken om te integreren met de Adobe Pass Authentication-backendservices.

### Conclusies

Gebaseerd op ons het testen, die de aanstaande updates van Google Chrome simuleren, zullen de primaire bedrijfsstromen van TVE **blijven functioneren zoals verwacht**.

Het is echter van cruciaal belang dat de bredere strategie van Google wordt erkend, die niet alleen inhoudt dat cookies van derden worden stopgezet, maar ook dat de opslag van derde partijen wordt verdeeld.

Dit betekent dat Chrome-gebruikers te maken krijgen met onderbrekingen met Single Sign-On (SSO), Single Logout (SLO) en passieve verificatiefuncties, waarvoor aparte aanmeldings- en afmeldingsacties nodig zijn voor elke TVE-toepassing die zij gebruiken (in overeenstemming met de huidige ervaring in Safari).

## Oproep tot zelfbeoordeling

We dringen er bij onze klanten op aan om proactief soortgelijke evaluaties uit te voeren om potentiële problemen ruim van tevoren vast te stellen en zich vertrouwd te maken met de herziene gebruikerservaring van Google Chrome.

Deze evaluatie moet zowel betrekking hebben op services van de eerste partij als op diensten van derden, met name wat betreft de integratie van Adobe Pass Access Enabler JavaScript SDK v4.

Voor het geval u om het even welke moeilijkheden met de bedrijfsstromen van TVE, met inbegrip van authentificatie, pre-vergunning, vergunning, gebruikersmeta-gegevens, of logout tegenkomt, nodigen wij u uit om een rapport door een kaartje van Zendesk bij ons team van de klantenzorg in te dienen.

Raadpleeg de volgende secties voor hulp bij het ontwikkelen van uw zelfbeoordelingsplan.

### Controle van het gebruik van cookies

Beginnend met Chrome 118, benadrukt het [&#128279;](https://developer.chrome.com/docs/devtools/issues/) lusje van de Kwesties DevTools potentieel beïnvloede koekjes met het volgende bericht: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Cookies die zijn gemarkeerd voor gebruik door derden, kunnen worden geïdentificeerd aan de hand van hun kenmerkwaarde `SameSite=None` .

Klik op deze koppeling voor meer informatie: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Test op breuk

Start Chrome met behulp van de opdrachtregelmarkering `--test-third-party-cookie-phaseout` of schakel `#test-third-party-cookie-phaseout` in `chrome://flags/` in om te testen op brainstorming.

Hiermee wordt Google Chrome ingesteld om cookies van derden te blokkeren en ervoor te zorgen dat de toekomstige functionaliteit actief is om de status na de uitfasering het best te simuleren.

De technische specificaties van de volgende Chrome-vlaggen moeten grondig worden herzien:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Klik op deze koppeling voor meer informatie: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Andere browsers

### Firefox

Firefox had een uitrol van zijn mechanisme: `Enhanced Tracking Protection` een paar jaar geleden.

Zie hieronder enkele nuttige bronnen van Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari had een uitrol van hun mechanisme: `Intelligent Tracking Prevention` een paar jaar geleden.

Zie hieronder enkele nuttige bronnen van Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Zie hieronder enkele nuttige bronnen van Adobe Pass:

* [Trackingpreventiebeoordeling - Apple Safari](tracking-prevention-assessment-apple-safari.md)
