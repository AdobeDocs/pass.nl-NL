---
title: TVE-dashboardomgevingen
description: Begrijp het gebruik en het werk van verschillende milieu's in het Dashboard van TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Omgevingen {#environments}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Het TVE-dashboard biedt verschillende omgevingen die zijn aangepast voor specifieke doeleinden binnen de Adobe Pass-verificatie. Er zijn twee primaire omgevingen:

* **Prequal**: De pre-kwalificatieomgeving dient als testbasis voor het voorbereiden en testen van nieuwe gebouwen voordat deze worden geïmplementeerd.

* **Geen**: In de releaseomgeving worden de voltooide en geteste builds gehost voor productie.

Binnen elke omgeving zijn er twee verschillende profielen:

* **Staging**: Het staging-profiel maakt verbinding met de staging-server van de MVPD voor het testen en valideren van integraties voordat u live gaat.

* **Productie**: Het productieprofiel sluit aan op het productieprofiel van het MVPD voor werkelijke productieactiviteiten.

## Gebruik hoofdletters

De omgevingen in het TVE-dashboard hebben gedurende de hele levenscyclus van de toepassing verschillende gebruiksgevallen. Met deze omgevingen kunt u:

### Periodieke stapeling

* Valideer nieuwe niet-vrijgegeven eigenschappen van de server van de Authentificatie van Adobe Pass gebruikend de het opvoeren eindpunten van MVPD.
* Primair gebruikt door het productteam van de Authentificatie van Adobe Pass om nieuwe integratie toe te voegen en te bevestigen MVPD.

### Prequente productie

* Valideer nieuwe niet-vrijgegeven eigenschappen of configuraties van de server van de Authentificatie van Adobe Pass gebruikend de productie eindpunten van MVPD.
* Valideer nieuwe toepassingsversies voor elk kanaal gebruikend de productie eindpunten van MVPD.
* Bevestig elke configuratieverandering alvorens het aan productie te duwen.

### Staging vrijgeven

* Valideer nieuwe toepassingsversies voor elk kanaal gebruikend het opvoeren eindpunten van MVPD.
* Voer prestatie- of capaciteitstests uit in deze omgeving.

### Geen productie

* Vertegenwoordigt de live omgeving met de nieuwste Adobe Pass-releasebuild die algemeen beschikbaar is voor alle eindgebruikers.
* Behoudt stabiliteit in code en configuratie en weerspiegelt onmiddellijk configuratieveranderingen in de toepassing van de eindgebruiker.

## Switch-omgevingen {#switch-environments}

Voer de stappen uit om te schakelen tussen Adobe Pass Authentication TVE Dashboard-omgevingen.

1. Meld u aan met uw programmeergegevens.
1. Selecteer de vereiste testomgeving of productieomgeving in het menu **Omgeving** vervolgkeuzemenu boven aan het linkerdeelvenster.

   ![Vervolgkeuzelijst voor TVE-dashboardomgevingen](assets/tve-dashboard-env.png)

   *Het vervolgkeuzemenu voor de Adobe Pass Authentication TVE-dashboardomgeving*

>[!NOTE]
>
> De configuraties kunnen in elke omgeving variëren op basis van uw instellingen.

