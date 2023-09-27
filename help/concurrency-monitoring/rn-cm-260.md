---
title: Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.6.0
description: Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:



## Releasedatum: 10-11-2016



## Nieuwe functies

Met deze release kunt u bestaande stream(s) beëindigen zodat de huidige stream kan worden gestart (ook wel &#39;kill stream&#39; genoemd).



**Externe beëindiging**

* Op een 409 Conflict reactie, zal elke zitting die binnen &quot;conflicten&quot;gebied van het advies wordt vermeld een attribuut beëindigingscode dragen.
* De gebruiker kan worden gevraagd de lijst met conflicterende sessies op te geven en de te doden sessie(s) te kiezen
* Externe sessies kunnen alleen worden gedood door een X-Terminate aanvraagheader (met de geselecteerde beëindigingscodes als waarden) door te geven binnen een poging tot insluiten van een sessie.
* Er is een nieuw type &#39;advies&#39; gedefinieerd voor de reactie van 410 Gone om aan te geven welke sessie de huidige heeft gedood.


Raadpleeg de bijgewerkte documentatie voor meer informatie.



>[!NOTE]
>
>De sessiedefinitie die wordt gebruikt voor het weergeven van actieve sessies, is bijgewerkt en bevat nu de naam van de toepassing en het apparaat (in plaats van de toepassings-id).




## Opgeloste problemen {#bug-fixes}

Gedupliceerde koppen zijn verwijderd in reactie op de server (de correctie betreft zowel de CORS-koppen als de Datum 1).




## Bekende problemen {#known-issues}

NVT
