---
title: Adobe Single Sign-On Service
description: Meer informatie over de Adobe Pass SSO Service die naadloze verificatie mogelijk maakt voor meerdere apparaten en toepassingen.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 1%

---


# Adobe Single Sign-On Service {#sso-service}

In dit document worden gebruiksgevallen, eindpunten en API voor Single Sign-On Service van Adobe beschreven.

**Huidige Herziening - 1.0.0**

## Toepassingsgebied {#scope}

De Adobe Pass SSO-service maakt naadloze verificatie mogelijk op meerdere apparaten en toepassingen en biedt een uniforme gebruikerservaring met behoud van beveiligings- en compatibiliteitsnormen. Deze service is bedoeld om tegemoet te komen aan de groeiende behoefte aan platformonafhankelijke verificatie in het huidige landschap voor het streamen van meerdere apparaten.

## Overzicht {#overview}

### Wat het is {#what-it-is}

Een uitgebreide Single Sign-On oplossing die gebruikers toestaat eens voor authentiek te verklaren en de overdracht van authentificatie op een geïnformeerde manier aan andere apparaten te beheren

### Huidige uitdagingen voor verificatie {#current-challenges}

* Gebruiker moet zich bij elke streamingservice waarvoor hij een abonnement heeft, verifiëren
* Gebruiker moet op elk apparaat of elke toepassing afzonderlijk verifiëren
* Omdat het typen van een wachtwoord in authentificatiestroom op sommige platforms moeilijk is, kan dit tot verhoogde verlaten tarieven leiden

### Mogelijkheid voor een Adobe Pass SSO-service {#opportunity}

* Het verhogen van aantal het stromen diensten die hun eigen authentificatie vereisen
* Toenemende vraag naar naadloze ervaringen tussen apparaten
* Het aantal streaming devices per huishouden verhogen
* Noodzaak van uniforme authenticatie per huishouden

### Bedrijfsvoordelen {#business-benefits}

#### Inhoudsproviders {#content-providers}

* **Verhoogde Betrokkenheid van de Gebruiker** - De naadloze ervaring leidt tot langere zittingstijden
* **Verminderde wrijving** - de lagere authentificatiestrijdheden verhogen inhoudsverbruik
* **Verbeterd Behoud** - de betere gebruikerservaring vermindert koor
* **Kostenvermindering** - Minder steunkaartjes met betrekking tot authentificatiekwesties

#### Eindgebruikers {#end-users}

* **Naadloze Ervaring** - verifieer eens, toegang overal
* **Besparingen van de Tijd** - Geen herhaalde login processen
* **Flexibiliteit van het Apparaat** - Schakelaar tussen apparaten zonder onderbreking
* **Consistente Ervaring** - Uniforme authentificatie over alle platforms

#### IdPs (MVPDs, Telcos, enz.) {#idps}

* MVPDs kan van extra apparaten worden geïnformeerd gebruikend voor authentiek verklaarde SSO
* **Verbeterde Tevredenheid van de Gebruiker** - verbeterde authentificatieervaring
* **Verminderde Lading van de Steun** - Minder op authentificatie betrekking hebbende steunvraag
* **Concurrerend Voordeel** - betere gebruikerservaring dan concurrenten

## Gevallen gebruiken {#use-cases}

### Identiteitskaart {#identity-mapping}

De service bouwt de mogelijkheid om een D2C- en een TVE-account in dezelfde app te koppelen.

Meer streaming services zijn bundels die aan derden (MVPD/Virtual MVPD/Telco, enz.) worden verkocht. Het kan zijn dat gebruikers meerdere accounts in dezelfde app moeten verwerken. Voor een naadloze verificatie moet een service deze accounts overbruggen om de aanmeldingservaring te vereenvoudigen.

De gebruiker zal met zijn D2C- rekening voor authentiek verklaren, dan verklaart ONCE met een verschillende rekening (ex. MVPD). Met onze service zullen deze accounts gekoppeld zijn, wat betekent dat in de toekomst verdere verificaties op verschillende apparaten alleen met de D2C-account kunnen worden uitgevoerd.

### SSO voor meerdere apparaten {#cross-device-sso}

Verificatie op een apparaat met een tv-verbinding kan lastiger zijn dan op een mobiele telefoon. Een goede gebruikerservaring zou op de telefoon voor authentiek moeten verklaren en dan die authentificatie aan slimme TV overgaan.

## Belangrijkste onderdelen {#key-components}

* **Symbolische API van de Dienst** - zeer belangrijke component die veilig het Enige Sign On mechanisme beheert
* **Lijst API** - de Toepassing kan de gebruiker helpen de lijst van apparaten in zijn ecosysteem begrijpen
* **Verbinding API** - de Toepassing laat de gebruiker toe om extra apparaten in zijn ecosysteem toe te voegen
* **ontkoppelen API** - de Toepassing laat de gebruiker toe om apparaten in zijn ecosysteem te verwijderen

## Gedetailleerde gevallen gebruiken {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

Met dit gebruiksgeval kunt u koppelingen tot stand brengen tussen D2C- en TVE(MVPD)-referenties op één apparaat en dat gekoppelde profiel gebruiken op andere apparaten in dezelfde toepassing.

![ D2C-TVE SSO-stroom ](../../../assets/sso_service_d2c_1.png)

![ stroom van 0} dwars-ApparaatSSO ](../../../assets/sso_service_d2c_2.png)

### SSO voor meerdere apparaten {#cross-device-sso-detailed}

Met dit gebruiksgeval kunnen gebruikers die al op het ene apparaat zijn geverifieerd, het geverifieerde profiel hergebruiken voor dezelfde D2C- of TVE-toepassing die op andere apparaten is geïnstalleerd (toepassing die is vereist voor het implementeren van Adobe Pass REST V2 voor verificatie en autorisatie).

## Adobe SSO Service integreren in een D2C-TVE-toepassing {#integration}

### Stap 1 - krijg een gemeenschappelijke herkenningsteken {#step-1}

Om Adobe SSO Service te integreren, zou een toepassingsimplementatie een unieke en blijvende identiteitskaart moeten vestigen om als gemeenschappelijke herkenningstekenattribuut SSO in x-SSO-ID te gebruiken. Dit kan worden verkregen door een gebruiker met de dienst te verifiëren D2C en een attribuut over deze authentificatie te behouden.

### Stap 2 - verkrijg een Token van de Dienst {#step-2}

Het gebruiken van het gemeenschappelijke herkenningsteken in x-SSO-identiteitskaart in POST /serviceToken eindpunt zal een ondertekende JWT met de volgende nuttige lading terugwinnen:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

De token Service heeft &quot;iat&quot; - uitgegeven bij en &quot;exp&quot; - vervaltijd waarin het interval van de servicetoker geldig is. In het geval van afloop, kan nieuw JWT gebruikend het eindpunt van GET /serviceToken met het verlopen Symbolische van de Dienst worden verkregen.

### Stap 3 - Verificatie met Adobe Pass REST API V2 met een TVE MVPD {#step-3}

De authentificatie met Adobe Pass zou moeten worden uitgevoerd gebruikend het Token van de Dienst: [ REST API V2 - Enige Sign-On de Symbolische Stroom van de Dienst ](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Stap 4 - Een ander apparaat koppelen {#step-4}

Vanuit de geverifieerde toepassing kan een koppeling worden gemaakt voor gebruik op een ander apparaat met de /link-API

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

De &quot;code&quot;is een korte levende verbindingscode in een vorm van een opeenvolging van 6 cijfers die de gebruiker in een niet voor authentiek verklaarde toepassing op een secundair apparaat zal introduceren

### Stap 5 - ontvang Single Sign On authentificatie {#step-5}

Op een ander apparaat kan de toepassing, zodra de gebruiker de code heeft geïntroduceerd, het volgende doen:

* Identiteit ophalen van de D2C-service
* MVPD-profiel ophalen van Adobe Pass met REST V2 API

Het MVPD-profiel is geldig via de SSO voor de periode dat de initiële verificatie is verkregen.

Als MVPD het profiel ongeldig maakt of als de gebruiker zich afmeldt bij MVPD, heeft de toepassing die Adobe Pass REST API V2 gebruikt geen profielrecord meer en moet de gebruiker opnieuw worden geverifieerd bij de MVPD.

### Stap 6 - Single Sign On beheren op andere apparaten {#step-6}

Een toepassing kan informatie over alle andere apparaten krijgen verbonden met het zelfde gemeenschappelijke herkenningsteken gebruikend /list API.

Op elk ogenblik, kan een apparaat uit het Enige Teken worden verwijderd door dat apparaat met /unlink API los te maken.

## API&#39;s {#apis}

### Service Token-API {#service-token-api}

#### Beschrijving {#service-token-description}

De token-API van de service kan worden gebruikt om servicetokens aan te vragen en te beheren die SSO-functionaliteit (Single Sign-On) tussen meerdere toepassingen of apparaten inschakelen. Deze servicetokens identificeren geverifieerde profielen (SSO-profielen) en zijn essentieel voor het tot stand brengen en onderhouden van SSO-verbindingen.

>[!WARNING]
>
>De tokens van de dienst bevatten gevoelige authentificatieinformatie. Toepassingen moeten deze tokens veilig verwerken en deze nooit aan onbetrouwbare partijen laten zien.

De API van het Token van de Dienst verstrekt twee primaire eindpunten:

* **POST /api/{serviceProvider} /serviceToken** - verkrijg een pas gecreeerd JWS de dienstteken
* **GET /api/{serviceProvider}/serviceToken** - vernieuw een bestaand JWS de dienstteken

Als de service Token API-aanvraag niet kan worden onderhouden vanwege een fout met de Adobe Pass-verificatieservices, wordt aanvullende foutinformatie opgenomen als onderdeel van de API-reactie.

#### POST - serviceToken {#post-service-token}

##### Verzoek {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De dienstverlener herkenningsteken waarvoor het teken wordt gevraagd.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>
         De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> AP-apparaat-Identifier </a> kopbaldocumentatie.
         <br/><br/>
         Deze id wordt gebruikt als de standaard SSO-id wanneer X-SSO-ID niet wordt opgegeven.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Apparaat-Info</td>
      <td>
         De apparateninformatie zoals die in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info"> x-apparaat-Info </a> kopbaldocumentatie wordt gespecificeerd.
         <br/><br/>
         <b> sterk geadviseerd </b> om te gebruiken wanneer het het apparatenplatform van de toepassing het verstrekken van geldige waarden uitdrukkelijk toestaat.
         <br/><br/>
         Met de Adobe Pass-verificatiebackend worden expliciet ingestelde waarden samengevoegd met impliciet geëxtraheerde waarden. Als deze optie niet is opgegeven, worden standaard geëxtraheerde waarden gebruikt.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         De koppelingscode die deze aanvraag koppelt aan een bestaand geverifieerd profiel. Wanneer verstrekt, zal de reactie een de dienstteken voor SSO met het profiel omvatten dat de verbindingscode produceerde.
         <br/><br/>
         Dit wordt doorgaans gebruikt wanneer een secundaire toepassing of apparaat verbinding wil maken met een geverifieerd profiel vanuit een primaire toepassing of apparaat.
      </td>
      <td>vereist als x-sso-id niet is opgegeven</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         De gemeenschappelijke herkenningsteken dat de toepassing verzoekt om SSO te baseren.
         <br/><br/>
         Wanneer deze id wordt opgegeven, wordt deze gebruikt om een gemeenschappelijk SSO-profiel voor verschillende apparaten en/of toepassingen tot stand te brengen.
      </td>
      <td>vereist als x-sso-verbinding niet wordt verstrekt</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd moet het application/json zijn.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

##### Antwoord {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Gemaakt</td>
      <td>
        Het de dienstteken werd met succes geproduceerd en teruggekeerd in het reactiemechanisme.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

###### Succes {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (bijvoorbeeld "CREATED")</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Een Base64-Gecodeerde handtekening van het Web JSON (JWS) die het de dienstteken bevat. Dit token kan worden gebruikt in volgende API-aanroepen om het geverifieerde profiel te identificeren en SSO-functionaliteit in te schakelen.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Zoeken naar ms, of 0 bij fout</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Zoeken naar ms, of 0 bij fout</td>
      <td><i>vereist</i></td>
   </tr>
</table>

###### Fout {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400 401 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples-post-service-token}

### &#x200B;1. Vraag een nieuw de dienstteken (met identiteitskaart SSO) aan

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Vraag een nieuw de dienstteken (met Verbinding SSO) aan

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Verzoek {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De dienstverlener herkenningsteken waarvoor het teken wordt gevraagd.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Een eerder verkregen service-token dat moet worden vernieuwd.
         <br/><br/>
         Deze token moet geldig zijn of onlangs zijn verlopen om te kunnen worden vernieuwd.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd moet het application/json zijn.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

##### Antwoord {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Het de dienstteken werd met succes verfrist en in het reactievermogen teruggekeerd.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken of de dienstteken is ongeldig, moet de cliënt een nieuw toegangstoken of de dienstteken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

###### Succes {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (bijvoorbeeld "OK")</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Een Base64-gecodeerde JSON Web Signature (JWS) die het vernieuwde servicetoken bevat. Dit token kan worden gebruikt in volgende API-aanroepen om het geverifieerde profiel te identificeren en SSO-functionaliteit in te schakelen.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Zoeken naar ms, of 0 bij fout</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Zoeken naar ms, of 0 bij fout</td>
      <td><i>vereist</i></td>
   </tr>
</table>

###### Fout {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400 401 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples-get-service-token}

### &#x200B;1. Verzoek om een de dienstteken te verfrissen

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### KoppelingsAPI {#link-api}

#### Beschrijving {#link-description}

De verbinding API kan worden gebruikt om een verbindingscode (of QR code) te verzoeken die Enige Sign-On (SSO) tussen veelvoudige toepassingen of apparaten kan toelaten. Met deze koppelingscode kunnen gebruikers een nieuwe toepassing of een nieuw apparaat verbinden met een bestaand geverifieerd profiel (SSO-profiel), zodat ze een naadloze SSO-ervaring hebben op verschillende toepassingen of apparaten.

De verbinding API vereist een geldig de dienstteken dat in de AD-Dienst-Symbolische kopbal moet worden verstrekt.

De gegenereerde koppelingscode wordt doorgaans weergegeven aan gebruikers in een primaire toepassing of apparaat en wordt ingevoerd in een secundaire toepassing of apparaat om de SSO-verbinding tot stand te brengen. De koppelingscode heeft een beperkte geldigheidsperiode (doorgaans 5-30 minuten) en is bedoeld voor eenmalig gebruik.

Als de koppelings-API-aanvraag niet kan worden onderhouden vanwege een fout in de Adobe Pass-verificatieservices, worden aanvullende foutgegevens opgenomen als onderdeel van het resultaat van de Link API-reactie.

#### POST - koppeling {#post-link}

##### Verzoek {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De dienstverlener herkenningsteken waarvoor het teken wordt gevraagd.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> AP-apparaat-Identifier </a> kopbaldocumentatie.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         De generatie van het de dienstteken wordt beschreven door de Symbolische API documentatie van de Dienst.
         <br/><br/>
         Dit de dienstteken identificeert het voor authentiek verklaarde profiel waarvoor de verbindingscode zal worden geproduceerd.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd moet het application/json zijn.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

##### Antwoord {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Gemaakt</td>
      <td>
        De koppelingscode is gegenereerd en geretourneerd in de hoofdtekst van de reactie.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

###### Succes {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (bijvoorbeeld "CREATED")</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">link</td>
      <td>Een korte numerieke of alfanumerieke code die kan worden gebruikt op een secundaire toepassing of apparaat om een SSO-verbinding tot stand te brengen.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>De tijdstempel (in milliseconden sinds het begin van het tijdperk) wanneer de koppelingscode geldig wordt.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>De tijdstempel (in milliseconden sinds het begin van het tijdperk) wanneer de koppelingscode verloopt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

###### Fout {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400 401 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples-post-link}

### &#x200B;1. Vraag een koppelingscode aan voor een bestaand geverifieerd profiel

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### API ontkoppelen {#unlink-api}

#### Beschrijving {#unlink-description}

De ontkoppelings-API kan worden gebruikt om te verzoeken dat een of meerdere apparaten uit een geverifieerd profiel (SSO-profiel) worden verwijderd. Met deze API kunnen gebruikers apparaten loskoppelen van hun SSO-installatie en zo bepalen welke apparaten toegang hebben tot hun geverifieerde profiel.

>[!WARNING]
>
>Voor de ontkoppelings-API moet een geldig servicetoken worden opgegeven in de header AD-Service-Token.

Als de Unlink API-aanvraag niet kan worden onderhouden vanwege een fout met de Adobe Pass Authentication-services, wordt aanvullende foutinformatie opgenomen als onderdeel van het Resultaat van de Unlink API-reactie.

#### POST - ontkoppelen {#post-unlink}

##### Verzoek {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De dienstverlener herkenningsteken.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Bodyparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">apparaten</td>
      <td>
         Array met apparaat-id's die moeten worden ontkoppeld.
         <br/><br/>
         Voorbeeld: <br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>
         Het geaccepteerde mediatype voor de bronnen die worden verzonden.
         <br/><br/>
         Het moet application/json zijn.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> AP-apparaat-Identifier </a> kopbaldocumentatie.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         De generatie van het de dienstteken wordt beschreven door de Symbolische API documentatie van de Dienst.
         <br/><br/>
         Dit de dienstteken identificeert het voor authentiek verklaarde profiel waarvoor de apparaten zullen worden losgemaakt.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd moet het application/json zijn.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

##### Antwoord {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        De aangevraagde apparaten zijn ontkoppeld van de SSO-installatie.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode niet toegestaan</td>
      <td>
        De HTTP-methode is ongeldig, de client moet een HTTP-methode gebruiken die is toegestaan voor de aangevraagde resource en het opnieuw proberen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

###### Succes {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Informatie over het resultaat van de bewerking: "OK"</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Lijst met goed ontkoppelde apparaten.
         <br/><br/>
         Voorbeeld: <br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>vereist</i></td>
   </tr>
</table>

###### Fout {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples-post-unlink}

### &#x200B;1. Verzoek om apparaten te ontkoppelen van een SSO-profiel

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Verzoek om apparaten te ontkoppelen met gedeeltelijk succes

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### Lijst-API {#list-api}

#### Beschrijving {#list-description}

De lijst-API kan worden gebruikt om een lijst op te halen met apparaten die momenteel zijn aangesloten op een geverifieerd profiel (SSO-profiel). Met deze API kunnen gebruikers en toepassingen zien welke apparaten deel uitmaken van hun SSO-installatie, waardoor ze zicht en beheermogelijkheden hebben voor hun geverifieerde ervaring op meerdere apparaten.

>[!WARNING]
>
>De lijst API vereist een geldig de dienstteken dat in de AD-Dienst-Symbolische kopbal moet worden verstrekt.

De lijst-API retourneert gegevens over elk apparaat in het geverifieerde profiel (SSO-profiel), inclusief apparaat-id&#39;s en metagegevens die gebruikers kunnen helpen hun apparaten te herkennen. Deze informatie kan worden gebruikt om gebruikers te helpen geïnformeerde besluiten nemen over welke apparaten in hun opstelling SSO zouden moeten blijven.

Als de API-aanvraag van de List niet kan worden onderhouden vanwege een fout in de Adobe Pass-verificatieservices, worden aanvullende foutgegevens opgenomen als onderdeel van het responsresultaat van de List API.

#### GET - lijst {#get-list}

##### Verzoek {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De dienstverlener herkenningsteken.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> AP-apparaat-Identifier </a> kopbaldocumentatie.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         De generatie van het de dienstteken wordt beschreven door de Symbolische API documentatie van de Dienst.
         <br/><br/>
         Dit de dienstteken identificeert het voor authentiek verklaarde profiel waarvoor de apparatenlijst zal worden teruggewonnen.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd moet het application/json zijn.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

##### Antwoord {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        De lijst van apparaten in de opstelling SSO werd met succes teruggewonnen en teruggekeerd in het reactielichaam.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode niet toegestaan</td>
      <td>
        De HTTP-methode is ongeldig, de client moet een HTTP-methode gebruiken die is toegestaan voor de aangevraagde resource en het opnieuw proberen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

###### Succes {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">apparaten</td>
      <td>
         JSON met een overzicht van sleutel-, waardeparen.
         <br/><br/>
         <b> Sleutel:</b> deviceId - de nuttige lading van het apparatenherkenningsteken zoals die in <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> wordt beschreven AP-Apparaat-Identifier </a> kopbaldocumentatie
         <br/><br/>
         <b> Waarde:</b> attributen - JSON die een kaart van apparatenmeta-gegevensattributen met inbegrip van bevatten:
         <ul>
            <li>apparaattype</li>
            <li>platform</li>
            <li>gebruikersagent</li>
            <li>andere relevante metagegevens die helpen het apparaat te identificeren</li>
         </ul>
         De waarden voor de kenmerken kunnen eenvoudig zijn (tekenreeks, geheel getal, Boolean, enz.)
      </td>
      <td><i>vereist</i></td>
   </tr>
</table>

###### Fout {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples-get-list}

### &#x200B;1. Verzoek om apparaten in een SSO-profiel op te nemen

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Verzoek om apparaten zonder gekoppelde apparaten weer te geven

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Foutcodes {#error-codes}

### Structuur van foutreactie {#error-structure}

Alle reacties op fouten bevatten de volgende velden:

| Veld | Type | Beschrijving |
|:---|:---|:---|
| status | Geheel | HTTP-statuscode (400, 401, 500, enz.) |
| code | String | Machine leesbare foutcode |
| message | String | Menselijke beschrijving |
| action | String | Voorgestelde handeling voor client |
| helpUrl | String | Documentatiekoppeling |
| traceren | String | Unieke aanvraag-id (UUID) voor correlatie |

**Voorbeeld van de Reactie van de Fout**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Voorbeeld van de Reactie van het Succes**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Succesreacties bevatten GEEN foutveld. Foutreacties bevatten GEEN succesvelden.

### Catalogus foutcodes {#error-catalog}

#### Algemene fouten

| code | status | message | action |
|:---|:---|:---|:---|
| token_invalid | 400 | De opgegeven token is ongeldig | get_new_token |
| token_expired | 401 | De token is verlopen | get_new_token |
| header_missing | 400 | Een vereiste koptekst ontbreekt | check_headers |
| onbevoegd | 401 | Onbevoegde toegang | none |
| internal_error | 500 | Er is een interne fout opgetreden | none |

#### Validatiefouten

| code | status | message | action |
|:---|:---|:---|:---|
| request_null | 400 | Aanvraagobject mag niet null zijn | none |

#### POST ServiceToken-fouten

| code | status | message | action |
|:---|:---|:---|:---|
| header_missing | 400 | De header x-sso-id of x-sso-link is vereist voor POST-aanvragen | check_headers |
| header_missing | 400 | AP-Device-Identifier-header is vereist voor POST-aanvragen | check_headers |

#### GET ServiceToken-fouten

| code | status | message | action |
|:---|:---|:---|:---|
| header_missing | 400 | AD-Service-Token header is vereist voor GET-aanvragen | check_headers |
| header_invalid | 401 | Ongeldige JWT-handtekening in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij valideren van JWT-handtekening | get_new_token |
| header_invalid | 401 | JWT-onderwerp (sub) ontbreekt of is leeg in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij uitpakken van JWT-onderwerp | get_new_token |

#### Validatiefouten koppeling

| code | status | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token header is vereist voor koppelingsaanvragen | check_headers |
| header_invalid | 401 | Ongeldige JWT-handtekening in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij valideren van JWT-handtekening | get_new_token |

#### Validatiefouten ontkoppelen

| code | status | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token header is vereist voor ontkoppelingsverzoeken | check_headers |
| header_invalid | 401 | Ongeldige JWT-handtekening in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij valideren van JWT-handtekening | get_new_token |
| header_invalid | 401 | JWT-onderwerp (sub) ontbreekt of is leeg in AD-Service-Token | get_new_token |
| request_invalid | 400 | De lijst met apparaten kan niet null of leeg zijn | check_request_body |

#### Validatiefouten lijst

| code | status | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token header is vereist voor lijstaanvragen | check_headers |
| header_invalid | 401 | Ongeldige JWT-handtekening in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij valideren van JWT-handtekening | get_new_token |
| header_invalid | 401 | JWT-onderwerp (sub) ontbreekt of is leeg in AD-Service-Token | get_new_token |
| header_invalid | 401 | Fout bij uitpakken van JWT-onderwerp | get_new_token |

