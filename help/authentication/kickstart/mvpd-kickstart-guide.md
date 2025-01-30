---
title: MVPD-gids voor kickstart
description: MVPD-gids voor kickstart
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# MVPD-gids voor kickstart {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Deze kickstart gids is voorgenomen voor de Distributeurs van de Video van de Programmering van Multikanaal Video (MVPDs) die van plan zijn om met de Authentificatie van de Volmacht te integreren Adobe®.

In dit document worden de belangrijkste eerste stappen beschreven die moeten zorgen voor een soepele en efficiënte start van het integratieproces. Het is bedoeld om de verwachtingen te verduidelijken en richtsnoeren te geven over de manier waarop we met partners zullen samenwerken om succesvolle integratie te bereiken.

Adobe biedt een reeks bronnen die u helpen bij de integratie met Adobe Pass-verificatie. Gelieve te verwijzen naar **&quot;u&quot;** en **&quot;Adobe zal verstrekken&quot;** commentaren van elke hieronder sectie.

>[!CAUTION]
>
> Telkens wanneer een gebruiker een machtigingsstroom start, wordt aan deze gebruikers een enkele, ondoorzichtige, unieke gebruikers-id toegewezen. Deze id, die afkomstig is uit de MVPD, wordt gebruikt om de gebruiker in de app van een programmeur te identificeren.
>
> <br/>
>
> De gebruikers-id mag geen PII&#39;s (Personal Identified Information) of gegevens bevatten die alleen of in combinatie met andere details kunnen worden gebruikt om de gebruiker te identificeren, te contacteren of te vinden.

## Installatieproces {#setup-process}

Het installatieproces omvat onder andere de volgende stappen:

![ Adobe® het Proces van de Integratie van de Authentificatie van de pas ](../assets/mvpd-int-lifecycle.png)

*Adobe® het Proces van de Integratie van de Authentificatie van de pas*

### Kickoff {#kickoff}

**u** tijdens de kickoff fase zult verstrekken:

* **Naam van de Vertoning**

  Dit is een tekenreeks die wordt weergegeven op websites of toepassingen van programmeurs wanneer gebruikers worden gevraagd hun leverancier van betaaltelevisie te selecteren.

* **Logo URL**

  Dit is een bestand van 112 x 33 pixels, dat het logo bevat dat wordt weergegeven op websites of toepassingen van Programmer wanneer gebruikers worden gevraagd hun Pay TV-provider te selecteren.

* **tijd-aan-Levend (TTL)**

  De TTL is een waarde die typisch door de MVPD als deel van de authentificatie of vergunningsprocessen wordt geplaatst. Adobe kan deze TTL-waarden echter overschrijven en verschillende waarden opgeven, afhankelijk van wat door zowel de programmeur als de MVPD is overeengekomen.

* **Referentiesets**

  Dit zijn referenties die worden gebruikt om de gebruiker te verifiëren en autoriseren, of alleen om de gebruiker te verifiëren met de MVPD. Deze gegevens bestaan meestal uit een gebruikersnaam en wachtwoord, die voor beide profielen (ophaling en productie) moeten worden opgegeven.

### Metagegevensuitwisseling (SAML) {#metadata-exchange-saml}

**Adobe zal** tijdens de fase van de meta-gegevensuitwisseling verstrekken:

* **het Staging milieu meta-gegevens**

  De meta-gegevens van SP van de Adobe kunnen van https://sp.auth-staging.adobe.com/sp/metadata worden teruggewonnen.

* **meta-gegevens van het milieu van de Productie**

  De meta-gegevens van SP van de Adobe kunnen van https://sp.auth.adobe.com/sp/metadata worden teruggewonnen.

**u** tijdens de fase van de meta-gegevensuitwisseling zult verstrekken:

* **het Staging meta-gegevens**

  De MVPD-metagegevens voor de testomgeving.

* **meta-gegevens van de Productie**

  De MVPD-metagegevens voor de productieomgeving.

### Connectiviteit {#connectivity}

**u** een manier zult verstrekken om IPs van Adobe te lijsten van gewenste personen, aangezien de Authentificatie van Adobe Pass firewalls vereist om verkeer door havens 80 en 443 toe te laten om toegang tot beperkte middelen tijdens zowel authentificatie als vergunningsprocessen toe te laten.

**u** een plaatsing in het opvoeren profiel zult verstrekken om de connectiviteit te testen.

### Ontwikkeling {#development}

**de Adobe zal** techniektijd verstrekken om nauw met MVPD te werken om de technische integratie correct te verzekeren wordt gevestigd. Dit proces omvat het ontwikkelen van aangepaste code die is afgestemd op de specifieke MVPD-vereisten.

### Implementatie in gefaseerde implementatie {#deployment-staging}

**de Adobe zal** een bouwstijl van de vereiste codeupdates voorzien die eerst in het PRE-QUAL het opvoeren milieu zullen worden opgesteld. Tijdens deze fase worden de benodigde configuratiewijzigingen ook geïmplementeerd om de MVPD voor testdoeleinden te integreren met het servicebureau `TestDistributors` .

**u en de Adobe zullen** de tijd van de kwaliteitsgarantie (QA) verstrekken om ervoor te zorgen dat de integratie met succes in het PRE-QUAL het opvoeren milieu wordt getest. Na deze fase wordt de MVPD verplaatst naar de testomgeving van RELEASE voor verdere tests met een echte programmeur.

### Plaatsing van productie {#deployment-production}

**u** een plaatsing in het productieprofiel zult verstrekken om de connectiviteit te testen.

**de Adobe zal** een bouwstijl van de vereiste codeupdates verstrekken die in het PRE-QUAL productiemilieu zullen worden opgesteld.

**u en de Adobe zullen** de tijd van de kwaliteitsgarantie (QA) verstrekken om ervoor te zorgen dat de integratie met succes gebruikend het productieprofiel wordt getest. Als alles op dit punt in orde is, kan de Adobe de integratie naar de het productiemilieu van de RELEASE (&quot;levend&quot;) verplaatsen, die voor alle gebruikers beschikbaar is.

>[!IMPORTANT]
>
> Zodra de integratie in de de productieomgeving van RELEASE levend is, is het handhaven van een optimale klantenervaring van het grootste belang. Om server-down scenario&#39;s effectief te richten, moet MVPDs gedetailleerde documentatie van de escalatieprocedure aan Adobe verstrekken voor het beheren van dergelijke kwesties.
>
> Als tegenprestatie zorgt de Adobe ervoor dat MVPD&#39;s de nieuwste versie van het escalatieproces van de Adobe Pass-verificatie ontvangen voor gestroomlijnde probleemoplossing.

## Toegang tot omgevingen {#access-environments}

**de Adobe zal** toegang tot milieu&#39;s voor verschillende stadia van het ontwikkelingsproces verlenen:

* **Prekeerbaarheid (PRE-QUAL)**

  De PRE-QUAL milieu gastheren de volgende versiekandidaat en dient als eerste integratieplatform voor nieuwe partners. Alvorens naar de milieu van de RELEASE over te gaan, krijgen de partners tijd om hun integratie in PRE-QUAL te testen.

* **Versie (VERSIE)**

  In de RELEASE-omgeving wordt de huidige (stabiele) productiebuild gehost.

Voor meer informatie over hoe te om deze milieu&#39;s te gebruiken, verwijs naar [ Begrijpend de ](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md) documentatie van de Milieu van de Adobe.

>[!IMPORTANT]
> 
> De veranderingen van de configuratie aan deze milieu&#39;s moeten uitdrukkelijk worden gevraagd door uw vertegenwoordiger van de Adobe, na het gevestigde proces van het veranderingsverzoek.

## Toegang tot klantenondersteuning {#access-customer-support}

**de Adobe zal** toegang tot ons systeem van de klantensteun via [ Zendesk ](https://tve.zendesk.com/home) verlenen. Als u toegang wilt tot Zendesk, moet u zich registreren en een account maken op https://tve.zendesk.com/home.

Het Adobe Pass-verificatieteam is beschikbaar voor alle vragen of technische problemen die we tijdens het integratieproces kunnen tegenkomen. Gelieve te contacteren ons in [ tve-support@adobe.com ](mailto:tve-support@adobe.com).

## Toegang tot documentatie {#access-documentation}

**de Adobe zal** toegang tot onze openbare documentatie via [ Adobe Experience League ](https://experienceleague.adobe.com/en/docs/pass/authentication/home) verlenen.

Het team van de Authentificatie van Adobe Pass verstrekt uitvoerige documentatie voor de beschikbare eigenschappen en werkschema&#39;s onder de [ Gids van de Integratie voor MVPDs ](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md) sectie. Verwijs naar de inhoudstafel onder deze sectie voor verbindingen aan gedetailleerde informatie over elk onderwerp.

## Toegang tot het testgereedschap {#access-testing-tool}

**Adobe zal** toegang tot ons APIs exploratiehulpmiddel via [ Adobe Developer ](https://developer.adobe.com/adobe-pass/) website verstrekken.
