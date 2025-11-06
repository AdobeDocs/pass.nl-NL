---
title: Beleidsinformatie
description: Beleidsinformatie
exl-id: 964bb28d-cfef-4a37-b6c4-10cc59be0b47
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Beleidsinformatie {#pip}

>[!NOTE]
>
>Deze pagina is verouderd omdat deze van toepassing is op de vorige versie van de API die niet langer wordt aanbevolen voor nieuwe integratie

Het volgende diagram toont de stroom in het geval dat de klant voor het **Punt van de Informatie van het Beleid** kiest, in welk geval wordt CM slechts gebruikt voor het vragen van de activiteit en al toegangslogica ingebed in de cliënttoepassing):

![](assets/pip-workflow.png)



In het onderstaande diagram ziet u hoe het tellen van streams werkt voor een gebruiker die inhoud van 2 apparaten controleert.

![](assets/pip-sequence.png)

In een notendop, is de gebruikelijke berichtstroom als volgt:

1. Aanvankelijk, voor een gebruiker die de dienst nooit heeft gebruikt, wordt een Web-pagina geladen / een toepassing wordt geopend, waar een Gelijktijdige Toezichtdienst van instrumenten voorzien toepassing een vraag van de Initialisatie van de Zitting maakt.
1. De dienst van de Controle van de Gelijktijdige keert de nieuwe stroommiddel voor hartslagen, evenals de huidige gebruikersactiviteit terug.
1. Tijdens het afspelen van video doet de van instrumenten voorzien toepassing hartslagvraag aan de Dienst van de Controle van de Valuta, die toont dat de gebruiker momenteel een video verbruikt.
1. Op elk ander punt, kunnen andere van instrumenten voorzien toepassingen de vraagvraag van de Status aan de Dienst van de Controle van de Valuta maken, die de huidige gebruikersactiviteit zal terugkeren.
1. Aan het einde van het afspelen van video kan de van instrumenten voorziene toepassing een hartslagaanroep uitvoeren met &quot;event=stop&quot;, wat betekent dat de video is gestopt en dat de huidige stream niet meer als een actieve stream moet worden geteld.
