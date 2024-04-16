---
title: Deelvensters Gegevens op het dashboard
description: Het dashboard helpt de instanties van het delen van wachtwoorden te identificeren door een brede serie van abonneegegevens te analyseren.
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# Deelvensters Gegevens op het dashboard {#data-panels}

Zodra u een segment en tijdinterval hebt geselecteerd, toont het dashboard diverse gegevenspanelen, lijsten, en grafieken die op een high-level mening van het delen van activiteit binnen het geselecteerde segment wijzen.

De onderstaande tabel geeft een overzicht van de beschikbaarheid en verschillen tussen de gegevensdeelvensters in de verschillende [versies](/help/accountiq/versions-aiq.md) van account-IQ:

| Deelvensters Gegevens | D2C-diensten | TVE-programma&#39;s | TVE MVPD&#39;s |
|---|---|---|---|
| [Gemiddelde score voor delen geaggregeerd voor het huidige segment](#aggregated-sharing) | Beschikbaar en consistent | Beschikbaar en consistent | Beschikbaar en consistent |
| [Videocategorieën in segment](#video-categories-segment) | Beschikbaar met kleine variaties | Beschikbaar met kleine variaties | Beschikbaar met kleine variaties |
| [Muziek delen via kanalen en MVPD&#39;s](#sharin-score-by-channels-and-mvpds) | Niet beschikbaar | Beschikbaar | Niet beschikbaar |
| [Rekeningen met waarschijnlijkheid](#accounts-sharing-probability) | Beschikbaar en consistent | Beschikbaar en consistent | Beschikbaar en consistent |
| [Aantal rekeningen en gebruik door kansniveau te delen](#number-of-accounts-usage-sharing-probability) | Beschikbaar en consistent | Beschikbaar en consistent | Beschikbaar en consistent |


## Gemiddelde score voor delen geaggregeerd voor het huidige segment {#aggregated-sharing}

Het gemiddelde het delen score paneel verstrekt een top-line informatie die de hoeveelheid en de invloed van het delen in termen van rekeningen en het stromen volume samenvat.

Met behulp van de meetgegevens kunt u de grootte (variërend van laag, gemiddeld, hoog tot abnormaal) begrijpen van het delen van referenties door uw abonnees, uitgedrukt in accounts en verbruik.

![](assets/aggregate-sharing-score.png)


*Gemiddelde deelscore paneelsgewijs voor het huidige segment*

>[!NOTE]
>
> De blauwe indicator in de **Gemiddelde score voor delen geaggregeerd voor het huidige segment** voor D2C-diensten verschillende doeleinden dienen dan voor tv-diensten overal. Voor D2C-diensten vertegenwoordigt het **Service Gemiddelde Index** zoals weergegeven in de vorige afbeelding. Als u login als Programmer of MVPD, verandert dit etiket in **Gemiddelde industrie-index**.

De volgende meetgegevens zijn componenten van het deelvenster Gemiddelde score voor delen.

### Niveau voor delen {#sharing-level}

Het delende niveauprofiel toont het percentage van alle gedeelde abonneerekeningen binnen het bepaalde segment tijdens het geselecteerde tijdinterval.

Het percentage wordt berekend op basis van het gemiddelde van de waarschijnlijkheid van deelneming berekend voor elke rekening in het segment. Deze berekening omvat rekeningen die minstens eens tijdens het geselecteerde tijdinterval hebben gestroomd.

De indicator van de Trend toont de percentageverandering in de waarde van metrisch van het vorige tijdinterval.

![](assets/sharing-level.png){width="350" align="left"}


*Niveau voor delen*

### Gebruik van gedeelde accounts {#usage-from-shared-accounts}

Het cijfer wijst op het percentage van gebruik door de gedeelde rekeningen onder alle abonneerekeningen voor het bepaalde segment en de tijdspanne. Deze waaiers, genoemd Laag, Normaal, Hoog, en Abnormal, zijn gebaseerd op de industriegemiddelden.

De Trend-indicator, die een toename of afname in het gebruik van gedeelde accounts weergeeft in vergelijking met het vorige tijdinterval.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*Gebruik van gedeelde accounts*

### Algehele score voor delen {#overall-sharing-score}

De algemene score voor delen is een combinatie van het delen van scores, waaronder &quot;Niveau delen&quot; en &quot;Gebruik van gedeelde accounts&quot;.

Het biedt een score die de algemene impact van delen weerspiegelt. Het doel is vergelijkbaar met dat van een creditscore, waarbij het niveau voor delen wordt samengevat met één getal. Maar in dit geval geeft een hogere score een groter niveau van delen aan.

![](assets/overall-sharing-score.png){width="350" align="left"}


*Algehele score voor delen*

## Videocategorieën in segment {#video-categories-segment}

U kunt de kolomkoppen selecteren om de gegevens in alle versies van Account IQ te sorteren.

+++D2C diensten: Gebieden in segment

Als u zich aanmeldt als D2C-service, **Gebieden in segment** de tabel biedt een vergelijkende weergave van de verschillende geaggregeerde delingscores voor de [videocategorieën](/help/accountiq/product-concepts.md#video-category-def) in het huidige segment.

![](assets/sharing-scores-by-regions-in-segment.png)

*Score delen op regio&#39;s in segment*

>[!NOTE]
>
> De [videocategorieën](product-concepts.md#video-category-def)  in de vorige afbeelding, zoals **Regio&#39;s** in segment is slechts een voorbeeld. Wanneer u zich aanmeldt bij Account IQ, wordt in dit deelvenster de specifieke videocategorie van uw bedrijf weergegeven.

Selecteren **Exporteren** om de gegevens in een CSV-bestand te downloaden. Meer informatie [hoe u rapporten van het deelvenster Gegevens exporteert](/help/accountiq/export-reports.md).

+++

+++Programmers: MVPD&#39;s in segment

Wanneer u login als Programmer, **MVPD&#39;s in segment** De tabel biedt een vergelijkende weergave van de verschillende geaggregeerde delingsscores voor de MVPD&#39;s in het huidige segment.

![](assets/sharing-scores-by-mvpds-in-segment.png)

Selecteren **Exporteren** om de gegevens in een CSV-bestand te downloaden. Meer informatie [hoe u rapporten van het deelvenster Gegevens exporteert](/help/accountiq/export-reports.md).

+++

+++MVPDs: Programmers in segment

Wanneer u login als MVPD, **Programmeurs in segment** De tabel biedt een vergelijkende weergave van de verschillende geaggregeerde delingsscores voor de programmeurs in het huidige segment.

Selecteer de kolomkoppen om de gegevens te sorteren.

![](assets/sharing-scores-by-programmers-in-segment.png)

*Score delen door programmeurs in segment*

Selecteren **Exporteren** om de gegevens in een CSV-bestand te downloaden. Meer informatie [hoe u rapporten van het deelvenster Gegevens exporteert](/help/accountiq/export-reports.md).

+++

## Muziek delen via kanalen en MVPD&#39;s  {#sharin-score-by-channels-and-mvpds}

Wanneer u login als Programmer, deze lijst verstrekt een vergelijkende mening van het delen van scores van de geselecteerde kanalen voor MVPDs in het huidige segment.

Selecteer de kolomkoppen om de gegevens te sorteren.

![](assets/sharing-scores-by-channels-mvpds.png)


*Muziek delen via kanalen en MVPD&#39;s*

## Rekeningen met waarschijnlijkheid {#accounts-sharing-probability}

In dit diagram worden de accounts onderverdeeld in reeksen van kanskwintielen, variërend van zeer laag (0-20%) tot zeer hoog (80-100%). Meer informatie over de bereiken van [Rekeningskans voor delen](#accounts-sharing-probability).

>[!NOTE]
>
>De staafgrafiek gebruikt een logaritmische schaal.


![](assets/dashboard-ac-sharing-prob.png)


*Aantal en percentage abonneerekeningen in verschillende waarschijnlijkheidsbereiken bij delen*


## Aantal rekeningen en gebruik door kansniveau te delen {#number-of-accounts-usage-sharing-probability}

Dit paneel biedt een tabelweergave van accounts die zijn opgedeeld in reeksen van kanskwintielen, variërend van zeer laag (0-20%) tot zeer hoog (80-100%), waarbij elke kwintiel gebruik maakt van gedeelde accounts. Meer informatie over de bereiken van [Rekeningskans voor delen](#accounts-sharing-probability).

![](assets/no-acc-usage-prob-level.png)

*Aantal rekeningen, tendensen, en gebruik die in diverse kansbereiken vallen*

Selecteren **Exporteren** om de gegevens in een CSV-bestand te downloaden. Meer informatie [hoe u rapporten van het deelvenster Gegevens exporteert](/help/accountiq/export-reports.md).

