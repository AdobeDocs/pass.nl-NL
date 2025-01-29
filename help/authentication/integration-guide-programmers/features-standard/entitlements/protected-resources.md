---
title: Beveiligde bronnen
description: Beveiligde bronnen
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Beveiligde bronnen {#protected-resources}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Beveiligde bronnen vertegenwoordigen inhoud die gebruikers kunnen streamen en worden geïdentificeerd door unieke waarden die zijn vastgelegd in overeenkomsten tussen MVPD&#39;s en deelnemende programmeurs.

De beschermde middelen volgen een hiërarchische boomstructuur, met elk niveau dat grotere granulariteit voor inhoudsvergunning verstrekt:

* Netwerk
   * Kanaal
      * Tonen
         * Episode
            * Element

## Id&#39;s van beveiligde bronnen {#identifiers}

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

## REST API V2 {#rest-api-v2}

De Adobe Pass-verificatie-API&#39;s die beslissingen vóór autorisatie en autorisatie ophalen, vereisen dat de clienttoepassing de id&#39;s van beveiligde bronnen als parameters opneemt.

De voorafgaande autorisatie- en autorisatiebeslissingen kunnen worden opgehaald met behulp van de volgende API&#39;s:

* [Besluiten vóór toelating met specifieke mvpd ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Autorisatiebesluiten ophalen met specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Verwijs naar het **Verzoek** en **Steekproeven** secties van bovengenoemde APIs om het vereiste formaat te begrijpen voor het verstrekken van unieke herkenningstekens van beschermde middelen.

De unieke id&#39;s zijn ondoorzichtig voor Adobe Pass-verificatie, omdat ze rechtstreeks worden doorgegeven aan de MVPD. Als MVPD een middelherkenningsteken niet herkent of ontleedt, keert het een fout aan de Authentificatie van Adobe Pass terug, die dan de fout terug naar de cliënttoepassing gebruikend een [ Verbeterde Code van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) leest.

Raadpleeg de volgende documenten voor meer informatie over hoe en wanneer u de bovenstaande API&#39;s wilt integreren:

* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
