---
title: Amazon FireOS-toepassingsregistratie
description: Amazon FireOS-toepassingsregistratie
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Amazon FireOS-toepassingsregistratie {#amazon-fireos-application-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Inleiding {#intro}

Vanaf versie 3.0 van de FireOS AccessEnabler SDK veranderen we het verificatiemechanisme met de servers van de Adobe. In plaats van het gebruiken van een openbare sleutel en een geheim systeem om requestorID te ondertekenen, introduceren wij het concept een koord van de Verklaring van de Software dat kan worden gebruikt om een toegangstoken te verkrijgen dat later voor alle vraag wordt gebruikt die SDK aan onze servers maakt. Naast een Software Statement zult u ook een diepe verbinding voor uw toepassing moeten tot stand brengen.

Meer informatie, zie [Dynamische clientregistratie](/help/authentication/dynamic-client-registration.md)

## Wat is een Software Statement? {#what}

Een verklaring van de Software is een teken JWT dat informatie over uw toepassing bevat. Elke toepassing zou een unieke Verklaring van de Software moeten hebben die door onze servers wordt gebruikt om de toepassing in het systeem van de Adobe te identificeren. De verklaring van de Software moet worden overgegaan wanneer u AccessEnabler SDK initialiseert en het zal worden gebruikt om de toepassing met Adobe te registreren. Na registratie ontvangt de SDK een client-id en een clientgeheim die worden gebruikt om een toegangstoken te verkrijgen. Om het even welke vraag die SDK aan onze servers maakt zal een geldig toegangstoken vereisen. De SDK is verantwoordelijk voor het registreren van de toepassing, het verkrijgen en vernieuwen van het toegangstoken.

**Opmerking:** Softwareinstructies zijn specifiek voor de toepassing en een afzonderlijke Software Statement kan niet voor meerdere toepassingen worden gebruikt. Dit geldt ook voor toepassingen die toegang bieden tot meerdere kanalen.

## Hoe te om een Verklaring van de Software te verkrijgen? {#how-to}

### Als u toegang hebt tot het TVE-dashboard van de Adobe:

1. Uw browser openen en naar `https://console.auth.adobe.com`.

1. Ga naar de **[!UICONTROL Channels]** selecteert u vervolgens het kanaal.

1. Ga naar de **[!UICONTROL Registered Applications]** tab.

1. Klikken **[!UICONTROL Add new application]**.

1. Geef een naam en een versie op voor uw toepassing en selecteer de platforms waarop deze beschikbaar is (zoals Android).

1. Geef een **[!UICONTROL Domain Name]** door van een lijst van domeinen te kiezen die reeds voor uw Programmer worden gevormd.

1. Breng de wijzigingen door naar de server en navigeer vervolgens terug naar de kanalen van uw **[!UICONTROL Registered Applications]** tab.

   Er moet een lijst met alle geregistreerde toepassingen worden weergegeven.

1. Klikken **[!UICONTROL Download]** op de toepassing die u net hebt gemaakt.

   Mogelijk moet u een paar minuten wachten voordat de Software Statement klaar is voor downloaden.

   Een tekstbestand wordt gedownload. Gebruik de inhoud ervan als de Software Statement.

Meer informatie, zie [Dynamisch clientregistratiebeheer](/help/authentication/dynamic-client-registration-management.md)

### Als u geen toegang hebt tot het TVE-dashboard van de Adobe:

Een ticket verzenden naar [tve-support@adobe.com](mailto:tve-support@adobe.com). Neem alle benodigde informatie op, zoals het kanaal, de naam van de toepassing, de versie en de platforms. Iemand van ons ondersteuningsteam zal een softwareinstructie voor u maken.

## Hoe te om de Verklaring van de Software te gebruiken {#use}

Nadat u uw Verklaring van de Software verkrijgt, moet u het als parameter in de aannemer van Enabler van de Toegang overgaan. Adobe raadt u aan de Software Statement op een externe locatie te hosten. Op deze manier kunt u de Software Statement eenvoudig intrekken en wijzigen zonder een nieuwe versie van uw toepassing vrij te geven.

## Hoe te om de Verklaring van de Software te gebruiken {#use-both}

In het bronbestand van uw toepassing `strings.xml` Voeg de volgende code toe:

```XML
<string name="software_statement">softwarestatement value</string>
```
