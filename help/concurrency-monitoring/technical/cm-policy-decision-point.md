---
title: Beleidsbeslissingspunt
description: Beleidsbeslissingspunt
exl-id: 94bc638c-bef8-45ea-b20a-9b7038adecdd
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Beleidsbeslissingspunt {#policy-desc-pt}

## Domeinmodel {#domain-model}

Deze pagina moet dienen als referentie voor verschillende toepassingen en implementaties van beleid. Wij adviseren u ook om het [&#x200B; 1&rbrace; deel van de Verklarende woordenlijst &lbrace;van de documentatie voor term definities te raadplegen.](/help/concurrency-monitoring/cm-glossary.md)

A **huurder** bezit **toepassingen** waarvoor het **beleid** wenst af te dwingen. **de toepassingen van de Cliënt** moeten met **toepassingsidentiteitskaart** worden gevormd (die door Adobe wordt verstrekt).

De huurder associeert dan elke toepassing met één of meerdere beleid, of gecreeerd door hem of gecreeerd en gedeeld door anderen. Het beleid kan tussen veelvoudige huurders worden verbonden.

De **onderwerpactiviteit** bestaat uit alle stromen (geen kwestie de toepassing) die aan Gelijktijdige Controle voor een bepaald onderwerp worden gemeld.

Wanneer een stream voor een bepaald onderwerp moet worden geautoriseerd, controleert het systeem eerst alle beleidsregels die zijn gedefinieerd voor de toepassing die de stream heeft gemaakt.

Voor elk van het toepasselijke beleid, moeten wij dan alle **relevante activiteit** verzamelen die tot de regel zal worden overgegaan. De **relevante activiteit** voor beleid P zal slechts een stroom S omvatten als het aan de volgende voorwaarde voldoet:

**de stroom &quot;S&quot;is begonnen door een toepassing die beleid &quot;P&quot;onder zijn beleid omvat.**

![&#x200B; de stroom &quot;S&quot;is begonnen door een toepassing die beleid &quot;P&quot;onder zijn beleid omvat.](../assets/pdp-domain-model.png)

## Droog gebruik {#dry-run-use-cases}

De analyse hieronder is bedoeld om het model tegen sommige gebruiksgevallen te bevestigen. We doen dat geleidelijk, door te beginnen met een basisconfiguratie en complexiteit op verschillende manieren toe te voegen.

### &#x200B;1. Eén huurder. Eén toepassing. Eén beleid. Eén stream {#onetenant-oneapp-onepolicy-onestream}

Wij zullen met één enkele huurder, met één enkele toepassing en één enkel verbonden beleid beginnen. Laten we aannemen dat het beleid stelt dat er maximaal één actieve stream voor elke gebruiker kan zijn (de laatste stream mag worden afgespeeld).

Wanneer een stream is gestart, bestaat de activiteit alleen uit die stream en kan deze worden afgespeeld.

![&#x200B; Één huurder. Eén toepassing. Eén beleid. Eén stream &#x200B;](../assets/onetenant-app-policy-stream.png)


### &#x200B;2. Eén huurder. Eén toepassing. Eén beleid. Twee stromen. {#onetenant-oneapp-onepolicy-twostreams}

Zodra een tweede stroom (door het zelfde onderwerp gebruikend de zelfde toepassing) is begonnen, zal de activiteit die voor bevestiging wordt gebruikt uit zowel **s1** en **s2** bestaan.

De grens wordt overschreden omdat het beleid verklaart dat slechts één stroom wordt toegestaan om te spelen, zodat zullen wij slechts de recentste stroom (**s2**) toestaan om te spelen.

![&#x200B; Één huurder. Eén toepassing. Eén beleid. Twee stromen.](../assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>De diagrammen vertegenwoordigen de systeemmening op de gebruikersactiviteit. Voor streaminitialisatiepogingen wordt het toegangsbesluit opgenomen in de reactie. Voor actieve stromen zal het besluit op de hartslagreactie worden teruggekeerd.

### &#x200B;3. Twee huurders. Twee toepassingen. Eén beleid. Twee stromen. {#twotenant-twoapp-onepolicy-twostreams}

Laten wij nu veronderstellen dat een nieuwe huurder het zelfde beleid in hun toepassingen wil afdwingen:

![&#x200B; Twee huurders. Twee toepassingen. Eén beleid. Twee stromen.](../assets/onepolicy-twotenant-app-stream.png)

Wegens de twee huurders die door het zelfde beleid worden verbonden, is de situatie die in gebruik 2 wordt beschreven hier van toepassing en **s3** wordt toegestaan om te spelen aangezien het de recentste stroom is.

### &#x200B;4. Twee huurders. Drie toepassingen. Twee beleidsvormen. Twee stromen. {#twotenants-threeapps-twopolicies-twostreams}

Nu, veronderstellen dat de tweede huurder een nieuwe toepassing opstelt en een nieuw beleid wil bepalen dat tussen **app2** en **app3** zal worden gedeeld.

![&#x200B; Twee huurders. Drie toepassingen. Twee beleidsvormen. Twee stromen.](../assets/twotenant-policies-streams-threeapps.png)

Op dit ogenblik, worden de actieve stromen **s3** en **s4** allebei toegestaan. Voor **s3**, wanneer beleid **P1** wordt geëvalueerd, zal het systeem **s3** slechts tellen als **relevante activiteit** (**s4** is op geen manier verwant met beleid **P1**) zodat is er geen schending.

Het beleid **P2** wordt toegepast op beide stromen en het zal zowel **s3** en **s4** als relevante activiteit omvatten. Aangezien deze activiteit binnen de grenzen van twee stromen is, worden beide stromen toegestaan.

### &#x200B;5. Twee huurders. Drie toepassingen. Twee beleidsvormen. Drie stromen. {#twotenants-threeapps-twopolicies-threestreams}

Nu veronderstellend dat een nieuwe poging van de stroominitialisatie gebruikend **app2** wordt uitgevoerd:

![&#x200B; Twee huurders. Drie toepassingen. Twee beleidsvormen. Drie stromen.](../assets/twotenants-policies-threeapps-streams.png)

**s5** wordt toegestaan om door **P1** te beginnen (die nieuwere stromen toestaat om over te nemen) maar het wordt ontkend door **P2**, zodat zal het niet beginnen.

Hetzelfde gebeurt als een stroominit wordt geprobeerd met app3: hetzelfde beleid P2 weigert de toegang.

![](../assets/stream-init-attempted-app3.png)

Nu, zien wat zou gebeuren als de gebruiker probeert om een nieuwe stroom tot stand te brengen gebruikend app1:

![](../assets/new-stream-with-app1.png)

De toepassing app1 is op geen manier verwant met het beleid **P2**, zodat zal het slechts het beleid **P1** toepassen: dat de nieuwe stroom toestaat om te beginnen en oudere te ontkennen (**s3** in dit geval).
