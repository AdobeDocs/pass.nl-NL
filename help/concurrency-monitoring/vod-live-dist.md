---
title: Hoe wordt onderscheid gemaakt tussen VOD en Levende Inhoud in Gelijktijdige Controle
description: Hoe wordt onderscheid gemaakt tussen VOD en Levende Inhoud in Gelijktijdige Controle
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Hoe kan ik: maak onderscheid tussen VOD en Levende Inhoud in Gelijktijdige Controle {#dist-vod-live}

**V:** Kan de dienst van de Controle van de Gelijktijdige tussen het type van inhoud onderscheiden die wordt gespeeld (Levende inhoud versus Video op bestelling)?



**A:** Gelijktijdige bewaking kan geen rechtstreeks onderscheid maken tussen live-inhoud en video op aanvraag (VOD). De videospeler moet weten welk type inhoud wordt afgespeeld en moet deze informatie verzenden tijdens het [sessie-initialisatieaanroep](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (vereist voor Gelijktijdige bewaking). De normale workflow ziet er als volgt uit:

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
