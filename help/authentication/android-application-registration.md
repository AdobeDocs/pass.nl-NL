---
title: Registratie van Android-toepassingen
description: Registratie van Android-toepassingen
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---

# Registratie van Android-toepassingen {#android-application-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Vanaf versie 3.0 van de Android AccessEnabler SDK veranderen we het verificatiemechanisme met de servers van de Adobe. In plaats van het gebruiken van een openbare sleutel en een geheim systeem om requestorID te ondertekenen, introduceren wij het concept een koord van de Verklaring van de Software dat kan worden gebruikt om een toegangstoken te verkrijgen dat later voor alle vraag wordt gebruikt die SDK aan onze servers maakt. Naast een verklaring van de Software zult u ook een diepe verbinding voor uw toepassing moeten creëren.

Zie voor meer informatie [Dynamische clientregistratie](/help/authentication/dynamic-client-registration.md)

## Wat is een Software Statement? {#what}

Een verklaring van de Software is een teken JWT dat informatie over uw toepassing bevat. Elke toepassing moet een unieke software-instructie hebben die door onze servers wordt gebruikt om de toepassing in het systeem van de Adobe te identificeren.

De verklaring van de Software moet worden overgegaan wanneer u initialiseert `AccessEnabler` SDK. Deze wordt gebruikt om de toepassing met Adobe te registreren. Bij registratie ontvangt de SDK een client-id en een clientgeheim, die worden gebruikt om een toegangstoken te verkrijgen. Om het even welke vraag die SDK aan Adobe servers maakt vereist een geldig toegangstoken. De SDK is verantwoordelijk voor het registreren van de toepassing, het verkrijgen en het vernieuwen van het toegangstoken.

>[!NOTE]
>
>Softwareinstructies zijn specifiek voor de toepassing en een afzonderlijke Software Statement kan niet voor meerdere toepassingen worden gebruikt. Houd er rekening mee dat softwareinstructies op programmeerniveau dezelfde beperkingen hebben. Ze kunnen alleen worden gebruikt voor één toepassing, of het nu om één kanaal of meerdere kanalen betreft.

## Hoe te om een Verklaring van de Software te verkrijgen {#how-to-get-ss}

Hier zijn manieren u een Verklaring van de Software kunt verkrijgen.

### Als u toegang hebt tot het TVE-dashboard van de Adobe

1. Uw browser openen en naar [Adobe Pass TVE-dashboard](https://console.auth.adobe.com).

1. Navigeren naar **[!UICONTROL Channels]** selecteert u vervolgens het kanaal.

1. Ga naar de **[!UICONTROL Registered Applications]** tab.

1. Klikken **[!UICONTROL Add new application]**.

1. Geef de toepassing een naam en geef een versie op.

1. Selecteer de platforms waarop de toepassing beschikbaar zal zijn (in dit geval Android).

1. Geef een **[!UICONTROL Domain Name]** door van een lijst van domeinen te kiezen die reeds voor uw Programmer worden gevormd.

1. Verduw uw veranderingen in de server, dan navigeer terug naar uw Kanaal **[!UICONTROL Registered Applications]** tab.

   Er moet een lijst met alle geregistreerde toepassingen worden weergegeven. Selecteren **[!UICONTROL Download]** in de toepassing die u hebt gemaakt. Mogelijk moet u een paar minuten wachten voordat de Software Statement wordt weergegeven. U kunt deze instructie dan downloaden.

   Een tekstbestand wordt gedownload. Gebruik de inhoud ervan als de Software Statement.

Zie voor meer informatie [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md)

### Als u geen toegang hebt tot het TVE-dashboard van de Adobe

Een ticket verzenden naar `tve-support@adobe.com`. Neem de benodigde informatie op, zoals kanaal, toepassingsnaam, versie en platforms. Iemand van ons ondersteuningsteam zal een softwareverklaring voor u creëren.

## Hoe te om de Verklaring van de Software te gebruiken {#how-to-use-ss}

Nadat u uw Verklaring van de Software verkrijgt, moet u het als parameter in de aannemer van Enabler van de Toegang overgaan. Wij adviseren het ontvangen van de Verklaring van de Software op een verre plaats. Op deze manier kunt u de Software Statement eenvoudig intrekken en wijzigen zonder een nieuwe versie van uw toepassing vrij te geven.

## Een diepe koppeling voor uw toepassing maken en gebruiken {#create}

Bij Android gebruikt u als diepe koppelingswaarde de omgekeerde domeinnaam die is geselecteerd toen u de Software-instructie maakte.

Gemaakte diepe koppeling moet een unieke waarde hebben op het Android-apparaat. Wanneer de veelvoudige toepassingen de zelfde diepe verbindingswaarde gebruiken, zullen de authentificatie en logout stromen zich mengen.

## Hoe de verklaring van de Software en de diepe verbinding gebruiken {#use-both}

In het bronbestand van uw toepassing `strings.xml` Voeg de volgende code toe:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
