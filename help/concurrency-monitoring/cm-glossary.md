---
title: Verklarende woordenlijst
description: Woordenlijst met termen in Gelijktijdige bewaking
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Verklarende woordenlijst {#glossary}

## Account-id {#accid-defn}

* MVPD-account van een abonnee, meestal overeenkomend met de werkelijke factureringsaccount. Deze rekening moet door de MVPD in haar eigen systeem kunnen worden geïdentificeerd.

## Handeling {#action-defn}

* Het type van toegang dat het onderwerp verzoekt; de mogelijke waarden voor cm ****** in werking stellen of ***blijven*** een het stromen zitting.

## Actieve stroom {#active-stream-defn}

* Een stroom die minstens 1 gebeurtenis (hartslag) in de laatste 90 seconden heeft ontvangen.

* ***Nota:*** als de laatste gebeurtenis in de stroom van typeeinde (`?event=stop`) is, zal het niet worden geteld. Dit is een optimalisatie die een speler toestaat om een stroom uitdrukkelijk te sluiten zodat het niet als &quot;actief&quot;meer wordt beschouwd.

## Toepassing {#application-defn}

* Ontwikkeld door de huurder voor toegang tot video-inhoud
* Maakt en handhaaft besluiten over inhoudstoegang die op informatie wordt gebaseerd die door de Dienst van de Controle van de Valuta wordt verstrekt (dit is geldig in het [ geval van het Punt van de Informatie van het Beleid 0})](/help/concurrency-monitoring/technical/policy-info-pt-versionone.md)
* Zal een unieke **toepassingsidentiteitskaart** hebben die door Adobe wordt verstrekt.

## Gelijktijdige bewakingsservice {#cm-service-defn}

* treedt op als een controlesysteem voor de abonnees, dat de MVPD&#39;s en de programmeurs steunt in hun eisen inzake de handhaving van het intertoepassingsbeleid.
* Ontvangt hartslagen die op stroomactiviteit wijzen.
* Handelt als Punt van het Besluit van het a _Beleid_ door vergunningsverzoeken te evalueren die op gebruikersactiviteit worden gebaseerd en toe te staan/ontkennen reactie te verstrekken.
* Handelt als Punt van de Informatie van het a _Beleid_ door het aantal actieve stromen (en extra stroommeta-gegevens) voor een abonnee te melden.

## Omgeving {#env-defn}

* Aanvullende informatie die relevant is voor de aanvraag, zoals configuratie-instellingen of systeemtijd.

## MVPD {#mvpd-defn}

* Multikanaaldistributie voor videoprogrammering.
* Handelt als Verificatie en Vergunning Leverancier, maar kan ook de Dienst of Inhoudsleverancier zijn.

## Beleid {#policy-defn}

* Het kernbegrip van toegangsbeheer in CM die als doel en één of meerdere regels wordt bepaald die onder een unieke naam worden gegroepeerd.

## Beleidsbeheerpunt (PAP) {#policy-admin-pt-defn}

* Punt dat het beleid van de toegangsvergunning beheert. Dit zal hier niet worden gedocumenteerd, maar uiteindelijk zullen wij een zelfbediening console voor klanten verstrekken om hun toegangsbeleid te beheren.

## Beleidsbeslissingspunt (PDP) {#policy-decn-pt-defn}

* Punt dat toegangsverzoeken tegen vergunningsbeleid evalueert alvorens toegangsbesluiten uit te vaardigen.

## Beleidshandhavingspunt (PEP) {#policy-ef-pt-defn}

* Punt dat het de toegangsverzoek van de gebruiker aan een middel onderschept, een besluitvormingsverzoek aan PDP doet en dat besluit op het verzoek afdwingt. Dit is momenteel de cliënttoepassing en er is geen plan om deze rol naar Gelijktijdige Controle over te brengen.

## Beleidsinformatiepunt (PIP) {#policy-info-pt-defn}

* Een bron van kenmerkwaarden. Gelijktijdige bewaking fungeert als informatiepunt door het verstrekken van:
   * pass-through streammetagegevens.
   * activiteitswaarden met betrekking tot de gelijktijdige streams.

## Programmeur {#programmer-defn}

* Handelt als een service- en contentprovider.
* vertrouwt op de opgestelde cliënttoepassing die met de dienst van de Controle van de Zitting integreert om het bepaalde veiligheidsbeleid af te dwingen dat op de bovengenoemde dienstgegevens wordt gebaseerd.
* Moet de MVPD ondersteunen bij het verzamelen van abonneeactiviteiten en het afdwingen van de beperkende regels wanneer ze zich op hun eigendommen bevinden.
* Misschien ook geïnteresseerd in het beperken van gelijktijdige toegang tot hun inhoud voor alle bestemmingspoorten, als afzonderlijke regel.

  *Q: Waarom programmeur en niet identiteitskaart van de Aanvrager zoals in de rest van de Authentificatie van Adobe Pass?*

  *A: De reden moet Programmeurs toestaan om deze parameter flexibel te gebruiken om gegevens tussen hun eigenschappen afhankelijk van hun gebruiksgevallen over te gaan of te isoleren.*

## Bron {#resource-defn}

* De feitelijke inhoud die een onderwerp wil consumeren. De bron heeft gewoonlijk kenmerken die betrekking hebben op de eigenaar (uitgever) en kan ook extra informatie zoals genre of wat dan ook verschaffen.

## Regel {#rule-defn}

* Een Booleaanse functie die wordt geëvalueerd op basis van een bepaalde stream en de relevante gebruikersactiviteit om te bepalen of toegang moet worden toegestaan of geweigerd voor die stream.

## Streaming sessie {#streaming-session-defn}

* Een streaming videosessie die door een onderwerp wordt gestart om een bepaalde bron te verbruiken.

## Onderwerp {#subj-defn}

* De consument van de (video)inhoud via internet. Wij vermijden opzettelijk de termijn _**gebruiker**_, aangezien de Controle van de Zalk gewoonlijk MVPD rekening IDs behandelt (die verscheidene daadwerkelijke gebruikers impliceren die het zelfde contract, bijvoorbeeld familieleden voor een huishouden delen).

* Voor elke stroom, kan het onderwerp met attributen met betrekking tot de daadwerkelijke persoon worden verbeterd die de dienst, hun netwerk aangesloten apparaat gebruikt etc.

## Abonnement {#subscriber-defn}

* De betalende klant van een MVPD of een persoon die de gegevens van een betalende klant deelt
* Kan worden gestopt met het bekijken van inhoud door de Concurrency Monitoring Service, door de clienttoepassing die de bovengenoemde service gebruikt.
* In het gunstigste geval merkt hij of zij nooit het bestaan van de Dienst van de Controle van de Valuta op

## Doel {#target-defn}

* Een stroom voorspelt die zal terugkeren of de regel op een bepaalde stroom van toepassing is. Het impliciete doel in CM is om het even welke stroom die door een toepassing wordt gecreeerd die verwijzingen het beleid in kwestie. Bovendien, kunnen de voorwaarden van de attributenwaarde worden toegevoegd om de activiteit te verfijnen filtrerend alvorens de regels toe te passen.

## Aannemer {#tenant-defn}

* Een organisatie van de klant van de Controle van de Gelijktijdige.
