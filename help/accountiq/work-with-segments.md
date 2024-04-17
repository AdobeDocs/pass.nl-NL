---
title: Werken met segmenten
description: Begrijpen en segmenten gebruiken. Leer hoe u een segment maakt en beheert.
exl-id: 01431437-55f5-464d-8ee4-7a79ec553e4f
source-git-commit: 38402b411dd4fad3490115cff1854faa7f3cb293
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Werken met segmenten {#work-with-segments}

[Segmenten](product-concepts.md#segmet-def) zijn een inzameling van abonneerekeningen die u toestaan om geloofsbrieven te analyseren delend onder user-defined voorwaarden. U kunt segmenten gebruiken om verschillende reeksen abonneerekeningen te onderzoeken en overeenkomstige gegevensrapporten in lijsten en grafieken te produceren. Er zijn twee soorten segmenten in Account IQ:

1. **Standaardsegment**: **Alle accounts in uw eigenschappen** is een out-of-the-box segment in het systeem dat alle actieve abonneerekeningen zonder specifieke toegepaste voorwaarden omvat.

   >[!NOTE]
   >
   >Als u het standaardsegment gebruikt, kunnen bepaalde tabellen, zoals [Videocategorieën in segment](data-panels.md#video-categories-segment), [Muziek delen via kanalen en MVPD&#39;s](data-panels.md#sharin-score-by-channels-and-mvpds), en [Gebruik van patroondistributie voor videocategorieën](usage-patterns.md#usage-pattern-dis-video-categories). Deze tabellen kunnen alleen gegevens voor maximaal 20 rijen tegelijk bevatten en weergeven. De overige tabellen, grafieken en rapporten zijn hetzelfde voor standaardsegmenten en aangepaste segmenten.

1. **Aangepaste segmenten**: Dit zijn op maat gemaakte segmenten die u toestaan om abonneerekeningen van specifieke categorieën, zoals D2C inhoudstypes, programmeurs, kanalen, en MVPDs voor het analyseren van referentie het delen onder user-defined voorwaarden te groeperen. Meer informatie over hoe [een aangepast segment maken](#create-new-segment).

   >[!IMPORTANT]
   >
   >Alle procedures die in deze handleiding worden beschreven, zijn gebaseerd op aangepaste segmenten. De concepten blijven echter hetzelfde voor standaardsegmenten en aangepaste segmenten.

Als je naar de **Handelingen** en selecteert u de **[!UICONTROL Segments]** in het linkerdeelvenster wordt een lijst met segmenten weergegeven die in het systeem beschikbaar zijn. Met de segmentpagina kunt u snel de belangrijkste details van elk segment in een tabelindeling beoordelen. De details omvatten de segmentnaam, het aantal [videocategorieën](product-concepts.md#video-category-def), metriek, [bewerkingen](product-concepts.md#operation-def) het gebruiken van het huidige segment, laatste veranderde datum en tijd, evenals de naam van de segmentschepper.

U kunt de volgende functies met segmenten uitvoeren:

* [Een nieuw segment maken](#create-new-segment)
* [Segmenten beheren](#manage-segments)


## Een nieuw segment maken {#create-new-segment}

Het proces om een nieuw segment te creëren is gelijkaardig voor D2C diensten en TV overal. De videocategorieën zijn verschillend voor elke respectievelijke versie van Account IQ.

+++D2C-services

Om een segment te bouwen en het delen van de abonnee gedrag te analyseren, selecteert **[!UICONTROL Create new segment]** rechtsboven.

![Nieuw segment maken](assets/create-new-segment-d2c.png)

*Nieuw segment maken*

>[!NOTE]
>
>De videocategorieën die in de vorige afbeelding worden weergegeven, zoals **Regio&#39;s** en **Inhoudstypen** Dit zijn slechts voorbeelden. Wanneer u zich bij Account IQ aanmeldt, geven deze labels de specifieke videocategorieën van uw bedrijf weer.

Het opent een **Nieuw segment** pagina, die de volgende elementen bevat:

![Nieuwe segmentpagina](assets/d2c-new-segment-dialog.png)

*Nieuwe segmentpagina*

**A.** Segmentcomponenten **B.** Segmentdefinitie **C.** Overzicht van segment

* **Segmentcomponenten**: Een overzicht van [videocategorieën](product-concepts.md##video-category-def) en berekende waarden die worden gebruikt om een segment te definiëren.

  >[!NOTE]
  >
  >Gebruiken **[!UICONTROL Show all]** om de lijst van segmentcomponenten uit te breiden. Als u snel een component wilt zoeken, zoekt u de naam van de component in **componenten van zoeksegmenten** in plaats van door de gehele lijst te schuiven.

* **Segmentdefinitie**: Een canvas waar u verschillende segmentcomponenten kunt slepen en neerzetten om een segment te maken.

* **Overzicht van segment**: Een samenvatting die een raming maakt van de gekwalificeerde rekeningen op basis van de componenten in de segmentdefinitie en een kort overzicht geeft van het segment tijdens de evaluatieperiode.

Voer de volgende stappen uit om een segment te maken:

1. Typ de naam van het segment in **Segmentnaam** die in de lijst van segmenten en tijdens segmentselectie zichtbaar zullen zijn.
1. Typ een gedetailleerde beschrijving van het segment in **Segmentbeschrijving**.
1. Sleep bijvoorbeeld **Gebieden en inhoudstypen** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Gebieden/inhoudstypen** in de **Segmentdefinitie**.

   >[!NOTE]
   >
   >U kunt een segment maken op basis van gebieden of inhoudstypen. De bijbehorende inhoudstypen van een gebied weergeven in een vervolgkeuzemenu.

   Als u begint door een **inhoudstype** in de **Gebieden/inhoudstypen** kunt u alleen inhoudstypen toevoegen als volgende componenten.

   Als u begint door een **Regio** in de **Gebieden/inhoudstypen** , wordt een beslissingsdialoogvenster weergegeven.

   ![Segmentcomponent toevoegen als een gebied of inhoudssoort ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Segmentcomponent toevoegen als een gebied of dialoogvenster met inhoudssoorten*

   Bepaal of u specifieke gebieden of een segment wilt vergelijken dat op de inhoudstypes wordt gebaseerd verbonden aan een gebied.

   Selecteren **[!UICONTROL As a region]** om gebieden toe te voegen aan **Gebieden/inhoudstypen** sectie.

   Selecteren **[!UICONTROL As its content types]** om inhoudstypen van een gebied toe te voegen.

1. Slepen **Metrisch** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Metrisch** in de **Segmentdefinitie**.

   ![Selecteer een operator en stel een waarde in voor de toegevoegde waarde](assets/component-metrics.png)

   *Selecteer een exploitant en wijs een waarde voor toegevoegde metrisch toe*

   Na het toevoegen van metriek in de segmentdefinitie, kies een exploitant van **[!UICONTROL Select an operator]** vervolgkeuzemenu en een waarde toewijzen met **[!UICONTROL Select an option]**.

   Pas waarden voor bepaalde metriek aan door de naar boven wijzende pijl te gebruiken om te verhogen en de naar beneden liggende pijl om te verminderen.

1. Slepen **Berekende cijfers** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Berekende cijfers** in de **Segmentdefinitie**.

   ![Selecteer een operator en stel een waarde in voor de toegevoegde berekende metrische waarde](assets/component-calculated-metrics.png)

   *Selecteer een exploitant en wijs een waarde voor toegevoegde berekende metrisch toe*

   Na het toevoegen van berekende metriek in de segmentdefinitie, **[!UICONTROL Select an operator]** in het vervolgkeuzemenu en wijs een waarde toe met **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Alle metriek en berekende metriek u onder de segmentdefinitie daalt gaan vergezeld van aangewezen exploitanten om waarden aan de respectieve metriek en berekende metriek toe te wijzen.

1. Controleer de segmentdetails in het dialoogvenster **Overzicht van segment** om de veranderingen te beslissen u over het segment wilt uitvoeren.
1. Selecteren **[!UICONTROL Last week]** of **[!UICONTROL Last month]** van de **Evaluatieperiode** vervolgkeuzemenu voor het schatten van de overzichtswaarden voor de afgelopen week of maand.
1. Selecteren **[!UICONTROL Update estimation]** het aantal geraamde gekwalificeerde rekeningen in het huidige segment te berekenen op basis van de geselecteerde evaluatieperiode.
1. Selecteren **[!UICONTROL Save segment]**.

Het segment u hebt gecreeerd is nu beschikbaar in de segmentlijst.

+++

+++tv overal

Om een segment te bouwen en het delen van de abonnee gedrag te analyseren, selecteert **[!UICONTROL Create new segment]** rechtsboven.

![Nieuw segment maken](assets/create-new-segment.png)

*Nieuw segment maken*

Het opent een **Nieuw segment** pagina, die de volgende elementen bevat:

![Nieuwe segmentpagina](assets/new-segment-dialog.png)

*Nieuwe segmentpagina*

**A.** Segmentcomponenten **B.** Segmentdefinitie **C.** Overzicht van segment

* **Segmentcomponenten**: Een overzicht van programmeurs en kanalen, MVPDs, metriek, en berekende metriek die wordt gebruikt om een segment te bepalen.

  >[!NOTE]
  >
  >Gebruiken **[!UICONTROL Show all]** om de lijst van segmentcomponenten uit te breiden. Als u snel een component wilt zoeken, zoekt u de naam van de component in **componenten van zoeksegmenten** in plaats van door de gehele lijst te schuiven.

* **Segmentdefinitie**: Een canvas waar u verschillende segmentcomponenten kunt slepen en neerzetten om een segment te maken.

* **Overzicht van segment**: Een samenvatting die een raming maakt van de gekwalificeerde rekeningen op basis van de componenten in de segmentdefinitie en een kort overzicht geeft van het segment tijdens de evaluatieperiode.

Voer de volgende stappen uit om een segment te maken:

1. Typ de naam van het segment in **Segmentnaam** die in de lijst van segmenten en tijdens segmentselectie zichtbaar zullen zijn.
1. Typ een gedetailleerde beschrijving van het segment in **Segmentbeschrijving**.
1. Slepen **Programmeurs en kanalen** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Programmeurs/kanalen** in de **Segmentdefinitie**.

   >[!NOTE]
   >
   >U kunt een segment tot stand brengen dat op of programmeurs of kanalen wordt gebaseerd. Bekijk de bijbehorende kanalen met een programmeur van een dropdown menu.

   Als u begint door een **Kanaal** in de **Programmeurs/kanalen** kunt u alleen kanalen toevoegen als volgende componenten.

   Als u begint door een **Programmeur** in de **Programmeurs/kanalen** , wordt een beslissingsdialoogvenster weergegeven.

   ![Segmentcomponent toevoegen als programmeur of zijn kanalen ](assets/segment-basis-selector.png){width="550" align="left"}


   *Segmentcomponent toevoegen als programmeur of dialoogvenster Kanalen*

   Bepaal of u specifieke programmeurs of een segment wilt vergelijken dat op de kanalen verbonden aan een programmeur wordt gebaseerd.

   Selecteren **[!UICONTROL As a programmer]** om programmeurs toe te voegen aan **Programmeurs/kanalen** sectie.

   Selecteren **[!UICONTROL As its channels]** om alle kanalen van een programmeur toe te voegen.

1. Slepen **MVPD&#39;s** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **MVPD&#39;s** in de **Segmentdefinitie**.

   >[!NOTE]
   >
   >Wanneer u login als programmeur, genoemd MVPD **oneindigheid** verschijnt als zelfstandige optie in het dialoogvenster **MVPD&#39;s** sectie. U kunt het niet met een andere MVPD combineren.

1. Slepen **Metrisch** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Metrisch** in de **Segmentdefinitie**.

   ![Selecteer een operator en stel een waarde in voor de toegevoegde waarde](assets/component-metrics.png)

   *Selecteer een exploitant en wijs een waarde voor toegevoegde metrisch toe*

   Na het toevoegen van metriek in de segmentdefinitie, kies een exploitant van **[!UICONTROL Select an operator]** vervolgkeuzemenu en een waarde toewijzen met **[!UICONTROL Select an option]**.

   Pas waarden voor bepaalde metriek aan door de naar boven wijzende pijl te gebruiken om te verhogen en de naar beneden liggende pijl om te verminderen.

1. Slepen **Berekende cijfers** van de segmentcomponenten in het linkerdeelvenster en zet ze neer in de **Berekende cijfers** in de **Segmentdefinitie**.

   ![Selecteer een operator en stel een waarde in voor de toegevoegde berekende metrische waarde](assets/component-calculated-metrics.png)

   *Selecteer een exploitant en wijs een waarde voor toegevoegde berekende metrisch toe*

   Na het toevoegen van berekende metriek in de segmentdefinitie, **[!UICONTROL Select an operator]** in het vervolgkeuzemenu en wijs een waarde toe met **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Alle metriek en berekende metriek u onder de segmentdefinitie daalt gaan vergezeld van aangewezen exploitanten om waarden aan de respectieve metriek en berekende metriek toe te wijzen.

1. Controleer de segmentdetails in het dialoogvenster **Overzicht van segment** om de veranderingen te beslissen u over het segment wilt uitvoeren.
1. Selecteren **[!UICONTROL Last week]** of **[!UICONTROL Last month]** van de **Evaluatieperiode** vervolgkeuzemenu voor het schatten van de overzichtswaarden voor de afgelopen week of maand.
1. Selecteren **[!UICONTROL Update estimation]** het aantal geraamde gekwalificeerde rekeningen in het huidige segment te berekenen op basis van de geselecteerde evaluatieperiode.
1. Selecteren **[!UICONTROL Save segment]**.

Het segment u hebt gecreeerd is nu beschikbaar in de segmentlijst.
+++

## Segmenten beheren {#manage-segments}

U kunt een segment selecteren in de segmentlijst en vervolgens de volgende handelingen uitvoeren:

* [Een segment bewerken](#edit-segment)
* [Een segment dupliceren](#duplicate-segment)
* [Een segment verwijderen](#delete-segment)

![Een segment bewerken, dupliceren of verwijderen](assets/manage-segments-list.png)

*Selecteer een segment dat u wilt bewerken, dupliceren of verwijderen*

**A.** [Standaardsegment](#work-with-segments) **B.** [Videocategorieën](product-concepts.md#video-category-def)

>[!NOTE]
>
>De videocategorieën die in deze sectie worden getoond, zoals **MVPD&#39;s**, **Programmeurs**, en **Kanalen** de labels weergeven die worden gebruikt in de IQ-versie van account op tv overal. Als u als dienst D2C het programma wordt geopend, tonen deze etiketten de specifieke videocategorieën van uw bedrijf.

U kunt het standaardsegment met de naam **Alle accounts in uw eigenschappen**.

### Een segment bewerken {#edit-segment}

1. Ga naar de **[!UICONTROL Segments]** tab onder **Handelingen** in het linkerdeelvenster om een lijst met segmenten weer te geven.
1. Selecteer een segment dat u wilt bewerken.
1. Selecteren **[!UICONTROL Edit]**.
1. Wijzig segmentdetails, zoals de segmentnaam, beschrijving of componenten in de **Segmentdefinitie**.

   >[!TIP]
   >
   >Gebruiken **[!UICONTROL Clear all]** om alle segmentcomponenten binnen elke sectie onder segmentdefinitie in één keer te verwijderen. U kunt ook de kruisknop selecteren om afzonderlijke items te verwijderen.

   ![Alle segmentcomponenten in elke sectie onder segmentdefinitie wissen ](assets/clear-all-components.png)

   *Alles wissen selecteren om alle segmentcomponenten tegelijk te verwijderen*

1. Selecteer een van beide **[!UICONTROL Update segment]** om het bestaande segment bij te werken of **[!UICONTROL Save as new segment]** om een nieuw segment met de wijzigingen te maken.

   >[!NOTE]
   >
   >Het is niet toegestaan segmenten bij te werken die momenteel bewerkingen uitvoeren. Het opslaan van veranderingen als nieuw segment is de enige optie voor segmenten met aan de gang zijnde verrichtingen.

### Een segment dupliceren {#duplicate-segment}

1. Ga naar de **[!UICONTROL Segments]** tab onder **Handelingen** in het linkerdeelvenster om een lijst met segmenten weer te geven.
1. Selecteer een segment dat u wilt dupliceren.
1. Selecteren **[!UICONTROL Duplicate]**.

Er wordt een kopie van het geselecteerde segment gegenereerd en aan het einde van de segmentlijst geplaatst. U kunt de noodzakelijke details in het gedupliceerde segment bewerken en het gedupliceerde segment vervolgens bijwerken of opslaan als een nieuw segment.

### Een segment verwijderen {#delete-segment}

1. Ga naar de **[!UICONTROL Segments]** tab onder **Handelingen** in het linkerdeelvenster om een lijst met segmenten weer te geven.
1. Selecteer een segment dat u wilt verwijderen.

   Selecteer meerdere segmenten om deze in één bewerking te verwijderen. U kunt ook een selectievakje links van het dialoogvenster **Segmentnaam** om alle segmenten tegelijk te verwijderen.

   >[!NOTE]
   >
   > U kunt slechts meer dan één segment of alle segmenten verwijderen als geen van de segmenten door bewerkingen wordt gebruikt. Bovendien wordt het standaardsegment met de naam **Alle accounts in uw eigenschappen** is niet toegestaan. Deze optie blijft uitgeschakeld wanneer u probeert alle segmenten tegelijk te verwijderen.

   ![Meer dan één segment verwijderen](assets/delete-more-than-one-segment.png)

   *Meerdere segmenten selecteren om meerdere segmenten te verwijderen*

1. Selecteren **[!UICONTROL Delete]**.
1. Bevestigen tot **[!UICONTROL Delete]** om het segment permanent te verwijderen.

   >[!NOTE]
   >
   >Het segment wordt permanent verwijderd uit het systeem en u kunt deze handeling niet ongedaan maken.
