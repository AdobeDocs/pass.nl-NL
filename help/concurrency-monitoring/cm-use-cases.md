---
title: Gebruik hoofdletters
description: Gebruik gevallen in Gelijktijdige controle.
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---


# Gevallen gebruiken {#use-cases}

Het belangrijkste geval van het gebruik van de Dienst van de Tellende van de Stroom telt het aantal gezamenlijke videostromen die door een gebruiker worden gecontroleerd en verstrekt een besluit betreffende zijn gelijktijdig gebruik voor zelfde rekening identiteitskaart

Om het gebruik door abonnee te controleren, is er een behoefte aan de gecentraliseerde dienst die gebruikersactiviteit kan bijeenvoegen ongeacht of het op de website of de toepassing van de programmeur, op het MVPD inhoudsportaal of op een syndicated bezit gebeurt.

De belangrijkste gebruiksgevallen die door deze gecentraliseerde dienst worden gesteund moeten zijn:

1. Zodra een abonnee begint met het bekijken van een video, kan de toepassing **een streaming sessie initialiseren** en starten **rapportageactiviteit** gegevens.
1. In dezelfde centrale dienst ontvangt een andere instantie ***CM-besluiten*** - indien de toepassing een of meer beleidsregels heeft die in de CM-dienst zijn geregistreerd, zal de dienst reageren met een toegangsbesluit dat is gebaseerd op de huidige activiteit.


## Een sessie maken {#create-session}

Met deze API-aanroep kan de client een nieuwe CM-sessie maken wanneer de gebruiker op de knop &quot;Afspelen&quot; drukt om inhoud te bekijken. De serverreactie bevat de nieuwe stream-URL (die de stream-id bevat) om de stream in leven te houden en de tijd waarop de time-out van de stream plaatsvindt. Verwacht wordt dat de clienttoepassing activiteit via hartslagen rapporteert. De initialisatieaanroep van de sessie moet metagegevens bevatten in de vorm van sleutel-/waardeparen die als formuliergegevens (of parameters voor queryreeksen) worden verzonden. Bovendien zal de reactie ook een vlag omvatten om erop te wijzen of de playback &quot;beleid volgzaam&quot;is. Als dit niet het geval is, is het afspelen niet toegestaan.

## Rapportage {#reporting-activity}

Nadat een sessie is gemaakt, moet de toepassing regelmatig hartslagen verzenden om de desbetreffende stream actief te laten blijven. Bovendien wordt aangeraden dat de client-app de stream stopt zodra de gebruiker het afspelen heeft gestopt, zodat de stream tot de time-out niet als actief wordt beschouwd.

De reactie van de hartslagvraag kan de cliënttoepassing toestaan om videoplayback voort te zetten (wanneer het beleid volgzaam is) of het kan het instrueren om het videoplayback tegen te houden. Als de videostream niet compatibel is, moet de clienttoepassing deze stoppen. Het antwoord geeft informatie zodat de clienttoepassing een foutbericht en/of beschikbare acties kan weergeven zodat de gebruiker het afspelen kan voortzetten.
