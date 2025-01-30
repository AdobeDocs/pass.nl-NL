---
title: MVPD-integratiegids
description: MVPD-integratiegids
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# MVPD-integratiegids {#mvpd-integration-guide}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Deze integratiehandleiding is bedoeld voor Multi-channel Video Programming Distributors (MVPD&#39;s) die willen integreren met Adobe® Pass Authentication.

TVE Overal is een transformatief initiatief in de betaaltelevisieindustrie, waardoor abonnees toegang hebben tot de inhoud die zij al voor meerdere apparaten betalen, zowel thuis als onderweg. Voor betaaltelevisieaanbieders biedt TVE aanzienlijke mogelijkheden, waaronder het versterken van bestaande klantenrelaties en het openen van deuren voor nieuwe klanten. Deze kansen komen echter met uitdagingen.

In het ecosysteem van TVE, **leveren de Programmers** de inhoud, terwijl **MVPDs** (Multichannel Video Programmerende Distributeurs) de klantengegevens nodig beheren om te verifiëren als de kijkers in aanmerking komende abonnees zijn. Hoewel de coördinatie van authenticatie en autorisatie met één programmeur hanteerbaar kan zijn, leidt dit met tientallen of zelfs honderden programmeurs tot aanzienlijke complexiteit.

Dit is waar **Adobe® de Authentificatie van de pas** het proces vereenvoudigt. MVPD&#39;s hoeven slechts één gestroomlijnde integratie met Adobe Pass te implementeren om toegang te krijgen tot het hele TVE-ecosysteem. Het meegeleverde integratieframework versnelt de &#39;time-to-market&#39;, biedt een veilige omgeving om fraude te beperken en verbetert de gebruikerservaring door meer tv-inhoud op meerdere platforms te leveren.

## Adobe Pass-verificatie voor tv overal {#adobe-pass-authentication-for-tv-everywhere}

De Authentificatie van Adobe Pass werkt als oplossing SaaS (Software als Dienst) die wordt ontworpen om snelle backend (server-aan-server) integratie toe te laten, die de bedrijfsregels van zowel Multichannel Video Programming Distributors (MVPDs) als Programmers volgt.

### Normen en protocollen {#standards-protocols}

Adobe Pass Authentication is volledig in overeenstemming met de opkomende standaarden en protocollen voor TV Anywhere (TVE), die naadloze integratie en naleving in het ecosysteem ondersteunen.

* **CableLabs OLCA (Online Toegang van de Inhoud) Specificatie**\
  Adobe Pass Authentication volgt de CableLabs OLCA-specificatie, die de technische vereisten en architectuur definieert voor het leveren van video-inhoud van online bronnen aan betaaltelevisieklanten. Adobe nam actief deel aan het project voor het testen van de interoperabiliteit van CableLabs in juni 2011, dat met succes het testproces voor een implementatie van de Dienstverlener overging.

De Authentificatie van Adobe Pass is ontworpen om veelvoudige protocollen (b.v., SAML, OAuth 2.0, enz.) te steunen, deze flexibiliteit die voor toekomstige uitbreidingen, met inbegrip van douaneprotocollen, op evoluerende behoeften wordt gebaseerd.

De meeste integraties gebruiken het SAML (de Taal van de Prijsverhoging van de Bevestiging van de Veiligheid) protocol, een primaire norm voor authentificatie. De Authentificatie van Adobe Pass doet dienst als volmachtsdienstverlener binnen het kader van SAML, die de authentificatiereactie van SAML als veilig teken in het gemeenschappelijke domein van de Adobe voortduurt.

Op de onderste regel is Adobe Pass Authentication protocol-agnostic, ontworpen om nauw te worden afgestemd op OLCA-standaarden. Deze normen vestigen een gemeenschappelijk kader voor Programmers, MVPDs, en Dienstverleners, die een samenhangende benadering van het uitvoeren van de eigenschappen van TVE verzekeren.

### Integratie en ondersteuning {#integration-support}

Adobe Pass Authentication werkt als volgt samen met de technische teams van MVPD om integraties te configureren die zijn toegesneden op hun specifieke vereisten:

* **StandaardIntegraties**\
  Aangeboden gratis, inclusief documentatie en standaard e-mailondersteuning.

* **Verbeterde Steun**\
  Voor belangrijke aanpassingen of versnelde tijdlijnen, kan een ondersteuningstaks van toepassing zijn.

Adobe Pass Authentication biedt ondersteuning voor de efficiënte verwerking van MVPD-bedrijfslogica, als volgt:

* **Zelfstandige BedrijfsLogica**\
  Voor bedrijfslogica die volledig door MVPD tijdens vergunningsverzoeken wordt afgedwongen, verstrekt de Adobe de noodzakelijke gegevens. Deze gegevens kunnen, maar zijn niet beperkt tot, de unieke apparaat-id en het IP-adres voor het apparaat van de gebruiker dat de aanvraag doet.

* **Steun van het Bezit van de Douane**\
  Voor bedrijfslogica die gebruikersinterventie of Adobe-specifieke behandeling vereist, kan de Adobe douaneconfiguraties voor elke MVPD handhaven. Met dit beleid kunnen vooraf gedefinieerde workflows worden geactiveerd op specifieke punten in de machtigingsworkflow. Neem voor meer informatie contact op met uw Adobe.

## Entitlement Flow {#entitlement-flow}

De machtigingsstroom is een reeks stappen die een programmeertoepassing (TVE) moet voltooien om beveiligde inhoud te streamen. De flow omvat verschillende fasen die interactie met de MVPD inhouden:

* [Verificatiefase](#authentication-phase)
* (Optioneel) Preautorisatiefase
* [Autorisatiefase](#authorization-phase)
* Afmeldingsfase

Bij het eerste bezoek van een gebruiker aan een programmeertoepassing (TVE) volgt de stroom voor machtigingen de geschetste volgorde. Bij volgende bezoeken kan de toepassing echter bepaalde stappen omzeilen op basis van de status van de verificatie en het toepasselijke weergavebeleid.

>[!NOTE]
>
> De toepassing van de programmeur (TVE) wordt gebruikt in dit document om collectief naar de types van toepassingen te verwijzen die op verschillende platforms (browsers, mobiele apparaten, TV aangesloten apparaten, enz.) worden loopt die door de Authentificatie van Adobe Pass worden gesteund.

### Verificatiefase {#authentication-phase}

De volgende stappen schetsen de stappen op hoog niveau in het geval van een integratie van SAML:

1. **de Lading van de Toepassing van de Programmer (Website)**\
   De gebruiker navigeert aan de toepassing van de Programmer (website), die de Authentificatie van Adobe Pass [ REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) integreert.

1. **het Beschermde Verzoek van de Inhoud**\
   Wanneer de gebruiker toegang probeert te krijgen tot beveiligde inhoud, geeft de toepassing van de programmeur een lijst van MVPD&#39;s weer waaruit de gebruiker kan kiezen.

1. **Initialisatie van het Verzoek van de Authentificatie**\
   Als MVPD is geselecteerd, wordt de gebruiker omgeleid naar een Adobe Pass-verificatieserver. Hier, wordt een gecodeerd de authentificatieverzoek van SAML voor geselecteerde MVPD geproduceerd, in het geval van een integratie SAML. Dit verzoek wordt namens de programmeur naar de MVPD verzonden. Afhankelijk van het MVPD-systeem wordt de browser van de gebruiker omgeleid naar de aanmeldingspagina van MVPD of wordt een inlogiFrame ingesloten in de toepassing van de programmeur.

1. **Login MVPD**\
   MVPD accepteert de aanvraag en presenteert zijn aanmeldingsinterface via omleiding of iFrame.

1. **Login en Bevestiging van de Gebruiker**\
   De gebruiker meldt zich aan met zijn MVPD-referenties. De MVPD valideert de abonnementsstatus van de gebruiker en stelt een eigen HTTP-sessie in.

1. **MVPD Reactie op de Authentificatie van Adobe Pass**\
   Nadat de validatie is voltooid, genereert de MVPD een (gecodeerde) SAML-reactie en stuurt deze terug naar de Adobe Pass-verificatie.

1. **de Generatie van het Profiel**\
   De Authentificatie van Adobe Pass verifieert de reactie van SAML, produceert een gebruikersprofiel dat caching wordt, en richt de gebruiker terug naar de toepassing van de Programmer (website).

### Autorisatiefase {#authorization-phase}

**Stappen op hoog niveau**

In de volgende stappen worden de stappen op hoog niveau beschreven:

1. **Verwerking van het Herkenningsteken van het Middel**\
   De beschermde inhoud wordt geïdentificeerd door a [ middelherkenningsteken ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), dat een eenvoudig koord of een complexere structuur kan zijn. Deze id is vooraf gedefinieerd en overeengekomen door de programmeur en de MVPD. De toepassing van de Programmer verzendt het middelherkenningsteken naar de Authentificatie van Adobe Pass [ REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **de Controle van de Vergunning van MVPD**\
   De server van de Authentificatie van Adobe Pass communiceert met het de vergunningseindpunt van MVPD gebruikend gestandaardiseerde protocollen.

1. **MVPD Reactie op de Authentificatie van Adobe Pass**\
   Zodra de validatie is voltooid, bevestigt de MVPD dat de gebruiker (of niet) gerechtigd is om toegang te krijgen tot de inhoud en een reactie terug te sturen naar de Adobe Pass-verificatie.

1. **Besluit en de Symbolische Generatie van Media**\
   De Authentificatie van Adobe Pass verifieert de reactie, produceert a [ besluit ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) dat in het voorgeheugen ondergebracht wordt, en keert het besluit terug dat een media teken terug naar de toepassing van de Programmer (website) bevat.

1. **de Verificatie van de Toegang van de Inhoud**\
   De toepassing van de Programmer gebruikt [ Symbolische Verifier van Media ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) om te bevestigen dat de correcte gebruiker tot de correcte inhoud toegang heeft. Na de validatie krijgt de gebruiker toegang om de beveiligde inhoud weer te geven.

## Entitlement {#understanding-entitlements}

De oplossing van de Authentificatie van Adobe Pass draait rond de verwezenlijking van recht-specifieke stukken gegevens die op de succesvolle voltooiing van authentificatie en vergunningswerkschema&#39;s worden geproduceerd. Deze rechten verlenen toegang tot beschermde inhoud, maar hebben een beperkte duur. Nadat een machtiging is verlopen, moet deze worden vernieuwd door de verificatie- of autorisatieprocessen opnieuw te starten.

Raadpleeg de volgende documenten voor meer informatie over rechten:

* **Profielen**

  Na succesvolle verificatie maakt Adobe Pass Authentication een geverifieerd profiel (&quot;long-live&quot;) dat is gekoppeld aan de aanvraag-id voor de toepassing, het apparaat en de serviceprovider (aanvrager-id).

* **[Metagegevens van de Gebruiker](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Bij geslaagde verificatie (en in sommige gevallen ook na autorisatie) ontvangt Adobe Pass Authentication gebruikersmetagegevens van de MVPD die deze toegankelijk kunnen maken voor de toepassing die het verzoek indient.

* **[Besluiten](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Na een geslaagde autorisatie maakt Adobe Pass Authentication een machtigingsbesluit (&quot;langlevende&quot;) dat verband houdt met de aanvraag, het apparaat, de dienstverlener-id (aanvrager-id) en een specifieke beschermde bron (bron-id).

* **[Tokens van Media](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Na een geslaagde autorisatie maakt Adobe Pass Authentication een mediatoken (&quot;kortstondig&quot;) die aan een succesvol afspeelverzoek is gekoppeld.
