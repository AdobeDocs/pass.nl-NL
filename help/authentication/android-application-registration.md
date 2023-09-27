---
title: Registratie van Android-toepassingen
description: Registratie van Android-toepassingen
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Registratie van Android-toepassingen {#android-application-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Vanaf versie 3.0 van de Android AccessEnabler SDK veranderen we het verificatiemechanisme met de servers van de Adobe. In plaats van het gebruiken van een openbare sleutel en een geheim systeem om requestorID te ondertekenen, introduceren wij het concept een het verklaringskoord van de Software dat kan worden gebruikt om een toegangstoken te verkrijgen dat later voor alle vraag wordt gebruikt die SDK aan onze servers maakt. Naast een software-instructie moet u ook een diepe koppeling voor uw toepassing maken.

Zie voor meer informatie [Dynamische clientregistratie](/help/authentication/dynamic-client-registration.md)

## Wat is een Software Statement? {#what}

Een verklaring van de Software is een teken JWT dat informatie over uw toepassing bevat. Elke toepassing moet een unieke software-instructie hebben die door onze servers wordt gebruikt om de toepassing in het systeem van de Adobe te identificeren. De verklaring van de Software moet worden overgegaan wanneer u AccessEnabler SDK initialiseert en het zal worden gebruikt om de toepassing met Adobe te registreren. Na registratie ontvangt de SDK een client-id en een clientgeheim die worden gebruikt om een toegangstoken te verkrijgen. Om het even welke vraag die SDK aan onze servers maakt zal een geldig toegangstoken vereisen. De SDK is verantwoordelijk voor het registreren van de toepassing, het verkrijgen en vernieuwen van het toegangstoken.

>[!NOTE]
>
>Softwareinstructies zijn specifiek voor de toepassing en een afzonderlijke Software Statement kan niet voor meerdere toepassingen worden gebruikt. Houd er rekening mee dat softwareinstructies op programmeerniveau dezelfde beperkingen hebben. Ze kunnen alleen worden gebruikt voor één toepassing, of het nu om één kanaal of meerdere kanalen betreft.

## Hoe te om een Verklaring van de Software te verkrijgen? {#how-to-get-ss}

### Als u toegang hebt tot het TVE-dashboard van de Adobe:

* Uw browser openen en naar [Adobe Pass TVE-dashboard](https://console.auth.adobe.com).
* Navigeren naar `Channels` en selecteert u het kanaal.
* Navigeren naar `Registered Applications` Tab.
* Klikken op `Add new application`.
* Geef een naam en een versie op voor uw toepassing en selecteer de platforms waarop deze beschikbaar zal zijn. Android in onze zaak.
* Verstrek een Naam van het Domein door van een lijst van domeinen te kiezen die reeds voor uw Programmer worden gevormd.
* Duw uw veranderingen aan de server en navigeer dan terug naar uw Gedeponeerde Toepassingen van het Kanaal tabel.
* Er moet een lijst met alle geregistreerde toepassingen worden weergegeven. Selecteer de **Downloaden** op de toepassing die u net hebt gemaakt. Mogelijk moet u een paar minuten wachten voordat de Software Statement wordt weergegeven. U kunt deze instructie dan downloaden.
* Er wordt een tekstbestand gedownload. Gebruik de inhoud ervan als de Software Statement.

Zie voor meer informatie [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md)

### Als u geen toegang hebt tot het TVE-dashboard van de Adobe:

Een ticket verzenden naar `tve-support@adobe.com`. Neem alle benodigde informatie op, zoals kanaal, toepassingsnaam, versie en platforms. Iemand van ons ondersteuningsteam zal een softwareinstructie voor u maken.

## Hoe de Software Statement te gebruiken? {#how-to-use-ss}

Nadat u uw Verklaring van de Software krijgt moet u het als parameter in de aannemer van Enabler van de Toegang overgaan. Wij adviseren het ontvangen van de Verklaring van de Software op een verre plaats. Op deze manier kunt u de Software Statement eenvoudig intrekken en wijzigen zonder een nieuwe versie van uw toepassing vrij te geven.

## Een diepe koppeling voor uw toepassing maken en gebruiken {#create}

Bij Android gebruikt u als diepe koppelingswaarde de omgekeerde domeinnaam die is geselecteerd toen u de Software-instructie maakte.

Gemaakte diepe koppeling moet een unieke waarde hebben op het Android-apparaat. Wanneer de veelvoudige toepassingen de zelfde diepe verbindingswaarde gebruiken, zullen de authentificatie en logout stromen zich mengen.

## Hoe de verklaring van de Software en de diepe verbinding gebruiken {#use-both}

In het bronbestand van uw toepassing `strings.xml` Voeg de volgende code toe:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
