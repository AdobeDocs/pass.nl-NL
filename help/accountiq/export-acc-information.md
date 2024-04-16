---
title: Informatie exporteren voor accounts met hoge score voor delen
description: Exporteer gegevens voor accounts met een hoge score voor delen.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Informatie exporteren voor accounts met hoge score voor delen {#export-account-info-high-score}

[!UICONTROL Account IQ] staat u toe om rekening het delen details voor hoogste 1000 abonneerekeningen uit te voeren die op hun worden gebaseerd [waarschijnlijkheid delen](/help/accountiq/product-concepts.md#account-sharing-probability-def). U kunt de gegevens voor het delen van de account exporteren voor de huidige [segment](/help/accountiq/product-concepts.md#segment-def) en [opgegeven tijdsinterval](/help/accountiq/product-concepts.md#time-interval-def) op de [Rapporten over gedeelde accounts](/help/accountiq/shared-acc-reports.md) pagina.

Volg de stappen om de rekening uit te voeren die informatie deelt van abonneerekeningen voor een specifiek segment.

1. Meld u aan met uw referenties.
1. Ga naar de **Gedeelde accounts** tab onder **Rapporten** sectie.
1. Selecteer het vereiste segment en tijdinterval van segment en tijdintervalpaneel. Meer informatie [hoe te om een segment en tijdinterval te selecteren](segments-timeinterval.md).

   Raadpleeg indien nodig de instructies voor [een segment maken](work-with-segments.md#create-new-segment) of [een segment bewerken](work-with-segments.md#edit-segment).

1. Selecteren **[!UICONTROL Export top 1000 accounts]** in de rechterbovenhoek van het segment en het tijdinterval.

   ![Bovenste 1000-accounts exporteren](assets/export-top-1000-accounts.png)

   *Selecteer de optie Bovenste 1000 accounts exporteren*

Het bestand wordt automatisch naar uw lokale computer gedownload als een .csv-bestand.

Dit dossier bevat de gegevens voor hoogste 1000 rekeningen die op de het delen waarschijnlijkheden van de abonneerekeningen in het huidige segment in dalende orde worden gebaseerd.

Hieronder ziet u een voorbeeld van het geëxporteerde CSV-bestand.

![geëxporteerde gegevens in CSV-bestand](assets/exported-csv.png)

*Geëxporteerde gegevens in CSV-bestand*

## Kolommen in het geëxporteerde rapport {#columns-in-export}

**Week/maand**

De geselecteerde week of maand binnen de **[!UICONTROL Granularity and Time Interval]** in de segmentkiezer.

**MVPD**

Als u een programmeur bent, toont de kolom de verdeler met wie de rekening wordt geabonneerd.

>[!NOTE]
>
> De **MVPD** -kolom is alleen beschikbaar voor tv-versies overal.

**Abonnee-id**

De unieke id voor de specifieke account.

**Minimum aantal apparaten**

Het minimale aantal apparaten waaruit gebruikers actief inhoud streamen.

>[!NOTE]
>
>Het werkelijke aantal apparaten dat inhoud streamt, is groter dan het minimumaantal apparaten dat voor een bepaalde account is opgegeven.

**Minimum aantal personen**

Het minimale aantal personen dat actief inhoud streamt met deze apparaten.

>[!NOTE]
>
>Het werkelijke aantal personen dat inhoud streamt, is groter dan het minimumaantal personen dat is toegewezen aan een bepaalde account.

**[!UICONTROL # IPs]**

Het aantal IP-adressen waaruit inhoud wordt gestreamd.

**[!UICONTROL # Locations]**

Het aantal locaties (op basis van postcode) waaruit inhoud wordt gestreamd.

**[!UICONTROL # Cities]**

Het aantal steden waar de streamingactiviteit heeft plaatsgevonden.

**[!UICONTROL # States]**

Het aantal staten waar de streamingactiviteit heeft plaatsgevonden.

**[!UICONTROL # Clusters]**

Het aantal afzonderlijke [clusters](/help/accountiq/product-concepts.md#cluster-def) waar streaming heeft plaatsgevonden.

**[!UICONTROL Geographic span (miles)]**

De maximale afstand tussen de streaminglocaties die aan de account zijn gekoppeld.

**[!UICONTROL # AuthN OK]**

Het aantal logins dat gebruikers maken tijdens de opgegeven periode met dat account.

>[!NOTE]
>
> Sommige D2C-services zijn mogelijk niet zichtbaar **[!UICONTROL # AuthN OK]** gegevens die mogelijk niet in de gegevens van hun bedrijf worden opgenomen.

**[!UICONTROL # AuthZ OK]**

Het aantal keren dat een MVPD een stream heeft geautoriseerd of toegang heeft verleend tot inhoud voor die account.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** is niet beschikbaar voor D2C-services.

>[!NOTE]
>
>Voor tv overal, **[!UICONTROL # AuthZ OK]** is gecorreleerd met het aantal **[# Aanvragen afspelen](/help/accountiq/product-concepts.md##play-requests-def)**. Het zal altijd minder zijn dan **[!UICONTROL # Play Requests]** omdat de Adobe de vergunningen van MVPDs typisch voor ongeveer 24 uren in het voorgeheugen onderbrengt.


**[!UICONTROL # Play Requests]**

Het daadwerkelijke aantal stromen kwam tijdens een gespecificeerde tijdspanne voor.

>[!NOTE]
>
>De [# Aanvragen afspelen](/help/accountiq/product-concepts.md##play-requests-def) de kolom is niet beschikbaar in TV overal MVPD versie.

**[!UICONTROL # Channels]**

Het totale aantal kanalen dat de account gedurende een bepaalde periode heeft gecontroleerd.

>[!NOTE]
>
> Voor D2C-diensten **[!UICONTROL # Channels]** is gelijk aan het aantal **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Voor TV overal, omvatten zij de kanalen die niet tot het programma geopende programmeur kunnen behoren. Dit nummer voor de account bevat uw kanaal en andere kanalen die tijdens de opgegeven periode worden geopend.


**Gebruikspatroon**

De waarden binnen deze kolommen dienen als id&#39;s die overeenkomen met een van de 14 patronen die we gebruiken om alle gebruikersaccounts te categoriseren.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Gebruikspatronen</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Gewone gebruiker</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Reiziger of pendelaar</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Grote familie</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Familie en vrienden sluiten</td>
      </tr>
      </tr>
         <td>5 en 8</td>
         <td>Delen van sociale groepen</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Grote groep vrienden</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Gelijktijdige streaming</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Delen door Gemeenschap</td>
      </tr>
      </tr>
         <td>10 en 11</td>
         <td>Onduidelijk gedrag</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Kleine familie</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Tweede thuis </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Abnormaal gebruik</td>
      </tr>
    </tbody>
  </table>

*Gebruikspatroon-id&#39;s in geëxporteerde .csv-toewijzing met gebruikspatronen*

**Deelbaarheid**

De waarschijnlijkheid dat een specifieke account zijn gegevens deelt.

>[!NOTE]
>
> Het gemiddelde van de waarschijnlijkheid van delen van alle rekeningen in het geselecteerde segment wordt gebruikt om het [deelniveau](/help/accountiq/data-panels.md#sharing-level) van de [gemiddelde delingsscore](/help/accountiq/data-panels.md#aggregated-sharing).
