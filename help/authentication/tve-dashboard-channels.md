---
title: Kanalen
description: Leer meer over kanalen en hun verschillende configuraties in het TVE-dashboard.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 0%

---


# Kanalen {#channels}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De **Kanalen** kunt u de instellingen voor de kanalen die aan een specifieke programmeur zijn gekoppeld, weergeven en beheren. U kunt [een nieuw kanaal toevoegen](#add-new-channel) volgens uw vereiste.

De **Kanalen** in het linkerdeelvenster wordt een lijst met gekoppelde kanalen weergegeven met de volgende details:

* **Weergavenaam**: De merknaam van het kanaal dat voor commerciële doeleinden wordt gebruikt.
* **Kanaal-id**: Een unieke id, ook wel de id van de aanvrager genoemd.
* **Integraties**: Het aantal verbindingen dat is gemaakt met [MVPD&#39;s](/help/authentication/glossary.md#mvpd).

![Lijst van bestaande kanalen](assets/channels-list.png)

*Lijst van bestaande kanalen*

Typ de naam van het kanaal in het dialoogvenster **Zoeken** balk boven de lijst voor meer informatie over het kanaal.

## Kanaalconfiguraties beheren {#manage-channel-conf}

Voer de stappen uit om verschillende instellingen van een specifiek kanaal te beheren.

1. Selecteer de **Kanalen** in het linkerdeelvenster.
1. Selecteer het kanaal in de beschikbare lijst.
1. Selecteer een van de volgende tabbladen om de bijbehorende instellingen van het geselecteerde kanaal weer te geven en te bewerken:

   * [Algemene instellingen](#general-settings)
   * [Integraties](#integrations)
   * [Certificaten](#certificates)
   * [Domeinen](#domains)
   * [Geregistreerde toepassingen](#registered-applications)
   * [Aangepaste schema&#39;s](#custom-schemes)

   ![Kanaalinstellingen](assets/channel-settings.png)

   *Kanaalinstellingen*

>[!IMPORTANT]
>
> Weergave [Wijzigingen controleren en duwen](/help/authentication/tve-dashboard-review-push-changes.md) voor meer informatie over het activeren van de configuratieveranderingen.

### Algemene instellingen {#general-settings}

Dit tabblad bevat **Kanaalgegevens** en **Analyseconfiguratie**.

#### Kanaalgegevens {#channel-information}

In deze sectie kunt u de volgende details bewerken:

* **Weergavenaam**: De merknaam van het kanaal dat voor commerciële doeleinden wordt gebruikt.

* **Standaard omleidings-URL**: De back-up leidt URL om verificatie en afmelden.

* **Foutmelding**: Bij selecteren **Ja**, sturen de SDK&#39;s van Adobe Pass foutrapporten naar Adobe Pass backend voor analyses.

![Kanaalgegevens bewerken](assets/channel-information.png)

*Kanaalgegevens bewerken*

#### Analyseconfiguratie {#analytics-configuration}

Deze sectie staat u toe om het door:sturen van de gebeurtenissen van de Authentificatie van Adobe Pass aan Adobe Analytics te vormen.

Inschakelen **Analyseconfiguratie**, contacteer uw Technische Manager van de Rekening (TAM) voor meer details over vestiging identiteitskaart van de Reeks van het Rapport (RSID).

![Analytische configuraties inschakelen](assets/channel-analytical-conf.png)

*Analytische configuraties inschakelen*

Selecteren **Nieuwe analyseconfiguratie toevoegen** meerdere configuraties toevoegen.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Om de nieuwe analytische configuratie van te gebruiken **Analyseconfiguratie** sectie, gaat u verder met de [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md) stroom.

### Integraties {#integrations}

Dit lusje toont een lijst van beschikbare integratie tussen het momenteel geselecteerde kanaal en MVPDs. De lijst geeft elke integratie samen met de status weer, en geeft aan of deze is ingeschakeld of niet. Selecteer een specifieke integratie in deze lijst voor toegang tot gedetailleerde informatie in het dialoogvenster [Integraties](tve-dashboard-integrations.md) sectie.

![Lijst met beschikbare integratie](assets/channel-integrations.png)

*Lijst met beschikbare integratie*

### Certificaten {#certificates}

Op dit tabblad wordt een lijst met [beschikbare certificaten](#available-certificates) en [overerfde beschikbare certificaten](#inherited-avail-certificates) gebruikt in de coderingsstromen van gebruikersmetagegevens. Er worden details weergegeven over elk certificaat dat het volgende bevat:

* De status (of ingeschakeld voor **gebruikersmetagegevenscodering** gebruik of niet)
* Serienummer
* Naam van de emittentenorganisatie
* Naam van de organisatie die het onderwerp vormt
* Datum van afgifte
* Vervaldatum
* Een vervolgkeuzemenu voor het versleutelen van gebruikersmetagegevens (als u **Ja**, versleutelt het certificaat gevoelige gebruikersgegevens, zoals zip-codewaarden.

#### Beschikbare certificaten {#available-certificates}

Deze certificaten fungeren als openbare of persoonlijke sleutels en worden gebruikt voor versleuteling van gebruikersmetagegevens.
U kunt de volgende wijzigingen aanbrengen in de sectie met beschikbare certificaten:

* [Nieuw certificaat toevoegen](#add-new-certificate)
* [Certificaat verwijderen](#delete-certificate)

##### Nieuw certificaat toevoegen {#add-new-certificate}

Ga als volgt te werk om een nieuw certificaat toe te voegen:

1. Selecteren **Nieuw certificaat toevoegen** boven aan het dialoogvenster **Beschikbare certificaten** sectie.

   ![Een nieuw certificaat toevoegen](assets/add-new-certificate.png)

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

1. Houd de muisaanwijzer boven het certificaat dat u uit de lijst met **Beschikbare certificaten**.
1. Selecteren **Verwijderen**.

   ![Het geselecteerde certificaat verwijderen](assets/channel-delete-certificate.png)

   *Het geselecteerde certificaat verwijderen*

1. Selecteren **Verwijderen** van de **Actief certificaat verwijderen** in.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Het certificaat wordt verwijderd uit het **Beschikbare certificaten** alleen secties na [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md).

#### Overgenomen beschikbare certificaten {#inherited-avail-certificates}

Mediabedrijven definiëren deze certificaten op hun eigen niveau. Alle kanalen die aan hetzelfde mediabedrijf zijn gekoppeld, kunnen deze certificaten gebruiken.

![Overgenomen beschikbare certificaten](assets/inherited-available-certificates.png)

*Overgenomen beschikbare certificaten*

### Domeinen {#domains}

Op dit tabblad wordt een lijst weergegeven met beschikbare domeinen via welke het desbetreffende kanaal communiceert met Adobe Pass-verificatie.

U kunt de volgende wijzigingen aanbrengen in domeinen:

* [Een nieuw domein toevoegen](#add-domains)
* [Domein verwijderen](#delete-domain)

>[!TIP]
>
> Voeg geen nieuw subdomein toe als de lijst een meer algemeen domein bevat.

#### Nieuw domein toevoegen {#add-domains}

Ga als volgt te werk om een domein toe te voegen.

1. Selecteren **Nieuw domein toevoegen** in de rechterbovenhoek van het **Beschikbare domeinen** sectie.

   ![Een nieuw domein toevoegen](assets/add-new-domain.png)

   *Een nieuw domein toevoegen*

1. Typ de naam van het domein in het dialoogvenster **Nieuw domein** in.

1. Selecteren **Domein toevoegen** om een nieuw domein voor het geselecteerde kanaal toe te voegen.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Om het nieuwe domein te gebruiken dat in **Beschikbare domeinen** sectie, gaat u verder met de [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md) stroom.

#### Domein verwijderen {#delete-domain}

Ga als volgt te werk om een domein te verwijderen.

1. Houd de muisaanwijzer boven het domein dat u uit de lijst met **Beschikbare domeinen**.
1. Selecteren **Verwijderen**.

   ![Het geselecteerde domein verwijderen](assets/remove-domain.png)

   *Het geselecteerde domein verwijderen*

1. Selecteren **Verwijderen** op de **Domein verwijderen** in.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Het domein wordt verwijderd uit het **Beschikbare domeinen** alleen secties na [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md).

Het geselecteerde domein is niet meer beschikbaar voor gebruik. Als gevolg hiervan verliest de toepassing die aan dit domein is gekoppeld toegang tot de Adobe Pass-verificatieservices.

### Geregistreerde toepassingen {#registered-applications}

Dit tabblad bevat een lijst met toepassingsregistraties. Weergave [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md) voor meer informatie .

### Aangepaste schema&#39;s {#custom-schemes}

Op dit tabblad wordt een lijst met aangepaste schema&#39;s weergegeven. Weergave [iOS/tvOS-toepassingsregistratie](/help/authentication/iostvos-application-registration.md) en [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md) voor meer informatie .

## Nieuw kanaal toevoegen {#add-new-channel}

Ga als volgt te werk om een nieuw kanaal toe te voegen.

1. Selecteer de **Kanalen** in het linkerdeelvenster.
1. Selecteren **Nieuw kanaal toevoegen** in de rechterbovenhoek van het **Kanalen** sectie.

   ![Een nieuw kanaal toevoegen](assets/add-new-channel.png)

   *Een nieuw kanaal toevoegen*

1. Selecteren **Programma-id** in het vervolgkeuzemenu in het dialoogvenster **Nieuw kanaal** in.

1. Typ een unieke id in **Kanaal-id**.
1. Typ de merknaam van het kanaal dat voor commerciële doeleinden in het **Weergavenaam**.
1. Selecteren **Kanaal toevoegen**.

Er is een nieuwe configuratiewijziging gemaakt en deze is gereed voor serverupdate. Als u het nieuwe kanaal wilt gebruiken dat in het dialoogvenster **Kanalen** sectie, gaat u verder met de [revisie- en pushwijzigingen](/help/authentication/tve-dashboard-review-push-changes.md) stroom.

