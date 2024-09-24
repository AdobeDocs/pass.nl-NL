---
title: Android-toepassingsregistratie
description: Android-toepassingsregistratie
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Android-toepassingsregistratie {#android-application-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Vanaf versie 3.0 van de Android AccessEnabler SDK veranderen we het verificatiemechanisme met de servers van de Adobe. In plaats van het gebruiken van een openbare sleutel en een geheim systeem om requestorID te ondertekenen, introduceren wij het concept een koord van de Verklaring van de Software dat kan worden gebruikt om een toegangstoken te verkrijgen dat later voor alle vraag wordt gebruikt die SDK aan onze servers maakt. Naast een verklaring van de Software zult u ook een diepe verbinding voor uw toepassing moeten creëren.

Voor meer informatie, zie [ Dynamisch Overzicht van de Registratie van de Cliënt 1}.](./dcr-api/dynamic-client-registration-overview.md)

## Wat is een Software Statement? {#what}

Een verklaring van de Software is een teken JWT dat informatie over uw toepassing bevat. Elke toepassing moet een unieke software-instructie hebben die door onze servers wordt gebruikt om de toepassing in het systeem van de Adobe te identificeren.

De Software-instructie moet worden doorgegeven wanneer u de `AccessEnabler` SDK initialiseert. Deze wordt gebruikt om de toepassing met Adobe te registreren. Bij registratie ontvangt de SDK een client-id en een clientgeheim, die worden gebruikt om een toegangstoken te verkrijgen. Om het even welke vraag die SDK aan Adobe servers maakt vereist een geldig toegangstoken. De SDK is verantwoordelijk voor het registreren van de toepassing, het verkrijgen en het vernieuwen van het toegangstoken.

>[!NOTE]
>
>Softwareinstructies zijn specifiek voor de toepassing en een afzonderlijke Software Statement kan niet voor meerdere toepassingen worden gebruikt. Houd er rekening mee dat softwareinstructies op programmeerniveau dezelfde beperkingen hebben. Ze kunnen alleen worden gebruikt voor één toepassing, of het nu om één kanaal of meerdere kanalen betreft.

## Hoe te om een Verklaring van de Software te verkrijgen {#how-to-get-ss}

Hier zijn manieren u een Verklaring van de Software kunt verkrijgen.

### Als u toegang hebt tot het TVE-dashboard van de Adobe

1. Open uw browser en navigeer aan [ Adobe Pass TVE Dashboard ](https://experience.adobe.com/#/pass/authentication).

1. Navigeer naar de sectie **[!UICONTROL Channels]** en selecteer vervolgens het kanaal.

1. Ga naar het tabblad **[!UICONTROL Registered Applications]**.

1. Klik op **[!UICONTROL Add new application]**.

1. Geef de toepassing een naam en geef een versie op.

1. Selecteer de platforms waarop de toepassing beschikbaar zal zijn (Android in dit geval).

1. Verstrek **[!UICONTROL Domain Name]** door van een lijst van domeinen te kiezen die reeds voor uw Programmer worden gevormd.

1. Verplaats uw wijzigingen naar de server en navigeer vervolgens terug naar de tab **[!UICONTROL Registered Applications]** van uw kanaal.

   Er moet een lijst met alle geregistreerde toepassingen worden weergegeven. Selecteer **[!UICONTROL Download]** in de toepassing die u hebt gemaakt. Mogelijk moet u een paar minuten wachten voordat de Software Statement wordt weergegeven. U kunt deze instructie dan downloaden.

   Een tekstbestand wordt gedownload. Gebruik de inhoud ervan als de Software Statement.

Voor meer informatie, zie [ Dynamisch Beheer van de Registratie van de Cliënt ](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Als u geen toegang hebt tot het TVE-dashboard van de Adobe

Een ticket verzenden naar `tve-support@adobe.com` . Neem de benodigde informatie op, zoals kanaal, toepassingsnaam, versie en platforms. Iemand van ons ondersteuningsteam zal een softwareverklaring voor u creëren.

## Hoe te om de Verklaring van de Software te gebruiken {#how-to-use-ss}

Nadat u uw Verklaring van de Software verkrijgt, moet u het als parameter in de aannemer van Enabler van de Toegang overgaan. Wij adviseren het ontvangen van de Verklaring van de Software op een verre plaats. Op deze manier kunt u de Software Statement eenvoudig intrekken en wijzigen zonder een nieuwe versie van uw toepassing vrij te geven.

## Een diepe koppeling voor uw toepassing maken en gebruiken {#create}

Gebruik in Android de omgekeerde waarde van de domeinnaam bij het maken van de Software-instructie als diepe koppelingswaarde

Gemaakte diepe koppeling moet een unieke waarde hebben op het Android-apparaat. Wanneer de veelvoudige toepassingen de zelfde diepe verbindingswaarde gebruiken, zullen de authentificatie en logout stromen zich mengen.

## Hoe de verklaring van de Software en de diepe verbinding gebruiken {#use-both}

Voeg de volgende code toe in het bronbestand van uw toepassing `strings.xml` :

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
