---
title: Algemene gebruiksrapporten
description: Meer informatie over algemene gebruiksrapporten
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage] rapporten {#general-usage-reports}

[!UICONTROL Account IQ] rapporten zijn fundamentele analytische hulpmiddelen die u in uw gegevens laten boren om te isoleren [cohorten](/help/accountiq/product-concepts.md#segmet-def), anomalieën te identificeren en inzicht in uw accountkenmerken te krijgen.

[!UICONTROL General usage] De rapportpagina verstrekt hulpmiddelen om subgroepmetriek uit te splitsen die op het aantal gebruikte rekeningsapparaten, ontdekt IPs, en hun respectieve postcodes worden gebaseerd.

De rapporten zijn allemaal gebaseerd op het huidige segment dat is geselecteerd in het [Segmenten en tijdsinterval](/help/accountiq/segments-timeinterval.md) deelvenster. U kunt uw selectie verfijnen en deze verder verkleinen door drempelwaarden (aantal apparaten, aantal IP&#39;s en aantal postcodes) op te geven in het dialoogvenster [Overzicht-accounts van momentopnamen boven drempelwaarden](#snapshot-overview) deelvenster.

## Aanvragen en unieke abonnees afspelen {#playreq-uniquesubs}

De lijngrafieken hier geeft u een mening van de veranderingen in tijd van waarden, zoals de Verzoeken van het Spel en de Unieke Abonnees in een geselecteerd tijdinterval voor het bepaalde segment.

+++ D2C-services: Aanvragen afspelen/Unieke abonnees

![](assets/d2c-line-graph-gu.png)


*Aanvragen/unieke abonnees voor D2C-services afspelen*

+++

+++Programmers: Verzoeken afspelen/unieke abonnees

![](assets/progr-line-graph-gu.png)


*Aanvragen/unieke abonnees afspelen voor programmeurs*

+++

+++MVPDs: Unieke Abonnees

![](assets/mvpd-line-graph-gu.png)

*Unieke abonnees voor MVPD&#39;s*

+++

<br/>

De x-as vertegenwoordigt de tijd die op het huidige interval wordt gebaseerd en de y-as vertegenwoordigt basisgegevens van de abonneeactiviteit tijdens die periode. De lijngrafieken helpen u activiteit van de abonnees in het huidige segment visualiseren en vergelijken. Afhankelijk van de versie van Account IQ bevatten de meetgegevens:

* **AuthN OK**: Aantal geslaagde verificaties. Meer informatie over [AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**: Aantal geslaagde toelatingen. Meer informatie over [AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Verzoeken afspelen**: Aantal afspeelverzoeken. Meer informatie over [Aanvragen afspelen](/help/accountiq/product-concepts.md#play-requests-def).

* **Unieke abonnees**: Aantal geslaagde unieke abonnees. Meer informatie over [Unieke abonnees](/help/accountiq/product-concepts.md#unique-subscriber-def).

>[!NOTE]
>
>De beschikbaarheid van maateenheden is afhankelijk van de versie van Account IQ.

## Overzicht van momentopnamen rekeningen boven drempelwaarden {#snapshot-overview}

Stel uw analyses en rapporten nauwkeurig af met dit extra filter om verschillende gebruiksdrempels in te stellen. Nadat u een segment hebt geselecteerd, kunt u ook de volgende filters gebruiken om het gedrag van de abonnee verder te analyseren:

* Drempel aantal apparaten

* Aantal IPs Drempel

* Drempel voor aantal postcodes

Als u drempelwaarden bijwerkt in [Accounts Segment based on selected threshold](#account-segments-basedon-segments) geeft u het effect weer in:

* [Apparaten per week (of maand) per account](#devices-week-account)

* [Locaties per week (of maand) per account](#locations-week-account)

* [IPs per week (of maand) per rekening](#ip-week-account)

* [Historische weergave van het segment rekeningen](#account-segment-historical-view)

>[!NOTE]
>
>Elke drempel wordt ingesteld op een standaardwaarde van 4. Wat betekent, toont de Algemene pagina van het Gebruik analyse voor abonnees die meer dan vier apparaten gebruiken, die inhoud van meer dan vier verschillende IP adressen verbruiken, *en* meer dan vier verschillende postcodes.

### Accounts op basis van geselecteerde drempels {#account-segments-basedon-segments}

De **Accounts Segment based on selected threshold** biedt u opties om drempelwaarden (tussen 1 en 10) in te stellen voor het aantal apparaten, het aantal IP&#39;s en het aantal ZIP-codes.

In de grafiek ziet u het volgende:

* Absoluut aantal abonneeaccounts.

* Percentage van de totale abonneerekeningen in het segment die het aantal apparaten, van het aantal IPs, in het aantal codes van het PIT gebruiken zoals die door de drempels worden gespecificeerd.

![](assets/select-thresholds.png)

## Apparaten per week (of maand) per account {#devices-week-account}

Deze staafgrafiek biedt inzicht in het gebruiksgedrag in termen van hoe de abonnees hun apparaten gebruiken om toegang te krijgen tot inhoud.

Het aantal accounts van de x-asplots en het aantal apparaten van de y-as. Gebaseerd op de drempel u voor aantal apparaten per rekening plaatst, merkt het het absolute aantal abonneerekeningen die inhoud van een specifiek aantal apparaten in de duur van een week verbruiken.

![](assets/bar-gr-devices-w-acc.png)

Wanneer het bedekken over een bar (specifiek voor aantal apparaten), verschijnt een etiket dat informatie over het aantal abonneerekeningen (en het percentage van totale abonneerekeningen in het segment) geeft die kanaalinhoud stromen gebruikend die vele apparaten in een week.

De grafiek geeft ook het volgende aan:

* Een rode lijn om de ingestelde drempel te markeren.

* Een groene lijn om het gemiddelde van het aantal verschillende apparaten te merken die door een abonneerekening per week (of maand) worden gebruikt.

De donut verstrekt een afwisselende mening van de apparaten die door rekeningen in het huidige segment boven de vastgestelde drempel worden gebruikt.

![](assets/donut-devices-w-acc.png)

## Locaties per week (of maand) per account {#locations-week-account}

Vergelijkbaar met de metrische waarde voor [Apparaten per week (of maand) per account](#devices-week-account)Met Locaties per week (of maand) per Account kunt u het gebruik van de abonneeaccount op verschillende locaties analyseren. Het x-as palet Aantal Rekeningen, en de y-asPunten Aantal Plaatsen.

![](assets/graph-loc-week-acc.png)

Als u de drempel voor het aantal locaties hebt ingesteld, kunt u de grafiek gebruiken om het volgende te identificeren:

* Aantal (en percentage) abonnees die inhoud van (a specifiek) x aantal plaatsen in een week verbruiken.

* Percentage van totale abonneerekeningen die inhoud van meer plaatsen dan de drempel bekijken.

* Vergelijk het wekelijkse gemiddelde (aantal verschillende locaties voor een account) met de drempel.

## Ips per week (of maand) per account {#ip-week-account}

Vergelijkbaar met de metrische waarde voor **Aantal locaties per week per account** de **Aantal IP&#39;s per week per account** Metrisch laat u de hoeveelheid verandering bij de bron van het stromen voor de huidige plaatsing evalueren.

Het x-as plots Aantal Rekeningen, en het y-asPunten Aantal IPs.

![](assets/graph-ip-week-acc.png)

Zodra u een segment hebt bepaald en de drempel voor het aantal IPs plaatst, kunt u de grafiek gebruiken om het volgende te identificeren:

* Aantal (en percentage) abonnees die inhoud van een specifiek aantal IP in een week verbruiken.

* Percentage van totale abonneerekeningen die inhoud van meer IP adressen dan de drempel bekijken.

* Vergelijk het wekelijkse gemiddelde (aantal verschillende IPs voor een rekening) met de drempel.

## Accounts-segment-historische weergave {#account-segment-historical-view}

De grafiek van de bar van de Historische Mening helpt u de gebruiksmetriek over verschillende tijdintervallen vergelijken. Bovendien worden de verschillende gebruiksmetriek, zoals [Apparaten per week (of maand) per account](#devices-week-account), [Locaties per week (of maand) per account](#locations-week-account), en [IPs per week (of maand) per rekening](#ip-week-account).

* De x-as zet het tijdinterval in kaart, en de y-as geeft het aantal van abonneerekeningen, apparaten, plaatsen en IPs.

* De oranje gekleurde balken geven segmenten aan in verschillende tijdsintervallen.

* De lijngrafiek geeft een overzicht van de wijzigingen in [Apparaten per week (of maand) per account](#devices-week-account), [Locaties per week (of maand) per account](#locations-week-account), en [IPs per week (of maand) per rekening](#ip-week-account) waarden over het tijdinterval die op de drempel worden gebaseerd.

![](assets/historical-view.png)

* De blauwe balken geven het totale aantal actieve abonnees in de hele branche aan voor een tijdsinterval.

* U kunt specifieke legenda selecteren en deze helpen u de grafiek te schalen.

![](assets/historical-view-total.png)
