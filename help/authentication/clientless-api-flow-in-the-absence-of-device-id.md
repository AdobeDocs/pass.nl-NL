---
title: Clientless API Flow bij afwezigheid van apparaat-id
description: Clientless API Flow bij afwezigheid van apparaat-id
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Clientless API Flow bij afwezigheid van apparaat-id {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


## Probleem

Niet alle toepassingen voor slimme apparaten kunnen een unieke apparaat-id opgeven.  Aangezien deviceId een verplichte parameter is, keert de dienst een fout 400 terug als het niet wordt overgegaan.


## Tijdelijke oplossing/oplossing

Voor clients zonder apparaat-id:

1. Bel de dienst van de registratiecode de eerste keer met `deviceId=dummy`
1. Haal de UUID uit de reactie. De UUID is beschikbaar in het element &quot;id&quot; van de registratiecode-respons (XML- en JSON-responsindelingen).
1. Bel de registratieservice nogmaals. Deze keer passeren `deviceId=<uuid obtained in step #2>`
1. De registratiecode weergeven die in Stap 3 is verkregen op de gebruikersinterface van de console


Nadat deze stappen zijn uitgevoerd, gebruikt Adobe Pass-verificatie de UUID als de apparaat-id. Sla deze apparaat-id (UUID) op in de lokale opslag van het apparaat. Als de gebruiker een nieuwe registratiecode produceert, zou u stap 1 door 4 opnieuw moeten in werking stellen, en dan eerder opgeslagen identiteitskaart van het Apparaat (UUID) met nieuwe vervangen.



## Permanente oplossing

Adobe zal dit in een toekomstige versie veranderen, door `deviceId` een optionele payload bij het maken van de reg-code en het gebruik van UUID als tokensleutel in plaats van `deviceId`, wanneer `deviceId` is niet aanwezig.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
