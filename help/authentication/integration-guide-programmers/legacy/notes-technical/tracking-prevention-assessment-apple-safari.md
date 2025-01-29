---
title: Trackingpreventiebeoordeling Apple Safari
description: Trackingpreventiebeoordeling Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '1849'
ht-degree: 0%

---

# (Legacy) Tracking Prevention Assessment - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Safari 10 {#safari10}

**Details**

Beginnend met Safari 10, zullen de standaardbrowser privacymontages de Enige Sign-On (SSO), Enige Logout (SLO) en passieve authentificatiefuncties veroorzaken ophouden werkend. Single Sign-On (SSO) en passieve authentificatie zullen niet werken zelfs binnen
dezelfde sessie tussen meerdere tabbladen of browservensters.

Deze wijzigingen zijn van invloed op en zijn van invloed op Adobe Pass-verificatieprocessen
voor de volgende versies van AccessEnabler JavaScript SDK: v2 (versies 2.x), v3 (versies 3.x), v4 (versies 4.x).

### Oplossing {#mitigation-safari10}

Om deze beperkingen te verlichten kunt u de gebruiker instrueren om de Safari 10 browser privacymontages te veranderen, en de &quot;**altijd toestaan**&quot;optie voor de &quot;**Cookies en de ingang van websitegegevens**&quot;in het lusje van de Privacy van browser van Voorkeur, zoals die in hieronder beeld wordt getoond.

![](../../../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit rubriek Safari 10 zijn nog steeds van toepassing in het geval van Safari 11.

Beginnend met Safari 11, introduceert browser [ Intelligente het Volgen Preventie ](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP) mechanisme, een technologie die heuristiek gebruikt om dwars-plaats het volgen te verhinderen. Deze heuristiek is van invloed op de manier waarop cookies van derden worden opgeslagen en opnieuw afgespeeld op netwerkaanroepen. Dit betekent dat, afhankelijk van de activering van het ITP-mechanisme, de Safari-browser cookies van derden in de communicatie tussen client en server blokkeert.

De dienst van de Authentificatie van Adobe Pass gebruikt en baseert zich op koekjes als deel van het authentificatieproces **om** te functioneren. In situaties waarin het verificatieproces automatisch plaatsvindt (bijvoorbeeld Temperatuurcontrole) of in implementaties waarin iFrames of de functie &quot;Zonder refressless&quot; wordt gebruikt, worden cookies van Adoben beschouwd als cookies van derden en worden deze standaard geblokkeerd. In andere gevallen gebruikt Safari een computerleeralgoritme dat alle de dienstkoekjes van de Authentificatie van de Voldoende Adobe als het volgen van koekjes zou kunnen merken, daarom onderworpen voor het blokkeren van ITP.

Concluderend kan een gebruiker van Safari 11 browser niet op een Adobe Pass Authentificatie toegelaten website na de activering van het Intelligent Tracking Preventie (ITP) mechanisme voor authentiek verklaren, vooral wanneer de gebruiker veelvoudige authentificatie toegelaten websites gebruikt. Daarom kan de verificatieervaring van de gebruiker onverwacht en ongedefinieerd zijn, variërend van onmogelijkheid tot aanmelden tot een kortere dan verwachte verificatieduur.

Deze wijzigingen zijn van invloed op en hebben invloed op de Adobe Pass-verificatieprocessen van de volgende versies van de AccessEnabler JavaScript SDK: v2 (versies 2.x), v3 (versies 3.x).

### Oplossing {#mitigation-safari11}

Voor zowel AccessEnabler JavaScript SDK v3 (versies 3.x) als AccessEnabler JavaScript SDK v4 (versies 4.x) bevat de bibliotheek een mechanisme waarmee kan worden vastgesteld in welke situaties de verificatie van de gebruiker is geblokkeerd omdat de vereiste cookies ontbreken. In deze situaties teweegbrengt de bibliotheek een specifieke foutencallback [ N130 ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) teweeg, die terug naar de toegelaten website van de Authentificatie van Adobe Pass wordt overgegaan om als signaal te worden gebruikt om de gebruiker op te dragen acties te voeren die de kwestie kunnen verlichten. Om van dit mechanisme te profiteren moet de website de [ Fout uitvoeren die ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specificatie meldt.

Voor AccessEnabler JavaScript SDK v2 (versies 2.x) biedt de bibliotheek het hierboven beschreven mechanisme niet. Daarom kan de voor Adobe Pass-verificatie ingeschakelde website niet worden gesignaleerd wanneer de gebruiker moet worden geïnstrueerd om actie te ondernemen om het probleem op te lossen.

De lijst van acties die de bovengenoemde kwesties **kunnen verlichten is op alle drie versies** van AccessEnabler JavaScript SDK van toepassing.

Wanneer [ N130 ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) foutencallback door de website van de uitvoerder wordt ontvangen, zou de gebruiker moeten worden opgedragen om Intelligente het Volgen Preventie (ITP) onbruikbaar te maken en 3de partijkoekjes toe te laten door:

* In geval van de Hoge Sierra van Mac OS X en later: Het ongedaan maken van het controleren van &quot;**verhindert dwars-plaats het volgen**&quot;optie voor &quot;**het volgen van de Website**&quot;ingang in het lusje van de Privacy van browser van Voorkeur, zoals afgebeeld in hieronder beeld.

  ![](../../../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* In geval van de Sierra van Mac OS X en vorig: Het controleren van &quot;**staat altijd**&quot;optie voor de &quot;**Cookies en de ingang van websitegegevens**&quot;in het lusje van de Privacy van browser van Voorkeur toe, zoals die in hieronder beeld wordt getoond.

  ![](../../../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit sectie Safari 10 en sectie Safari 11 zijn nog steeds van toepassing in het geval van Safari 12.

Deze sectie detailleert de verenigbaarheidskwesties van **AccessEnabler JavaScript SDK versies 4.x** op Safari 12.

>[!NOTE]
>
>Houd er rekening mee dat in het geval van AccessEnabler JavaScript SDK versie 2.x en AccessEnabler JavaScript SDK versie 3.x, beide cookies van derden gebruiken voor de verificatieprocessen en dat vanwege het ITP- en cookiebeleid van andere bedrijven dat begint met Safari 11, de verificatieervaring van de gebruiker onverwacht en ongedefinieerd kan zijn, variërend van inmogelijkheid tot aanmelding tot een kortere verificatieduur.


### Gecertificeerde functionaliteit van AccessEnabler JavaScript SDK v4 (versies 4.x) op Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**stromen van de Authentificatie** die gebruik van gebruikersinteractie maken zullen altijd werken, zelfs als browser van de gebruiker 3rd partijkoekjes gehandicapt heeft, omdat het beginnen met versie 4.0 JavaScript SDK AccessEnabler geen derdekoekjes meer voor de authentificatieprocessen gebruikt.

>[!NOTE]
>
>De gebruiker MOET communiceren met de site om aanmeldingspopups te openen en/of te communiceren met de MVPD-aanmeldingspagina.

**de verrichtingen van Meta-gegevens van de Vergunning/Preflight/van de Gebruiker zijn volledig functioneel, op voorwaarde dat de gebruiker reeds voor authentiek verklaard is.**

### Bekende problemen met AccessEnabler JavaScript SDK v4 (versies 4.x) op Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO en SLO

   * Vanwege de implementatie van localStorage in Safari vanaf Safari 10 kan de JS SDK de aanmeldingsstatus niet meer delen via een gemeenschappelijke domein-iFrame. Dit betekent dat de gebruiker zich op elke plaats moet aanmelden die AccessEnabler JavaScript SDK gebruikt. Als u zich afmeldt, worden er ook geen verificatietokens verwijderd van verschillende sites. De gebruiker moet zich daarom afmelden bij elke website waarop Adobe Pass-verificatie is ingeschakeld.

* Temperatuurcontrole

   * Voor tijdelijke overgangen, gebruikt AccessEnabler JavaScript SDK een individualisatiemechanisme, om een authentificatietoken aan een specifiek apparaat (browser instantie) te sluiten. Wegens nieuwe mechanismen in Safari 12 die worden ontworpen om het volgen te verhinderen, zal de vingerafdruk die wij in het individualisatiemechanisme **berekenen en gebruiken het zelfde voor alle gebruikers zijn die het zelfde IP adres** hebben. Wij nemen cliëntIP voor individualisatiedoeleinden in overweging, maar zelfs zo is het effect op gebruikers die het zelfde openbare IP adres delen. Voor deze gebruikers berekenen we dezelfde individualisatie-id en de tijdelijke pas wordt eraan gekoppeld. Dit betekent dat als een dergelijke gebruiker een tijdelijke pas gebruikt, niemand anders er toegang toe heeft \! Dit heeft vooral gevolgen voor zakelijke gebruikers, onderwijsinstellingen of andere organisaties die meerdere gebruikers hebben die NAT of een gemeenschappelijke proxy gebruiken om toegang te krijgen tot internet.

>[!NOTE]
>
>Deze kwestie beïnvloedt gebruikers slechts als de uitvoerder de Pas van Temperatuur als resultaat van gebruikersinteractie gebruikt, anders is de authentificatie van de Pas van de Temperatuur onderworpen aan **Automatische stromen** hieronder.

* Automatische stromen

   * Verificatiestromen die in een geautomatiseerde modus worden geprobeerd, zonder gebruikersinteractie zullen niet slagen in Safari 12 wanneer u JS SDK 4.0 gebruikt. De komende JS SDK 4.1 verhelpt alle problemen met geautomatiseerde stromen.

Gebruik gevallen die door dit probleem worden beïnvloed:

* Automatische TempPass-verificatie (Free Preview) - voor dergelijke stromen geeft de SDK een N130-fout.

* Passieve verificatie (mislukt in stilte) - de gebruiker wordt gevraagd deze MVPD te selecteren en referenties in te voeren

### Oplossing {#mitigation-safari12}

**SSO en SLO**

Er is op het moment van schrijven geen bekende beperking beschikbaar of mogelijk. Apple heeft in Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`) wel een API voor opslagtoegang geïntroduceerd, maar de huidige implementatie is niet van toepassing op localStorage, maar alleen op cookies. Bovendien vereist de API gebruikersinteractie om te worden gebruikt, en zodra u het gebruikt, wordt de gebruiker ook ertoe aangezet met een toestemmingsdialoog gelijkend op hieronder.

![](../../../assets/permission-dialog-apple.png)


Op dit punt richten deze vereisten/herinneringen van Safari zich niet op onze vereisten van UX en wij hebben geen verenigbaar gedrag zoals op andere browsers, waar SSO &quot;enkel&quot;werkt zodra wij een teken in een gemeenschappelijk domein localStorage bewaarde.

**Controle van het Temperatuur**

Om de individualisatieproblemen te verlichten en een gebruikersinteractie te hebben adviseren wij u om **[de Voldoende Pas van de Bevordering](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)** op een interactieve manier te gebruiken, en minstens één extra informatie over de gebruiker (bijvoorbeeld, e-mailadres) te verstrekken.

## Safari 13 {#safari13}

**Details**

>[!IMPORTANT]
>
>Alle bovenstaande gegevens uit de secties Safari 10 tot en met Safari 12 zijn nog steeds van toepassing in het geval van Safari 13.


Beginnend met Safari 13, introduceert browser nieuwe veranderingen in [ Intelligente het Volgen Preventie ](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), makend de heuristiek achter het mechanisme strikter in het proces om derdekoekjes als het volgen van koekjes te vlaggen, om dwars-plaats het volgen te verhinderen.

Zoals in vorige secties wordt beschreven gebruikt de dienst van de Authentificatie van Adobe Pass en baseert zich op derdekoekjes als deel van de authentificatieprocessen wanneer de implementors AccessEnabler JavaScript SDK v2 (versies 2.x) en AccessEnabler JavaScript SDK v3 (versies 3.x) gebruiken. Vergeleken met eerdere versies van Safari browser toen ITP intrad nadat het enige tijd was doorgebracht om te &quot;leren&quot;over de interactie tussen de gebruiker en de betrokken partijen (de websites en de Adobe van de programmeur), blokkeert Safari 13 browser van de start derde partijkoekjes die als het volgen van koekjes in de cliënt - server modelmededeling worden beschouwd.

Concluderend kan een gebruiker van Safari 13 browser hoogstwaarschijnlijk geen nieuwe authentificaties op een Adobe Pass Authentificatie toegelaten website in werking stellen die een oudere versie van AccessEnabler JavaScript SDK, v2 (versies 2.x) of v3 (versies 3.x) gebruikt. Dit gebeurt wegens het feit dat alle vereiste de dienstkoekjes van de Authentificatie van de Adobe Primetime door ITP worden geblokkeerd, daarom makend de dienst niet aan het authentificatieverzoek kan voldoen.

AccessEnabler JavaScript SDK v4 (versies 4.x)-bibliotheek gebruikt geen cookies van andere leveranciers voor het verificatieproces, zodat de wijzigingen in Safari 13 op geen enkele wijze van invloed zijn op de bewerkingen.

### Oplossing {#mitigation-safari13}

Eerst en vooral adviseren wij sterk **migratie aan AccessEnabler JavaScript SDK versies 4.x** om een stabiel en voorspelbaar gedrag op browser Safari te hebben.

Ten tweede bevat de bibliotheek voor AccessEnabler JavaScript SDK v3 (versies 3.x) een mechanisme waarmee kan worden vastgesteld in welke situaties gebruikersverificatie is geblokkeerd omdat vereiste cookies ontbreken. In deze situaties teweegbrengt de bibliotheek een specifieke foutencallback ([ N130 ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)) teweeg die terug naar de toegelaten website van de Authentificatie van Adobe Pass wordt overgegaan om als signaal te worden gebruikt om de gebruiker op te dragen acties te voeren die de kwestie kunnen verlichten. Om van dit mechanisme te profiteren moet de website de [ Fout uitvoeren die ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specificatie meldt.

Voor AccessEnabler JavaScript SDK v2 (versies 2.x) biedt de bibliotheek het hierboven beschreven mechanisme niet. Daarom kan de voor Adobe Pass-verificatie ingeschakelde website niet worden gesignaleerd wanneer de gebruiker moet worden geïnstrueerd om actie te ondernemen om het probleem op te lossen.

Wanneer [ N130 ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) foutencallback door de website van de uitvoerder wordt ontvangen, zou de gebruiker moeten worden opgedragen om Intelligente het Volgen Preventie (ITP) onbruikbaar te maken en 3de partijkoekjes toe te laten door:

* In geval van de Hoge Sierra van Mac OS X en later: Het ongedaan maken van het controleren van &quot;**verhindert dwars-plaats het volgen**&quot;optie voor &quot;**het volgen van de Website**&quot;ingang in het lusje van de Privacy van browser van Voorkeur, zoals afgebeeld in hieronder beeld.

  ![](../../../assets/prvnt-cross-site-tr-safari13.png)

* In geval van Mac OS X Sierra en vroeger: Het controleren van </span> staat altijd **** optie voor de &quot;**Cookies en website- gegevens**&quot;ingang in het lusje van de Privacy van browser van Voorkeur, zoals die in hieronder beeld wordt getoond toe.

  ![](../../../assets/always-allow-safari13.png)
