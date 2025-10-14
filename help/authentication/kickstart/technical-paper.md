---
title: Informatie over Adobe Pass-verificatie
description: Informatie over Adobe Pass-verificatie
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Over Adobe® Pass Authentication {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Over tv overal {#about-tv-everywhere}

De televisiekijkers van vandaag verwachten naadloze toegang tot de inhoud van de Tv van de Betaal, overal en altijd. Met de opkomst van internetapparaten verbruiken gebruikers inhoud op een groter aantal platforms, waaronder:

* Laptops
* Tablets
* Smartphones
* Websites
* Federale apps
* Gameconsoles
* Bovenste vakken instellen
* Slimme tv&#39;s

Tv Overal is een brancheinitiatief dat ervoor zorgt dat betaaltelevisieabonnees toegang hebben tot de inhoud die zij al voor meerdere apparaten betalen, zowel binnen als buiten hun woning.

Hoewel traditionele (lineaire) tv sterk blijft, zijn de snelst groeiende segmenten van videogebruik tijdverschuivende inhoud, online streaming en alternatieve schermen. Deze verschuiving heeft de markt van de videodistributie verstoord, die TV overal maken een cruciale oplossing die de belangen van **Programma&#39;s, de leveranciers van betaaltelevisie, en abonnees richt.**

### Doelstellingen van tv overal {#goals-tv-everywhere}

**Technisch Doel**

* Klanten van betaaltelevisie de mogelijkheid bieden hun geabonneerde inhoud naadloos op alle apparaten en platforms te openen.

**BedrijfsDoelstellingen**

* Behoud en verbeter bestaande klantenverhoudingen terwijl het toelaten van nieuwe kansen.
* Programmeurs en eigenaars van inhoud in staat stellen een groter publiek te bereiken en de waarde van premiuminhoud te maximaliseren.
* Breid merken uit via directe online betrokkenheid met viewers.

### Uitdagingen van tv overal {#challenges-tv-everywhere}

Met de mogelijkheden van tv overal komen er grote uitdagingen, waarbij recht de meest kritieke is. Voordat een viewer toegang kan krijgen tot abonnementsinhoud, moet een systeem zijn of haar rechten verifiëren.

De belangrijkste vragen zijn:

* Heeft de gebruiker een actief abonnement bij een Pay TV-provider?
* Bevat hun abonnement de gevraagde inhoud?

Het bepalen van machtigingen is vooral een uitdaging voor programmeurs en eigenaars van inhoud, omdat betaaltelevisie-operators de gegevens van klanten en toegangsrechten beheren.

Naast het recht op toegang doen zich verschillende technische en integratieproblemen voor, waaronder:

* Ontwikkeling van een strategie voor meerdere apparaten die naadloze toegang garandeert.
* Het beheren van complexe relaties tussen programmeurs en betaaltelevisieproviders.
* Het voorkomen van frauduleuze toegang en het afdwingen van de voorwaarden van de dienst.
* Een consistente, gebruikersvriendelijke verificatieervaring bieden op verschillende websites en toepassingen.
* Het handhaven van snelle tijd-aan-markt om zich aan partnerovereenkomsten aan te passen.
* De kosten voor meerdere integraties beheersen.

Deze uitdagingen maken directe integratie tussen programmeurs en veelvoudige de authentificatiesystemen van de Tv van het Betaal zeer middel-intensief, die zowel tijd als technische deskundigheid vereisen.

Om deze obstakels te overwinnen, **vereenvoudigt de Authentificatie van de Voldoende Authentificatie van Adobe®** en stroomlijnt machtigingscontrole, toelatend naadloze, veilige toegang tot TV overal inhoud.

## Inleiding tot Adobe Pass-verificatie {#introduction-adobe-pass-authentication}

Met Adobe Pass Authentication worden machtigingstransacties tussen Programmers en Pay TV providers veilig door elkaar geleid, zodat de juiste klanten moeiteloos toegang hebben tot de juiste inhoud.

![](../assets/programmers-connect-authn.png)

*Sommige van de Programma&#39;s en betalen TV leveranciers die door de Authentificatie van Adobe Pass* verbinden

**Wie van de Authentificatie van Adobe Pass** profiteert

* **Programmeurs**

  Wie eenvoudig met de leveranciers van de Tv van het Betaal, ook genoemd MVPDs (de Verdelers van de Video van de Programmering van Multikanaal) wilt integreren, om het breedste publiek te bereiken en opbrengst te optimaliseren.

* **betalen TV-providers (MVPDs)**

  Wie via één integratie verbinding wil maken met meerdere eigenaars van inhoud (Programmers), verbetert de klanttevredenheid door online toegang tot abonnementsinhoud te vergemakkelijken.

* **betalen de klanten van TV**

  Wie wil de inhoud bekijken die ze al betalen, op elk moment, overal, op elk apparaat.

**Programmeurs**

Programmeurs die het bereik en de inkomsten van het publiek willen maximaliseren, kunnen:

* Kan eenvoudig worden geïntegreerd met betaaltelevisie zonder meerdere rechtstreekse verbindingen te beheren.
* Breid hun publiek uit door authentificatie over alle belangrijke leveranciers en platforms te verzekeren.
* Bescherm premiuminhoud met beveiligde verificatie, waarbij de toegang tot geautoriseerde gebruikers en apparaten wordt beperkt.
* Schakel Single Sign-On (SSO) in om de gebruikerservaring in apps en websites te verbeteren.

**betalen TV-providers (MVPDs)**

Betaal TV leveranciers kunnen klantenervaring verbeteren en verrichtingen stroomlijnen door:

* Via één integratie verbinding maken met meerdere eigenaars van inhoud.
* Een merkloze, naadloze kijkervaring op verschillende apparaten en platforms.
* Zorgen voor beveiligde verificatie om ongeoorloofde toegang te voorkomen en gelijktijdige streams per huishouden te beheren.

**betalen de klanten van TV**

Abonnees profiteren van tv overal, zodat ze de inhoud kunnen bekijken die ze al betalen, altijd, overal en op elk apparaat.

### Integreren met Adobe Pass-verificatie {#integrating-adobe-pass-authentication}

Of u nu een Pay TV-provider of een programmeur bent, voor integratie met Adobe Pass-verificatie is actieve deelname vereist. Hieronder vindt u een uitgebreid overzicht van het proces voor beide rollen.

#### Integratieproces van programmeerprogramma&#39;s {#programmer-integration-process}

Er zijn aanvullende instructies beschikbaar zodra de integratie formeel is gestart, maar het proces omvat doorgaans de volgende stappen:

**Eerste vereisten**

* Een contentbeheersysteem (CMS).
* Een mechanisme van de inhoudslevering, dat een netwerk van de derdeinhoudslevering (CDN) kan omvatten.
* Een bestaand online videoplatform met een mediaspeler die is ingesloten in een website of een zelfstandige toepassing.

**de Taken van de Integratie**

* Integreer de Authentificatie van Adobe Pass [&#x200B; REST API DCR &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Integreer de Authentificatie van Adobe Pass [&#x200B; REST API V2 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integreer de Symbolische Verifier van de Authentificatie van Adobe Pass [.](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)
* Een gebruikersinterface ontwikkelen voor de workflow voor verificatie, autorisatie en afmelding.

Voor meer details op het integratieproces van de Programmer, verwijs naar de [&#x200B; gids van de Kickstart van de Programmer &#x200B;](/help/authentication/kickstart/programmer-kickstart-guide.md) en [&#x200B; de integratiegids van de Programmer &#x200B;](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md) documenten.

#### Integratieproces van tv-aanbieders betalen {#pay-tv-provider-integration-process}

Er zijn aanvullende instructies beschikbaar zodra de integratie formeel is gestart, maar het proces omvat doorgaans de volgende stappen:

**Eerste vereisten**

* Onderteken de Adobe Pass Authentication Non-Disclosure Agreement (NDA).
* Geef het technische team van Adobe Pass Authentication specificaties voor het verificatie- en autorisatiesysteem. Een op SAML-Gebaseerde identiteitsleverancier (IdP) voor authentificatie en een op SOAP-Gebaseerd vergunningssysteem worden geadviseerd voor de eenvoudigste integratie.

**de Taken van de Integratie**

* Maak verbinding met Adobe Pass-verificatieservers.
* Complete Staging Release en zorg voor kwaliteitsborging.
* Volledige productie en kwaliteitsborging.

De standaardintegratie is gratis voor betaaltelevisieproviders, inclusief documentatie en standaard e-mailondersteuning. Nochtans, kunnen de leveranciers die uitgebreide hulp of versnelde chronologie vereisen een steunprijs of verkiezen om met een ervaren derdepartner, zoals Synacor te werken.

Adobe Pass-verificatie biedt op twee manieren efficiënt ondersteuning voor bedrijfslogica die specifiek is voor de Pay TV-provider:

* Voor bedrijfslogica die op zichzelf staand is en door de leverancier van de Tv van het Betaal wordt toegepast wanneer een vergunningsverzoek wordt ontvangen, verstrekt Adobe de noodzakelijke gegevens (b.v., unieke apparatenidentiteitskaart, IP adres) om handhaving te steunen.
* Voor bedrijfslogica die gebruikersinterventie of specifieke behandeling door Adobe vereist, kunnen aangepaste eigenschappen voor elke leverancier van betaaltelevisie worden gehandhaafd. Deze configuraties kunnen vooraf gedefinieerde workflows bevatten die worden geactiveerd op specifieke punten in het verificatieproces.

Voor meer details op het de leveranciersintegratieproces van de Tv van de Betaal, verwijs naar de [&#x200B; handleiding van de kickstart van MVPD &#x200B;](/help/authentication/kickstart/mvpd-kickstart-guide.md) en [&#x200B; de integratiegids van MVPD &#x200B;](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md) documenten.

### Entitlement Flow {#entitlement-flow}

De Authentificatie van Adobe Pass werkt als volmacht en vergemakkelijkt de machtigingsstroom tussen Programmers en MVPDs door veilige en verenigbare interfaces voor beide partijen aan te bieden.

Voor Programmeurs, verstrekt de Authentificatie van Adobe Pass APIs als deel van a **Standaard** of a **Premium** rij:

* Standaard Adobe Pass-verificatie-API&#39;s:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium-API&#39;s voor Adobe Pass-verificatie:
   * [Tijdcontrole-API opnieuw instellen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Functie TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [Verslechterings-API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Afbraakkenmerk](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [Entitlement Service Monitoring API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

Voor meer details op de machtigingsstroom, verwijs naar de [&#128279;](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow) documentatie van de Gids van de Integratie van de 0&rbrace; Programmer.

#### Entitlement {#understanding-entitlements}

De oplossing van de Authentificatie van Adobe Pass draait rond de verwezenlijking van recht-specifieke stukken gegevens die op de succesvolle voltooiing van authentificatie en vergunningswerkschema&#39;s worden geproduceerd. Deze rechten verlenen toegang tot beschermde inhoud, maar hebben een beperkte duur. Nadat een machtiging is verlopen, moet deze worden vernieuwd door de verificatie- of autorisatieprocessen opnieuw te starten.

Raadpleeg de volgende documenten voor meer informatie over rechten:

* **Profielen**

  Na succesvolle verificatie maakt Adobe Pass Authentication een geverifieerd profiel (&quot;long-live&quot;) dat is gekoppeld aan de aanvraag-id voor de toepassing, het apparaat en de serviceprovider (aanvrager-id).

* **[Metagegevens van de Gebruiker](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Bij geslaagde verificatie (en in sommige gevallen ook na autorisatie) ontvangt Adobe Pass Authentication gebruikersmetagegevens van de MVPD die deze toegankelijk kunnen maken voor de toepassing die het verzoek indient.

* **[Besluiten](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Na een geslaagde autorisatie maakt Adobe Pass Authentication een machtigingsbesluit (&quot;langlevende&quot;) dat verband houdt met de aanvraag, het apparaat, de dienstverlener-id (aanvrager-id) en een specifieke beschermde bron (bron-id).

* **[Tokens van Media](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Na een geslaagde autorisatie maakt Adobe Pass Authentication een mediatoken (&quot;kortstondig&quot;) die aan een succesvol afspeelverzoek is gekoppeld en die ondersteuning biedt voor best practices van de branche voor het beperken van fraude (bijvoorbeeld &#39;streaming ripping&#39;).

De time-to-live (&quot;TTL&quot;) waarden voor profielen en besluiten worden vastgesteld op basis van overeenkomsten tussen programmeurs en betaaltelevisieproviders, die het eens zijn over een waarde die het best alle betrokkenen ten goede komt.

#### De gebruikersinterface samenstellen {#building-user-interface}

Programmeurs zijn verantwoordelijk voor het ontwerpen en implementeren van de gebruikersinterface (UI) voor de machtigingsworkflow op hun website of app, terwijl bepaalde elementen, zoals het aanmeldingsproces, worden afgehandeld door de leverancier van betaaltelevisie.

De programmeurs moeten minstens:

* **voert een interface van de leveranciersselectie uit**
   * Laat nieuwe gebruikers hun leverancier van betaaltelevisie identificeren en meld ze voor het eerst aan.
   * Sommige Pay TV-providers leiden gebruikers om naar een externe aanmeldingspagina, terwijl andere gebruikers zich moeten aanmelden binnen een iframe. Programmeurs moeten een callback functie uitvoeren om iframe te produceren wanneer nodig.

* **beheer een lijst van gesteunde leveranciers van de Tv van het Betaal**
   * Zorg ervoor dat gebruikers alleen via erkende providers toegang hebben tot inhoud.

* **wijs authentificatiestatus** aan
   * Weergeven wanneer een gebruiker binnen de app of website is geverifieerd.

* **identificeer beschermde middelen**
   * Geef duidelijk aan welke inhoud autorisatie vereist voordat deze wordt weergegeven.
   * Werk UI bij om succesvolle vergunning te weerspiegelen zodra de toegang wordt verleend.

## Veelgestelde vragen {#faqs}

**wat is TV overal?**

Tv Overal is een brancheinitiatief dat klanten van betaaltelevisie toegang biedt tot de premiuminhoud waarop zij zich al abonneren op verschillende apparaten met internetverbinding. Dit zijn personal computers, tablets, smartphones, spelconsoles, set-top boxes en Smart TV&#39;s. De belangrijkste uitdaging is het verzekeren van een naadloos en gebruikersvriendelijk authentificatieproces, toelatend klanten om tot hun abonnementsinhoud zonder veelvoudige logins of technische barrières toegang te hebben.

**wat de Authentificatie van Adobe Pass is, en hoe steunt het TV overal?**

Met Adobe Pass-verificatie komt tv overal tot leven door op eenvoudige en efficiënte wijze te controleren of een gebruiker recht heeft op inhoud. Het is een gehoste service die snelle back-end integratie mogelijk maakt op basis van bedrijfsregels die door zowel programmeurs als betaaltelevisieproviders zijn ingesteld. Dit resulteert in snellere tijd aan markt voor alle belanghebbenden, een veiliger milieu dat fraude minimaliseert, en een betere gebruikerservaring, met meer TV inhoud die over veelvoudige platforms toegankelijk is.

**hoe wordt de Authentificatie van Adobe Pass geleverd?**

De Authentificatie van Adobe Pass wordt verstrekt als Software als oplossing van de Dienst (SaaS). Deze aanpak zorgt voor veilige communicatie tussen eindgebruikers, programmeurs en betaaltelevisieproviders voor validatie van machtigingen voor inhoud.

**wat maakt de Authentificatie van Adobe Pass verschillend van andere TV overal oplossingen?**

Adobe Pass-verificatie biedt verschillende voordelen ten opzichte van alternatieve oplossingen:

* **naadloze Enige Sign-On (SSO)** - in tegenstelling tot directe integratie met individuele leveranciers, laat de Authentificatie van Adobe Pass een blijvende login ervaring toe aangezien de gebruikers zich tussen verschillende websites en apps bewegen.
* **Brede Prijsverhoging van de Markt** - Zodra een Programmer met de Authentificatie van Adobe Pass integreert, krijgen zij onmiddellijk toegang tot de exploitanten van de Tv van de Betaling die meer dan 90% van de huishoudens van de V.S. bestrijken.
* **Integratie met Ecosystem van Adobe** - werkt foutloos met andere oplossingen van Adobe voor inhoudslevering, bescherming, en monetisatie, met inbegrip van Adobe Analytics.

**Hoe veilig is de Authentificatie van Adobe Pass?**

Veiligheid is een topprioriteit. Adobe Pass-verificatie zorgt ervoor dat alleen geautoriseerde gebruikers toegang hebben tot premiuminhoud door toegang tot het apparaat van de gebruiker te binden. Het biedt ook opties om het aantal gelijktijdige streams, sessies of apparaten per huishouden te beperken.

**Welke apparaten steunt de Authentificatie van Adobe Pass?**

Adobe Pass-verificatie is ontworpen voor gebruik op een groot aantal apparaten, zoals webapparaten, mobiele apparaten of apparaten met een tv-verbinding.

**Kosten de Authentificatie van Adobe Pass om het even wat voor eind - gebruikers?**

Nee. Adobe Pass-verificatie is gratis voor eindgebruikers.
