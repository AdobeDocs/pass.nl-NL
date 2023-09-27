---
title: JavaScript SDK API-naslag
description: JavaScript SDK API-naslag
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2836'
ht-degree: 0%

---

# JavaScript SDK API-naslag {#javascript-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## API-naslag {#api-reference}

Deze functies stellen verzoeken om interactie met MVPD in werking. Alle vraag is asynchroon; u moet uitvoeren [callbacks](#callbacks) om de reacties af te handelen:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorisedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestID, eindpunten, opties){#setrequestor(inRequestorID,endpoints,options)}

**Omschrijving:** Identificeert de plaats waarvan de verzoeken voortkomen.  U moet deze vraag vóór een andere API vraag in een communicatie zitting maken.

**Parameters:**

- *inRequestID* - De unieke id die door de Adobe tijdens de registratie aan de oorspronkelijke site is toegewezen.

- *eindpunten* - Deze parameter is optioneel. Dit kan een van de volgende waarden zijn:

   - Een serie die u toestaat om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). Als er meerdere URL&#39;s worden opgegeven, bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt. Standaard wordt de Adobe-serviceprovider gebruikt (<http://sp.auth.adobe.com/>).

  Voorbeeld:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *opties* - Een JSON-object met de waarden voor toepassings-id, Vernieuwingsinstellingen voor de bezoeker-id (aanmeldingsgegevens voor de achtergrond) en MVPD-instellingen (iFrame). Alle waarden zijn optioneel.
   1. Indien gespecificeerd, zou Experience Cloud bezoekerID op alle netwerkvraag worden gemeld die door de bibliotheek wordt uitgevoerd. De waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   2. Als de unieke id van de toepassing is opgegeven -`applicationId` - de waarde zal aan alle verdere vraag worden toegevoegd die door de toepassing als deel van de x-Apparaat-Info HTTP- kopbal wordt gemaakt. Deze waarde kan later worden opgehaald [ESM](/help/authentication/entitlement-service-monitoring-overview.md) rapporten die de juiste vraag gebruiken.

  **Opmerking:** Alle JSON-toetsen zijn hoofdlettergevoelig.

  Voorbeeld:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- De programmeur kan de montages met voeten treden MVPD die in de Authentificatie van Adobe Pass worden gevormd, door te specificeren of een iFrame voor login wordt vereist (*iFrameRequired* key) en de iFrame-afmetingen (*iFrameWidth* en *iFrameHeight* toetsen). Het JSON-object heeft de volgende sjabloon:

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Alle toetsen op het hoogste niveau in de bovenstaande sjabloon zijn optioneel en hebben standaardwaarden (*backgroundLogin*, *backgroundLogut* zijn door gebrek, en mvpdConfig ongeldig - betekenend dat geen montages MVPD worden met voeten getreden).


- **Opmerking**: Het opgeven van ongeldige waarden/typen voor de bovenstaande parameters resulteert in ongedefinieerd gedrag.



Hier is een voorbeeldconfiguratie voor het volgende scenario: het activeren van login en logout verfrist zich zonder, veranderend MVPD1 in volledige pagina-omleiding login (niet-iFrame) en MVPD2 aan iFrame login met width=500 en height=300:

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Callbacks geactiveerd:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Terug naar boven](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Omschrijving:** Verzoekt om toestemming voor de gespecificeerde bron. Telkens als een klant probeert om tot een toegelaten middel toegang te hebben, roep deze functie om een kort-levend vergunningsteken van Toegang te verkrijgen Enabler. Middel IDs wordt overeengekomen met MVPD die vergunning verstrekt.

Gebruikt het caching authentificatietoken voor de huidige klant. Als zulk een teken niet wordt gevonden, stelt eerst het authentificatieproces in werking, dan gaat met vergunning verder.

**Parameters:**

- `inResourceID` - De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- `redirect_url` - Geef optioneel een omleidings-URL op, zodat de gebruiker tijdens het MVPD-autorisatieproces naar die pagina terugkeert in plaats van naar de pagina vanwaar de autorisatie is gestart.


**Callbacks geactiveerd:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) over succes, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) bij mislukken

>[!CAUTION]
>
>Gebruik indien mogelijk checkAuthorization() in plaats van getAuthorization(). De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit zou tot een ingewikkelde implementatie aan de kant van de Programmer kunnen leiden.

</b>

[Terug naar boven](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Omschrijving:** Verzoekt verificatie voor de huidige klant. Wordt doorgaans aangeroepen als reactie op een klik op een knop Aanmelden. Controleert op een in de cache opgeslagen verificatietoken voor de huidige klant. Als een dergelijk token niet wordt gevonden, wordt het verificatieproces gestart. Dit roept het gebrek of de douane leverancier-selectie dialoog aan, dan gebruikt de geselecteerde leverancier om aan de login van MVPD interface om te leiden.

Als de verificatie is gelukt, wordt een verificatietoken voor de gebruiker gemaakt en opgeslagen. Als de verificatie mislukt, geeft de provider een geschikt foutbericht aan uw [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) callback.

**Parameters:**

- redirect_url - Naar keuze verstrek URL, zodat het de authentificatieproces van MVPD de gebruiker aan die pagina eerder dan de pagina terugkeert waarvan authentificatie werd in werking gesteld.

**Callbacks geactiveerd:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Terug naar boven](#top)

</br>

## checkAuthN {#checkauthn}

**Omschrijving:** Controleert de huidige authentificatiestatus voor de huidige klant.  Niet gekoppeld aan een gebruikersinterface.

**Callbacks geactiveerd:** [setAuthentationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Terug naar boven](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Omschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstatus voor de huidige klant en het bepaalde middel te controleren. Het begint door de authentificatiestatus eerst te controleren. Indien niet geverifieerd, wordt de tokenRequestFailed() callback geactiveerd en wordt de methode afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie de details over de [getAuthorization()](#getAuthZ methode.

>[!TIP]
>
> **Functies voor het controleren van de status gebruiken**  U hoeft de status van verificatie of autorisatie niet te controleren voordat u een autorisatie aanvraagt. U kunt deze functies bijvoorbeeld aanroepen om uw eigen statusweergave bij te werken. Gebruik deze niet wanneer u gebruikersinteractie nodig heeft.

**Parameters:**

- `inResourceID` - De id van de bron waarvoor de gebruiker toestemming aanvraagt.


**Callbacks geactiveerd:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorisedResources(resources) {#checkPreauthorizedResources(resources)}

**Omschrijving:** Vraagt de &quot;preflight&quot;vergunningsstatus voor een lijst van middelen.

**Parameters:**

- *bronnen*: De parameter resources is een array van bronnen waarvoor de autorisatie moet worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, namelijk is het een overeengekomen waarde die tussen Programmer en MVPD, of een mediaRSS fragment wordt gevestigd.

</br>

## checkPreauthorisedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Deze API-variant is beschikbaar vanaf JS SDK versie 4.0


**Parameters:**

- *bronnen*: De parameter resources is een array van bronnen waarvoor de autorisatie moet worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, namelijk is het een overeengekomen waarde die tussen Programmer en MVPD, of een mediaRSS fragment wordt gevestigd.

- *cachegeheugen*: Of de interne cache moet worden gebruikt bij het controleren op vooraf geautoriseerde bronnen. Dit is een optionele parameter, die standaard wordt ingesteld op **true**. Als de waarde true is, is het gedrag identiek aan de bovenstaande API. Dit betekent dat volgende aanroepen naar deze functie een interne cache gebruiken om vooraf geautoriseerde resource op te lossen. Passend **false** voor deze parameter het interne cachegeheugen onbruikbaar maken, resulterend in een servervraag telkens als **checkPreauthorisedResources** API wordt aangeroepen.

**Callbacks geactiveerd:** [preauthorisedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Terug naar boven](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Omschrijving:** Hiermee wordt informatie opgehaald die als metagegevens wordt weergegeven door de bibliotheek Access Enabler.

Er zijn twee typen metagegevens:

- **Statisch** (Het teken van de authentificatie TTL, het teken van de Vergunning TTL, en identiteitskaart van het Apparaat)
- **Metagegevens gebruiker** (Dit omvat gebruikersspecifieke informatie die van MVPD aan het apparaat van de gebruiker tijdens de Authentificatie en/of de stromen van de Vergunning wordt overgegaan)

**Meer informatie:** [Metagegevens gebruiker](#UserMetadata)

**Parameters:**

- *key*: Een id die de gevraagde metagegevens opgeeft:
   - Als key is `"TTL_AUTHN",` dan wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.

   - Als key is `"TTL_AUTHZ"` en params is een serie die middelidentiteitskaart als koord bevatten, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.

   - Als key is `"DEVICEID"` dan wordt de vraag gemaakt om huidige apparatenidentiteitskaart te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.

   - Als de sleutel uit de volgende lijst van gebruikers meta-gegevenstypes is, wordt een voorwerp JSON die de overeenkomstige gebruikersmeta-gegevens bevat verzonden naar [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) callback-functie:

   - `"zip"` - Postcode

   - `"encryptedZip"` - Gecodeerde postcode

   - `"householdID"` - Huishoudelijke identificatiecode. Als een MVPD geen subaccounts ondersteunt, is dit identiek aan userID.

   - `"maxRating"` - Maximale ouderlijke classificatie voor de gebruiker

   - `"userID"` - De gebruikersnaam. In het geval waarin een MVPD subaccounts ondersteunt en de gebruiker niet het hoofdaccount is, zal de gebruikersnaam anders zijn dan de huishoudelijke id.

   - `"channelID"` - De lijst met kanalen die de gebruiker mag bekijken

   - `"is_hoh"` - Markering die aangeeft of een gebruiker het hoofd van een huishouden is

   - `"encryptedZip"` - Gecodeerde postcode

   - `"typeID"` - Markering die aangeeft of de gebruikersaccount een primaire/secundaire account is

   - `"primaryOID"` - Huishoudelijke identificatiecode

   - `"postalCode"` - Vergelijkbaar met postcode

   - `"acctID"` - Account-ID

   - `"acctParentID"` - Bovenliggende ID account

  **Opmerking**: De feitelijke metagegevens van de gebruiker die beschikbaar zijn voor een programmeur, zijn afhankelijk van wat een MVPD beschikbaar maakt.  Zie [Metagegevens gebruiker](#UserMetadata) voor de huidige lijst met beschikbare metagegevens van gebruikers.


Bijvoorbeeld:

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Callbacks geactiveerd:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Terug naar boven](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Omschrijving:** Roep deze functie aan wanneer de gebruiker een MVPD van uw leverancier-selectie UI heeft geselecteerd om de leveranciersselectie naar Toegankelijkheid te verzenden of deze functie met een ongeldige parameter te roepen voor het geval de gebruiker uw leverancier-selectie UI zonder een leverancier te selecteren verwierp.

**Callbacks geactiveerd:**[ setAuthentationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Terug naar boven](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Omschrijving:** Hiermee worden de resultaten opgehaald van de selectie van de klant in het dialoogvenster voor het selecteren van de provider. Dit kan op elk ogenblik na de eerste authentificatiecontrole worden gebruikt.

Deze functie is asynchroon en retourneert het resultaat naar uw `selectedProvider()` callback-functie.

- **MVPD** De momenteel geselecteerde MVPD, of ongeldig als geen MVPD werd geselecteerd.
- **AE_State** Het resultaat van authentificatie voor de huidige klant één van &quot;Nieuwe Gebruiker&quot;, &quot;Gebruiker niet voor authentiek verklaard&quot;, of &quot;Gebruiker voor authentiek verklaard&quot;

**Callbacks geactiveerd:** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Terug naar boven](#top)

</br>

## afmelden {#logout}

**Omschrijving:** Logs uit de huidige klant, die alle authentificatie en vergunningsinformatie voor die gebruiker ontruimt. Verwijdert alle authN en authZ tokens van het systeem van de klant.

**Callbacks geactiveerd:** [setAuthentationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Terug naar boven](#top)

</br>

## Callback-definitie {#calllback-definitions}

U moet deze callbacks uitvoeren om de reacties op uw asynchrone verzoekvraag te behandelen:

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorisedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**Omschrijving:** Teweeggebracht wanneer Toegang Enabler initialisering heeft voltooid en klaar is om verzoeken te ontvangen. Implementeer deze callback om te weten wanneer u de communicatie met de API van Enabler van de Toegang kunt beginnen.
</br>

[Terug naar boven](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Omschrijving:** Voer deze callback uit om de configuratieinformatie en lijst te ontvangen MVPD.

**Parameters:**

- *configXML*: xml-object met de configuratie voor de huidige REQUESTOR, inclusief de MVPD-lijst.


**geactiveerd door:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Terug naar boven](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Omschrijving:** Voer deze callback uit om uw eigen douane leverancier-selectie UI aan te halen. Het dialoogvenster moet de weergavenaam (en het optionele logo) gebruiken om de keuze van de klant te bepalen. Wanneer de klant een keuze heeft gemaakt en het dialoogvenster heeft gesloten, verzendt u de bijbehorende id voor de gekozen provider in de oproep naar *setSelectedProvider()*.

**Parameters:**

- *providers* - Een array met objecten die de gevraagde MVPD&#39;s vertegenwoordigen:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**geactiveerd door:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Terug naar boven](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Omschrijving:** Implementeer deze callback als de gebruiker een MVPD selecteerde die een iFrame vereist waarin om zijn authentificatie login pagina UI te tonen.

**geactiveerd door:**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Terug naar boven](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Omschrijving:** Voer deze callback uit om de authentificatiestatus (1=voor authentiek verklaard of 0=niet voor authentiek verklaard) en een beschrijvend foutenbericht te ontvangen als om het even welke fout terwijl het proberen om de authentificatiestatus (lege koord op succesvolle voltooiing van de controle) voorkwam.

>[!NOTE]
> 
>Als u de huidige [Geavanceerde foutmelding](/help/authentication/error-reporting.md) systeem, kunt u de errorCode parameter negeren die naar deze functie wordt verzonden.  Nochtans, is de isAuthenticated vlag nog van gebruik voor het volgen van de authentificatiestatus van een gebruiker in de machtigingsstroom


**Parameters:**

- *isAuthenticated* - Verstrekt authentificatiestatus: 1 (voor authentiek verklaard) of 0 (niet voor authentiek verklaard).
- *errorCode* - Een fout die is opgetreden bij het bepalen van de verificatiestatus. Een lege tekenreeks als deze geen is.


**geactiveerd door:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Terug naar boven](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Het apparaattype en besturingssysteem worden afgeleid via een openbare Java-bibliotheek (<http://java.net/projects/user-agent-utils>) en de userAgent-tekenreeks. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.

**Omschrijving:** Voer deze callback uit om het volgen gegevens te ontvangen wanneer de specifieke gebeurtenissen voorkomen. U kunt dit bijvoorbeeld gebruiken om bij te houden hoeveel gebruikers zich met dezelfde referenties hebben aangemeld. Tekstspatiëring kan momenteel niet worden geconfigureerd. Met Adobe Pass Authentication 1.6, `sendTrackingData()` meldt ook informatie over het apparaat, de cliënt van Enabler van de Toegang, en het werkende systeemtype. De `sendTrackingData()` callback blijft compatibel met oudere versies.

- Mogelijke waarden voor apparaattype:
   - computer
   - tablet
   - mobiel
   - gameconsole
   - onbekend

- Mogelijke waarden voor het clienttype Access Enabled:
   - html5
   - ios
   - androïde


Geeft het gebeurtenistype en een array van bijbehorende informatie door. Gebeurtenistypen zijn:

| mvpdSelection | De gebruiker selecteerde een MVPD in een leverancier-selectie dialoog. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Een verificatiecontrole is voltooid. |
| authentication | Een vergunningsverzoek is volledig. |

</br>
Gegevens zijn specifiek voor elk gebeurtenistype:
</br>

| Type gebeurtenis (String) | Gegevens (Array) |
|:--- | :--- |
| mvpdSelection | 0: Geselecteerde MVPD |
|  | 1: Apparaattype |
|  | 2: Toegang tot clienttype inschakelen |
|  | 3: OS |
| authenticationDetection | 0: Of het token-verzoek succesvol was (true/false) |
|  | 1: MVPD-id |
|  | 2: GUID |
|  | 3: token is al in cache geplaatst (true/false) |
|  | 4: Apparaattype |
|  | 5: Toegangsbeheer voor clienttype |
|  | 6: OS |
| authentication | 0: Of het token-verzoek succesvol was (true/false) |
|  | 1: MVPD-id |
|  | 2: GUID |
|  | 3: token is al in cache geplaatst (true/false) |
|  | 4: Fout |
|  | 5: Details |
|  | 6: Apparaattype |
|  | 7: Toegangsbeheer voor clienttype |
|  | 8: OS |


**geactiveerd door:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Terug naar boven](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Omschrijving:** Voer deze callback uit om het kortstondige media teken (inToken) en identiteitskaart van het middel (inRequestedResourceID) te ontvangen waarvoor een vergunningsverzoek of een controle-vergunning verzoek werd gemaakt en met succes voltooid.

**geactiveerd door:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Terug naar boven](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailErrorMessage) {#token-request-failed-error-msg}

**Omschrijving:** Voer deze callback uit die moet worden gesignaleerd wanneer een vergunning of een controle-vergunning verzoek is ontbroken. Kan optioneel door een MVPD worden gebruikt om een aangepast bericht te verstrekken dat door de programmeur moet worden getoond.

>[!IMPORTANT]
>
>Deze callback-functie maakt deel uit van het oudere, originele Adobe Pass Authentication-foutrapportagesysteem. Het wordt behouden voor achterwaartse verenigbaarheid, maar het is niet noodzakelijk om deze functie bij allen te gebruiken als u uw eigen callbacks gebruikend het huidige, Geavanceerde systeem van de Rapportering van de Fout hebt uitgevoerd. Het nieuwere systeem voor foutenrapportering biedt gedetailleerdere informatie over waarom een vergunning (of andere verrichting) ontbrak, samen met voorgestelde cursussen voor elk type van fout of waarschuwing.

**Parameters:**

- *inRequestedResourceID* - Een tekenreeks die de bron-id bevat die is gebruikt in het vergunningsverzoek.
- *inRequestErrorCode* - Een tekenreeks die de foutcode voor de Adobe Pass-verificatie weergeeft en de oorzaak van de fout aangeeft. Mogelijke waarden zijn &quot;Gebruiker niet geverifieerde fout&quot; en &quot;Gebruiker niet geautoriseerde fout&quot;. Zie &quot;Callback-foutcodes&quot; hieronder voor meer informatie.
- *inRequestDetalErrorMessage* - Een extra beschrijvende tekenreeks die geschikt is voor weergave. Als deze beschrijvende tekenreeks om welke reden dan ook niet beschikbaar is, verzendt Adobe Pass Authentication een lege tekenreeks **(&quot;&quot;)**.  Dit kan door MVPD worden gebruikt om de berichten van de douanefout of verkoop-verwante berichten over te gaan. Bijvoorbeeld, als een abonnee vergunning voor een middel wordt ontkend, kon MVPD met antwoorden `*inRequestDetailedErrorMessage*` zoals: **&quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik \*hier\* als u het pakket wilt bijwerken.&quot;** Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan de plaats van de Programmer. De programmeur heeft dan de optie om het te tonen of te negeren. Adobe Pass-verificatie kan ook worden gebruikt `*inRequestDetailedErrorMessage*` om de programmeur op de hoogte te stellen van de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld: **&quot;Er is een netwerkfout opgetreden bij de communicatie met de machtigingsservice van de provider.&quot;**



**geactiveerd door:**  [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Terug naar boven](#top)

</br>


## preauthorisedResources(authorisedResources) {#preauthorizedResources(authorizedResources)}

**Omschrijving:** Callback die door Toegang wordt teweeggebracht toelaat die de erkende middelenlijst levert die na een vraag aan is teruggekeerd `checkPreauthorizedResources()`.

**Parameters:**

- *authorisedResources*: De lijst met toegestane middelen.

**geactiveerd door:** [checkPreauthorisedResources()](#checkPreauthRes)
</br>

[Terug naar boven](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die de meta-gegevens levert die via wordt gevraagd `getMetadata()` vraag.

**Meer informatie:** [Metagegevens gebruiker](#userMetadata)

**Parameters:**

- *key (String)*: De sleutel van de metagegevens waarvoor het verzoek is ingediend.
- *gecodeerd (Boolean)*: Een vlag die aangeeft of de &quot;waarde&quot; al dan niet gecodeerd is. Als dit &quot;waar&quot;is dan zal de &quot;waarde&quot;eigenlijk een Gecodeerde vertegenwoordiging van het Web JSON van de daadwerkelijke waarde zijn.
- *data (JSON-object)*: Een JSON-object met de weergave van de metagegevens.Voor eenvoudige aanvragen (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), is het resultaat een tekenreeks (die de TTL voor verificatie, TTL voor autorisatie of apparaat-id vertegenwoordigt). In het geval van een verzoek van Metagegevens van de Gebruiker, kan het resultaat een primitief of JSON voorwerp zijn die de meta-gegevens lading vertegenwoordigen. De daadwerkelijke structuur van objecten met JSON-gebruikersmetagegevens is vergelijkbaar met het volgende:

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Bijvoorbeeld:

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**geactiveerd door:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Terug naar boven](#top)

</br>

## selectedProvider(resultaat) {#selectedProvider(result)}

**Omschrijving:** Voer deze callback uit om momenteel geselecteerde MVPD en het resultaat van authentificatie van huidige gebruiker te ontvangen die in wordt verpakt `result` parameter. De `result` parameter is een Object met de volgende eigenschappen:

- **MVPD** De momenteel geselecteerde MVPD, of ongeldig als geen MVPD werd geselecteerd.
- **AE\_State** Het resultaat van verificatie voor de huidige gebruiker, een van &quot;Nieuwe gebruiker&quot;, &quot;Gebruiker niet geverifieerd&quot; of &quot;Gebruiker geverifieerd&quot;

**geactiveerd door:** [getSelectedProvider()](#getSelProv)

</br>

[Terug naar boven](#top)

</br>

### Callback-foutcodes {#callback-error-codes}

| Algemene fouten | |
|:--- | :--- | 
| Interne fout | Er is een systeemfout opgetreden bij het verwerken van de aanvraag. |
| Provider niet geselecteerd | Vindt plaats wanneer klant annuleert in het dialoogvenster voor providerselectie |
| Fout: provider niet beschikbaar | Vindt plaats wanneer er geen providers beschikbaar zijn. |

| Verificatiefouten | |
|:--- | :--- | 
| Algemene verificatiefout | Wordt geretourneerd als de reden onbekend is of niet kan worden gepubliceerd. |
| Interne verificatiefout | Er is een systeemfout opgetreden bij het verifiëren. |
| Fout door gebruiker niet geverifieerd | Gebruiker is niet geverifieerd. |
| Fout bij meerdere verificatieverzoeken | Er zijn aanvullende verificatieverzoeken ontvangen voordat de eerste is voltooid. |

| Autorisatiefouten | |
|:--- | :--- | 
| Algemene autorisatiefout | Wordt geretourneerd als de reden onbekend is of niet kan worden gepubliceerd. |
| Interne autorisatiefout | Er is een systeemfout opgetreden bij het autoriseren. |
| Fout: gebruiker niet geautoriseerd | De klant is niet geautoriseerd om de gevraagde inhoud te bekijken. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Terug naar boven](#top)
