---
title: Gebruik hoofdletters
description: Gebruik gevallen in Gelijktijdige controle.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Gevallen gebruiken {#use-cases}

Het belangrijkste geval van het gebruik van de Dienst van de Tellende van de Stroom telt het aantal gezamenlijke videostromen die door een gebruiker worden gecontroleerd en verstrekt een besluit betreffende zijn gelijktijdig gebruik voor zelfde rekening identiteitskaart

Om het gebruik door abonnee te controleren, is er een behoefte aan de gecentraliseerde dienst die gebruikersactiviteit kan bijeenvoegen ongeacht of het op de website of de toepassing van de programmeur, op het de inhoudsportaal van MVPD of op een syndicated bezit gebeurt.

De belangrijkste gebruiksgevallen die door deze gecentraliseerde dienst worden gesteund moeten zijn:

1. Zodra een abonnee begint te letten op een video, kan de toepassing **een het stromen zitting** initialiseren en **beginnen die activiteitengegevens** melden.
1. In de zelfde centrale dienst, zal een andere instantie ***besluiten van CM*** ontvangen - voor het geval dat de toepassing één of meerdere beleid heeft dat in de dienst van cm wordt geregistreerd, zal de dienst met toegangsbesluit antwoorden dat op de huidige activiteit wordt gebaseerd.

## Vaak voorkomende gevallen {#common-use-cases}

### Standaardstreamlimiet

Beperk het aantal gelijktijdige streams per abonnee in al uw toepassingen.

### Apparaatgebaseerde beperkingen

Slechts een bepaald aantal streams per apparaattype toestaan (mobiel, tablet, tv, enz.).

### Inhoudspecifieke regels

Verschillende limieten toepassen op live-inhoud versus VOD-inhoud.

### Op locatie gebaseerd beleid

Streaming beperken op basis van geografische locatie of netwerktype.

## Een sessie maken {#create-session}

Met deze API-aanroep kan de client een nieuwe CM-sessie maken wanneer de gebruiker op de knop &quot;Afspelen&quot; drukt om inhoud te bekijken. De serverreactie bevat de nieuwe stream-URL (die de stream-id bevat) om de stream in leven te houden en de tijd waarop de time-out van de stream plaatsvindt. Verwacht wordt dat de clienttoepassing activiteit via hartslagen rapporteert. De initialisatieaanroep van de sessie moet metagegevens bevatten in de vorm van sleutel-/waardeparen die als formuliergegevens (of parameters voor queryreeksen) worden verzonden. Bovendien zal de reactie ook een vlag omvatten om erop te wijzen of de playback &quot;beleid volgzaam&quot;is. Als dit niet het geval is, is het afspelen niet toegestaan.

## Rapportage {#reporting-activity}

Nadat een sessie is gemaakt, moet de toepassing regelmatig hartslagen verzenden om de desbetreffende stream actief te laten blijven. Bovendien wordt aangeraden dat de client-app de stream stopt zodra de gebruiker het afspelen heeft gestopt, zodat de stream tot de time-out niet als actief wordt beschouwd.

De reactie van de hartslagvraag kan de cliënttoepassing toestaan om videoplayback voort te zetten (wanneer het beleid volgzaam is) of het kan het instrueren om het videoplayback tegen te houden. Als de videostream niet compatibel is, moet de clienttoepassing deze stoppen. Het antwoord geeft informatie zodat de clienttoepassing een foutbericht en/of beschikbare acties kan weergeven zodat de gebruiker het afspelen kan voortzetten.
