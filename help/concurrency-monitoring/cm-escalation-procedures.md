---
title: Toezicht op de valutamarkt
description: Toezicht op de valutamarkt
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Toezicht op de valutamarkt {#esc-procedures}

>[!NOTE]
>
>Bel de hotline: +1-205-693-9813 en stuur een e-mail naar `tve-support@adobe.com` , inclusief &quot;URGENT - INCIDENT&quot; in de onderwerpregel.


## Inleiding {#cm-escalation-intro}

Dit document beschrijft de steunprocedures voor belangrijke incidenten (**SEVERITY 1** niveau) die de Authentificatie van Adobe Pass, de Gelijktijdige Controle van Adobe Pass en zijn partners beïnvloeden.

## Definitie van escalatieniveau 1 niveau {#defn-escl-sevrityone-level}

A **SEVERITY 1** niveauincident is a **LIVE** situatie, **die in het productiemilieu** gebeurt, dat niet de voltooiing van de authentificatie en/of vergunningsstromen voor één kanaal en één MVPD toestaat, die een groot aantal abonnees van MVPD beïnvloeden die de stroom uitvoeren.

## Voorbeelden van incidenten met prioriteitsgraad 1 {#exampl-sevone-incident}

* De productietoegang die op <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> wordt gehost, is niet beschikbaar.

* Voor een specifieke MVPD wordt de aanmeldingspagina niet meer door Adobe omgeleid/weergegeven nadat de gebruiker de MVPD heeft geselecteerd (in een van de ondersteunde browsers).

* De partner ontvangt een groot aantal rapporten die de gebruikers niet met een specifieke MVPD voor authentiek kunnen verklaren/machtigen.

* Tijdens het verificatieproces zit de gebruiker vast op een Adobe-foutpagina zonder de mogelijkheid om de verificatie-/autorisatiestroom opnieuw te starten.


## Voorbeelden van wat *NIET* aSeverity 1 incident is {#exampl-not-sev1}

*voor kwesties van deze types, zal Adobe steun voor onderzoeken verlenen maar zij zijn niet de incidenten van de Ernst 1:*

* Een of enkele abonnees kunnen de flow niet uitvoeren vanwege een probleem met de Flash-versie (Flash ontbreekt, Flash-blokkers ontbreken, de Flash-versie is onjuist).
* Een of enkele abonnees kunnen niet verifiëren en blijven op de MVPD-aanmeldingspagina.
* Een of enkele abonnees zijn geverifieerd, maar kunnen geen video&#39;s afspelen.
* Een/weinigen/alle abonnees hebben een JavaScript-fout op de programmeersite.

## Prioriteit 1 Escalatiestromen {#sevone-escalation-flows}

Problemen met prioriteitsniveau 1 kunnen worden gestart door Adobe of een Adobe Pass-verificatiepartner. De stappen voor elk worden hieronder weergegeven.

### Door partners geïnitieerde stroom {#partner-initiated-flow}

1. De partner identificeert een incident met prioriteitsniveau 1 (zoals hierboven beschreven) dat onmiddellijk aandacht van Adobe vereist.

1. De partner verzendt een e-mail naar tve-support@adobe.com met daarin &quot;URGENT - INCIDENT&quot; in de onderwerpregel en voegt de volgende informatie toe:

   * Titel
   * Beschrijving en stappen om te reproduceren
   * OS
   * Browser
   * Flash-versie
   * (optioneel) Alle beschikbare schermafbeeldingen of video-opnamen die het probleem aantonen

1. Als Adobe niet binnen 30 minuten op het ticket reageert, roept de partner het onderstaande nummer aan:

   * **1-205-693-9813**


**als u &quot;URGENT-INCIDENT&quot;in de titel van het kaartje niet omvat zal het niet door ons berichtsysteem worden opgenomen.**

### Door Adobe geïnitieerde flow {#adobe-initiated-flow}

**...for an Adobe Pass Authentication issue**

1. Adobe identificeert een intern probleem en opent een ticket met ons trackingsysteem.

1. Adobe brengt de programmamanager en de technische contactpersoon van de partner op de hoogte, met vermelding van het nummer van het ticket en de geschatte impact van de kwestie.

1. Adobe werkt aan de oplossing van het incident en houdt alle betrokken partners op de hoogte.


**...voor een partnerkwestie (Programmer/MVPD)**

1. Adobe stelt een probleem vast in verband met de integratie met een MVPD of op een van de sites van de programmeur.

1. Adobe brengt de beïnvloede partner **na de steunprocedures op zijn plaats met die partner** op de hoogte en opent een kaartje met de de steunorganisatie van de partner.

1. Indien Adobe tijdens de effectbeoordeling vaststelt dat de kwestie tot een van de vooraf overeengekomen besluiten over incidentscenario&#39;s behoort (zie de paragraaf &quot;Vooraf overeengekomen besluiten over incidentscenario&#39;s&quot; hieronder), zal het dienovereenkomstig handelen zonder op partner1 te wachten. -invoer.

1. Adobe zal op updates van de partner en een bericht van de partner wachten wanneer de dienst is hersteld.

### Vooraf overeengekomen besluiten inzake incidentscenario&#39;s {#pre-agreed-decisions}

Er zijn situaties waarin een standaardhandeling wordt uitgevoerd in het geval dat dat scenario zich voordoet:

|    | Scenario | Beschrijving | Handelingen |
|:---:|:---|:---|:---|
| S1 | Adobe stelt een probleem vast met de integratie van MVPD tijdens normale productieactiviteiten. | Tijdens normale productieactiviteiten stelt Adobe een probleem vast met een van de MVPD&#39;s waardoor het onmogelijk wordt om de verificatie-/machtigingsstromen uit te voeren (bv. verlopen certificaten, verlopen SAML-reacties, gesloten poorten, gewijzigde parameters, enz.) | Adobe zal de betrokken MVPD en programmeurs op de hoogte stellen. Adobe deactiveert deze MVPD voor alle betrokken programmeurs. Adobe opent een ticket met de MVPD volgens de overeengekomen ondersteuningsprocedure met die MVPD |
| S2 | Adobe activeert een nieuwe MVPD voor een programmeur, en de programmeur whitelist MVPD vóór de lanceringsdatum. | Adobe activeert een nieuwe MVPD voor de site van een programmeur. De site geeft de nieuwe MVPD al in de kiezer weer, zelfs als dat niet het geval was. | Adobe stelt de programmeur vóór de geplande datum in kennis van de nieuwe MVPD die in de kiezer wordt weergegeven. De programmeur zal actie ondernemen om het uit de plukker indien nodig te verwijderen. |
| S3 | Adobe activeert een nieuwe MVPD voor een programmeur, ook al is de MVPD nog niet klaar om in productie te worden genomen | Adobe activeert een nieuwe MVPD voor een programmeur, maar de MVPD heeft de steun voor de integratie nog niet opgesteld zodat de authentificatie/vergunningsstromen niet kunnen worden uitgevoerd | Adobe zal de implementatie alleen uitvoeren als de programmeur hierom vraagt De programmeur zal ervoor zorgen dat de MVPD wordt gewhitelliseerd zodra alle tests zijn uitgevoerd. |

### Responsverwachtingen voor incidenten met prioriteitsniveau 1 {#response-expectations}

* Eerste antwoord: 30 minuten (24/7)
* Actieplan: 1 uur (24/7)
* Resolutie: ASAP (24/7)
