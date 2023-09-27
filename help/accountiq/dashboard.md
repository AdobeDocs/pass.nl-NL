---
title: Account IQ-dashboard
description: Het dashboard helpt de instanties van het delen van wachtwoorden te identificeren door een brede serie van abonneegegevens te analyseren.
exl-id: 616da2a5-c9fe-40ea-90cf-f565bc13e764
source-git-commit: ffc7caa9a857a60380d15f69abf346718de6fa4c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---

# Het dashboard {#dashboard}

Het dashboard vat gegevens samen en voegt deze samen in een verzameling grafieken en rapporten die zijn ontworpen om een uitgebreid overzicht te geven van de reikwijdte en de impact van het delen van accounts. Het verstrekt één enkele pagina die de belangrijkste rapporten en metriek van bevat [!UICONTROL Account IQ].


+++Programmeringsdashboard

![dashboard van [!UICONTROL Account IQ] voor programmeergebruikers](assets/dashboard-programr.png)


Afbeelding: Het dashboard voor programmeergebruikers

+++

+++MVPD- dashboard

Het dashboard voor gebruikers MVPD is lichtjes verschillend van die van de programmeurs gebruikers.

![dashboard van [!UICONTROL Account IQ] voor programmeergebruikers](assets/dashboard-mvpd.png)

Afbeelding: Het dashboard voor MVPD-gebruikers

+++

## Gemiddelde score voor delen - samengevoegd voor het huidige segment {#aggregated-sharing}

In het deelvenster Geaggregeerde delende score vindt u een overzicht van de hoeveelheid en impact van het delen van accounts en het streamingvolume.

De waarden helpen u de grootte van credentiedelen door uw abonnees begrijpen, vandaar die een maatregel van de behoefte verstrekken om op het te handelen.

![](assets/aggregate-sharing-score.png)


*Afbeelding: Deelvenster Gemiddelde score voor delen - samengevoegd voor het huidige segment*

De volgende drie metriek zijn componenten van de Gemiddelde het Delen Score.

### Niveau voor delen {#sharing-level}

De het delen niveaumaat toont het percentage van al uw abonneerekeningen (in het bepaalde segment) die, tijdens het geselecteerde tijdkader worden gedeeld.

Een waarde die wordt berekend op basis van het gemiddelde van de waarschijnlijkheid van delen die is berekend voor elke account in de set geselecteerde MVPD&#39;s die tijdens het geselecteerde tijdframe vanuit een van de geselecteerde programmeerkanalen is gestreamd.

![](assets/sharing-level.png)


*Afbeelding: Niveau voor delen*

De indicator van de Trend toont de percentageverandering in de waarde van metrisch binnen van het vorige tijdkader.

### Gebruik van gedeelde accounts {#usage-from-shared-accounts}

Dit cijfer wijst op welk percentage van het gebruik van alle abonneerekeningen van de gedeelde rekeningen voor het bepaalde segment en de tijdspanne is. De maatstaf geeft de gebruikscategorieën (van gedeelde accounts) aan op een schaal van 0 tot 100%. Deze waaiers-genoemd Laag, Normaal, Hoog, en Abnormal-zijn gebaseerd op het de industriestandgemiddelde.

U kunt ook de Trend-indicator zien, die een toename of afname in het gebruik van gedeelde accounts in vergelijking met het vorige tijdkader weergeeft.

![](assets/usage-4mshared-accounts.png)


*Afbeelding: Gebruik van gedeelde accounts*

### Algehele score voor delen {#overall-sharing-score}

De algemene score voor delen bestaat uit het delen van scores, waaronder &quot;Niveau delen&quot; en &quot;Gebruik z van gedeelde accounts&quot;.

Het levert een waarde op die de relatieve impact van delen weergeeft in vergelijking met de bedrijfstak. Het doel is vergelijkbaar met dat van een creditscore, waarbij de situatie wordt samengevat met één cijfer. Maar in dit geval, hoe hoger het aantal hoe groter de potentiële schade.

![](assets/overall-sharing-score.png)


*Afbeelding: Algehele score voor delen*

<!--### MVPDs in segment {#mvpd-in-segment}

It is a table of risk indices and accounts totals for the top MVPDs ranked by overall usage or account sharing.

![](assets/mvpds-in-segment.png)-->

## Globale score voor delen voor MVPD&#39;s voor de hele industrie {#top-mvpds}

Deze lijst verstrekt een vergelijkende mening van de verschillende Geaggregeerde het Delen Scores voor MVPDs in het segment.

>[!NOTE]
>
>In deze tabel worden algemene bedrijfsgegevens gebruikt voor vergelijkende doeleinden, niet de gegevens die door die MVPD&#39;s in het segment worden vertegenwoordigd.

![](assets/top-mvpds.png)


*Figuur: Bovenste MVPDs in segment door algemene score*

## Muziek delen via kanalen en MVPD&#39;s {#sharin-score-by-channels-and-mvpds}

Deze tabel biedt een vergelijkende weergave van het delen van scores van de geselecteerde kanalen voor de MVPD&#39;s in het huidige segment.

![](assets/sharing-scores-by-channels-mvpds.png)


*Afbeelding: scores delen via kanalen en MVPD&#39;s*

## Rekeningen met waarschijnlijkheid {#accounts-sharing-probability}

Dit diagram verdeelt de rekeningen in reeksen van het delen kanskwintielen van zeer laag (0-20%) aan zeer hoog (80=100%).

>[!NOTE]
>
>De staafgrafiek gebruikt een logaritmische schaal.


![](assets/dashboard-ac-sharing-prob.png)


*Afbeelding: Aantal en percentage abonneeaccounts in verschillende waarschijnlijkheidsbereiken voor delen*

## Aantal rekeningen en gebruik door kansniveau te delen {#number-of-accounts-usage-sharing-probability}

In dit deelvenster vindt u een tabeloverzicht van de accounts die zijn verdeeld in reeksen waarschijnlijkheidskwintielen van zeer laag (0-20%) tot zeer hoog (80-100%) waarbij elke kwintiel gebruik maakt van gedeelde accounts.

![](assets/no-acc-usage-prob-level.png)


*Figuur: Aantal rekeningen, tendensen, en gebruik die in diverse kansbereiken vallen*

<!--
+++Dashboard for programmers

![dashboard of account IQ](assets/dashboard-capture.png)


*Figure: The dashboard*

>>>>>>> 7ab48cf61552febab21a5d5c05586e0aefe8ce17
## Average sharing score - aggregated for the current segment {#aggregated-sharing}

The Aggregated Sharing Score panel provides a top line readout summarizing the quantity and impact of sharing in terms of accounts and streaming volume.

The values help you understand the magnitude of credential sharing by your subscribers, hence providing a measure of the need to act upon it.

![](assets/aggregate-sharing-score.png)


*Figure: Average sharing score panel - aggregated for the current segment*

The following three metrics are components of the Average Sharing Score.

### Sharing level {#sharing-level}

The sharing level gauge shows the percentage of all your subscriber accounts (in the defined segment) that are shared, during the selected time frame.  

A value calculated based on an average of the sharing probability computed for every account for the selected MVPD(s) that has streamed from a one of the selected programmer channels during the selected time frame.

![](assets/sharing-level.png)


*Figure: Sharing level*

The Trend indicator shows the percentage change in the value of the metric in from the previous time frame.

### Usage from shared accounts {#usage-from-shared-accounts}

This gauge indicates what percent of the usage of all the subscriber accounts is from the shared accounts for the defined segment and time period. The gauge marks the ranges of usage (from shared accounts) on the scale of 0 to 100%. These ranges (named Low, Medium, High, and Abnormal) are based on the industry average.

You can also see the Trend indicator, which depicts a rise or fall in the usage from shared accounts as compared to the previous time frame.

![](assets/usage-4mshared-accounts.png)


*Figure: Usage from shared accounts*

### Overall sharing score {#overall-sharing-score}

Overall sharing score is composite of sharing scores including "Sharing level" and "Usage from shared accounts".

It provides a value meant to reflect the relative impact of sharing when compared to the industry. Its purpose is similar to that of a credit score, summarizing the situation with a single number. But in this case, the higher the number the greater the potential harm.

![](assets/overall-sharing-score.png)


*Figure: Overall sharing score*

## Industrywide overall sharing scores {#mvpd-in-segment}

+++Programmer- MVPDs in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the MVPDs in the segment.

![](assets/mvpds-in-segment.png)


*Figure: Panel showing top MVPDs in a segment*


>[!NOTE]
>
>This table uses overall industry data for comparative purposes, not the data represented by those MVPDs in the segment.

+++

+++MVPD- Programmers in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the programmers in the segment.

![](assets/programmers-in-segment.png)


*Figure: Panel showing top programmers in a segment*

+++


## Sharing score by channels and MVPDs {#sharin-score-by-channels-and-mvpds}

+++Programmer- MVPDs in segment

This table provides a comparative view of sharing scores of the selected channels for the MVPDs in the current segment.

![](assets/sharing-scores-by-channels-mvpds.png)


*Figure: Sharing scores by channels and MVPDs*

>[!NOTE]
>
>**Sharing score by channels and MVPDs** panel is available only for programmer login.

+++

## Accounts sharing probability distribution{#accounts-sharing-probab-dist}

This panel partitions accounts into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%).

Pie chart shows the proportions (in term of percentages) of user accounts in various sharing probability ranges. Whereas, column chart shows the absolute numbers of accounts in different probability ranges.

>[!NOTE]
>
>The column chart uses a logarithmic scale.


![](assets/dashboard-ac-sharing-prob.png)


*Figure: Percentages and number of subscriber accounts in different sharing probability ranges*

### Accounts over threshold in current segment {#acc-over-threshold-in-segment}

You can select a level of sharing probability, out of the following to view number and percentage of accounts above it:

* Over very low (0%-20%) probability

* Over low (20%-40%) probability

* Over moderate (40%-60%) probability

* Over high (60%-80%) probability

## Number of accounts and usage by sharing probability level {#number-of-accounts-usage-sharing-probability}

This panel provides tabular view of  accounts partitioned into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%) with each quintile's associated usage from shared accounts.

![](assets/no-acc-usage-prob-level.png)

*Figure: Number of accounts, trends, and usages falling in various probability ranges*

-->