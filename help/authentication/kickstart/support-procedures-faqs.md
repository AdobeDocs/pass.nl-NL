---
title: Veelgestelde vragen over ondersteuningsprocedures
description: Veelgestelde vragen over ondersteuningsprocedures
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Veelgestelde vragen over ondersteuningsprocedures {#support-procedures-faqs}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

In dit document worden de veelgestelde vragen (FAQ) beschreven over ondersteuningsprocedures voor ernstige incidenten (niveau SEVERITY 1) die gevolgen hebben voor de Adobe Pass-verificatie en haar partners.

## Veelgestelde vragen {#faqs}

### Wat is een incident op het niveau SEVERITY 1? {#support-procedures-faqs-1}

Een incident op niveau 1 van ERNST is een live situatie in de productieomgeving die de voltooiing van verificatie- of vergunningsstromen voor één kanaal en één MVPD verhindert, wat een groot aantal abonnees beïnvloedt.

Voorbeelden van incidenten van ernst 1

* Tijdens het verificatieproces wordt de gebruiker niet omgeleid naar de aanmeldingspagina nadat de gebruiker de MVPD in een ondersteunde browser heeft geselecteerd.

* Tijdens het authentificatieproces, is de gebruiker geplakt op een de foutenpagina van de Adobe zonder de capaciteit om de authentificatiestroom opnieuw in werking te stellen.

* De partner ontvangt talrijke rapporten die de gebruikers niet met een specifieke MVPD voor authentiek kunnen verklaren of machtigen.

* De productietoegang die op https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js wordt gehost, is niet beschikbaar.

### Wat is een incident op het niveau van niet-SEVERITY 1?

De Adobe zal onderzoek naar deze kwesties ondersteunen, maar deze worden niet beschouwd als incidenten op niveau 1 van ernst:

* Een of enkele abonnees kunnen niet worden geverifieerd en blijven op de MVPD-aanmeldingspagina.

* Een of enkele abonnees zijn geverifieerd, maar kunnen geen video&#39;s afspelen.

* Sommige of alle abonnees ondervinden een JavaScript-fout op de programmeersite.

### Hoe worden incidenten op het niveau van ernst 1 afgehandeld?

Een fout op het niveau SEVERITY 1 kan worden geïnitieerd door een Adobe of een Adobe Pass Authentication-partner. De stappen voor elk worden hieronder beschreven.

**Partner-In werking gestelde stroom**

1. De partner identificeert een incident op het niveau van de Ernst 1 dat de directe aandacht van de Adobe vereist.

1. De partner verzendt een e-mail naar **tve-support@adobe.com** met inbegrip van **URGENT - INCIDENT** in de onderwerpregel en het toevoegen van de volgende informatie:
   * Titel
   * Beschrijving en stappen om te reproduceren
   * Besturingssysteem/browser
   * SDK en versie
   * Betrokken apparaten
   * % betrokken gebruikers
   * HTTP-tracerings- of apparaatlogboeken waarin het probleem wordt aangegeven
   * (optioneel) Alle beschikbare schermafbeeldingen of video-opnamen die het probleem aantonen

1. Als de Adobe niet aan het kaartje binnen een periode antwoordt, kan de partner het volgende aantal roepen: **1-657-312-4623**.

>[!IMPORTANT]
>
> Als u &quot;URGENT-INCIDENT&quot; niet opneemt in de titel van het ticket, wordt dit niet opgepikt door ons meldingssysteem.

**Adobe-in werking gestelde stroom**

Voor een Adobe Pass-verificatieprobleem:

1. Adobe identificeert een interne kwestie en opent een kaartje in ons volgsysteem.

1. De Adobe brengt de het programmamanager en technische contact van de partner op de hoogte, die het kaartkaartaantal en het geschatte effect van de kwestie specificeren.

1. Adobe werkt aan het oplossen van het incident en houdt alle betrokken partners op de hoogte.

Voor een partnerkwestie (programmeur/MVPD):

1. Adobe stelt een probleem vast dat verband houdt met de integratie met een MVPD of op een van de sites van de programmeur.

1. De Adobe brengt de beïnvloede partner op de hoogte na de steunprocedures op zijn plaats met die partner en opent een kaartje met de de steunorganisatie van de partner.

1. Als de Adobe tijdens de effectbeoordeling vaststelt dat de kwestie onder een van de vooraf overeengekomen besluiten inzake incidentscenario&#39;s valt, zal zij dienovereenkomstig handelen zonder op de inbreng van de partner te wachten.

1. De Adobe zal op updates van de partner en een bericht wachten wanneer de dienst is hersteld.

### Wat zijn vooraf overeengekomen besluiten over incidentscenario&#39;s?

Bepaalde situaties met standaardacties die worden uitgevoerd als het scenario zich voordoet:

|    | Scenario | Beschrijving | Handelingen |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe stelt een probleem vast waarbij de integratie van MVPD tijdens normale productieactiviteiten een rol speelt. | Tijdens normale productieactiviteiten stelt de Adobe een probleem vast met een van de MVPD&#39;s waardoor het onmogelijk wordt om de verificatie-/machtigingsstromen uit te voeren (bv. verlopen certificaten, verlopen SAML-reacties, gesloten poorten, gewijzigde parameters, enz.) | Adobe zal de betrokken MVPD en programmeurs op de hoogte stellen.  </br></br> Adobe deactiveert deze MVPD voor alle betrokken programmeurs. </br></br> Adobe opent een ticket met de MVPD volgens de overeengekomen ondersteuningsprocedure met die MVPD |
| S2 | Adobe activeert een nieuwe MVPD voor een programmeur, en de programmeur staat MVPD vóór de lanceringsdatum toe. | Adobe activeert een nieuwe MVPD voor de site van een programmeur en de site geeft de nieuwe MVPD al in de kiezer weer, zelfs als dat niet het geval was. | De Adobe stelt de programmeur vóór de geplande datum in kennis van de nieuwe MVPD die in de kiezer wordt weergegeven. </br></br> De programmeur zal actie ondernemen om het uit de plukker indien nodig te verwijderen. |
| S3 | Adobe activeert een nieuwe MVPD voor een programmeur, ook al is de MVPD niet klaar om in productie te gaan | Adobe activeert een nieuwe MVPD voor een programmeur, maar MVPD heeft de steun voor de integratie nog niet opgesteld zodat de authentificatie/vergunningsstromen niet kunnen worden uitgevoerd | De Adobe zal de implementatie alleen uitvoeren als de programmeur hierom vraagt </br></br> De programmeur is verantwoordelijk voor het garanderen van de vergunning van de MVPD zodra alle tests zijn uitgevoerd. |
