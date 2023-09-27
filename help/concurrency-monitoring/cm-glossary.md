---
title: Verklarende woordenlijst
description: Woordenlijst met termen in Gelijktijdige bewaking
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Verklarende woordenlijst {#glossary}

## Account-id {#accid-defn}

* MVPD-account van een abonnee, meestal overeenkomend met de werkelijke factureringsaccount. Deze rekening moet door het MVPD in zijn eigen systeem identificeerbaar zijn.

## Handeling {#action-defn}

* Het type toegang dat het onderwerp aanvraagt; de mogelijke waarden voor CM zijn ***initiëren*** of ***continue*** een streaming sessie.

## Actieve stroom {#active-stream-defn}

* Een stroom die minstens 1 gebeurtenis (hartslag) in de laatste 90 seconden heeft ontvangen.

* ***Opmerking:*** Als de laatste gebeurtenis in de stream van het type stop is (`?event=stop`), wordt het niet meegeteld. Dit is een optimalisatie die een speler toestaat om een stroom uitdrukkelijk te sluiten zodat het niet als &quot;actief&quot;meer wordt beschouwd.

## Toepassing {#application-defn}

* Ontwikkeld door de huurder voor toegang tot video-inhoud
* Maakt en handhaaft besluiten over inhoudstoegang op informatie die door de Dienst van de Controle van de Valuta wordt verstrekt (dit is geldig in [Beleidsinformatie](/help/concurrency-monitoring/policy-info-pt-versionone.md) case)
* Heeft een unieke **toepassings-id** verstrekt door Adobe.

## Gelijktijdige bewakingsservice {#cm-service-defn}

* treedt op als een controlesysteem voor de abonnees, dat de MVPD&#39;s en de programmeurs steunt in hun eisen inzake de handhaving van het intertoepassingsbeleid.
* Ontvangt hartslagen die op stroomactiviteit wijzen.
* Handelt als een _Beleidsbeslissingspunt_ door vergunningsverzoeken te evalueren die op gebruikersactiviteit worden gebaseerd en toe te staan/ontkennen antwoord te verstrekken.
* Handelt als een _Beleidsinformatie_ door het aantal actieve stromen (en extra stroommeta-gegevens) voor een abonnee te melden.

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
* Moet MVPD in het verzamelen van abonneeactiviteit steunen en de beperkende regels afdwingen wanneer op hun eigenschappen.
* Misschien ook geïnteresseerd in het beperken van gelijktijdige toegang tot hun inhoud voor alle bestemmingspoorten, als afzonderlijke regel.

  *V: Waarom programmeur en niet aanvrager-id in de rest van de Adobe Pass-verificatie?*

  *A: De reden is om programmeurs toe te staan om deze parameter flexibel te gebruiken om gegevens tussen hun eigenschappen door te geven of te isoleren afhankelijk van hun gebruiksgevallen.*

## Bron {#resource-defn}

* De feitelijke inhoud die een onderwerp wil consumeren. De bron heeft gewoonlijk kenmerken die betrekking hebben op de eigenaar (uitgever) en kan ook extra informatie zoals genre of wat dan ook verschaffen.

## Regel {#rule-defn}

* Een Booleaanse functie die wordt geëvalueerd op basis van een bepaalde stream en de relevante gebruikersactiviteit om te bepalen of toegang moet worden toegestaan of geweigerd voor die stream.

## Streaming sessie {#streaming-session-defn}

* Een streaming videosessie die door een onderwerp wordt gestart om een bepaalde bron te verbruiken.

## Onderwerp {#subj-defn}

* De consument van de (video)inhoud via internet. We vermijden bewust de term _**user**_, aangezien Gelijktijdige bewaking doorgaans betrekking heeft op de ID&#39;s van de MVPD-account (waarbij verschillende werkelijke gebruikers betrokken zijn die hetzelfde contract delen, bijvoorbeeld gezinsleden voor een huishouden).

* Voor elke stroom, kan het onderwerp met attributen met betrekking tot de daadwerkelijke persoon worden verbeterd die de dienst, hun netwerk aangesloten apparaat gebruikt etc.

## Abonnement {#subscriber-defn}

* De betalende klant van een MVPD of een persoon die de geloofsbrieven van een betalende klant deelt
* Kan worden gestopt met het bekijken van inhoud door de Concurrency Monitoring Service, door de clienttoepassing die de bovengenoemde service gebruikt.
* In het gunstigste geval merkt hij of zij nooit het bestaan van de Dienst van de Controle van de Valuta op

## Doel {#target-defn}

* Een stroom voorspelt die zal terugkeren of de regel op een bepaalde stroom van toepassing is. Het impliciete doel in CM is om het even welke stroom die door een toepassing wordt gecreeerd die verwijzingen het beleid in kwestie. Bovendien, kunnen de voorwaarden van de attributenwaarde worden toegevoegd om de activiteit te verfijnen filtrerend alvorens de regels toe te passen.

## Aannemer {#tenant-defn}

* Een organisatie van de klant van de Controle van de Gelijktijdige.
