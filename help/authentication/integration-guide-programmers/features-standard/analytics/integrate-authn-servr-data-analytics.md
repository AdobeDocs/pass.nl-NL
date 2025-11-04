---
title: Gegevens van de Adobe Pass-verificatieserver integreren in Adobe Analytics
description: Gegevens van de Adobe Pass-verificatieserver integreren in Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Gegevens van de Adobe Pass-verificatieserver integreren in Adobe Analytics

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Klanten van Adobe Pass Authentication willen de gegevens op de Adobe Pass Authentication-server (Adobe Pass) in het Adobe Analytics-dashboard bekijken, zodat ze deze gemakkelijker kunnen gebruiken.

De gegevens dienen voor het bijhouden van belangrijke TVE-meetgegevens, zoals verificatieomzettingspercentages per MVPD, unieke gebruikers op basis van de MVPD-gebruikersnaam en meer.

Het is niet bedoeld om een clientimplementatie te vervangen als er al een bestaat, aangezien de gebruikersactiviteit bij afwezigheid van een bezoeker-id niet verder kan worden gevolgd dan de onderstaande specifieke gebeurtenissen. Als de klanten bezoekersidentiteitskaart op de vraag van de pas verstrekken dan kunnen wij een ander type van de integratie van Analytics - echt ontgrendelen - tijd - dat zich bij alle gebeurtenissen van de pas met de bestaande klantengegevens kan aansluiten, meer details over dit nieuwe type van mogelijke integratie hier: &quot;[ Gebruikend identiteitskaart van Experience Cloud in de Authentificatie van Adobe Pass ](/help/authentication/integration-guide-programmers/features-standard/analytics/exp-cloud-id-authn.md)&quot;

## Inbegrepen cijfers {#metrics-included-int-authn-analyt}

| Gebeurtenis | Beschrijving |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN opgevraagd | Aantal geïnitieerde verificatiestromen |
| AuthN in behandeling | Aantal met succes geproduceerde authentificatietokens (het negeren van al dan niet de cliënt werkelijk het verwierf) |
| AuthN OK | Aantal verificatietokens dat gebruikers hebben opgehaald |
| AuthZ aangevraagd | Aantal pogingen tot het verlenen van vergunningen |
| AuthZ OK | Aantal geslaagde vergunningen |
| AuthZ is mislukt | Aantal door de MVPD’s geweigerde vergunningen op toepassingsniveau |
| Aanvraag afspelen | Aantal gegenereerde korte mediatokens (die overeenkomen met het aantal afspeelverzoeken) |
| Aanmelding aangevraagd | Aantal geïnitieerde afmeldingsstromen |
| Afmelden voltooid | Aantal met succes voltooide logout-stromen |
| Afmelden mislukt | Aantal mislukte logout-stromen |
| Voorafgaande toestemming aangevraagd | Aantal geïnitieerde preautorisatiestromen |
| Voorkeursprocedure | Aantal geslaagde preautorisatiegebeurtenissen met de middelen die vooraf werden gemachtigd |
| Toestemming vooraf geweigerd | Aantal gebeurtenissen voorafgaand aan autorisatie met de middelen die vooraf werden geweigerd |
| Voorvertoning is mislukt | Aantal mislukte voorafgaande verificatie-gebeurtenissen |

| Adobe Analytics-naam | Beschrijving |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kanaal | De aanvrager-id die wordt gebruikt voor het uitvoeren van de machtigingsaanvraag |
| MVPD | De MVPD die verantwoordelijk is voor het toekennen van rechten aan de gebruiker |
| Proxy | De proxy MVPD (die &quot;Direct&quot; zal zijn voor directe integratie) |
| SDK-type | De SDK van de client wordt gebruikt (Flash, HTML5, Android native, iOS, Clientless enz.) |
| SDK-versie | De versie van de Adobe Pass Authentication-client SDK |
| Resource ID | De feitelijke titel van de bron die bij het vergunningsverzoek is betrokken (geëxtraheerd uit de MRSS-lading als item/titel indien verstrekt) |
| AuthZ-fouttype | De oorzaak van fouten, zoals gemeld door Adobe Pass-verificatie <br/> Hier zijn de meest voorkomende waarden <br/> **noAuthZ** = MVPD antwoordde dat de gebruiker niet het kanaal in hun pakket <br/> heeft **netwerk** = wij konden niet MVPD bereiken (MVPD heeft een kwestie op het ogenblik van de vraag en antwoordde niet) <br/> **norefreshtoken** = dit is strikt voor implementaties OAuth en het zou kunnen resulteren als de gebruiker hun wachtwoord veranderde of MVPD het om één of andere reden ontkende. Het resulteert typisch in een nieuwe authentificatie <br/> **wanmatch** = als het verzoek van een apparaat wordt gemaakt dat verschillend is dan die die het authentificatietoken had. Dit kan het gevolg zijn als gebruikers proberen het systeem te bedriegen, maar de meeste hiervan deden zich voor in de context van onze oude JavaScript SDK, waar de apparaat-id het IP-adres als onderdeel van de berekening gebruikte. Als een gebruiker TVE thuis en op het werk keek, zou deze fout teweeggebracht worden en zij zouden opnieuw voor authentiek moeten verklaren <br/> **ongeldig** = ongeldig verzoek, ontbrekende of ongeldige parameters <br/>  **authzNone** = De programmeurs hebben de capaciteit om toestemmingen voor een specifieke channelxMVPD combinatie te ontkennen. Dit wordt teweeggebracht door een achterste API dat de Programmeurs toegang tot <br/> hebben **fraude** = het is een beschermingsmechanisme aan onze kant. Als de gebruiker vergunning ontbreekt en het dan een aantal tijden in een kort interval (seconden) opnieuw verzoekt ontkennen wij direct de vraag. Het gebeurt typisch wanneer een Programmer een insect in hun implementatie heeft die om vergunning constant vraagt als het ontbreekt. |
| Type token | Wanneer tokens door AuthZ allen en AuthN allen worden gecreeerd, moeten wij weten wat door een degradatiemaatregel wordt veroorzaakt.<br/> zij zijn:<br/> &quot;normal&quot; = de normale geval <br/> &quot;authall&quot; = wanneer AuthN allen <br/> &quot;authzall&quot;wordt toegelaten = wanneer AuthZ allen <br/> &quot;hba&quot; = wordt toegelaten wanneer HBA wordt toegelaten |
| Apparaattype zonder clip | Het apparaatplatform (alternatief), dat momenteel wordt gebruikt voor Clientless.<br/> De waarden kunnen zijn:<br/> N.v.t. - de gebeurtenis kwam niet uit een Clientless SDK <br/> Onbekend voort - aangezien de deviceType parameter van a **Clientless API** facultatief is, zijn er vraag die geen waarde bevat.<br/> Om het even welke andere waarde die door **Clientless API** werd verzonden. Bijvoorbeeld xbox, appletv en roku. |
| MVPD-gebruikersnaam | Vervangt op cookie gebaseerde bezoeker-id |


## Details {#details-int-authn-analyt}

* De metriek wordt, gebeurtenis voor gebeurtenis, ingevoegd in het specifieke rapportpakket via de API voor het invoegen van gegevens van Adobe Analytics
* De invoeging wordt elke 30 minuten in een batch geplaatst en verzonden. Daarom moet het verslag worden voorzien van een tijdstempel
* Elke klant zal één of veelvoudige rapportseries hebben. Eén aanvrager-id (kanaal) wijst slechts aan één rapportsuite toe. Meerdere verzoek-id&#39;s kunnen aan slechts één rapportsuite worden toegewezen.
* Historische gegevens kunnen worden verstrekt, maar er moet speciale aandacht worden besteed aan verkeersproblemen/prestatieproblemen.
* De unieke bezoekersvariabele wordt ingesteld op de MVPD-gebruikersnaam
* De toewijzing van gebeurtenissen en eVars kan niet worden geconfigureerd.


## SLA {#sla-int-authn-serv-anal}

Aangezien het geen kritieke component is, is er geen SLA-garantie voor de integratie.

## Configuratie van rapportsuite {#report-suite-config}

Het rapport moet een tijdstempel hebben, omdat de gebeurtenissen batchgewijs worden verzonden.

### Gebeurtenissen {#report-suite-config-events}


>[!NOTE]
>Alles moet worden ingesteld met:
>
>* Teller (geen subrelaties)

| Gebeurtenis | Adobe Analytics, gebeurtenis |
|---------------------------------------|-----------------------|
| AuthN opgevraagd | event1 |
| AuthN in behandeling | event2 |
| AuthN OK | event3 |
| AuthZ aangevraagd | event4 |
| AuthZ OK | event5 |
| AuthZ is mislukt | event6 |
| Aanvraag afspelen | event7 |
| AuthN mislukt | event8 |
| Clientless AuthN Token Request OK | event9 |
| Aanvraag voor token voor client-less AuthN is mislukt | event10 |
| Aanvraag afspelen is mislukt | event11 |
| Aanvraag tot aanmelding | event12 |
| Afmelden voltooid | event13 |
| Afmelden mislukt | event14 |
| Preflight-aanvraag | event15 |
| Preflight is mislukt | event16 |
| Preflight toegekend | event17 |
| Preflight geweigerd | event18 |


### eVars {#evars}


>[!NOTE]
>Alles moet worden ingesteld met:
>
>* Toewijzing: meest recente (laatste)
>* Verlopen na: Actief
>* Type: Tekstreeks

| Eigenschap | eVar |
|-----------------------------------|--------------------------------|
| Kanaal | eVar1 |
| MVPD | eVar2 |
| Proxy | eVar3 |
| SDK-type | eVar4 |
| SDK-versie | eVar5 |
| Resource ID | eVar6 |
| AuthZ-fouttype | eVar7 |
| Type token | eVar8 |
| Apparaattype zonder clip | eVar9 |
| MVPD-gebruikersnaam | bezoekerID (automatisch gedaan) |
| MVPD-gebruikersnaam | eVar10 |
| Apparaattype | eVar11 |
| Besturingssysteem | eVar12 |
| Primair hardwaretype | eVar13 |
| TTL | eVar14 |
| Type verificatie | eVar15 |
| Versie van serverarchitectuur | eVar16 |
| Externe verificatieprovider | eVar17 |
| Latentie | eVar18 |
| Bezoeker-id | eVar19 |
| SSO-mechanisme | eVar20 |
| Apparaatmodel | eVar21 |
| Apparaatversie | eVar22 |
| Hardware-model apparaat | eVar23 |
| Hardware-leverancier van apparaat | eVar24 |
| Hardware-fabrikant apparaat | eVar25 |
| Hardware-versie van apparaat | eVar26 |
| Besturingssysteemnaam apparaat | eVar27 |
| Apparaatbesturingssysteem | eVar28 |
| Leverancier van besturingssystemen van apparaat | eVar29 |
| Versie van besturingssysteem van apparaat | eVar30 |
| Naam apparaatbrowser | eVar31 |
| Leverancier apparaatbrowser | eVar32 |
| Versie apparaatbrowser | eVar 33 |
| Genormaliseerd apparaattype zonder clip | eVar34 |

## Prijsstelling {#pricing}

Neem contact op met uw TAM voor meer informatie.
