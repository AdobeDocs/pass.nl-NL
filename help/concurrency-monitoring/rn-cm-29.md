---
title: Opmerkingen bij de release Adobe Gelijktijdige controle 2.9
description: Opmerkingen bij de release Adobe Gelijktijdige controle 2.9
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Opmerkingen bij de release Gelijktijdige controle 2.9 {#rn-cm29}

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven.

## Releasedatum {#release-date}

03/14/2019


## Overzicht van release {#release-overview}

* Om te beginnen met deze versie hebben we een nieuw rapport geïntroduceerd voor een beter begrip van het gelijktijdige gebruik: een histogram voor gelijktijdig gebruik, met de volgende informatie:

* het aantal gebruikers dat elk gelijktijdig niveau heeft bereikt (d.w.z. hoeveel gebruikers ooit 2 gezamenlijke stromen, 3 gelijktijdige stromen, etc. hebben gehad) tijdens elk korrelige interval
* de totale duur voor elk valutaniveau, in minuten (de gemiddelde waarde kan worden berekend door deze waarde eenvoudig te delen door het bovenstaande aantal)
* het totale aantal keren dat gebruikers elk gelijktijdige niveau hebben bereikt, om het effect van een bepaalde regel in termen van zowel betrokken gebruikers als geaggregeerde gebruikerservaring te schatten Meer details zijn te vinden over de [Gebruiksrapporten](/help/concurrency-monitoring/cm-usage-reports.md) pagina.

We hebben ook de beveiliging van SQL-injecties verbeterd en verschillende oplossingen voor problemen toegevoegd.

## Bekende problemen {#known-issues}

Geen.
