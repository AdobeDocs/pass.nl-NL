---
title: Programmeurs
description: Leer over programmeurs en zijn configuraties binnen het dashboard van TVE.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# Programmeurs {#programmers}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De **sectie van Programmeurs** van het Dashboard van TVE staat u toe om montages voor de [ programmeurs ](/help/authentication/glossary.md#programmer) verbonden aan uw rekeningsrechten te bekijken en te beheren. U kunt ook [ een nieuwe programmeur ](#add-new-programmer) volgens uw vereiste toevoegen.

Het **lusje van Programmeurs** in het linkerpaneel toont een lijst van bestaande programmeurs met de volgende details:

* **identiteitskaart van de Programmer**: Een herkenningsteken van het media bedrijf binnen het systeem.
* **Kanalen**: Het aantal bijbehorende kanalen verbonden aan een programmeur.

![ Lijst van bestaande programmeurs ](assets/programmers-list.png)

*Lijst van bestaande programmeurs*

Typ de naam van de programmeur in de **bar van het Onderzoek** boven de lijst om meer over een programmeur te weten te komen.

## Programmeerconfiguraties beheren {#manage-programmer-conf}

Voer de volgende stappen uit om verschillende instellingen van een specifieke programmeur te beheren.

1. Selecteer het **lusje van Programmeurs** in het linkerpaneel.
1. Selecteer een programmeur in de lijst.
1. Selecteer een van de volgende tabbladen om de bijbehorende instellingen van de geselecteerde programmeur weer te geven en te bewerken:

   * [Kanalen](#channels)
   * [Certificaten](#certificates)
   * [Geregistreerde toepassingen](#registered-applications)
   * [Aangepaste schema&#39;s](#custom-schemes)

   ![ montages van de Programmer ](assets/programmer-settings.png)

   *montages van de Programmer*

>[!IMPORTANT]
>
> De Controle van de mening [ en duw verandert ](/help/authentication/tve-dashboard-review-push-changes.md) voor meer informatie bij het activeren van de configuratieveranderingen.

### Kanalen {#channels}

Op dit tabblad wordt een lijst weergegeven met kanalen die zijn gekoppeld aan een huidige programmeur. Selecteer een specifiek kanaal van deze lijst om tot gedetailleerde informatie in de [ sectie van Kanalen ](/help/authentication/tve-dashboard-channels.md) toegang te hebben.

Om een nieuw kanaal voor de geselecteerde programmeur toe te voegen, **voeg nieuw kanaal** van de hoger-juiste hoek van **Beschikbare Kanalen** sectie toe. Leer [ hoe te om een nieuw kanaal ](/help/authentication/tve-dashboard-channels.md#add-new-channel) toe te voegen.

![ voeg een nieuw kanaal ](assets/programmers-channels.png) toe

*voeg een nieuw kanaal* toe

### Certificaten {#certificates}

Dit lusje toont een lijst van [ beschikbare certificaten ](#available-certificates) die in de de encryptiesstromen van gebruikersmeta-gegevens worden gebruikt. Er worden details weergegeven over elk certificaat dat het volgende bevat:

* De status (of toegelaten voor **encryptie van gebruikersmeta-gegevens** gebruik of niet)
* Serienummer
* Naam van de emittentenorganisatie
* Naam van de organisatie die het onderwerp vormt
* Datum van afgifte
* Vervaldatum
* Een dropdown menu om gebruikersmeta-gegevens te coderen (als u **ja** selecteert, zal het certificaat gevoelige gebruikersinformatie, zoals zip codewaarden coderen).

#### Beschikbare certificaten {#available-certificates}

Deze certificaten fungeren als openbare of persoonlijke sleutels en worden gebruikt voor versleuteling van gebruikersmetagegevens. Alle kanalen die aan hetzelfde mediabedrijf zijn gekoppeld, kunnen deze certificaten gebruiken.

U kunt de volgende wijzigingen aanbrengen in beschikbare certificaten:

* [Nieuw certificaat toevoegen](#add-new-certificate)
* [Certificaat verwijderen](#delete-certificate)

##### Nieuw certificaat toevoegen {#add-new-certificate}

Ga als volgt te werk om een nieuw certificaat toe te voegen.

1. Selecteer **nieuw certificaat** bij de hoger-juiste hoek van de **Beschikbare sectie van Certificaten** toevoegen.

   ![ voeg een nieuw certificaat toe ](assets/programmer-add-new-certificate.png)

   *voeg een nieuw certificaat toe*

1. Plak de openbare sleutel van uw certificaat in het **Nieuwe certificaat** dialoogvakje.
1. Selecteer **toevoegen certificaat**.

   Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Om het nieuwe certificaat te gebruiken dat in de **Beschikbare sectie van Certificaten** wordt vermeld, ga met de [ overzicht en duw veranderingen ](/help/authentication/tve-dashboard-review-push-changes.md) stroom te werk.

1. Bepaal de plaats van het nieuwe certificaat in de lijst van **Beschikbare Certificaten**.

   >[!IMPORTANT]
   >
   > Zorg ervoor dat uw systemen up-to-date zijn en klaar zijn om het nieuwe certificaat te gebruiken.

1. Selecteer **ja** van **Gebruikt aan gecodeerde gebruikersmeta-gegevens** dropdown menu om een nieuw certificaat te activeren.

##### Certificaat verwijderen {#delete-certificate}

Ga als volgt te werk om een certificaat te verwijderen.

1. Beweeg op het certificaat u van de lijst van **Beschikbare certificaten** wilt schrappen.
1. Selecteer **verwijderen**.

   ![ verwijder het geselecteerde certificaat ](assets/programmer-remove-certificate.png)

   *verwijder het geselecteerde certificaat*

1. Selecteer **Schrapping** op het **certificaat van de Schrapping** dialoogvakje.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Het certificaat zal van de **Beschikbare certificaten** sectie slechts na [ overzicht en duw veranderingen ](/help/authentication/tve-dashboard-review-push-changes.md) worden geschrapt.

### Geregistreerde toepassingen {#registered-applications}

Dit tabblad bevat een lijst met toepassingsregistraties.

### Aangepaste schema&#39;s {#custom-schemes}

Op dit tabblad wordt een lijst met aangepaste schema&#39;s weergegeven. De toepassingsregistratie van de mening [ iOS/tvOS ](/help/authentication/iostvos-application-registration.md).

## Nieuwe programmeur toevoegen {#add-new-programmer}

Ga als volgt te werk om een nieuwe programmeerentiteit toe te voegen.

1. Selecteer het **lusje van Programmeurs** in het linkerpaneel.
1. Selecteer **nieuwe programmeur** bij de hoger-juiste hoek van de **sectie van Programmers** toevoegen.

   ![ voeg een nieuwe programmeur ](assets/add-new-programmer.png) toe

   *voeg een nieuwe programmeur* toe

1. Het bedrijfherkenningsteken van het type media in **identiteitskaart van de Programmer** in het **Nieuwe programmeur** dialoogvakje.
1. Typ een commerciële merknaam u in de console onder **naam van de Vertoning** wilt worden getoond.
1. Selecteer **programmer** toevoegen.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Om de nieuwe programmeur te gebruiken die in de **sectie wordt vermeld van 0} Programmers {, ga met de [ overzicht te werk en druk veranderingen ](/help/authentication/tve-dashboard-review-push-changes.md) stroom.**
