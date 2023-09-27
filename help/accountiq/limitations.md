---
title: Beperkingen en bekende problemen
description: Bekende problemen in het product.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Bekende problemen en beperkingen {#known-issues}

Adobe streeft ernaar robuuste functionaliteit en naadloze gebruikerservaring te bieden via haar aanbod. De huidige versie (versie 1.0) van Account IQ biedt analyses voor het delen van gebruik en abonnementen voor streamingproviders met een hoge mate van vertrouwen. De volgende beperkingen worden echter in toekomstige releaseversies opgelost.

* Bij het definiëren van cohorten in het dashboard of de rapportpagina&#39;s is er momenteel geen optie om metriek toe te voegen, zoals **aantal apparaten** het segment verfijnen. Deze mogelijkheid is beschikbaar in een toekomstige versie.

* Bij het schatten van de delingsscores voor individuele rekeningen, neemt Account IQ een conservatieve benadering die bedrijven in staat stelt om met grote mate van vertrouwen op het delen te handelen. Deze benadering onderschat echter meestal het totale bedrag aan delen wanneer dit over veel rekeningen wordt geaggregeerd.

* De [Algemene score voor delen](/help/accountiq/dashboard.md#overall-sharing-score) momenteel alleen factoren [Niveau delen](/help/accountiq/dashboard.md#sharing-level) en [Gebruik van gedeelde accounts](/help/accountiq/dashboard.md#usage-from-shared-accounts). Toekomstige versies zullen extra metriek beïnvloeden.

* Bij het definiëren van cohorten in het dashboard of de rapportpagina&#39;s ontbreekt het zoekmechanisme momenteel bij de kiezers voor MVPD&#39;s en kanalen.

* Wanneer het bepalen van cohorts in het dashboard of de rapportpagina&#39;s, is er een beperking om slechts tot 10 MVPDs en Programmeurs (of individuele kanalen) te selecteren.

* De mogelijkheid om accountstatistieken uit te voeren is beperkt tot het exporteren van 1000 accounts.

* De optie om te selecteren [Segmenttype](#segment-type) wanneer het definiëren van bewerkingen beperkt is tot **Vast aantal accounts**. De **Variabel aantal rekeningen** deze optie zal in een komende versie beschikbaar zijn .

* De secties Benchmarking, Detection Models, Segments, Snapshots, en Rules in de linkernavigatie zijn momenteel uitgeschakeld en zullen beschikbaar zijn in een komende versie.

* Bij het maken [Bewerkingen](/help/accountiq/operation-affecting-user-segment.md)kunt u slechts twee soorten [Handelingen](/help/accountiq/operation-affecting-user-segment.md) vanaf nu — Gelijktijdige toezichtregels en externe acties.

* Bewerkingen kunnen momenteel alleen worden gemaakt en [gepland](/help/accountiq/operation-affecting-user-segment.md#action). In toekomstige versies kunt u deze bestanden pauzeren, hervatten en volledig beheren.

* Vanwege de beperktere set gegevens die wordt gebruikt, geeft de isolatiemodus niet echt de mate van delen aan. Daarom kan MVPD in de isolatiemodus niet worden vergeleken met een andere MVPD. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* Wanneer u een nieuwe [segment](/help/accountiq/segments-timeframe.md) voor een bewerking kunt u metriek toevoegen. Maar als u een opgeslagen segment selecteert, kunt u niet meer metriek toevoegen om het segment te verfijnen.

* De selector voor granulariteit en tijdframes is beperkt tot één week of één maand, wat betekent dat gegevens slechts op één week of één maand kunnen worden geëvalueerd.

* Vooraf gedefinieerde intervallen zijn momenteel uitgeschakeld in de kiezer voor granulariteit en tijdframes en zijn beschikbaar in een toekomstige versie.
