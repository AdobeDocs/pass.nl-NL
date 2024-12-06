---
title: Beveiligde bronnen identificeren
description: Beveiligde bronnen identificeren
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Beveiligde bronnen identificeren {#identifying-protected-resources}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

Elk vergunningsverzoek (of verzoek om verificatie) moet een unieke identificatie bevatten voor de beschermde bron waarvoor de gebruiker toegang aanvraagt. Een beschermde bron kan elk niveau van toegestane inhoud zijn, zoals overeengekomen tussen een MVPD en deelnemende programmeurs. Potentiële beschermde hulpbronnen moeten passen in deze boomstructuur van steeds meer specifieke granulariteit:

- Netwerk
   - Kanaal
      - Tonen
         - Episode
            - Element

</br>

## RSS-indeling van media {#media_rss}

De middelen kunnen door een eenvoudig koord (uniek herkenningsteken voor een kanaal) worden geïdentificeerd, of kunnen in formaat van Media worden vertegenwoordigd RSS (MRSS), zoals overeengekomen tussen Adobe (of een erkende partner van de Authentificatie van Adobe Pass) en deelnemende MVPDs en Programmers. De RSS-tekenreeks die als resource specifier wordt gebruikt, kan aanvullende informatie bevatten, zoals classificaties en metagegevens voor ouderlijk toezicht.


Als u een eenvoudige middelherkenningsteken, zoals &quot;TNT&quot;gebruikt, wordt het verondersteld om een kanaal te vertegenwoordigen, en in dit RSS middelspecifier vertaald:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


Een complexere specifier kan bijvoorbeeld aanvullende ratinginformatie bevatten. U kunt het volledige RSS koord tot de functies van de Toegankelijkheid overgaan die een middelidentiteitskaart, zoals [`getAuthorization()`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md) vereisen:

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Specifiers van het middel zijn ondoorzichtig aan de Authentificatie van Adobe Pass; zij worden eenvoudig overgegaan tot MVPD. Als MVPD uw middelspecifier niet herkent of niet kan ontleden, keert het een fout aan de Authentificatie van Adobe Pass terug, die de fout terug naar uw `tokenRequestFailed()` callback overgaat.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
