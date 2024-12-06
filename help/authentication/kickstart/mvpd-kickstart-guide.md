---
title: MVPD Direct Integration Plan
description: MVPD Direct Integration Plan
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# MVPD kickstart guide: MVPD Direct Integration Plan {#mvpd-dir-int-plan}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#mvpd-kickstart-intro}

Welkom bij Adobe Pass Authentication for TV Anywhere.  Wij kijken ernaar uit met u samen te werken.

>[!NOTE]
>
>Dit is de Gids van Kickstart voor de Verdelers van de Video van Multikanaal (MVPDs). Als u een Programmer (inhoudsleverancier) bent, zie de [ Gids van Kickstart van Programmers ](/help/authentication/kickstart/programmer-kickstart-guide.md).

Ondersteuning is altijd beschikbaar via het Adobe Pass Authentication-kaartsysteem op Zendesk. Hier vindt u ook voorbeelden, documentatie en videozelfstudies voor onze processen. Om [ te gebruiken Zendesk ](https://adobeprimetime.zendesk.com/), zult u een rekening in https://tve.zendesk.com/home moeten registreren en creëren. Er is geen limiet voor het aantal gebruikers dat u kunt registreren en die opmerkingen op een ticket kunnen zien of plaatsen. Alle ondersteuningsvragen moeten worden gericht aan: tve-support op adobe.com

**contacten van het Team**:

**Steun** - voor alle vragen, incidenten, of eigenschapverzoeken **tve-support@adobe.com**.

## 1. Kick-offvergaderingen {#kickoff-meetings}

De mogelijkheid voor deze bijeenkomsten is het begin van technische besprekingen tussen Adobe en de MVPD. Op dit moment van het proces moeten beide partijen documenten uitwisselen. Als follow-up moet de Adobe een ticket openen in ons kaartverkoopsysteem (https://tve.zendesk.com/) om de status van de integratie te kunnen volgen.

## 2. Functies {#features}

De Adobe zal een wekelijkse statusvraag opstellen om het algemene programma, de stappen, de chronologie, en de implementatiedetails van de integratie te bespreken en te volgen. In deze fase voert de Adobe een herziening uit van de specificaties van het MVPD. Het resultaat hiervan moet een speciale pagina zijn die alle functies bevat die de MVPD nodig heeft. De MVPD zal de Adobe een specificatiedocument sturen met gedetailleerde informatie over de uitvoering van de verificatie/machtiging van de MVPD.

Punten om te verduidelijken, zie {de Eigenschappen van de Integratie 0} MVPD ](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md).[

Er zijn verschillende instellingen die op dit punt in detail moeten worden beschreven:

* **het embleem URL van MVPD** - dit is een dossier met de volgende afmetingen: 112 x 33 pixel. Het logo wordt weergegeven door Programmers op hun sites wanneer de gebruiker op de knop Aanmelden klikt om hun Pay TV-provider te selecteren.
* **TTL (tijd-aan-levende) Waarden** - TTL wordt typisch geplaatst door MVPD tijdens het authentificatie/goedkeuringsproces. Nochtans kan de Adobe die waarden van TTL met voeten treden en verschillende waarden verstrekken afhankelijk van wat door zowel de Programmer als MVPD wordt overeengekomen.
* **naam van de Vertoning** - dit wordt getoond door Programmers op hun plaatsen wanneer de gebruiker op de &quot;Teken binnen&quot;knoop klikt om hun leverancier van de Tv van de Betaal te selecteren.
* **geloofsbrieven van de Test** - Beide profielen (het opvoeren en productie) zouden een lijst van testgeloofsbrieven moeten hebben.

>[!IMPORTANT]
>
>Elke keer dat een gebruiker een machtigingsstroom start, wordt hij gekoppeld aan één ondoorzichtige, unieke gebruikersnaam.  De gebruikers-id wordt gebruikt om de gebruiker van de app van een programmeur te identificeren, maar is afkomstig van de MVPD.

>[!CAUTION]
>
>Een belangrijke kennisgeving voor elke MVPD: de gebruikers-id mag GEEN PII (Persoonlijk Identificeerbare Informatie) bevatten, informatie die zelfstandig of met andere informatie kan worden gebruikt om de gebruiker te identificeren, te contacteren of te vinden.

## 2. Metagegevensuitwisseling {#metadata-ex}

Beide partijen moeten de metagegevens uitwisselen voor alle betrokken omgevingen (productie, staging, enz.).

* **Adobe**
   * Voor de het opvoeren milieu kan de meta-gegevens van SP van de Adobe worden teruggewonnen van: [ het opvoeren van de Authentificatie sp meta-gegevens ](https://sp.auth-staging.adobe.com/sp/metadata)
   * Voor de meta-gegevens van SP van de Adobe van het productiemilieu kan worden teruggewonnen van: [ de productie sp van de Authentificatie meta-gegevens ](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * Metagegevens (opvoeren/maken) toevoegen.

## 4. IP-lijst toestaan {#allow-ip-list}

De volgende IPs zou in de firewall van MVPD moeten worden gewhitelisteerd. Gelieve te contacteren Adobe voor de lijst van IPs.

* Voor Adobe Pass-verificatie moeten firewalls worden geopend op poort 80 en 443, zodat beperkte bronnen toegankelijk zijn.

* MVPD moet een lijst van IP adressen voor authentificatie en vergunningsservers (als dit het geval is) toevoegen.

## 5. Ontwikkeling {#deve}

De duur van de ontwikkelingsfase wordt bepaald na de herziening van de specificaties en rekening houdend met de punten die beide zijden intekenen. Adobe moet aangepaste code voor het machtigingsonderdeel schrijven.

>[!NOTE]
>
>Merk op dat de vergunning per middel wordt uitgevoerd. De vergunningstransactie wordt typisch uitgevoerd met een koord van identiteitskaart, dat van de plaats van de Programmer wordt overgegaan, die het kanaal vertegenwoordigen waarvoor de gebruiker om toestemming verzoekt. Deze identiteitskaart van Middel wordt gevestigd tussen Programmer en MVPD en kan zo korrelig zijn zoals noodzakelijk.

## 6. Adobe {#adobe-env}

Adobe biedt verschillende omgevingen voor verschillende stadia van het ontwikkelingsproces:

* **Prequalification** (PRE-QUAL): Het milieu PRE-QUAL bevat de volgende versiekandidaat. De Adobe integreert aanvankelijk nieuwe partners in dit milieu, alvorens de integratie aan het milieu van de Versie te bevorderen. De partners hebben twee weken om op het PRE-QUAL milieu te testen en moeten uitdrukkelijk om het even welke veranderingen in de PRE-QUAL configuratie verzoeken (contacteer uw vertegenwoordiger van de Adobe voor details over het proces van het veranderingsverzoek). Bugfixes brengen nieuwe plaatsingen in dit milieu teweeg.
* **Versie** (RELEASE): De huidige productie van de Adobe bouwt wordt opgesteld aan een levend milieu hier.

Voor meer informatie over hoe te om de milieu&#39;s van de Adobe te gebruiken, zie [ Begrijpend de Milieu&#39;s van de Adobe ](/help/authentication/notes-technical/understanding-the-adobe-environments.md)

## 7. Stationering {#stag-env}

Gebaseerd op de meta-gegevens die van MVPD worden ontvangen, zal de Adobe tot een nieuwe MVPD in het systeem van de Authentificatie van Adobe Pass leiden en vormen. Dit zal in het prequal het opvoeren milieu van de Adobe worden opgesteld, en zal met onze testprogrammeur (TestDistributors) worden gevormd.

MVPD moet de zelfde plaatsing in hun QA/het opvoeren/testmilieu doen.

## 8. Testen en problemen oplossen {#tes-troubleshoot}

In deze fase, Adobe en de test MVPD en los de integratie problemen op. Het Adobe Pass Authentication-team kan de API-testsite van de Adobe gebruiken om de integratie te testen. Om meer over het gebruiken van de de testplaats van API van de Adobe te weten zie [ de authentificatie en de toestemmingsstromen van de Test gebruikend de Adobe API testplaats ](/help/authentication/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).

Wanneer het testen en het oplossen van problemen met succes heeft voltooid, wordt de integratie toegelaten in de het opvoeren van de versie van de Adobe milieu. Op dit punt, kan de Adobe MVPD met daadwerkelijke programmeur integreren.

## 9. Productie {#prod-dep}

* MVPD moet eerst in het productieprofiel opstellen, om de connectiviteit te testen.

* Adobe wordt ingezet in productie op voorhand.

* Alle partijen kunnen nu het productieprofiel testen.

* Als alles op dit punt goed is, kan de Adobe de integratie naar de milieu van de versieproductie (&quot;levend&quot;) verplaatsen, die aan alle gebruikers beschikbaar is.

## 10. Doorverwijsprocedures {#esc-proc}

Zodra de integratie in productie levend is is het kritiek om de beste klantenervaring te verstrekken. Om de beste reactie in het geval van een server neer kwestie te verzekeren, moeten MVPDs de documentatie van de escalatieprocedure voor kwesties verstrekken die aan de aandacht van de Adobe worden gebracht.

Op zijn beurt levert de Adobe MVPD&#39;s het meest recente escalatieproces van de Adobe Pass-verificatie.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
