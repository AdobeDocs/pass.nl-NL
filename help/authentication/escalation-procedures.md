---
title: Doorverwijsprocedures
description: Doorverwijsprocedures
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '854'
ht-degree: 0%

---

# Doorverwijsprocedures {#escalation-procedures}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
> 
>Bel de hotline: **+1-205-693-9813** en stuur een e-mail naar **tve-support@adobe.com** inclusief **URGENT - INCIDENT** op de onderwerpregel.

## Inleiding {#introduction}

In dit document worden de ondersteuningsprocedures voor grote incidenten beschreven (**ERNST 1** niveau) die van invloed is op Adobe Pass Authentication, Adobe Pass Concurrency Monitoring en haar partners.


## Definitie van een incident op niveau 1 van ernst {#definition-of-a-severity-1-level-incident}

A **ERNST 1** incident op niveau is **LEVEN** situatie, **in de productieomgeving**, waardoor de verificatie- en/of machtigingsstromen voor één kanaal en één MVPD niet kunnen worden voltooid, wat een groot aantal abonnees van de MVPD die de stroom uitvoeren, betreft.


## Voorbeelden van incidenten van ernst 1 {#examples-of-severity-1-incidentcs}

* Productietoegang ingeschakeld bij  `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (of `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) is niet beschikbaar.

* Voor een specifieke MVPD, leidt de Adobe niet meer de login pagina opnieuw of toont, nadat de gebruiker MVPD (in om het even welke gesteunde browsers) selecteert.

* De partner ontvangt een groot aantal rapporten dat de gebruikers niet met specifieke MVPD kunnen voor authentiek verklaren/machtigen.

* Tijdens het verificatieproces zit de gebruiker vast op een foutpagina van een Adobe zonder de mogelijkheid om de verificatie-/autorisatiestroom opnieuw te starten.


| Voorbeelden van **NOT** een incident met prioriteitsniveau 1 |
|---|
| Voor dit soort kwesties zal de Adobe steun verlenen aan onderzoeken, maar het zijn geen incidenten van ernst 1:<ul><li>Een of enkele abonnees kunnen de stroom niet uitvoeren vanwege een probleem met de versie van de Flash (ontbrekende Flash, blokkeerders van Flash, onjuiste versie van Flash).</li><li>Een of enkele abonnees kunnen niet verifiëren en blijven op de MVPD-aanmeldingspagina.</li><li>Een of enkele abonnees zijn geverifieerd, maar kunnen geen video&#39;s afspelen.</li><li>Een/paar/alle abonnees hebben een JavaScript-fout aangetroffen op de programmeersite</li></ul> |

## Prioriteit 1 Escalatiestromen {#severity-1-escalation-flows}

Ernst 1-incidenten kunnen worden geïnitieerd door een Adobe of een Adobe Pass-verificatiepartner. De stappen voor elk worden hieronder weergegeven.

### Door partners geïnitieerde stroom {#partner-initiated-flow}

1. De partner identificeert een incident met ernst 1 (zoals hierboven beschreven) dat de onmiddellijke aandacht van de Adobe vereist.
1. De partner verzendt een e-mail naar **tve-support@adobe.com** inclusief **URGENT - INCIDENT** in de onderwerpregel en door de volgende informatie toe te voegen:
   * Titel
   * Beschrijving en stappen om te reproduceren
   * Besturingssysteem/browser
   * SDK en versie
   * Betrokken apparaten
   * % betrokken gebruikers
   * HTTP-tracerings- of apparaatlogboeken waarin het probleem wordt aangegeven
   * (optioneel) Alle beschikbare schermafbeeldingen of video-opnamen die het probleem aantonen
1. Als de Adobe niet aan het kaartje binnen 30 minuten antwoordt, roept de partner het volgende aantal:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Als u &quot;URGENT-INCIDENT&quot; niet opneemt in de titel van het ticket, wordt het niet opgepakt door ons meldingssysteem**.

### Door de Adobe geïnitieerde stroom {#adobe-initiated-flow}

#### ...voor een Adobe Pass-verificatieprobleem {#adobe-initiated-flow-authn-issue}

1. Adobe identificeert een interne kwestie en opent een kaartje met ons volgsysteem.

1. De Adobe brengt de het programmamanager en technische contact van de partner op de hoogte, die het kaartkaartkaartaantal en het geschatte effect van de kwestie specificeren.

1. Adobe werkt aan de oplossing van het incident en houdt alle betrokken partners op de hoogte.

#### ...voor een partnerkwestie (Programma/MVPD) {#adobe-initiated-flow-partner-issue}

1. In de Adobe wordt een probleem vastgesteld dat verband houdt met de integratie met een MVPD of op een van de sites van de programmeur.

1. Adobe brengt de beïnvloede partner op de hoogte <u>na de ondersteunende procedures die met die partner van kracht zijn</u> en opent een kaartje met de de steunorganisatie van de partner.

1. Indien de Adobe tijdens de effectbeoordeling vaststelt dat het probleem tot een van de vooraf overeengekomen besluiten over incidentscenario&#39;s behoort, zie **Vooraf overeengekomen besluiten inzake incidentscenario&#39;s**, zal zij dienovereenkomstig handelen zonder op de input van de partner te wachten.

1. De Adobe zal op updates van de partner en een bericht van de partner wachten wanneer de dienst is hersteld.

## Vooraf overeengekomen besluiten inzake incidentscenario&#39;s {#pre-agreed-descn}

Er zijn situaties waarin een standaardhandeling wordt uitgevoerd in het geval dat dat scenario zich voordoet:

|   | Scenario | Beschrijving | Handelingen |
|---|---|---|---|
| S1 | Adobe stelt vast dat er een probleem is met de integratie van een MVPD tijdens normale productieactiviteiten. | Tijdens normale productieactiviteiten stelt de Adobe een probleem vast met een van de MVPD&#39;s waardoor het onmogelijk wordt om de verificatie-/machtigingsstromen uit te voeren (bv. verlopen certificaten, verlopen SAML-reacties, gesloten poorten, gewijzigde parameters, enz.) | - de Adobe zal de betrokken MVPD en programmeurs op de hoogte brengen.  </br> </br> - Adobe deactiveert dit MVPD voor alle betrokken programmeurs. </br> </br> - Adobe zal een ticket openen met het MVPD volgens de overeengekomen steunprocedure met dat MVPD |
| S2 | De Adobe activeert een nieuwe MVPD voor een Programmer, en de programmeur staat MVPD vóór de lanceringsdatum toe. | Adobe activeert een nieuwe MVPD voor de plaats van een Programmer, en de plaats toont nieuwe MVPD reeds in de plukker, zelfs als het niet verondersteld was. | - de Adobe zal de programmeur vóór de geplande datum in kennis stellen van de nieuwe MVPD die in de kiezer wordt weergegeven. </br> </br>  - De programmeur zal actie ondernemen om het uit de kiezer te verwijderen indien nodig. |
| S3 | Adobe activeert een nieuwe MVPD voor een Programmer zelfs als MVPD niet bereid is in productie te gaan | De Adobe activeert een nieuwe MVPD voor een Programmer, maar MVPD heeft nog niet de steun voor de integratie opgesteld zodat kunnen de authentificatie/de vergunningsstromen niet worden uitgevoerd | - de Adobe zal de implementatie alleen uitvoeren als de programmeur daarom vraagt </br> </br> - De programmeur zorgt ervoor dat de MVPD wordt goedgekeurd zodra alle tests zijn uitgevoerd. |

## Responsverwachtingen voor incidenten met prioriteitsniveau 1 {#response-expectations-for-severity-one-incidents}

* Eerste antwoord: 30 minuten (24/7)
* Actieplan: 1 uur (24/7)
* Resolutie: ASAP (24/7)
