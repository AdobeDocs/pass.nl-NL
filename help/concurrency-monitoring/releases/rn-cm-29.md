---
title: Opmerkingen bij de release Adobe Concurrency Monitoring 2.9
description: Opmerkingen bij de release Adobe Concurrency Monitoring 2.9
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# Opmerkingen bij de release Gelijktijdige controle 2.9 {#rn-cm29}

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven.

## Releasedatum {#release-date}

14-03-2019


## Overzicht van release {#release-overview}

* Om te beginnen met deze versie hebben we een nieuw rapport geïntroduceerd voor een beter begrip van het gelijktijdige gebruik: een histogram voor gelijktijdig gebruik, met de volgende informatie:

* het aantal gebruikers dat elk gelijktijdig niveau heeft bereikt (d.w.z. hoeveel gebruikers ooit 2 gezamenlijke stromen, 3 gelijktijdige stromen, etc. hebben gehad) tijdens elk korrelige interval
* de totale duur voor elk valutaniveau, in minuten (de gemiddelde waarde kan worden berekend door deze waarde eenvoudig te delen door het bovenstaande aantal)
* het totale aantal keren dat gebruikers elk gelijktijdige niveau hebben bereikt, om het effect van een bepaalde regel in termen van zowel betrokken gebruikers als geaggregeerde gebruikerservaring te schatten
Meer details zijn op de [ pagina van de Rapporten van het Gebruik ](/help/concurrency-monitoring/reports/cm-usage-reports.md).

We hebben ook de beveiliging van SQL-injecties verbeterd en verschillende oplossingen voor problemen toegevoegd.

## Bekende problemen {#known-issues}

Geen.
