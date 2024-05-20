---
title: Programmeurs
description: Leer over programmeurs en zijn configuraties binnen het dashboard van TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmeurs {#programmers}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De **Programmeurs** kunt u de instellingen voor het [programmeurs](/help/authentication/glossary.md#programmer) gekoppeld aan uw rekeningrechten. U kunt [een nieuwe programmeur toevoegen](#add-new-programmer) volgens uw vereiste.

De **Programmeurs** in het linkerdeelvenster wordt een lijst met bestaande programmeurs weergegeven met de volgende gegevens:

* **Programma-id**: Een id van een mediabedrijf binnen het systeem.
* **Kanalen**: Het aantal gekoppelde kanalen dat aan een programmeur is gekoppeld.

![Lijst met bestaande programmeurs](assets/programmers-list.png)

*Lijst met bestaande programmeurs*

Typ de naam van de programmeur in het dialoogvenster **Zoeken** balk boven de lijst voor meer informatie over een programmeur.

## Programmeerconfiguraties beheren {#manage-programmer-conf}

Voer de volgende stappen uit om verschillende instellingen van een specifieke programmeur te beheren.

1. Selecteer de **Programmeurs** in het linkerdeelvenster.
1. Selecteer een programmeur in de lijst.
1. Selecteer een van de volgende tabbladen om de bijbehorende instellingen van de geselecteerde programmeur weer te geven en te bewerken:

   * [Kanalen](#channels)
   * [Certificaten](#certificates)
   * [Geregistreerde toepassingen](#registered-applications)
   * [Aangepaste schema&#39;s](#custom-schemes)

   ![Programmeerinstellingen](assets/programmer-settings.png)

   *Programmeerinstellingen*

>[!IMPORTANT]
>
> Weergave [Wijzigingen controleren en duwen](/help/authentication/tve-dashboard-review-push-changes.md) voor meer informatie over het activeren van de configuratieveranderingen.

### Kanalen {#channels}

Op dit tabblad wordt een lijst weergegeven met kanalen die zijn gekoppeld aan een huidige programmeur. Selecteer een specifiek kanaal in deze lijst voor toegang tot gedetailleerde informatie in het dialoogvenster [Kanalen](/help/authentication/tve-dashboard-channels.md) sectie.

Als u een nieuw kanaal voor de geselecteerde programmeur wilt toevoegen, selecteert u **Nieuw kanaal toevoegen** in de rechterbovenhoek van **Beschikbare kanalen** sectie. Meer informatie [hoe te om een nieuw kanaal toe te voegen](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Een nieuw kanaal toevoegen](assets/programmers-channels.png)

*Een nieuw kanaal toevoegen*

### Certificaten {#certificates}

Op dit tabblad wordt een lijst met [beschikbare certificaten](#available-certificates) gebruikt in de coderingsstromen van gebruikersmetagegevens. Er worden details weergegeven over elk certificaat dat het volgende bevat:

* De status (of ingeschakeld voor **gebruikersmetagegevenscodering** gebruik of niet)
* Serienummer
* Naam van de emittentenorganisatie
* Naam van de organisatie die het onderwerp vormt
* Datum van afgifte
* Vervaldatum
* Een vervolgkeuzemenu voor het versleutelen van gebruikersmetagegevens (Als u **Ja**, versleutelt het certificaat gevoelige gebruikersgegevens, zoals postcodewaarden).

#### Beschikbare certificaten {#available-certificates}

Deze certificaten fungeren als openbare of persoonlijke sleutels en worden gebruikt voor versleuteling van gebruikersmetagegevens. Alle kanalen die aan hetzelfde mediabedrijf zijn gekoppeld, kunnen deze certificaten gebruiken.

U kunt de volgende wijzigingen aanbrengen in beschikbare certificaten:

* [Nieuw certificaat toevoegen](#add-new-certificate)
* [Certificaat verwijderen](#delete-certificate)

##### Nieuw certificaat toevoegen {#add-new-certificate}

Ga als volgt te werk om een nieuw certificaat toe te voegen.

1. Selecteren **Nieuw certificaat toevoegen** in de rechterbovenhoek van het **Beschikbare certificaten** sectie.

   ![Een nieuw certificaat toevoegen](assets/programmer-add-new-certificate.png)

   *Een nieuw certificaat toevoegen*

1. Plak de openbare sleutel van uw certificaat in **Nieuw certificaat** in.
1. Selecteren **Certificaat toevoegen**.

   Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Als u het nieuwe certificaat wilt gebruiken dat in het dialoogvenster **Beschikbare certificaten** sectie, gaat u verder met de [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md) stroom.

1. Zoek het nieuwe certificaat in de lijst met **Beschikbare certificaten**.

   >[!IMPORTANT]
   >
   > Zorg ervoor dat uw systemen up-to-date zijn en klaar zijn om het nieuwe certificaat te gebruiken.

1. Selecteren **Ja** van **Wordt gebruikt voor gecodeerde gebruikersmetagegevens** een nieuw certificaat activeren.

##### Certificaat verwijderen {#delete-certificate}

Ga als volgt te werk om een certificaat te verwijderen.

1. Houd de muisaanwijzer boven het certificaat dat u wilt verwijderen uit de lijst met **Beschikbare certificaten**.
1. Selecteren **Verwijderen**.

   ![Het geselecteerde certificaat verwijderen](assets/programmer-remove-certificate.png)

   *Het geselecteerde certificaat verwijderen*

1. Selecteren **Verwijderen** op de **Certificaat verwijderen** in.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Het certificaat wordt verwijderd uit het **Beschikbare certificaten** alleen secties na [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md).

### Geregistreerde toepassingen {#registered-applications}

Dit tabblad bevat een lijst met toepassingsregistraties. Weergave [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md)voor meer informatie.

### Aangepaste schema&#39;s {#custom-schemes}

Op dit tabblad wordt een lijst met aangepaste schema&#39;s weergegeven. Weergave [iOS/tvOS-toepassingsregistratie](/help/authentication/iostvos-application-registration.md) en [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md)voor meer informatie.

## Nieuwe programmeur toevoegen {#add-new-programmer}

Ga als volgt te werk om een nieuwe programmeerentiteit toe te voegen.

1. Selecteer de **Programmeurs** in het linkerdeelvenster.
1. Selecteren **Nieuwe programmeur toevoegen** in de rechterbovenhoek van het **Programmeurs** sectie.

   ![Een nieuwe programmeur toevoegen](assets/add-new-programmer.png)

   *Een nieuwe programmeur toevoegen*

1. Id van mediabedrijf typen in **Programma-id** in de **Nieuwe programmeur** in.
1. Typ een commerciële merknaam die u in de console onder wilt tonen **Weergavenaam**.
1. Selecteren **Programmeur toevoegen**.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Om de nieuwe programmeur te gebruiken die in **Programmeurs** sectie, gaat u verder met de [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md) stroom.

