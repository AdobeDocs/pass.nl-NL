---
title: Activiteiten in rekening-IQ
description: De verrichtingen in Rekening IQ impliceren acties om automatiserings en bulkverrichtingen op abonneerekeningen uit te voeren en hun gevolgen te volgen.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Bewerkingen {#operations-tab-next-steps}

Nadat u de gebruikspatronen van uw abonnee hebt geanalyseerd en instanties van het delen van wachtwoorden hebt geïdentificeerd voor een geselecteerd segment, gebruikt u [!DNL Account IQ] analyseert, kunt u gerichte acties door gerichte procedures nemen genoemd verrichtingen in [!DNL Account IQ].

**Bewerkingen** zodat u op effectieve wijze het delen van referenties naar een groep accounts kunt bijhouden en beheren om het delen van wachtwoorden te beperken en de ervaring voor getaxeerde abonnees te verbeteren.

U kunt handelingen toepassen op een definitie [segment](/help/accountiq/product-concepts.md#segment-def) om het delen van wachtwoorden binnen een specifieke [tijdsinterval](/help/accountiq/product-concepts.md#time-interval-def) en plant de uit te voeren bewerking op een toekomstige datum. Deze acties omvatten beperkingen om het delen van wachtwoorden te minimaliseren of beperkingen op niet-gedeelde accounts te verlichten.

Met behulp van bewerkingen geeft u niet alleen acties en hun bereik op, maar ook de resultaten ervan.

Door de resultaten te evalueren, kunt u uw strategie verfijnen om gevolgen te optimaliseren, hetzij door leners om te zetten, credentiedelen te verlichten, of het drukken van kurn.

U kunt verschillende functies uitvoeren met bewerkingen:

* [Bewerkingsrapporten weergeven](#operation-reports)
* [Een nieuwe bewerking maken](#create-new-operation)
* [Bewerking stoppen](#stop-operation)

## Bewerkingsrapporten weergeven {#operation-reports}

U kunt de effecten van een bewerking controleren aan de hand van bewerkingsrapporten. Selecteer **Bewerkingen** tab onder **Handelingen** in het linkerdeelvenster van de IQ-toepassing Account. Er wordt een lijst weergegeven met bewerkingen die beschikbaar zijn in het systeem. U kunt de belangrijkste details over elke verrichting in een tabelvorm toegang hebben. De details zijn onder meer:

* Naam van de concrete actie
* Huidige status (zoals Gepland, Lopen, Beëindigd, Fout, of Gestopt)
* Percentage voltooide voortgang
* Doelpubliek of segment waarop de bewerking is toegepast
* Type actie geselecteerd voor de bewerking
* Begindatum van de operatie
* Einddatum van de operatie
* De datum waarop de bewerking is gemaakt
* Laatste wijzigingsdatum van de bewerking

![](assets/operations-page.png)

*Lijst en nadere gegevens van bestaande transacties in IQ van account*

Selecteer het gewenste **Handelingsnaam** in de lijst van concrete acties. De volgende rapporten worden weergegeven:

### Werking {#operation-performance}

De verrichtingsprestaties verstrekken een hoogste lijninformatie die het aantal rekeningen samenvat beïnvloedde, verrichtingsvooruitgang, en de algemene het delen score van de rekeningen in het segment tijdens de verrichting [evaluatieperiode](/help/accountiq/product-concepts.md#evaluation-period-def).

![Operationeel prestatierapport](assets/operation-performance.png)

*Operationeel prestatierapport*

**A.** Betrokken accounts **B.** Bewerking voortgang **C.** Algehele score voor delen

#### Betrokken accounts {#impacted-accounts}

Dit aantal toont het aantal abonneerekeningen die door de actie worden beïnvloed tijdens de de evaluatieperiode van de verrichting wordt genomen.

#### Bewerking voortgang {#operation-progress}

Dit cijfer toont het aantal dagen en het percentage van de uitgevoerde operatie dat volgens het geplande schema is uitgevoerd.

#### Algehele score voor delen {#overall-sharing-score}

Deze lijngrafiek vertegenwoordigt de [algemene score voor delen](/help/accountiq/data-panels.md#overall-sharing-score), dat het niveau van delen en het gebruik van gedeelde rekeningen in elke week tijdens de evaluatieperiode van de operatie omvat.

### Operatie-effect: rekeningen in segment {#impact-accounts}

Dit rapport wordt weergegeven als een gestapelde kolomgrafiek die het effect van een bewerking in de loop der tijd illustreert.

![Operatie-effect op rekeningen in segmentgrafiek](assets/accounts-in-segment.png)

*Operatie-effect op rekeningen in segmentgrafiek*

De x-as vertegenwoordigt de [evaluatieperiode](/help/accountiq/product-concepts.md#evaluation-period-def), terwijl de y-as de status van de accounts in het segment van de bewerking aangeeft. Elke balk in de grafiek bestaat uit drie kleuren:

* Roze staat voor het aantal accounts dat voldoet aan de voorwaarden van het segment die in deze bewerking worden gebruikt.

* Blauw vertegenwoordigt het aantal actieve rekeningen die oorspronkelijk in het segment waren maar niet aan de voorwaarden van het segment tijdens elke week of maand in de verrichting voldeden [evaluatieperiode](/help/accountiq/product-concepts.md#evaluation-period-def).

* Grijs geeft de rekeningen weer die tijdens de evaluatieperiode inactief waren.

>[!NOTE]
>
>De eerste roze balk geeft het aantal rekeningen weer dat aan het begin van de evaluatieperiode aan de voorwaarden van het exploitatiesegment voldoet.

In de loop der tijd toont de grafiek wijzigingen in het accountgedrag ten opzichte van de oorspronkelijke criteria (zo is de kans op delen groter dan 90 en zijn meer dan 5 apparaten inactief geworden).

### Effect van bewerking: metriek van gedeelde accounts {#impact-shared-accounts}

De gegevens van de gedeelde rekeningen verstrekken een overzicht van het delen niveau en spelverzoeken door de abonneerekeningen in het segment van de verrichting tijdens de verrichting [evaluatieperiode](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Niveau voor delen {#share-level}

Deze lijngrafiek vertegenwoordigt de [deelniveau](/help/accountiq/data-panels.md#sharing-level) elke week tijdens de evaluatieperiode van de operatie.

![Lijngrafiek op niveau delen](assets/share-level.png){width="550" align="left"}

*Lijngrafiek op niveau delen*

#### Aantal afspeelverzoeken {#play-requests}

Deze lijngrafiek vertegenwoordigt de [afspeelverzoeken](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) elke week tijdens de evaluatieperiode van de operatie.

![Aantal afspeelaanvragen lijngrafiek](assets/number-play-requests.png){width="550" align="left"}

*Aantal afspeelaanvragen lijngrafiek*

### Effect van de verrichting: algemene gebruiksmetriek {#impact-general-usage}

De algemene gebruiksmetriek verstrekt een overzicht van gemiddeld aantal apparaten, IPs, en plaatsen in het segment van de verrichting tijdens de verrichting [evaluatieperiode](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Aantal apparaten {#devices}

Deze lijngrafiek geeft het gemiddelde weer [aantal apparaten](/help/accountiq/general-usage-reports.md#devices-week-account) elke week tijdens de evaluatieperiode van de operatie.

![Aantal grafiek van apparatenlijn](assets/number-devices.png){width="550" align="left"}

*Aantal grafiek van apparatenlijn*

#### Aantal IPs en plaatsen {#IPs-locations}

Deze lijngrafiek geeft het gemiddelde weer [aantal IPs](/help/accountiq/general-usage-reports.md#ip-week-account) en [locaties](/help/accountiq/general-usage-reports.md#locations-week-account) elke week tijdens de evaluatieperiode van de operatie.

![Aantal IPs en grafiek van de plaatslijn](assets/number-ips-locations.png){width="550" align="left"}

*Aantal IPs en grafiek van de plaatslijn*

Om het rapport te sluiten en terug te gaan naar de hoofdlijn **Bewerkingen** pagina, selecteert u **Bewerkingen** tab onder **Handelingen** in het linkerdeelvenster.

## Nieuwe bewerking maken {#create-new-operation}

Als je naar de **Bewerkingen** tab onder **Handelingen** in het linkerdeelvenster selecteert u **Nieuwe bewerking maken** boven aan het dialoogvenster **Bewerkingen** pagina.

Volg de instructies in de volgende secties om een nieuwe bewerking te maken:

* [Bedrijfsgegevens](#operation-details)
* [Segment](#segment)
* [Handeling](#action)
* [Schema](#schedule)

### Bedrijfsgegevens {#operation-details}

Typ in deze sectie de naam voor de bewerking in **Handelingsnaam**.

>[!TIP]
>
>Beschrijf het doel van de actie of de aard van de actie in **bewerkingsnaam** voor een snelle identificatie. De optie om **Beschrijving en codes toevoegen** zal in toekomstige versies beschikbaar zijn.

![Naam van bewerking toevoegen in bewerkingsdetails](assets/operation-details.png)

*Naam van bewerking toevoegen*

### Segment {#segment}

Klik in deze sectie op **Segment selecteren** en kiest u een segment waarop u deze bewerking wilt toepassen. Meer informatie [hoe te om een segment te selecteren](/help/accountiq/segments-timeinterval.md#segment-selection).

Zodra u een segment hebt geselecteerd, gebruik <img alt= "Overzicht van segment uitvouwen" src="./assets/expand-segment-summary.svg" width="25"> pictogram om de gedetailleerde samenvatting van het segment te bekijken. Meer informatie over [Overzicht van segment](segments-timeinterval.md#segment-summary).

![Segment- en tijdinterval selecteren](assets/select-segment-timeinterval.png)

*Segment- en tijdinterval selecteren*

>[!NOTE]
>
>De [videocategorieën](product-concepts.md#video-category-def) in de vorige afbeelding, zoals **MVPD&#39;s**, **Programmeurs**, en **Kanalen** de labels weergeven die worden gebruikt in de IQ-versie van account op tv overal. Als u als dienst D2C het programma wordt geopend, tonen deze etiketten de specifieke videocategorieën van uw bedrijf.

Indien vereist, gebruik <img alt= "segment bewerken" src="./assets/edit-segment.svg" width="25"> pictogram om het geselecteerde segment te bewerken of  <img alt= "nieuw segment maken" src="./assets/create-new-segment.svg" width="25"> pictogram om een nieuw segment te maken. Raadpleeg voor meer informatie de instructies voor [een nieuw segment maken](work-with-segments.md#create-new-segment) of [een segment bewerken](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Segmenttype** benoemd **[!UICONTROL Fixed number of accounts]** is momenteel standaard geselecteerd. De optie om te selecteren **[!UICONTROL Variable number of accounts]** zijn beschikbaar in komende releases.

Selecteren **Korreligheid en tijdsinterval** gedurende een bepaalde periode toezicht op de verrichting uit te oefenen. Meer informatie over [granulariteit en tijdinterval selecteren](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Handeling {#action}

In deze sectie kiest u een **Handeling** u wilt op het geselecteerde segment van het dropdown menu uitvoeren.

![Selecteer het type actie](assets/apply-actions.png)

*Selecteer het type actie*

Er zijn twee opties beschikbaar:

* Selecteren **CM-beleid** voor het systeem voor gelijktijdige bewaking dat is geïntegreerd met Account IQ.

* Selecteren **Externe acties** om workflows te maken en te verwerken die zich buiten de IQ van de Rekening bevinden en niet met het systeem van IQ van de Rekening zijn geïntegreerd.

>[!NOTE]
>
>Externe maatregelen hebben misschien niet altijd rechtstreeks betrekking op het delen van wachtwoorden, maar kunnen er wel invloed op hebben, zoals het opstarten van een nieuw seizoen.

### Schema {#schedule}

Selecteer in deze sectie de optie **Begindatum** en **Einddatum** van de datumkiezer om de activering voor de bewerking in te stellen.

>[!IMPORTANT]
>
>De standaardactivering **Begindatum** en **Einddatum** zijn ingesteld op **Op datum**. De optie om te selecteren **Wanneer aan een voorwaarde wordt voldaan** en **Handmatig** zijn beschikbaar in komende releases.

>[!NOTE]
>
>Zorg ervoor dat zowel de begindatum als de einddatum overeenkomen met de granulariteit die is geselecteerd voor evaluatie in **Stap 4**.

* Als u hebt gekozen voor granulariteit geaggregeerd per week, selecteert u de begin- en einddatum in weken (bijvoorbeeld week 10).
* Als u hebt gekozen voor granulariteit geaggregeerd per maand, selecteert u de begin- en einddatum in maanden.

![Selecteer begindatum en einddatum in de datumkiezer](assets/add-schedule.png)

*Selecteer Begindatum en Einddatum in de datumkiezer*

**A.** Aanvangsdatumkiezer **B.** Einddatumkiezer

>[!NOTE]
>
>De **Begindatum** moet later zijn dan zowel de evaluatieperiode als de huidige datum, terwijl de **Einddatum** moet later zijn dan de begindatum en de huidige datum om bewerkingen in de toekomstige periode te plannen en uit te voeren.

Selecteren **Bewerking opslaan** boven aan het dialoogvenster **Bewerkingen** pagina om een nieuwe bewerking te verwerken.

## Bewerking stoppen {#stop-operation}

U kunt de bewerkingen die momenteel worden uitgevoerd, alleen stoppen **Wordt uitgevoerd** status. Voer de volgende stappen uit om een bestaande bewerking te stoppen:

1. Ga naar de **Bewerkingen** tab onder **Handelingen** in de linkernavigatie van de toepassing van IQ van de Rekening.
1. Selecteren **Opties** menu van de bewerking die u wilt stoppen.

   ![Selecteer het optiemenu om de bewerking te stoppen](assets/stop-operation.png)

   *Selecteer het menu Opties om de bewerking te stoppen*

1. Selecteren **Stoppen**.



