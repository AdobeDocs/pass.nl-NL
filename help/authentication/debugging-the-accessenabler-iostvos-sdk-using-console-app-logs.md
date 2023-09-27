---
title: Fouten opsporen in de AccessEnabler iOS/tvOS SDK met behulp van console-app-logboeken
description: Fouten opsporen in de AccessEnabler iOS/tvOS SDK met behulp van console-app-logboeken
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Fouten opsporen in de AccessEnabler iOS/tvOS SDK met behulp van console-app-logboeken {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Overzicht

Het bereik van dit document is om de evolutie van het iOS/tvOS SDK-logboekmechanisme van AccessEnabler vast te leggen en weer te geven, naast enkele nuttige details voor foutopsporing van het AccessEnabler-framework met behulp van de Console-app-logboeken.

## Status van het logboekmechanisme

Het doel van het registratiemechanisme van AccessEnabler iOS/tvOS is het uitzenden van nuttige berichten voor het oplossen van mogelijke problemen die een toepassing die het kader AccessEnabler gebruikt zou kunnen ontmoeten toe te schrijven aan het.

### AccessEnabler iOS/tvOS 3.5.0 en hoger

Vanaf de AccessEnabler iOS/tvOS 3.5.0-versie introduceert het registratiemechanisme de volgende verbeteringen als wijzigingen:

* Het AccessEnabler-framework gebruikt Apple aanbevolen [OSLog](https://developer.apple.com/documentation/os/oslog) uitvoering.

* AccessEnabler-framework introduceert de mogelijkheid om toepassingslogboeken van de console-toepassing te filteren op basis van subsysteem: **com.adobe.pass.AccessEnabled**. Alle berichten die door de SDK worden uitgegeven, maken deel uit van com.adobe.pass.AccessEnabler.

* Het AccessEnabler-framework introduceert de mogelijkheid om consolepagina&#39;s te filteren op basis van Willekeurige code (voorvoegsel): **[AccessEnabled]**. Alle berichten die door de SDK worden uitgezonden, worden voorafgegaan door [AccessEnabled].

* Het AccessEnabler-framework introduceert de mogelijkheid om toepassingslogboeken van de console-toepassing te filteren op basis van categorie: **foutopsporing**, **fout** in combinatie met een van de twee bovengenoemde criteria: Subsysteem of Willekeurig (prefix).

## Foutopsporing met console-app-logboeken

Afhankelijk van de kwesties die worden onderzocht kunt u de het registreren berichten willen omvatten of uitsluiten die door het kader AccessEnabler worden uitgegeven, daarom kunt u onder sommige nuttige details vinden die u tijdens onderzoeken en wanneer het gebruiken van de toepassingslogboeken van de Console kunnen helpen.


### AccessEnabler iOS/tvOS 3.5.0 en hoger

#### Inclusief {#including}

Om te beginnen om het even welke registrerenberichten te kunnen zien die door het kader AccessEnabler worden uitgegeven u **moet** Selecteer &#39;Inclusief berichten over info&#39; en &#39;Inclusief foutopsporingsberichten&#39; in het gedeelte Actie van de Console-app, zoals weergegeven in de onderstaande afbeelding.

![](assets/include-info-debug-msg.png)


Om de functionaliteit van AccessEnabler iOS/tvOS SDK en **zie** het AccessEnabler-framework logt u:

* Zoeken in de consoletoepassing met **Subsysteem** Deze optie is gelijk aan de waarde com.adobe.pass.AccessEnabler in de onderstaande afbeelding.

![](assets/subsys-console-app.png)

* Zoeken in de consoletoepassing met **Alle** Deze optie bevat de optie
  [AccessEnabled] de waarde in de onderstaande afbeelding.

![](assets/any-optn-console-app.png)

Naast de twee bovenstaande criteria kunt u ook de opdracht **Categorie** in combinatie met **Subsysteem** of **Willekeurig (voorvoegsel)** om uitdrukkelijk te zoeken naar **foutopsporing** of **fout** niveauberichten die door AccessEnabler iOS/tvOS SDK worden uitgegeven.

#### Exclusief

Om de functionaliteit van andere componenten en **uitsluiten** het AccessEnabler-framework logt u:

* Zoeken in de consoletoepassing met **Subsysteem** optie die niet gelijk is aan de waarde com.adobe.pass.AccessEnabler.
* Zoeken in de consoletoepassing met **Alle** Deze optie bevat niet de optie [AccessEnabled] waarde.

## Melding van een probleem

Neem de volgende suggesties in overweging wanneer u een probleem meldt voor Adobe Pass-verificatie:

* Geef de reproductiestappen op.
* Geef de versie(s) van het besturingssysteem en het apparaatmodel of de apparaatmodellen op waarop het probleem zich voordoet.
* Geef de versie op van de AccessEnabler iOS/tvOS SDK die het probleem ondervindt.
* Leg alle logboekberichten van AccessEnabler iOS/tvOS SDK vast met een van de twee opties in het dialoogvenster [Inclusief](#including) sectie.
