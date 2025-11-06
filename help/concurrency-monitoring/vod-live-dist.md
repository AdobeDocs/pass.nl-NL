---
title: Hoe onderscheid kan worden gemaakt tussen VOD en Live Content in Concurrency Monitoring
description: Hoe onderscheid kan worden gemaakt tussen VOD en Live Content in Concurrency Monitoring
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Hoe te: Distinguished between VOD and Live Content in Concurrency Monitoring {#dist-vod-live}

**Q:** Kan de dienst van de Controle van de Valuta tussen het type van inhoud onderscheiden die (Levende inhoud versus Video op bestelling) wordt gespeeld?



**A:** de Controle van de Gelijktijdige kan niet direct tussen levende inhoud en Video op bestelling (VOD) onderscheiden. De videospeler moet het type van inhoud kennen het speelt en deze informatie tijdens de [ vraag van de zittingsinitialisering ](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (die voor Gelijktijdige Controle wordt vereist) verzenden. De normale workflow ziet er als volgt uit:

1. De klanten van de Controle van de Gelijktijdige bepalen een reeks meta-gegevens die zij op (b.v. content-type=live|vod, apparaat-type=mobile|console|Desktop) zouden willen uitvoeren.
1. Het team van de Controle van de Valuta voert het gewenste beleid uit. Voorbeeld:
   1. als content-type=live, max streams=3, laatste wins
   1. als content-type=vod, max streams=1, laatste wins

1. Wanneer de eindgebruiker de inhoud speelt, moet de videospeler waarden voor die meta-gegevensgebieden verzenden die als deel van een beleid werden gevestigd.

1. De dienst van de Controle van de Valuta, die op het bepaalde beleid en ontvangen waarden wordt gebaseerd, zal een besluit (spel/einde) uitgeven.

1. Het systeem werkt alleen als de videospeler de beslissing heeft opgevolgd.



## Gerelateerde informatie {#related-info-vod-live-dist}

* [Kenmerken van standaardmetagegevens voor gelijktijdige bewaking](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Overzicht van API voor gelijktijdige bewaking](/help/concurrency-monitoring/cm-api-overview.md)
