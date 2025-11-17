---
title: ESM-dashboard
description: Leer hoe u het ESM-dashboard kunt gebruiken om gegevens over machtigingen en gebeurtenissen in MVPD-partners te controleren.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# ESM-dashboard {#esm-dashboard}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Het ESM dashboard verstrekt een verenigde mening van betitelings en gebeurtenisgegevens om u prestaties te controleren, anomalieën te identificeren, en gebruikerstoegangspatronen over de partners van MVPD te begrijpen. Deze gids verklaart hoe te om de filters van het dashboard te gebruiken, rapporten te interpreteren, en zeer belangrijke metriek over configureerbare tijdintervallen te begrijpen.

![&#x200B; ESM de mening van het Dashboard &#x200B;](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Gebruik hoofdletters {#use-cases}

- Tendensen visualiseren per platform of MVPD
- MVPD-prestaties vergelijken
- Begrijp klantengebruik per toepassing

Meer details over ESM gegevens en gebeurtenissen kunnen bij [&#x200B; het Overzicht van de Controle van de Dienst van het Entitlement &#x200B;](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview) worden gevonden.

## Rapporten {#reports}

De volgende rapporten zijn beschikbaar:

### Aanvragen afspelen {#play-requests}

Hiermee geeft u het aantal afspeelverzoeken voor het geselecteerde tijdinterval weer.

**de stijl van de Grafiek** - lijn.

### Verificatieomzetting {#authentication-conversion}

Toont de verhouding tussen succesvolle authentificatiegebeurtenissen en het totale aantal authentificatiepogingen.

**de stijl van de Grafiek** - horizontale bar

### Conversie van autorisatie {#authorization-conversion}

Toont de verhouding tussen succesvolle authentificatiegebeurtenissen en het totale aantal authentificatiepogingen.

**de stijl van de Grafiek** - horizontale bar

### Vergunningswachttijd {#authorization-latency}

Geeft de gemiddelde latentie (in milliseconden) voor MVPD-reacties weer.

**de stijl van de Grafiek** - lijn.

### MVPD-gebruik {#mvpd-usage}

Toont een vergelijking van het aantal unieke gebruikers per MVPD.

**stijl van de Grafiek** - gebied gestapeld.

### Succesvolle verificatie {#successful-authentications}

Toont het totale aantal succesvolle authentificatie voor het geselecteerde tijdinterval.

**de stijl van de Grafiek** - lijn.

### Goedgekeurde machtigingen {#successful-authorizations}

Hiermee wordt het totale aantal geslaagde autorisaties (MVPD op &#39;Machtiging&#39;) voor het geselecteerde tijdsinterval weergegeven.

**de stijl van de Grafiek** - lijn.

## Handelingen {#actions}

### Weergeven op {#view-by}

Voor elke grafiek is er een vervolgkeuzelijst &#39;Weergeven op&#39; waarmee u precies kunt selecteren welke gegevens u wilt weergeven.

- **Geaggregeerd** - toont algemene gegevens
- **MVPDs / Kanalen / Platforms** - schetst de specifieke geselecteerde filters

### Downloaden {#download}

U kunt de onbewerkte gegevens downloaden:

- **de grafiekgegevens van de Download als CSV** - downloadt gegevens voor een specifieke grafiek
- **Download alle gegevens als CSV** - downloadt alle gegevens ESM over alle grafieken

## Filters {#filters}

Gebruik filters om de dataset te verfijnen en de analyse te concentreren. De volgende filters zijn beschikbaar:

- **Kanaal**: omvat alle beschikbare kanalen (merken)
- **MVPD**: nadruk op één of meerdere leveranciers
- **Platform**: Web, mobiel, aangesloten TV, of apparatenfamilie

Als u een nieuw filter wilt toevoegen, selecteert u de knop Filters toevoegen.

Op de pagina &quot;Gegevenssetfilters&quot; kunt u de benodigde filters slepen en neerzetten.

![&#x200B; de filters van het Dashboard van ESM &#x200B;](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Voor elke sectie kunt u filters afzonderlijk verwijderen of de volledige selectie wissen.

### Knopinfo filteren {#filter-tips}

- Meerdere filters combineren om een cohort te isoleren (bijvoorbeeld één MVPD op een mobiel platform voor een kanaal).
- Voeg geen filters toe om uit te zoomen en een basislijn te bepalen voordat u gaat inboren.

## Tijdintervallen {#time-intervals}

Hiermee bepaalt u het venster en de granulariteit van de analyse.

![&#x200B; de tijdintervallen van het Dashboard van ESM &#x200B;](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Datumbereik {#date-range}

**vooraf instelt**: Vandaag, Huidige week, Afgelopen 7 dagen, Huidige maand, Afgelopen 30 dagen, Afgelopen 3 maanden, Afgelopen 6 maanden, Afgelopen 12 maanden

**Douane**: Selecteer gewenst tijdinterval

### Korreligheid {#granularity}

Dagelijks/Maandelijks
