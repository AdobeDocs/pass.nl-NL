---
title: Aangepaste metagegevens
description: Aangepaste metagegevens
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Aangepaste metagegevens {#cm}

>[!NOTE]
>
> Deze pagina is verouderd omdat deze van toepassing is op de vorige versie van de API die niet langer wordt aanbevolen voor nieuwe integratie

De dienst staat cliënten toe om gebruik van zowel standaard als douanegebieden in zowel vragen als besluitvorming te maken. De standaardvelden zijn altijd beschikbaar voor elke stream (omdat ze vereist zijn voor het maken van een stream of gegenereerd zijn door de server):

* toepassing
* mvpd
* accountId
* streamId
* streamStart
* initiator


De aangepaste velden zijn alle sleutel-/waardeparen die bij de initialisatie van de stream worden doorgegeven, zoals parameters voor formulieren of queryreeksen. De standaard- en aangepaste velden kunnen vervolgens in de volgende scenario&#39;s worden gebruikt (zie de documentatie bij de desbetreffende API-bronnen hieronder voor meer informatie):

* Extra velden aanvragen (via de parameter queryreeks voor velden) bij het ophalen van de streamlijst voor een account (de /streams-bron)
* De rekeningsactiviteit van de onderbreking door de afmetingen te specificeren om door (het /activity middel) te groeperen
* Het bepalen van server-zijbeleid dat op gebiedswaarden of kardinaliteiten wordt gebaseerd (de voorbeelden gebruiken pseudo-SQL enkel voor duidelijkheid):
* Een beleid configureren dat alleen van toepassing is op specifieke veldwaarden (bijvoorbeeld een specifiek iOS-beleid: WHERE osType IS &#39;iOS&#39;)
* Beperk het aantal verschillende waarden voor een bepaald veld (bijv. niet meer dan X verschillende apparaten: MET DISTINCT COUNT(deviceId) >= 2)
* Het aantal actieve streams per veldwaarde beperken (bijv. niet meer dan X actieve streams voor één apparaattype: GROUP BY deviceType HAVING COUNT(streamId) >= 3)


Op basis van de verzonden sleutel/waarden kunnen verschillende regels worden vastgesteld. Dit zou in deze richting kunnen gaan:

1. De klant beslist dat hij de parametergroep zou willen verzenden, die als waarden &quot;SPORTS&quot;en &quot;KIDS&quot;zal hebben.
1. Vervolgens moet de app het volgende doen:
   * Voor sportkanalen, bij stroominitialisering zou app ***type=SPORTS*** als vraagparameter verzenden
   * Voor kanalen met kinderen verwante inhoud, bij stroominitialisering zou app ***type=KIDS*** als vraagparameter verzenden
1. Dan zou een beleid als dit kunnen worden bepaald:
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Dit zou in feite betekenen dat wanneer een gebruiker naar sport kijkt, hij/zij het niet op meer dan 1 apparaat kan doen, echter wanneer de gebruiker de inhoud van kinderen bekijkt, wordt het bekijken toegestaan op maximum 3 apparaten.
