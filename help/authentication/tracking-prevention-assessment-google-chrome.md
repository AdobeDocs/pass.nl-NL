---
title: Trackingpreventie-evaluatie Google Chrome
description: Trackingpreventie-evaluatie Google Chrome
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Trackingpreventie-evaluatie - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht

In dit document worden nuttige bronnen gebundeld en worden de wijzigingen beoordeeld die Google Chrome in het vooruitzicht stelt als onderdeel van zijn initiatief om cookies van derden af te bouwen.

De evaluatie wordt uitgevoerd voor TVE-toepassingen (TV Anywhere) die worden uitgevoerd in de Google Chrome-browser en die Adobe Pass Access Enabler JavaScript SDK v4 gebruiken om te integreren met de Adobe Pass Authentication-backendservices.

## Openbare middelen

Hieronder vindt u een lijst met bronnen die zijn verzameld op de website van Google Developer en ook op hun officiële blog waarin we onze klanten aanbevelen om te raadplegen:

* [De volgende stap in de richting van de geleidelijke afschaffing van cookies van derden in Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentatie voor ontwikkelaars voor de privacysandbox](https://developers.google.com/privacy-sandbox)
* [Voorbereiden op cookie-beperkingen van derden](https://developers.google.com/privacy-sandbox/3pcd)
* [Voorbereiden op het uitfaseren van cookies van derden](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Voorbereiden op het einde van cookies van andere bedrijven](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Cookies van andere leveranciers die standaard zijn beperkt voor 1% van de Chrome-gebruikers](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Tijdlijn

Google Chrome is begonnen met testen [Trackingsbeveiliging](https://privacysandbox.com/), een nieuwe functie die het opsporen van andere sites beperkt en die van invloed is op alle cookies van derden.

Aanvankelijk begon dit begin 2024, wat ongeveer 1% van de gebruikers betreft en hun (tentatieve) plan is om dit met ingang van het derde kwartaal van 2024 met maximaal 100% van de gebruikers te verlengen.

## Beoordeling

Google heeft een document gepubliceerd waarin het aanbevolen afspeelboek wordt samengevoegd om de uitfasering van cookies van andere bedrijven voor te bereiden: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

We hebben ons aan dit afspeelboek gehouden voor de beoordeling van TVE-toepassingen (TV Anywhere) die worden uitgevoerd in de Google Chrome-browser en die Adobe Pass Access Enabler JavaScript SDK v4 gebruiken om te integreren met de Adobe Pass Authentication-backendservices.

### Conclusies

De belangrijkste TVE-bedrijfsstromen zijn gebaseerd op onze tests en simuleren de komende updates van Google Chrome **blijven functioneren zoals verwacht**.

Het is echter van cruciaal belang dat de bredere strategie van Google wordt erkend, die niet alleen inhoudt dat cookies van derden worden stopgezet, maar ook dat de opslag van derde partijen wordt verdeeld.

Dit betekent dat Chrome-gebruikers te maken krijgen met onderbrekingen met Single Sign-On (SSO), Single Logout (SLO) en passieve verificatiefuncties, waarvoor aparte aanmeldings- en afmeldingsacties nodig zijn voor elke TVE-toepassing die zij gebruiken (uitgelijnd op de huidige ervaring in Safari).

## Oproep tot zelfbeoordeling

We dringen er bij onze klanten op aan om proactief soortgelijke evaluaties uit te voeren om potentiële problemen ruim van tevoren vast te stellen en zich vertrouwd te maken met de herziene gebruikerservaring van Google Chrome.

Deze evaluatie moet zowel services van de eerste partij als services van derden omvatten, met name met betrekking tot de integratie van Adobe Pass Access Enabler JavaScript SDK v4.

Voor het geval u om het even welke moeilijkheden met de bedrijfsstromen van TVE, met inbegrip van authentificatie, pre-vergunning, vergunning, gebruikersmeta-gegevens, of logout tegenkomt, nodigen wij u uit om een rapport door een kaartje van Zendesk bij ons team van de klantenzorg in te dienen.

Raadpleeg de volgende secties voor hulp bij het ontwikkelen van uw zelfbeoordelingsplan.

### Controle van het gebruik van cookies

Vanaf Chrome 118 wordt de [Problemen met DevTools](https://developer.chrome.com/docs/devtools/issues/) op het tabblad worden mogelijk betrokken cookies gemarkeerd met het volgende bericht: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Cookies die zijn gemarkeerd voor gebruik door derden kunnen worden geïdentificeerd aan de hand van hun `SameSite=None` kenmerkwaarde.

Klik op deze koppeling voor meer informatie: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Test op breuk

Start Chrome met het gereedschap `--test-third-party-cookie-phaseout` opdrachtregelmarkering of uit Chrome 118 inschakelen `#test-third-party-cookie-phaseout` in `chrome://flags/`.

Hiermee wordt Google Chrome ingesteld om cookies van derden te blokkeren en ervoor te zorgen dat de toekomstige functionaliteit actief is om de status na de uitfasering het best te simuleren.

De technische specificaties van de volgende Chrome-vlaggen moeten grondig worden herzien:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Klik op deze koppeling voor meer informatie: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Andere browsers

### Firefox

Firefox had een uitrol van het mechanisme: `Enhanced Tracking Protection` een paar jaar geleden .

Zie hieronder enkele nuttige bronnen van Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari had een uitrol van het mechanisme: `Intelligent Tracking Prevention` een paar jaar geleden .

Zie hieronder enkele nuttige bronnen van Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Zie hieronder enkele nuttige bronnen van Adobe Pass:

* [Trackingpreventiebeoordeling - Apple Safari](tracking-prevention-assessment-apple-safari.md)
