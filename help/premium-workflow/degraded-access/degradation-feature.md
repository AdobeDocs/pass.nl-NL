---
title: Afbraakkenmerk
description: Afbraakkenmerk
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Afbraakkenmerk {#degradation-feature}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

In de dynamische wereld van live sport en belangrijke evenementen is het van essentieel belang een naadloze kijkervaring te garanderen. Het hoge verkeer tijdens deze gebeurtenissen kan Multichannel Video Programming Distributor (MVPD) authentificatie en vergunningseindpunten overweldigen, die tot vertragingen of verstoringen leiden.

De Authentificatie van Adobe Pass richt deze uitdagingen met zijn **Eigenschap van de Afbraak**, een oplossing die tijdelijke omzeiling van specifieke de authentificatie en toestemmingseindpunten van MVPD toelaat. Deze functie is met name nuttig tijdens piekverkeersgebeurtenissen, waarbij de responstijden kunnen dalen als gevolg van een zware belasting op MVPD-systemen.

De **Eigenschap van de Vermindering** kan een vitale waarborg voor Programmers zijn, die continuïteit van de dienst verzekeren. Hoewel het primaire publiek live sport- en nieuwszenders omvat, strekt het programma zich uit tot elke programmeur die het risico van verstoringen als gevolg van eindpunten van MVPD probeert te beperken.

>[!IMPORTANT]
>
> De Degradation API is een premiumfunctie en vereist een huidige licentie van Adobe.

Door een degradatieregel toe te passen, kunnen de programmeurs auto-authentificatie of auto-vergunning tijdelijk toelaten, die ononderbroken toegang tot inhoud voor de periode verzekeren de degradatie wordt toegepast. Degradatieacties worden altijd geïnitieerd door een programmeur op basis van vooraf overeengekomen overeenkomsten met MVPD&#39;s. Hoewel Adobe momenteel niet rechtstreeks een verslechtering veroorzaakt, kunnen toekomstige mogelijkheden ook proactief beheer omvatten als Service Level Agreements (SLA&#39;s) worden ingesteld.

Deze eigenschap wordt ontworpen om samen met a [&#x200B; gebruik controle API &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) en gebaseerd op pre-overeenkomsten met MVPDs worden gebruikt, biedt de Authentificatie van Adobe Pass een krachtig hulpmiddel aan om gebruikerservaring, betrouwbaarheid, en operationele controle tijdens kritieke momenten in evenwicht te brengen.

>[!IMPORTANT]
>
> Met deze functie kunt u de Adobe Pass-verificatieservice zelf niet omzeilen. Als Adobe Pass-verificatie niet beschikbaar is, beschikt deze service niet over een ingebouwd mechanisme dat de toegang van gebruikers vergemakkelijkt. In dergelijke gevallen, kunnen de plaatsen of de toepassingen verkiezen om hun eigen alternatieve verpletterende oplossingen uit te voeren om inhoudslevering te handhaven.

## Toegang tot degraderings-API {#degradation-api-access}

Alvorens tot de [&#x200B; Vermindering API &#x200B;](#degradation-api) toegang te hebben, moet u de vereiste stappen in het Dynamische proces van de Registratie van de Cliënt voltooien (DCR). Dit verplichte proces zorgt ervoor dat u over het benodigde toegangstoken beschikt voor interactie met de Degradation API.

Voor uitvoerige instructies, verwijs naar het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

## Verslechterings-API {#degradation-api}

De afbraakAPI is een RESTful API die programmeurs toestaat om afbraakregels voor specifieke MVPDs te beheren. De API biedt de middelen om de status voor de degradatieregels die actief zijn, te activeren, te verwijderen en op te halen.

Meer over de Verslechterings API leren, verwijs naar het volgende document van Zendesk [&#x200B; de Authentificatie van Adobe Pass | Degradatie API v3 &#x200B;](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) en zoek het PDF-bestand dat u wilt downloaden.

## REST API V2 {#rest-api-v2}

Leveraging de eigenschap van de Afbraak vereist het uitvoeren van codeupdates om te wijzigen hoe uw toepassing van TV overal (TVE) met de Authentificatie van Adobe Pass [&#x200B; REST API V2 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in wisselwerking staat.

Voor een uitvoerige gids over deze updates en de bijbehorende werkschema&#39;s, verwijs naar de [&#x200B; Verminderde documentatie van de Stroom van de Toegang &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
