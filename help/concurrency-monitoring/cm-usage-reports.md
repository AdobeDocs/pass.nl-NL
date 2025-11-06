---
title: Gebruiksrapporten voor gelijktijdige bewaking
description: Gebruiksrapporten voor gelijktijdige bewaking
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: 36da78fd66cfbc86e7bea7575c757fef536c0755
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Gebruiksrapporten voor gelijktijdige bewaking {#cm-usage-reports}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.



## Overzicht {#usage-rep-overview}

De **dienst van het Gebruik van de Controle van de Gelijktijdige** is beschikbaar via REST API die insight in het gezamenlijke gebruik zoals die door de toepassingen van de klant wordt gemeld verstrekt.

## Vereisten {#usage-rep-prerequisites}

Om tot het product van de Rapporten van het Gebruik van de Controle van de Gelijktijdige toegang te hebben, moet een klant eerst het Gelijktijdige Controle [&#x200B; Team van de Steun &#x200B;](mailto:tve-support@adobe.com) contacteren en zij zullen de noodzakelijke stappen uitvoeren om u toegang tot het API product te verlenen. Meer details op [&#x200B; CMU API Toegang &#x200B;](/help/concurrency-monitoring/cmu-api-access.md).

## Algemene rapportcijfers en uitsplitsingen {#general-rep-metrics-breakdown}

### Gebruikers van verbruiksrapporten kunnen de volgende meetgegevens controleren:{#monitor-metrics}

| Naam | Beschrijving |
|:---|:---|
| actieve gebruikers | aantal verschillende gebruikersaccounts die tijdens het interval ten minste één streamingsessie hebben gestart, uitgesplitst naar tijdsgranulariteit |
| actieve sessies | aantal het stromen zittingen die activiteit tijdens het interval hebben gemeld (het omvat geen ontbroken pogingen, zittingen die ontkennen antwoord voor de initialisatievraag hebben ontvangen) |
| aan de slag-sessies | aantal streamingsessies die binnen het interval zijn gestart |
| voltooide sessies | aantal streamingsessies die tijdens het interval zijn voltooid (expliciet of vanwege time-out) |
| mislukte pogingen | aantal pogingen van de zittingsinitialisering die ontkennen antwoord ontkennen |
| ontslagen sessies | aantal streamingsessies dat tijdens het afspelen (op hartslag) is voltooid wegens een beleidsovertreding. Deze metrische waarde is alleen relevant wanneer een FIFO- of last_one_wins-beleid is ingesteld. |
| doodssessies | aantal streamingsessies die expliciet zijn gedood bij het starten van een nieuwe sessie (via de X-Terminate-header) |
| duration_0-15 | aantal stromen met een duur tussen 0 en 15 minuten |
| duration_15-30 | aantal stromen met een duur tussen 15 en 30 minuten |
| duration_30-60 | aantal stromen met een duur tussen 30 en 60 minuten |
| duration_60-120 | aantal stromen met een duur tussen 60 en 120 minuten |
| duration_2h-4h | aantal streams met een duur tussen 2 uur (120 minuten) en 4 uur |
| duration_4h-8h | aantal stromen met een duur tussen 4 uur en 8 uur |
| duration_8h-16h | aantal stromen met een duur tussen 8 uur en 16 uur |
| duration_16h-1d | aantal stromen met een duur tussen 16 uur en 1 dag (24 uur) |
| duration_1d-3d | aantal stromen met een duur tussen 1 en 3 dagen |
| duration_3d-7d | aantal stromen met een duur tussen 3 dagen en 7 dagen |
| duration_1w-1m | aantal stromen met een duur tussen 1 week (7 dagen) en 1 maand (30 dagen) |
| duration_over-1m | aantal stromen met een duur langer dan 1 maand (30 dagen) |

### De gebruikers van de Rapporten van het gebruik kunnen de hierboven vermelde metriek door de volgende afmetingen filtreren: {#dimensions-2-filter-metrics}

| Dimension-naam | Beschrijving |
|:---------------|:------------------------------------------------------------------------------------------------------------------|
| jaar | Jaar met 4 cijfers |
| maand | De maand van het jaar (1-12) |
| dag | Dag van de maand (1-31) |
| uur | Het uur van de dag |
| minuut | De minuut van het uur [^1 ] |
| toepassing | De toepassingsnaam die is geregistreerd in Gelijktijdige bewaking die wordt gebruikt voor het beheren van sessies |
| application-id | De toepassings-id die is geregistreerd in Gelijktijdige bewaking die wordt gebruikt voor het beheren van sessies |
| kanaal | De kanaalmetagegevens die tijdens de initialisatie van de sessie worden verzonden (gemarkeerd als Onbekend als er geen metagegevens zijn verzonden) |
| mvpd | De MVPD die bij sessiebeheer wordt geleverd |
| platform | De platformmeta-gegevens die bij zittingsinitialisering of vooraf bepaald voor een toepassing op configuratieniveau worden verstrekt |

## Gelijktijdige rapportage Metriek en uitsplitsingen {#concurrency-reports-metrics-breakdown}

Beginnend met Concurrency Controle versie 2.9.0 hebben wij een nieuw rapport voor begrip gelijktijdig gebruik geïntroduceerd: een histogram voor **gelijktijdig-niveau** en **activiteit-niveau**.

Het belangrijkste doel van dit verslag is om u te helpen begrijpen wat het effect is van het vaststellen van een beleid met een bepaalde limiet van gelijktijdige uitvoering en om u voldoende informatie te geven om te beslissen of u de limiet moet verhogen.

### Gebruikers van verbruiksrapporten kunnen de volgende meetgegevens controleren: {#metrics-usage-rep-users}

| Dimension-naam | Beschrijving |
|:---|:---|
| gebruikers | Aantal gebruikers dat elk gelijktijdig/activiteitsniveau heeft bereikt |

### De gebruikers van de Rapporten van het gebruik kunnen de hierboven vermelde metriek door de volgende afmetingen filtreren: {#dimensions-to-filter-metrics}

| Dimension-naam | Beschrijving |
|:---|:---|
| jaar | Jaar met 4 cijfers |
| maand | De maand van het jaar (1-12) |
| dag | Dag van de maand (1-31) |
| gelijktijdig | Vertegenwoordigt om het even welke duidelijke **stroomactiviteit die bij de fase van de zittingsinitialisering** voor een gebruiker werd goedgekeurd om te kunnen waarnemen hoeveel gezamenlijke stromen **&#x200B;**&#x200B;door een gebruiker werden geopend en om het effect te begrijpen van het toepassen van een bepaalde gelijktijdig grens |
| activiteitsniveau | Vertegenwoordigt om het even welke duidelijke **stroomactiviteit (geen kwestie van zijn staat: begonnen, actief, tegengehouden, verworpen)** voor een gebruiker om te kunnen waarnemen hoeveel gezamenlijke stromen werden geprobeerd om door een gebruiker te worden geopend en het effect te begrijpen van het toepassen van een bepaalde gelijktijdig grens |
| mvpd | De MVPD die bij sessiebeheer wordt geleverd |

### Voorbeelden van rapporten

Voor beste gegevensnauwkeurigheid, adviseren wij de rapporten die op deze pagina [&#x200B; worden voorgesteld CMU voorbeelden van rapporten &#x200B;](/help/concurrency-monitoring/cm-usage-reports-examples.md)

[^1 ]: Mindere rapporten zijn niet standaard beschikbaar. Gelieve te contacteren het Gelijktijdige Controle [&#x200B; Team van de Steun &#x200B;](mailto:tve-support@adobe.com) om hen te verzoeken.