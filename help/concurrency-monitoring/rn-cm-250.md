---
title: Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.5.0
description: Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.5.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---


# Opmerkingen bij de release Adobe Pass Concurrency Monitoring 2.5.0 {#cm-250}

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

Releasedatum: 21-04-2016

## Nieuwe functies {#new-features}

De API voor gelijktijdige bewaking v2.0 is nu beschikbaar in productie.

De V2 versie verenigt hartslag en vraagvraag en vereenvoudigt zeer de implementatie wanneer beide APIs gelijktijdig worden gebruikt.



### Sessielevenscyclus {#session-lifecycle}

* Schrijf-enige APIs voor verwezenlijking, houden-levend en beëindiging van een zitting — d.w.z. geen meer vragen, wordt het besluit teruggekeerd op zowel zitting in als hartslagvraag
* Het hartslaginterval wordt nu gedreven door de server via Verloopt kopballen die op zowel zittingsinit worden geplaatst als houden-levende vraag. Hierdoor kunt u de server configureren met hartslagintervallen en een minder gecodeerde waarde in de apps.
* Het gelukkig pad (compatibel gedrag van zowel de gebruiker als de app) wordt &quot;geplakt&quot; met 2XX statuscodes

### Foutafhandeling {#error-handling}

* Weigeringen, ontbrekende metagegevens of toepassingsfouten worden gemarkeerd als 4XX reacties (Conflict, Onjuist verzoek, Onbevoegd, Gone enz.)

* Wanneer het zinvol is, omvat het antwoord:

   * bijbehorend advies — gedetailleerde uitleg van de fout, die aan de gebruiker moet worden gevraagd.

   * verplichtingen — verplichte acties die de toepassing moet uitvoeren (bijv. metagegevens vernieuwen, zich afmelden bij Adobe Pass).

### Metagegevens {#metadata}

* Standaard in plaats van aangepaste metagegevens: hiermee kan de API bepalen of onjuiste metagegevens worden verzonden of dat de door de regels vereiste metagegevens ontbreken.
* De vereiste kenmerken voor elke toepassing worden nu geleverd door een API-aanroep en moeten in de cache worden opgeslagen door clients.
Notities

>[!NOTE]
>
>De versie V1 blijft beschikbaar. Als de klanten vragen buiten de hartslagvraag moeten gebruiken kan de V1 versie worden gebruikt.




### Opgeloste problemen {#bug-fixes}

NVT

### Bekende problemen {#known-issues}

NVT
