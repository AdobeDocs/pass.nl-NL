---
title: Beperkingen van JS SDK voor Safari-browser
description: Beperkingen van JS SDK voor Safari-browser
exl-id: 5e5c3b36-ee09-49e0-b5b7-83b24854d69d
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1804'
ht-degree: 0%

---

# Beperkingen van JS SDK voor Safari-browser {#js-sdk-limitations-for-safari-browser}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

<!--
>[!IMPORTANT] 
>
>We are strongly recommending [migration to AccessEnabler JavaScript SDK versions 4.x](http://tve.helpdocsonline.com/accessenabler-js-v4-migration-guide) in order to have a stable and predictable behavior on Safari browser.-->


## Safari 10 {#safari10}

**Details**

* Beginnend met Safari 10, zullen de standaardbrowser privacymontages de Enige Sign-On (SSO), Enige Logout (SLO) en passieve authentificatiefuncties veroorzaken ophouden werkend. Single Sign-On (SSO) en passieve authentificatie zullen niet zelfs in de zelfde zitting tussen veelvoudige lusjes of browser vensters werken.

* Deze wijzigingen zijn van invloed op en hebben invloed op de Adobe Pass-verificatieprocessen voor de volgende versies van de JavaScript SDK van AccessEnabler: v2 (versies 2.x), v3 (versies 3.x) en v4 (versies 4.x).

### Oplossing {#mitigation-safari10}

* Om deze beperkingen te beperken, kunt u de gebruiker de instructie geven de privacyinstellingen van Safari 10 voor de browser te wijzigen en &quot;**Altijd toestaan**&quot; te gebruiken voor de &quot;**Cookies en websitegegevens**&quot;-item op het tabblad Privacy van de browser via Voorkeuren, zoals in de onderstaande afbeelding wordt weergegeven.

  ![](assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit rubriek Safari 10 zijn nog steeds van toepassing in het geval van Safari 11.

* Vanaf Safari 11 introduceert de browser [Intelligente traceringspreventie](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP), een technologie die heuristiek gebruikt om intersite tracering te voorkomen. Deze heuristiek is van invloed op de manier waarop cookies van derden worden opgeslagen en opnieuw afgespeeld op netwerkaanroepen. Dit betekent dat, afhankelijk van de activering van het ITP-mechanisme, de Safari-browser cookies van derden in de communicatie tussen client en server blokkeert.

* De Adobe Pass-verificatieservice gebruikt cookies als onderdeel van het verificatieproces en vertrouwt erop **om te functioneren**. In situaties waarin het verificatieproces automatisch plaatsvindt (bijvoorbeeld Temperatuurcontrole) of in implementaties waarin iFrames of de functie &quot;Zonder refressless&quot; wordt gebruikt, worden cookies van Adoben beschouwd als cookies van derden en worden deze standaard geblokkeerd. In andere gevallen gebruikt Safari een computerleeralgoritme dat alle de dienstkoekjes van de Authentificatie van de Adobe Primetime als het volgen van koekjes zou kunnen merken, daarom onderworpen voor het blokkeren van ITP.

* Concluderend kan een gebruiker van Safari 11 browser niet op een Adobe Pass Authentificatie toegelaten website na de activering van het Intelligent Tracking Preventie (ITP) mechanisme voor authentiek verklaren, vooral wanneer de gebruikers veelvoudige Adobe Primeime Authentificatie toegelaten websites gebruiken. Daarom kan de verificatieervaring van de gebruiker onverwacht en ongedefinieerd zijn, variërend van onmogelijkheid tot aanmelden tot een kortere dan verwachte verificatieduur.

* Deze wijzigingen zijn van invloed op en hebben invloed op de Adobe Pass-verificatieprocessen van de volgende versies van de JavaScript SDK van AccessEnabler: v2 (versies 2.x), v3 (versies 3.x).

### Oplossing {#mitigation-safari11}

* Voor zowel AccessEnabler JavaScript SDK v3 (versies 3.x) als AccessEnabler JavaScript SDK v4 (versies 4.x) bevat de bibliotheek een mechanisme waarmee kan worden vastgesteld in welke situaties de verificatie van de gebruiker is geblokkeerd omdat vereiste cookies ontbreken. In deze situaties activeert de bibliotheek een specifieke callback-fout [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference), die wordt doorgegeven aan de website die geschikt is voor Adobe Pass-verificatie, zodat deze kan worden gebruikt als signaal om de gebruiker te instrueren maatregelen te nemen die het probleem kunnen verhelpen. Om van dit mechanisme te kunnen profiteren, moet de website het [Fout bij rapporteren](/help/authentication/error-reporting.md) specificatie.

* Voor AccessEnabler JavaScript SDK v2 (versies 2.x) biedt de bibliotheek het hierboven beschreven mechanisme niet. Daarom kan de voor Adobe Pass-verificatie ingeschakelde website niet worden gesignaleerd wanneer de gebruiker moet worden opgedragen om actie te ondernemen om het probleem te verhelpen.

* De lijst met acties die de bovengenoemde problemen kunnen verhelpen **is van toepassing op alle drie versies** van AccessEnabled JavaScript SDK.

* Wanneer [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) de fout callback wordt ontvangen door de website van de uitvoerder, zou de gebruiker moeten worden geïnstrueerd om Intelligent het Volgen Preventie (ITP) onbruikbaar te maken en 3de partijkoekjes toe te laten door:

* In het geval van Mac OS X High Sierra en hoger: de optie &quot;**Spatiëring tussen sites voorkomen**&quot; optie voor &quot;**Websites bijhouden**&quot;-item op het tabblad Privacy van de browser via Voorkeuren, zoals in de onderstaande afbeelding wordt weergegeven.

  ![](assets/uncheck-prvnt-cr-st-tr-safari11.png)

* In het geval van Mac OS X Sierra en previous: Checking the &quot;**Altijd toestaan**&quot; te gebruiken voor de &quot;**Cookies en websitegegevens**&quot;-item op het tabblad Privacy van de browser via Voorkeuren, zoals in de onderstaande afbeelding wordt weergegeven.

  ![](assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit sectie Safari 10 en sectie Safari 11 zijn nog steeds van toepassing in het geval van Safari 12.

In dit gedeelte worden de compatibiliteitsproblemen van **AccessEnabler JavaScript SDK-versies 4.x** over Safari 12.

>[!NOTE]
>
>Houd er rekening mee dat in het geval van AccessEnabler JavaScript SDK versies 2.x en AccessEnabler JavaScript SDK versies 3.x, beide cookies van derden gebruiken voor de verificatieprocessen, en dat vanwege het beleid van ITP en andere cookies van andere bedrijven die beginnen met Safari 11, de verificatieervaring van de gebruiker onverwacht en ongedefinieerd kan zijn, variërend van het niet aanmelden tot een kortere dan verwachte verificatieduur.


### Gecertificeerde functionaliteit van AccessEnabler JavaScript SDK v4 (versies 4.x) op Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

* **Verificatie** De stromen die gebruik van gebruikersinteractie maken zullen altijd werken, zelfs als browser van de gebruiker heeft derde partijkoekjes onbruikbaar gemaakt, omdat vanaf versie 4.0 de JavaScript SDK AccessEnabler geen derdekoekjes meer voor de authentificatieprocessen gebruikt.

>[!NOTE]
>
>De gebruiker MOET met de plaats in wisselwerking staan om login popups te openen en/of met de MVPD login pagina in wisselwerking te staan.

* **Autorisatie/Preflight/gebruikersmetagegevens** bewerkingen zijn volledig functioneel, mits de gebruiker al is geverifieerd.

### Bekende problemen met AccessEnabler JavaScript SDK v4 (versies 4.x) op Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO en SLO

   * Vanwege de implementatie van localStorage in Safari vanaf Safari 10 kan de JS SDK de aanmeldingsstatus niet meer delen via een gemeenschappelijke domein-iFrame. Dit betekent dat de gebruiker zich op elke plaats moet aanmelden die JavaScript SDK AccessEnabler gebruikt. Als u zich afmeldt, worden er ook geen verificatietokens verwijderd van verschillende sites. De gebruiker moet zich daarom afmelden bij elke website waarop Adobe Pass-verificatie is ingeschakeld.

* Temperatuurcontrole

   * Voor tijdelijke overgangen, gebruikt AccessEnabler JavaScript SDK een individualisatiemechanisme, om een authentificatietoken aan een specifiek apparaat (browser instantie) te sluiten. Door de nieuwe mechanismen in Safari 12 die zijn ontworpen om tracering te voorkomen, zijn de vingerafdrukken die we berekenen en gebruiken in het individualisatiemechanisme **zal het zelfde voor alle gebruikers zijn die het zelfde IP adres hebben**. Wij nemen cliëntIP voor individualisatiedoeleinden in overweging, maar zelfs zo is het effect op gebruikers die het zelfde openbare IP adres delen. Voor deze gebruikers berekenen we dezelfde individualisatie-id en de tijdelijke pas wordt eraan gekoppeld. Dit betekent dat als een dergelijke gebruiker een tijdelijke pas gebruikt, niemand anders er toegang toe heeft \! Dit heeft vooral gevolgen voor zakelijke gebruikers, onderwijsinstellingen of andere organisaties die meerdere gebruikers hebben die NAT of een gemeenschappelijke proxy gebruiken om toegang te krijgen tot internet.

>[!NOTE]
>
>Dit probleem is alleen van invloed op gebruikers als de implementator de Temp-controle gebruikt als gevolg van gebruikersinteractie, anders is de Temp-controle onderworpen aan **Automatische stromen** hieronder.

* Automatische stromen

   * Verificatiestromen die in een geautomatiseerde modus worden geprobeerd, zonder gebruikersinteractie zullen niet slagen in Safari 12 bij het gebruik van JS SDK 4.0. De komende JS SDK 4.1 verhelpt alle problemen met geautomatiseerde stromen.

Gebruik gevallen die door dit probleem worden beïnvloed:

* Automatische TempPass-verificatie (Free Preview) - voor dergelijke stromen geeft de SDK een N130-fout.

* Passieve verificatie (mislukt stil) - de gebruiker wordt gevraagd om deze MVPD te selecteren en referenties in te voeren

### Oplossing {#mitigation-safari12}

**SSO en SLO**

Er is op het moment van schrijven geen bekende beperking beschikbaar of mogelijk. Apple heeft in Safari 12 een &quot;Storage Access API&quot; geïntroduceerd (`https://webkit.org/blog/8124/introducing-storage-access-api`), maar de huidige implementatie is niet van toepassing op localStorage, maar alleen op cookies. Bovendien vereist de API gebruikersinteractie om te worden gebruikt, en zodra u het gebruikt, wordt de gebruiker ook ertoe aangezet met een toestemmingsdialoog gelijkend op hieronder.

![](assets/permission-dialog-apple.png)


Op dit punt richten deze vereisten/herinneringen van Safari zich niet op onze vereisten van UX en wij hebben geen verenigbaar gedrag zoals op andere browsers, waar SSO &quot;enkel&quot;werkt zodra wij een teken in een gemeenschappelijk domein localStorage bewaarde.

**Temperatuurcontrole**

Om de individualisatieproblemen te verlichten en een gebruikersinteractie te hebben adviseren wij u om te gebruiken **[Tijdelijke controle voor speciale acties](/help/authentication/promotional-temp-pass.md)** Op een interactieve manier en geef minstens één aanvullende informatie over de gebruiker op (bijvoorbeeld een e-mailadres).

## Safari 13 {#safari13}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit de secties Safari 10 tot en met Safari 12 zijn nog steeds van toepassing in het geval van Safari 13.


Vanaf Safari 13 voert de browser nieuwe wijzigingen in het dialoogvenster [Intelligente traceringspreventie](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), waardoor de heuristiek achter het mechanisme strikter wordt bij het definiëren van cookies van derden als het bijhouden van cookies, om te voorkomen dat cookies van andere sites worden bijgehouden.

Zoals in vorige secties wordt beschreven, gebruikt de dienst van de Authentificatie van Adobe Pass en vertrouwt op derdekoekjes als deel van de authentificatieprocessen wanneer de implementors gebruiken AccessEnabler JavaScript SDK v2 (versies 2.x) en AccessEnabler JavaScript SDK v3 (versies 3.x). Vergeleken met eerdere versies van Safari browser toen ITP intrad nadat het enige tijd was doorgebracht om te &quot;leren&quot;over de interactie tussen de gebruiker en de betrokken partijen (de websites en de Adobe van de programmeur), blokkeert Safari 13 browser van de start derde partijkoekjes die als het volgen van koekjes in de cliënt - server modelmededeling worden beschouwd.

Samenvattend kan een gebruiker van Safari 13-browser hoogstwaarschijnlijk geen nieuwe verificaties starten op een website die geschikt is voor Adobe Pass-verificatie en die gebruikmaakt van een oudere versie van AccessEnabler JavaScript SDK, v2 (versies 2.x) of v3 (versies 3.x). Dit gebeurt wegens het feit dat alle vereiste de dienstkoekjes van de Authentificatie van de Adobe Primetime door ITP worden geblokkeerd, daarom makend de dienst niet aan het authentificatieverzoek kan voldoen.

AccessEnabler JavaScript SDK v4-bibliotheek (versie 4.x) gebruikt geen cookies van derden voor het verificatieproces. De bewerkingen van deze bibliotheek worden daarom op geen enkele wijze beïnvloed door de wijzigingen in Safari 13.

### Oplossing {#mitigation-safari13}

In de eerste plaats bevelen wij ten zeerste aan **migratie naar AccessEnabler JavaScript SDK versie 4.x** voor een stabiel en voorspelbaar gedrag in de Safari-browser.

Ten tweede bevat de bibliotheek voor AccessEnabler JavaScript SDK v3 (versies 3.x) een mechanisme waarmee kan worden vastgesteld in welke situaties gebruikersverificatie is geblokkeerd omdat vereiste cookies ontbreken. In deze situaties activeert de bibliotheek een specifieke callback-fout ([N130](/help/authentication/error-reporting.md#advanced-error-codes-reference)) die wordt doorgegeven aan de website die geschikt is voor Adobe Pass-verificatie, zodat deze kan worden gebruikt als signaal om de gebruiker te instrueren maatregelen te nemen om het probleem te verhelpen. Om van dit mechanisme te kunnen profiteren, moet de website het [Fout bij rapporteren](/help/authentication/error-reporting.md) specificatie.

Voor AccessEnabler JavaScript SDK v2 (versies 2.x) biedt de bibliotheek het hierboven beschreven mechanisme niet. Daarom kan de voor Adobe Pass-verificatie ingeschakelde website niet worden gesignaleerd wanneer de gebruiker moet worden opgedragen om actie te ondernemen om het probleem te verhelpen.

Wanneer [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) de fout callback wordt ontvangen door de website van de uitvoerder, zou de gebruiker moeten worden geïnstrueerd om Intelligent het Volgen Preventie (ITP) onbruikbaar te maken en 3de partijkoekjes toe te laten door:

* In het geval van Mac OS X High Sierra en hoger: de optie &quot;**Spatiëring tussen sites voorkomen**&quot; optie voor &quot;**Websites bijhouden**&quot;-item op het tabblad Privacy van de browser via Voorkeuren, zoals in de onderstaande afbeelding wordt weergegeven.

  ![](assets/prvnt-cross-site-tr-safari13.png)

* In het geval van Mac OS X Sierra en previous: controleren t</span>hij &quot;**Altijd toestaan**&quot; te gebruiken voor de &quot;**Cookies en websitegegevens**&quot;-item op het tabblad Privacy van de browser via Voorkeuren, zoals in de onderstaande afbeelding wordt weergegeven.

  ![](assets/always-allow-safari13.png)
