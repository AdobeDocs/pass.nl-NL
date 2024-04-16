---
title: Woordenlijst Account IQ
description: Een verklarende woordenlijst van productterminologieën.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Productconcepten en verklarende woordenlijst {#glossary}

## Gemeenschappelijke terminologie over D2C en TV overal

De volgende productterminologie en hun definities zijn gemeenschappelijk voor alle [versies van account-IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Een dashboard-deelvenster met grafieken die de delende scores van het huidige segment verdeelt in het delen van bereikcategorieën van Zeer laag, Laag, Modern, Hoog en Zeer hoog.

### [!UICONTROL Action] {#action-def}

Een directe of indirecte gebeurtenis in verband met een [Bewerking](#operation-def) die van invloed is op de kenmerken (bijvoorbeeld score delen of het aantal gebruikte apparaten) van een verwant bewerkingssegment.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Een dashboardpaneel met grafieken die de het delen van de scores van het huidige segment in het delen waaiercategorieën van Zeer Laag, Laag, Gemiddeld, Hoog, en Zeer hoog verdelen, samen met elk categoriepercentage van de totale hoeveelheid het stromen voor het segment.

### [!UICONTROL AuthN] {#authn-def}

Het aantal verificatiepogingen. Een verificatiepoging is het proces waarbij een gebruiker zich aanmeldt bij de D2C-service of MVPD. Voor TV-gebruikers overal wordt de gebruiker omgeleid naar de door hen gekozen MVPD, waar ze zich identificeren bij de MVPD - meestal met een gebruikersnaam en wachtwoord.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Het aantal geslaagde verificaties. Een succesvolle authentificatie komt voor wanneer de identificatie van een gebruiker door de dienst D2C of MVPD wordt bevestigd. Voor gebruikers op tv overal leidt dit ertoe dat de gebruiker wordt omgeleid naar de programmeur-app of -site.

### [!UICONTROL Cluster] {#cluster-def}

Een cluster is een verzameling locaties en apparaten. De clusters worden gemaakt door gemeenschappelijke locaties tussen apparaten te zoeken. De apparaten die in een gemeenschappelijke plaats zijn gezien zullen worden beschouwd als om in de zelfde cluster te behoren. Twee apparaten kunnen in de zelfde cluster zijn alhoewel zij geen gemeenschappelijke plaatsen hebben, maar kunnen door de plaatsen van andere apparaten worden aangesloten.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Een cluster zonder statische apparaten.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Een cluster met ten minste één statisch apparaat.

### [!UICONTROL Concurrency] {#consurrency-def}

De gelijktijdige uitvoering wordt gedefinieerd door twee (of meer) streams die tegelijkertijd of zeer dicht in de tijd worden afgespeeld, zodat het interval tussen de streams niet kan worden gerechtvaardigd door een normale snelheid te gebruiken.
Gelijktijdig gebruik wordt berekend met de maximale snelheid (mijl/uur) tussen twee verschillende clusters. Een gebruiker wordt geacht gelijktijdig gebruik te hebben als hij een snelheid van meer dan 124 m/h heeft op een afstand van minder dan 124 mijl of als hij een snelheid van meer dan 400 m/h heeft op een afstand van meer dan 124 mijl. De afstand wordt berekend tussen plaatsen van verschillende clusters. Gelijktijdig gebruik is toegestaan in dezelfde cluster.

### [!UICONTROL Device] {#device-def}

Een digitale videohardwareproduct waarmee upstreaming inhoud kan worden afgespeeld. Bijvoorbeeld smartphones, laptops en desktopcomputers, spelconsoles en smartphones.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

Evaluatieperiode is de tijd vanaf het begin van de actie die aan de bewerking is gekoppeld tot het einde van de actie of de meting ervan.

### [!UICONTROL Geographical Span] {#geographical-span-def}

De afstand tussen de verste punten in een set locaties.

### [!UICONTROL Granularity] {#granularity-def}

Met betrekking tot tijdsinterval, de grootte van de periode; zoals één week of één maand.

### [!UICONTROL IP] {#ip-def}

Het adres van het Internet-protocol dat aan een apparaat door een Dienstverlener van Internet wordt toegewezen. Bijvoorbeeld, de leverancier van de kabeldienst, en de leverancier van de celdienst.

### [!UICONTROL Location] {#location-def}

Een uniek punt op aarde. Het wordt ook de geolocatie genoemd voor een specifieke spelaanvraag met een precisie van 1000m x 1000m (één vierkante km).

### [!UICONTROL Media Company] {#media-company-def}

Het Bedrijf van media is een bedrijf dat een groep media netwerken bezit.

### [!UICONTROL Metric] {#metric}

Metrisch is een attribuut van abonneerekening (bijvoorbeeld, hun MVPD, de programmeurs en de kanalen van de inhoud zij stromen, en het aantal apparaten zij gebruiken).

### [!UICONTROL Mobile device] {#mobile-device-def}

Een apparaat met hoge mobiliteit. Bijvoorbeeld mobiele telefoon en tablet.

### Bewerking {#operation-def}

Bewerking is een record die is gemaakt om het effect van een bepaalde bewerking bij te houden [action](#action-def) op een gekoppeld segment. Een voorbeeld van een actie kan een grens zijn die op het aantal gezamenlijke stromen wordt geplaatst toegestaan voor rekeningen die door het segment worden geïdentificeerd.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Een waarde die gebruikers helpt de omvang van het delen van wachtwoorden te begrijpen en hen een gevoel van urgentie verstrekt om op het te handelen.

### [!UICONTROL Play Request] {#play-requests-def}

Gelijk aan een streamstart. Deze gebeurtenis markeert het begin van een gebruiker die inhoud stroomt.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Deze waarde, ook wel bekend als Gebruik van gedeelde accounts, wordt berekend op basis van het aantal afspeelaanvragen dat door elke account wordt ingediend, gewogen op basis van de waarschijnlijkheid dat elke account wordt gedeeld. Dit wordt ook wel &#39;Gebruik door gedeelde accounts&#39;-risicoindex genoemd.

### [!UICONTROL Segment] {#segmet-def}

Segment is een set accounts die voldoen aan de door de gebruiker gedefinieerde voorwaarden die worden bepaald door de geselecteerde metriek. Bijvoorbeeld &quot;gebruikers in gebieden A, B, C, D, of E die meer dan drie apparaten hebben&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Ook bekend als Risk Index - Accounts of Shared Accounts Risk Index, is het een waarde die wordt berekend op basis van het gemiddelde van de waarschijnlijkheid van delen berekend voor elke account in het huidige segment die minstens één keer is gestreamd tijdens het geselecteerde tijdsinterval.

### [!UICONTROL Static device] {#static-device-def}

Een apparaat met een zeer lage mobiliteit. Voorbeeld: spelconsole, set top box en televisieset.

### [!UICONTROL Time interval] {#time-interval-def}

Ook gekend als periode, is het het venster van tijd dat de activiteit van het spelverzoek bevat die in de gebruikersinterface en lijsten van begin tot eind wordt vertegenwoordigd.

### [!UICONTROL Trend] {#trend-def}

Het percentageverschil in bijbehorende metrisch (bijvoorbeeld, percentage van totale spelverzoeken) tussen de huidige en vorige periode.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Het aantal unieke accounts dat gedurende een bepaalde periode ten minste één keer is gestreamd.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Meer dan 37 spelverzoeken per maand.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Minder dan 9 verzoeken om afspelen per maand.

#### [!UICONTROL Regular user] {#regular-user-def}

Van 9 tot 37 verzoeken per maand afspelen.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Een eindige set categorielabels die op een account wordt toegepast en die de gebruikers van de account het best karakteriseert in termen van sociale groepen of gedrag (bijvoorbeeld een klein gezin, een reiziger of een forenmaker, sociaal delen, enz.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Een videocategorie is een specifiek label dat de aard van de video aangeeft. Bijvoorbeeld het gebied van de bron, inhoudstypen, zoals VOD of live, gebeurtenis of specifieke labels, zoals team.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Een videocategorie wordt gedefinieerd door de combinatie van Programmer, Channel en MVPD die aan de stream is gekoppeld.

### [!UICONTROL Zip Code] {#zip-code-def}

De Amerikaanse postcode die is gekoppeld aan locaties in de VS
<!--calculated metrics-->


## Tv overal specifieke terminologie

### [!UICONTROL AuthZ] {#authz-def}

Vergunning of het nummer van de vergunningsaanvraag. Een autorisatieverzoek is het proces waarbij een programmeur toestemming van een MVPD door Adobe vraagt om beginnen het stromen van de gevraagde inhoud van een gebruiker. Het MVPD verleent typisch het verzoek dat op de inhoudsrechten wordt gebaseerd verbonden aan het abonnement van de gebruiker MVPD (bijvoorbeeld, of het kanaal verbonden aan de inhoud in het abonnement van de gebruiker is). Sommige antwoorden van het vergunningsverzoek worden in het voorgeheugen ondergebracht door Adobe, die Adobe toestaat om onmiddellijk te antwoorden zonder het verzoek tot MVPD over te gaan.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Het aantal geslaagde toelatingen.

### [!UICONTROL Channel] {#channel-def}

Kanaal, ook wel Eigenschap genoemd, is een thematisch verwante bron van video-inhoud. Traditioneel die een verschillende, numeriek adresseerbare ononderbroken videovoer van een MVPD vertegenwoordigen. Het kanaal wijst rechtstreeks aan een toegankelijk kanaal van inhoud beschikbaar aan de abonnees door hun Reeks Top Box (STB) toe.

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Een waarde die voor elk van de Indexen van het Risico (Rekeningen, Gebruik, Algemeen) over alle Programmers en MVPDs tijdens het geselecteerde tijdinterval wordt berekend.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Een type van het delen analyse waar de evaluatie van een rekening tot gebeurtenissen beperkt is die direct op de programmeurs in het geselecteerde segment voorkwamen.  Normaliter worden alle accountgebeurtenissen geëvalueerd, wat een veel nauwkeuriger schatting van het delen biedt.  Sommige MVPD-gegevens zijn zodanig gestructureerd dat alleen de analyse van de isolatiemodus mogelijk is.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, ook gekend als Verdeler, is een aggregator, reseller, en verdeler van de video-inhoud van het Bedrijf van Media.

### [!UICONTROL Programmer] {#programmer-def}

Programmeur, ook bekend als Netwerk, is een bedrijf dat dochteronderneming is van een groter bedrijf (bedrijf) dat één of meerdere kanalen bezit en beheert.

### [!UICONTROL requestorID] {#requestorid-def}

Identiteitskaart een Bedrijf van Media gebruikt om zich of een dochteronderneming aan een MVPD te identificeren.  Afhankelijk van de implementatie van de Programmer, kon dit aan een Bedrijf van Media, een Programmer of een Kanaal in kaart brengen. Dit wordt traditioneel toegewezen aan een kanaal.  Met de verwezenlijking van pseudo-Kanalen zoals MML (Maart Madness Live) en technisch gedreven bewegingen om MVPD-gedreven gegevensbeperkingen te richten, begint requestID meer met het Bedrijf van Media te worden geassocieerd.

### [!UICONTROL resourceID] {#resource-id-def}

De inhoud die door de eindgebruiker wordt aangevraagd. Traditioneel is hiermee de Kanaal-koppeling geïdentificeerd die is gekoppeld aan de inhoud die de gebruiker heeft aangevraagd.  Dankzij systeemverbeteringen kan die id specifieke programma&#39;s vertegenwoordigen (bijvoorbeeld met specifieke classificaties), maar de id blijft het bijbehorende kanaal identificeren.


