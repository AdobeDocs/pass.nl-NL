---
title: Besluiten
description: Besluiten
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# Besluiten {#decisions}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De besluiten worden geproduceerd door de Authentificatie van Adobe Pass [&#x200B; VERTONING API V2 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) die op de de vergunning van MVPD van de gebruiker of voorvergunningsonderzoeken wordt gebaseerd, die bepalen of de toegang tot [&#x200B; beschermde inhoud &#x200B;](#protected-resources) wordt verleend of ontkend.

Er zijn twee soorten besluiten die, afhankelijk van aangehaalde API worden verstrekt:

* [&#x200B; Besluiten van de preAutorisatie &#x200B;](#preauthorization-decisions) die informatieve besluiten zijn.
* [&#x200B; Besluiten van de Toestemming &#x200B;](#authorization-decisions) die gebiedende besluiten zijn.

## Voorbereidende besluiten {#preauthorization-decisions}

Het pre-vergunningsbesluit is een informatief besluit dat de cliënttoepassing om toelaat worden geïnformeerd of MVPD de toegang van de gebruiker tot a [&#x200B; beschermde middel &#x200B;](#protected-resources) kan toestaan of ontkennen.

Het doel van voorafgaande toestemming (Preflight-autorisatie) is de toepassing in staat te stellen nauwkeurige informatie weer te geven over de inhoud die de gebruiker kan bekijken. Dit wordt bereikt door de gebruikersinterface te verbeteren met indicatoren, zoals vergrendelde of ontgrendelde pictogrammen, die de toegangsstatus weerspiegelen.

>[!IMPORTANT]
>
> Het pre-vergunningsbesluit moet niet op een gebiedende manier worden gebruikt om middelen te spelen, aangezien dat het doel van een [&#x200B; vergunningsbesluit &#x200B;](#authorization-decisions) is.

Het gebruik van de API voor voorafgaande toestemming is niet verplicht. De clienttoepassing kan dit overslaan als deze een catalogus met bronnen wil presenteren zonder filtering.

Als de clienttoepassing van plan is deze functie te gebruiken, is het belangrijk om te weten dat beslissingen vóór toelating alleen kunnen worden verkregen voor een beperkt aantal bronnen per API-aanvraag, meestal maximaal 5.

>[!IMPORTANT]
> 
> Het maximumaantal middelen kan pas worden verhoogd nadat overeenstemming is bereikt met de vertegenwoordigers van de MVPD en de Adobe Pass Authentication. Zodra u hiermee akkoord bent gegaan, kunnen de wijzigingen worden geïmplementeerd via het Adobe Pass TVE-dashboard door een beheerder in uw organisatie of een Adobe Pass-verificatievertegenwoordiger die namens u optreedt.
> 
> Voor meer details, verwijs naar de [&#128279;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0&rbrace; TVE.

MVPD&#39;s kunnen preautorisatie door diverse mechanismen steunen, elk met duidelijke implicaties voor prestaties en het maximumaantal middelen dat in één enkele API verzoek kan worden behandeld.

Voor meer details over de bestaande mechanismen ondersteunend prepermission, verwijs naar de [&#x200B; Preflight 1&rbrace; documentatie van MVPD.](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md)

>[!IMPORTANT]
>
> Voor MVPD&#39;s zonder volledige Preflight-autorisatieondersteuning moet vooraf overeenstemming worden bereikt over het gebruik van voorafgaande toestemming met de vertegenwoordigers van de MVPD&#39;s en de Adobe Pass-authenticatie, omdat dit kan leiden tot prestatieproblemen en tragere responstijden.

## Toelatingsbesluiten {#authorization-decisions}

Het vergunningsbesluit is een gebiedend besluit dat de cliënttoepassing toestaat om met het besluit van MVPD volgzaam te zijn om de toegang van de gebruiker tot a [&#x200B; beschermde middel &#x200B;](#protected-resources) toe te laten of te ontkennen.

Het doel van de autorisatie is om de toepassing in staat te stellen de door de gebruiker gevraagde bronnen af te spelen, na rechtenvalidatie met de MVPD en ontvangst van een media-token van Adobe Pass-verificatie.

>[!IMPORTANT]
> 
> Adobe Pass Authentication raadt programmeurs aan de Media Token Verifier-bibliotheek te gebruiken om het mediatoken te valideren dat in een autorisatiebesluit is opgenomen. Hierdoor wordt beveiligde toegang gegarandeerd voordat de videostream wordt gestart.
> 
> Voor meer details, verwijs naar de [&#x200B; Tokens van Media &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) documentatie.

Het gebruik van de autorisatie-API is verplicht. De clienttoepassing kan deze fase niet overslaan als deze bronnen wil afspelen die de gebruiker vraagt, omdat de toepassing eerst met de MVPD moet controleren of de gebruiker gerechtigd is voordat de stream wordt vrijgegeven.

Het is belangrijk om op te merken dat vergunningsbesluiten slechts voor een beperkt aantal middelen per API verzoek kunnen worden verkregen, typisch 1.

>[!IMPORTANT]
>
> Het maximumaantal middelen kan pas worden verhoogd nadat overeenstemming is bereikt met de vertegenwoordigers van de MVPD en de Adobe Pass Authentication.

## Autorisatietijd-aan-Levend (TTL) Beheer {#authorization-ttl-management}

De Tijd-aan-Levende van de vergunning (TTL) bepaalt hoe lang een middel alvorens het moet opnieuw machtigen wordt toegelaten. Deze termijn is beperkt en moet worden overeengekomen met vertegenwoordigers van MVPD. De waarden van TTL kunnen variëren gebaseerd op:

* Platformcategorie (bijvoorbeeld desktop, mobiel, met tv verbonden apparaten)
* Specifiek platform (bijvoorbeeld iOS, Android, tvOS, Roku, FireTV)

De vergunning (authZ) TTL kan door het Dashboard van Adobe Pass [&#x200B; worden bekeken en worden veranderd TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [&#128279;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0&rbrace; TVE.

## Beveiligde bronnen {#protected-resources}

De beschermde middelen verwijzen naar stroombare inhoud, die door unieke waarden wordt geïdentificeerd die door overeenkomsten tussen MVPDs en deelnemende Programmers worden bepaald.

De beschermde middelen volgen een hiërarchische boomstructuur, met elk niveau dat grotere granulariteit voor inhoudsvergunning verstrekt:

* Netwerk
   * Kanaal
      * Tonen
         * Episode
            * Element

>[!IMPORTANT]
>
> De pre-vergunning (Preflight vergunning) is geconcentreerd op kanaal-vlakke middelen met herkenningstekens die of een eenvoudig koord of formaat MRSS hebben.
> 
> We raden u niet aan bronnen te gebruiken met id&#39;s die `CDATA` -secties bevatten in geval van voorafgaande toestemming, omdat deze voornamelijk worden gebruikt voor bronnen op middelenniveau die door een MRSS worden gedefinieerd.

### Resource Identifier {#resource-identifier}

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

In het geval van een eenvoudige middel herkenningsteken, zoals &quot;REF30&quot;(verondersteld om een kanaal te vertegenwoordigen), kan het in een RSS middelherkenningsteken als volgt worden vertaald:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

In het geval van een complexere resource-id kan de RSS-resource-id als volgt aanvullende classificatiegegevens bevatten:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

De unieke id&#39;s zijn voornamelijk ondoorzichtig voor Adobe Pass-verificatie, maar transformatoren kunnen worden toegepast op basis van de MVPD-mogelijkheden en -vereisten. Als MVPD een middelherkenningsteken niet herkent of ontleedt, keert het een fout aan de Authentificatie van Adobe Pass terug, die later de fout aan de cliënttoepassing gebruikend een [&#x200B; Verbeterde Code van de Fout &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) opnieuw afspeelt.

## REST API V2 {#rest-api-v2}

De voorafgaande beslissingen kunnen worden opgehaald met behulp van de volgende API:

* [Besluiten vóór toelating met specifieke mvpd ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

De vergunningsbesluiten kunnen worden teruggewonnen gebruikend volgende API:

* [Autorisatiebesluiten ophalen met specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Verwijs naar de **secties van de Reactie** en **Steekproeven** van bovengenoemde APIs om de structuur van pre- toestemmings en vergunningsbesluiten te begrijpen.

Raadpleeg de volgende documenten voor meer informatie over hoe en wanneer u de bovenstaande API&#39;s wilt integreren:

* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [&#x200B; Veelgestelde vragen van de Fase van de Voorkeur &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [Veelgestelde vragen over de autorisatiefase &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
