---
title: TVE-dashboardintegratie
description: Wis over de integratie tussen uw kanalen en MVPDs en hoe te om integratie te beheren.
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integraties

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De **sectie van de Integraties** van het Dashboard van TVE staat u toe om montages voor de integratie tussen uw kanalen en MVPDs te bekijken en te beheren. U kunt [ een nieuwe integratie ](#create-new-integration) volgens uw vereiste ook tot stand brengen.

Het **lusje van de Integraties** in het linkerpaneel toont een lijst van bestaande integratie met de volgende details:

* Status die aangeeft of de integratie momenteel actief of inactief is
* Integratie die specifieke kanalen met respectieve MVPDs verbindt
* Kanaalnaam met kanaal-id
* MVPD-weergavenaam en MVPD-id

![ Lijst van bestaande Integraties ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integrations-list.png)

*Lijst van bestaande Integraties*

Typ de naam van het kanaal of MVPD in **Onderzoek** bar boven de lijst om meer over de integratie te weten te komen.

## Integratieconfiguraties beheren {#manage-integration-conf}

Voer de volgende stappen uit om een specifieke integratie te beheren.

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.
1. Selecteer een integratie in de lijst met opties om verschillende instellingen in de volgende secties weer te geven en te bewerken:

   * [Eindpuntselectie](#endpoint-selection)
   * [Platforminstellingen](#platform-settings)
   * [Metagegevens gebruiker](#user-metadata)

>[!IMPORTANT]
>
> De Controle van de mening [ en duw verandert ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) voor meer informatie bij het activeren van de configuratieveranderingen.

### Eindpuntselectie {#endpoint-selection}

In deze sectie kunt u de eindpunten van de MVPD kiezen die worden gebruikt voor verificatie-, autorisatie- en afmeldingsstromen in de respectieve vervolgkeuzemenu&#39;s.

![ Eindpunten voor authentificatie, vergunning, en logout stromen ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-endpoint-selection-panel-view.png)

*Eindpunten voor authentificatie, vergunning, en logout stromen*

>[!NOTE]
>
>MVPDs kan één of veelvoudige eindpunten voor elke stroom verstrekken. Wanneer het integreren van een nieuw kanaal, moet MVPD hun aangewezen eindpunt voor elke stroom specificeren.

>[!IMPORTANT]
>
>Elke wijziging in de eindpunten heeft gevolgen voor het algemene gedrag van een integratie. Deze wijzigingen mogen pas worden doorgevoerd nadat de MVPD dit heeft bevestigd.

### Platforminstellingen {#platform-settings}

Deze sectie staat u toe om integratiemontages over alle [ platforms ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md#platforms) te bekijken en uit te geven. U kunt deze instellingen wijzigen op basis van afzonderlijke platforms. U kunt bijvoorbeeld de duur van de TTL-autorisatie aanpassen op Android terwijl een standaardwaarde voor een ander platform wordt behouden.

Elke eigenschap in platforminstellingen neemt een standaardwaarde over die door de MVPD is ingesteld, maar kan indien nodig worden aangepast.

>[!IMPORTANT]
>
>Er is een overeenkomst met de MVPD vereist om de waarden te bepalen die voor elke eigenschap zijn ingesteld in de platforminstellingen.

>[!IMPORTANT]
>
> De montageenovererving volgt een ketting die van de montages van MVPD (die het meest algemeen zijn) begint, toen het eindpunt van MVPD, integratie, platformcategorie, en platform (die de meest specifieke waarde) houdt.

**de Montages van het Platform** wordt gebruikt om montages voor elk niveau in de overervingsketen met voeten te treden. De beschikbare niveaus in de keten zijn als volgt gegroepeerd:

* **Gebrek voor allen**: Plaats waarden voor eigenschappen die universeel op alle platforms van toepassing zijn als de specifieke platformwaarden niet, ongeacht de implementaties van de Programmer worden bepaald.

* **Apparaten van de Desktop**: Plaats waarden voor eigenschappen toepasselijk op alle Desktop en laptop computers, ongeacht de programmeringsmethode (JS SDK of REST API).

* **Mobiele Apparaten**: Plaats waarden voor eigenschappen toepasselijk op alle mobiele apparaten, met inbegrip van **iOS**, **Android**, en anderen, ongeacht de programmeringsbenadering (SDK of REST API).

* **TV Verbonden Apparaten**: Plaats waarden voor eigenschappen toepasselijk op alle TV verbonden apparaten, met inbegrip van **tvOS**, **Roku**, **FireTV**, en anderen, ongeacht de programmeringsmethode (SDK of REST API).

* **Niet-geïdentificeerde Apparaten**: Plaats waarden voor eigenschappen toepasselijk op alle apparaten waar het huidige mechanisme niet het platform kan nauwkeurig identificeren. In dergelijke gevallen past u de meest beperkende regels toe die door de MVPD zijn vastgesteld.

  ![ Categorie van platforms en hun apparaten ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-menu.png)

  *Categorie van platforms en hun apparaten*

Selecteren <img alt= "overervingskettingpictogram" src="/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-inheritance-chain-icon.svg" width="25"> -pictogram rechts van elke eigenschap om de eigenschappen te bekijken die worden gebruikt voor elk hierboven beschreven overervingsniveau.

#### Meest gebruikte bedrijfsstromen {#most-used-flows}

De **sectie van de Montages van het Platform** biedt een waaier van eigenschappen aan die in verschillende bedrijfsstromen worden gebruikt. De eigenlijke eigenschappen kunnen variëren, afhankelijk van de MVPD&#39;s die in de specifieke integratie zijn geselecteerd. Hieronder ziet u de meest gebruikte stromen:

**AuthN TTL en AuthZ TTL over alle platforms**

>[!IMPORTANT]
>
>De waarden van TTL van de authentificatie (AuthN) van TTL en van de Vergunning (AuthZ) moeten verenigbaar met de montages van MVPD richten.

Voer de volgende stappen uit om verificatie en autorisatie-TTL op alle platforms voor een specifieke integratie te wijzigen.

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.

1. Selecteer de integratie waarvoor u waarden van TTL wilt veranderen AuthN en AuthZ TTL.

1. Navigeer aan de **sectie van de Montages van het Platform**.

1. Selecteer **Gebrek voor Al** lusje onder **Montages van het Platform**.

   >[!NOTE]
   >
   >Als u de duur van **wilt veranderen AuthN TTL** en **AuthZ TTL** voor een platformcategorie of een specifiek platform, selecteer dienovereenkomstig het platform.

   ![ De duur van AuthN TTL AuthZ TTL van de Verandering over alle platforms ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-authn-ttl-authz-ttl-properties.png)

   *De duur van AuthN TTL AuthZ TTL van de Verandering over alle platforms*

   **A.** het bezit van AuthN TTL **B.** het bezit van AuthZ TTL

1. Selecteer naar boven en naar beneden pijlen om de duur voor het aantal dagen, uren, notulen, en seconden in **aan te passen TTL AuthN** en **Eigenschappen AuthZ TTL**.

De duur voor **AuthN TTL** en **AuthZ TTL** over alle platforms zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden bijgewerkt.

**laat platform SSO** toe

>[!IMPORTANT]
>
>**laat Enig Teken** bezit toe wordt uitsluitend gesteund op *iOS, tvOS, Roku, en* platforms FireTV. Het is slechts van toepassing op integratie met MVPDs die enig teken voor deze platforms steunen.

Voer de volgende stappen uit om SSO voor een specifieke integratie en een specifiek platform in of uit te schakelen.

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.

1. Selecteer de integratie waarvoor u Single Sign On wilt in- of uitschakelen.

1. Navigeer aan de **sectie van de Montages van het Platform**.

1. Selecteer een specifiek platform of een categorie van platforms waarvoor u enig teken binnen wilt toelaten onder **de Montages van het Platform**.

   ![ laat Enig Teken voor een specifiek platform toe ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-single-sign-on-properties.png)

   *laat Enig Teken voor een specifiek platform toe*

   **A.** Enig Teken op bezit **B.** dwingt het bezit van de Toestemmingen van het Platform af

1. Selecteer **ja** om of **Nr** toe te laten om van **onbruikbaar te maken toelaten Enig Teken** dropdown menu.

1. Selecteer **ja** om of **Nr** toe te laten om van **de Toestemming van het Platform van de Afdwingen** dropdown menu onbruikbaar te maken.

   **het bezitscontroles van de Toestemming van het Platform van 0} afdwingen {als het besluit van de gebruiker om** **toe te staan of** ontken **platformtoegang tot hun abonnement van TV-provider wordt gerespecteerd.**

   Bijvoorbeeld, als zowel **toelaten Enig Teken** en **de Toestemming van het Platform van de Afdwingen** wordt toegelaten, en de gebruiker verkiest om platformtoegang tot hun abonnement van de Leverancier van TV te ontkennen, dan zal de respectieve toepassing (kanaal) het teken van de Authentificatie van Adobe Pass niet kunnen gebruiken dat door een andere toepassing (kanaal) wordt verkregen.

Het **Enige Teken** bezit voor een geselecteerd platform zal worden toegelaten of slechts gehandicapt na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**laat op huis-gebaseerde authentificatie** toe

Volg deze stappen om op huis-gebaseerde authentificatie voor op OAuth2 gebaseerde MVPDs toe te laten of onbruikbaar te maken.

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.

1. Selecteer de integratie waarvoor u op huis-gebaseerde authentificatie wilt toelaten of onbruikbaar maken.

1. Navigeer aan de **sectie van de Montages van het Platform**.

1. Selecteer een specifiek platform of een categorie van platforms waarvoor u op huis-gebaseerde authentificatie onder **de Montages van het Platform** wilt toelaten.

   ![ laat op huis-gebaseerde authentificatie voor een specifiek platform ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-attempt-hba-properties.png) toe

   *laat op huis-gebaseerde authentificatie voor een specifiek platform* toe

   **A.** het bezit van HBA van de Poging **B.** het bezit van AuthN TTL van HBA

1. Selecteer **ja** om toe te laten en **Nr** om van het **Poging HBA** dropdown menu onbruikbaar te maken.

>[!IMPORTANT]
>
>Het veranderen van de duur van **HBA AuthN TTL** bezit zou moeten worden vermeden. Dit kan leiden tot onverwachte fouten in het autorisatieproces.

Het **bezit van HBA van de Poging** voor specifieke MVPD zal worden toegelaten of slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) onbruikbaar gemaakt.

#### Meer eigenschappen toevoegen {#add-more-properties}

**voegt meer eigenschappen** toe staat de flexibiliteit toe om extra specifieke eigenschappen voor integratie, in het bijzonder voor minder gemeenschappelijke stromen te omvatten.

U kunt de volgende eigenschappen toevoegen:

* Voor alle platforms, uitgezochte **Gebrek voor al** lusje op de linkerzijde.
* Voor een categorie van platform, selecteer **de Apparaten van de Desktop,** Mobiele Apparaten **, of** Verbonden Apparaten van TV **lusje op de linkerzijde.**
* Voor een specifiek apparaat, uitgezochte **iOS**, **Android**, **tvOS**, **Roku**, of **FireTV** lusje op de linkerzijde.

Hier volgen enkele voorbeelden van verschillende stromen die kunnen worden ingeschakeld door de volgende eigenschappen toe te voegen:

**verander het aantal voor pre-erkende middelen**

De meeste MVPDs steunen een preflight authZ vraag gebruikend tot 5 middel IDs door gebrek.
Nochtans, in gevallen waar MVPDs ermee akkoord gaat om deze grens op te heffen, kunt u aan **navigeren meer eigenschappen** toevoegen en **Preflight Max Middelen** van het optiemenu selecteren.

**Preflight Max Middelen** zal een nieuw attribuut toevoegen waar de overeengekomen grens met MVPD kan worden gespecificeerd.

![ voeg Preflight Max bezit van Middelen ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-preflight-max-resources-properties.png) toe

*voeg Preflight Max bezit van Middelen* toe

Het **Preflight Max bezit van Middelen** zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden toegevoegd.

**de vertoningsnaam of embleem URL van de Verandering MVPD**

Voor programmeertoepassingen die niet hun plukker van MVPD willen bouwen en in plaats daarvan op verstrekte configuraties baseren, kunt u aan **navigeren toevoegen meer eigenschappen** en **Naam van de Vertoning** of **Logo URL** selecteren om de vereiste vertoningsnaam of het embleem URLs voor elke MVPD van het optiemenu toe te voegen.

Afhankelijk van het apparaatplatform en de gewenste gebruikerservaring kunnen verschillende waarden voor deze eigenschappen voor dezelfde MVPD worden gebruikt.

![ voeg het bezit van de Naam van de Vertoning of van het Logo URL ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-display-name-logo-url-properties.png) toe

*voeg het bezit van de Naam van de Vertoning of van het Logo URL* toe

Het **bezit van de Naam van de Vertoning** of **Logo URL** zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden toegevoegd.

**verzoek een nieuwe authentificatiestroom op app (kanaal) omschakeling**

Als u een nieuwe verificatie wilt forceren wanneer gebruikers schakelen tussen apps. In dat geval, kunt u aan **navigeren voeg meer eigenschappen** toe, selecteer de **Auth per Samenvoegingsator** bezit.

Toevoegend **Auth per Samenvoeger** breekt effectief één enkel teken voor het respectieve kanaal.

![ voeg Auth per het bezit van de Samenvoeger ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-auth-per-aggregator-properties.png) toe

*voeg Auth per het bezit van de Samenvoeger* toe

Het **Auth per het samenvoegen** bezit zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden toegevoegd.

Zodra toegevoegd, uitgezochte **ja** om **Auth per het bezit van de Samenvoegaar** voor een geselecteerde integratie toe te laten.

#### Eigenschappen verwijderen {#delete-properties}

Selecteren <img alt= "delete, knop eigenschap" src="/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-delete-property-icon.svg" width="25"> rechts van elke eigenschap om de eigenschappen te verwijderen die niet meer vereist zijn.

>[!NOTE]
>
>Bepaalde eigenschappen kunnen niet worden verwijderd, omdat het verplichte vereisten voor de geselecteerde MVPD zijn.

Het bezit zal van de **montages van het Platform** sectie slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden geschrapt.

### Metagegevens gebruiker {#user-metadata}

In deze sectie kunt u instellingen bijwerken voor alle parameters voor gebruikersmetagegevens die door de MVPD worden gedeeld.

>[!NOTE]
>
>Elke MVPD kan verschillende parameters delen. Neem contact op met uw Adobe-vertegenwoordiger voor meer informatie over de parameters die een specifieke MVPD kan delen.

In de sectie met gebruikersmetagegevens worden de volgende kolommen weergegeven:

**Sleutel**: Vertegenwoordigt de daadwerkelijke parameters van gebruikersmeta-gegevens die in API moeten worden gebruikt om waarden te halen.

**Beschrijving**: Verstrekt een korte beschrijving van elke parameter van gebruikersmeta-gegevens.

**Gecodeerde**: Deze kolom staat u toe om parameters in API toe te laten of onbruikbaar te maken door **ja** of **Nr** van het dropdown menu respectievelijk te selecteren. Het kiezen voor **ja** wijst erop dat de parameterwaarde in API zal worden gecodeerd. De encryptie wordt uitgevoerd gebruikend een certificaat dat door het 1} werkingsgebied van meta-gegevens van de a **Gebruiker wordt bepaald.**

>[!TIP]
>
>
> Zorg altijd ervoor dat de **parameter van het ZIP** wordt gecodeerd.

Leer meer over beschikbare certificaten in [ Programmeurs ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#available-certificates) en [ Kanalen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#available-certificates) secties.

**Toegelaten**: Deze kolom staat u toe om de parameters in API toe te laten of onbruikbaar te maken door **ja** of **Nr** respectievelijk van het dropdown menu te selecteren.

![ Parameters beschikbaar voor Metagegevens van de Gebruiker ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-user-metadata-panel-view.png)

*Parameters beschikbaar voor Metagegevens van de Gebruiker*

## Nieuwe integratie maken {#create-new-integration}

Voer de volgende stappen uit om een nieuwe integratie te maken met een nieuwe MVPD op uw huidige installatie:

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.

1. Selecteer **creeer nieuwe integratie** bij het hoger-recht van de **sectie van de Integraties**.

   ![ creeer een nieuwe integratie ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-create-new-integration-button.png)

   *creeer een nieuwe integratie*

   De volgende secties worden weergegeven:

   **Uitgezochte Kanaal en MVPD**

   Selecteer a **Kanaal** van het **Uitgezochte drop-down menu van het Kanaal** om een nieuwe integratie toe te voegen. Zodra u het kanaal hebt geselecteerd, selecteer vereiste **MVPD** van **Uitgezochte MVPD** dropdown menu om met het geselecteerde kanaal worden geïntegreerd.

   ![ Uitgezochte Kanaal en MVPD ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-channel-and-mvpd-panel-view.png)

   *Uitgezochte Kanaal en MVPD*

   **Uitgezochte eindpunten**

   Na het selecteren van de vereiste MVPD, **Uitgezochte eindpunt** sectie zal met de standaardeindpunten worden bevolkt die voor die bepaalde MVPD worden gevormd.

   >[!IMPORTANT]
   >
   >Wijzig de standaardeindpunten in geen enkele stroom, tenzij dit specifiek door de MVPD wordt aangegeven.

   ![ Eindpunten selecteren ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-endpoints-panel-view.png)

   *Uitgezochte eindpunten*

   **Aanvullende informatie**

   Deze sectie omvat diverse eigenschappen die voor geselecteerde MVPD in **Uitgezochte Kanaal en MVPD** sectie moeten worden gevormd.

   >[!NOTE]
   >
   > De daadwerkelijke eigenschappen kunnen afhankelijk van MVPDs verschillen die in de **Uitgezochte Kanaal en MVPD** sectie wordt geselecteerd.

   Bijvoorbeeld, kunt u **AuthN TTL** of **identiteitskaart van de Partner** (identiteitskaart van het Kanaal) voor co-branding doeleinden op de login van MVPD pagina in het volgende beeld uitgeven.

   ![ geef Aanvullende informatie ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-additional-information-panel-view.png) uit

   *geef Aanvullende informatie* uit

   Selecteer **sparen integratie** bij het hoger-recht van **creeer nieuwe integratie** sectie.

Een nieuwe integratie zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden gecreeerd.


## Integratie uitschakelen {#disable-integration}

Voer de volgende stappen uit om een integratie uit te schakelen:

1. Selecteer het **lusje van de Integraties** in het linkerpaneel.

1. Selecteer de integratie die u wilt uitschakelen.

1. Schakel de schakeloptie rechtsboven in de geselecteerde integratie uit.

   ![ maak integratie ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-enabled-disabled-button.png) onbruikbaar

   *maak integratie* onbruikbaar

De integratie zal slechts na [ overzicht en duw veranderingen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) worden onbruikbaar gemaakt.

Nadat de integratie is uitgeschakeld, kunnen eindgebruikers het gebruik van de specifieke MVPD niet meer verifiëren of autoriseren.
