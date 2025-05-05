---
title: JavaScript SDK API-naslag
description: JavaScript SDK API-naslag
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2883'
ht-degree: 0%

---

# (Verouderd) JavaScript SDK API-naslag {#javascript-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## API-naslag {#api-reference}

Met deze functies worden aanvragen voor interactie met een MVPD gestart. Alle vraag is asynchroon; u moet [ callbacks ](#callbacks) uitvoeren om de reacties te behandelen:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorisedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, eindpunten, opties) {#setrequestor(inRequestorID,endpoints,options)}

**Beschrijving:** identificeert de plaats waarvan de verzoeken voortkomen.  U moet deze vraag vóór een andere API vraag in een communicatie zitting maken.

**Parameters:**

- *inRequestorID* - het unieke herkenningsteken dat Adobe aan de voortkomende plaats tijdens registratie toewees.

- *eindpunten* - Deze parameter is facultatief. Dit kan een van de volgende waarden zijn:

   - Een serie die u toestaat om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). Als er meerdere URL&#39;s worden opgegeven, bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD is gekoppeld aan de snelste serviceprovider, dat wil zeggen de provider die eerst heeft gereageerd en die die MVPD ondersteunt. Standaard (als geen waarde is opgegeven) wordt de Adobe-serviceprovider gebruikt (<http://sp.auth.adobe.com/>).

  Voorbeeld:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *opties* - een voorwerp JSON dat de waarde van identiteitskaart van de Toepassing, de waarde van identiteitskaart van de Bezoeker verfrist-minus montages (achtergrond login logout) en de montages van MVPD (iFrame) bevat. Alle waarden zijn optioneel.
   1. Indien gespecificeerd, zou Experience Cloud bezoekerID op alle netwerkvraag worden gemeld die door de bibliotheek wordt uitgevoerd. De waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   2. Als uniek herkenningsteken van de toepassing wordt gespecificeerd - `applicationId` - zal de waarde aan alle verdere vraag worden toegevoegd die door de toepassing als deel van de x-Apparaat-Info HTTP- kopbal wordt gemaakt. Deze waarde kan later van [ ESM ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) rapporten worden gehaald gebruikend de juiste vraag.

  **Nota:** Alle sleutels JSON zijn case-sensitive.

  Voorbeeld:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Programmeur kan de montages van MVPD met voeten treden die in de Authentificatie van Adobe Pass worden gevormd, door te specificeren als een iFrame of niet voor login (*iFrameRequired* sleutel) en de afmetingen iFrame (*iFrameWidth* en *iFrameHeight* sleutels) wordt vereist. Het JSON-object heeft de volgende sjabloon:

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


Alle top-level sleutels in het malplaatje hierboven zijn facultatief en hebben standaardwaarden (*backgroundLogin*, *backgroundLogut* zijn vals door gebrek, en mvpdConfig is ongeldig - betekenend dat geen montages van MVPD worden met voeten getreden).


- **Nota**: Het specificeren van ongeldige waarden/types voor de bovengenoemde parameters zal in ongedefinieerd gedrag resulteren.



Hier volgt een voorbeeldconfiguratie voor het volgende scenario: het activeren van login en logout zonder vernieuwen, het veranderen van MVPD1 in volledig pagina-redirect login (niet-iFrame) en MVPD2 in login iFrame met width=500 en height=300:

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


**teweeggebrachte callbacks:** [ setConfig () ](#setconfigconfigxml-setconfigconfigxml)
</br>

[Terug naar boven](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Beschrijving:** vraagt vergunning voor het gespecificeerde middel. Telkens als een klant probeert om tot een toegelaten middel toegang te hebben, roep deze functie om een kort-levend vergunningsteken van Toegang te verkrijgen Enabler. Resource IDs wordt overeengekomen met MVPD die vergunning verstrekt.

Gebruikt het caching authentificatietoken voor de huidige klant. Als zulk een teken niet wordt gevonden, stelt eerst het authentificatieproces in werking, dan gaat met vergunning verder.

**Parameters:**

- `inResourceID` - De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- `redirect_url` - Geef desgewenst een omleidings-URL op, zodat de gebruiker tijdens het autorisatieproces van MVPD naar die pagina terugkeert in plaats van naar de pagina vanwaar de autorisatie is gestart.


**Toegelaten callbacks:** [ setToken () ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) op succes, [ tokenRequestFailed ](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) op mislukking

>[!CAUTION]
>
>Gebruik indien mogelijk checkAuthorization() in plaats van getAuthorization(). De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit zou tot een ingewikkelde implementatie aan de kant van de Programmer kunnen leiden.

</b>

[Terug naar boven](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Beschrijving:** Verzoekt authentificatie voor de huidige klant. Wordt doorgaans aangeroepen als reactie op een klik op een knop Aanmelden. Controleert op een in de cache opgeslagen verificatietoken voor de huidige klant. Als een dergelijk token niet wordt gevonden, wordt het verificatieproces gestart. Dit roept het standaard of aangepaste provider-selection dialoogvenster aan en gebruikt vervolgens de geselecteerde provider om naar de MVPD-aanmeldinterface te leiden.

Als de verificatie is gelukt, wordt een verificatietoken voor de gebruiker gemaakt en opgeslagen. Als de authentificatie ontbreekt, keert de leverancier een aangewezen foutenmelding aan uw [ setAuthenticationStatus () terug ](#setauthenticationstatusisauthenticated-errorcode) callback.

**Parameters:**

- redirect_url - Verstrek naar keuze een omleidingsURL, zodat het de authentificatieproces van MVPD de gebruiker aan die pagina eerder dan de pagina terugkeert waarvan de authentificatie werd in werking gesteld.

**geroepen Callbacks teweeggebracht:** [ setAuthenticationStatus () ](#setauthenticationstatusisauthenticated-errorcode), [ displayProviderDialog () ](#displayproviderdialogproviders-displayproviderdialogproviders), [ sendTrackingData () ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Terug naar boven](#top)

</br>

## checkAuthN {#checkauthn}

**Beschrijving:** controleert huidige authentificatiestatus voor de huidige klant.  Niet gekoppeld aan een gebruikersinterface.

**Toegelaten Callbacks:** [ setAuthentcationStatus () ](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Terug naar boven](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstatus voor de huidige klant en het bepaalde middel te controleren. Het begint door de authentificatiestatus eerst te controleren. Indien niet geverifieerd, wordt de tokenRequestFailed() callback geactiveerd en wordt de methode afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie details op [ getAuthorization () ] (#getAuthZ methode.

>[!TIP]
>
> **Gebruikend controle-status functies** u te hoeven niet om het statuut van of authentificatie of vergunning te controleren alvorens om vergunning te verzoeken. U kunt deze functies bijvoorbeeld aanroepen om uw eigen statusweergave bij te werken. Gebruik deze niet wanneer u gebruikersinteractie nodig heeft.

**Parameters:**

- `inResourceID` - De id van de bron waarvoor de gebruiker toestemming aanvraagt.


**teweeggebrachte callbacks:**
[ setToken () ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [ tokenRequestFailed () ](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [ sendTrackingData () ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [ setAuthenticationStatus () ](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorisedResources(resources) {#checkPreauthorizedResources(resources)}

**Beschrijving:** verzoekt &quot;preflight&quot;vergunningsstatus voor een lijst van
middelen.

**Parameters:**

- *middelen*: De middelenparameter is een serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization()` , dat wil zeggen dat het een overeengekomen waarde betreft die is vastgesteld tussen de programmeur en de MVPD, of een RSS-fragment voor media.

</br>

## checkPreauthorisedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Deze API-variant is beschikbaar vanaf JS SDK versie 4.0


**Parameters:**

- *middelen*: De middelenparameter is een serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization()` , dat wil zeggen dat het een overeengekomen waarde betreft die is vastgesteld tussen de programmeur en de MVPD, of een RSS-fragment voor media.

- *geheime voorgeheugen*: Of om het interne geheime voorgeheugen te gebruiken wanneer het controleren op preauthorised middelen. Dit is een facultatieve parameter, die **waar** in gebreke blijft. Als de waarde true is, is het gedrag identiek aan de bovenstaande API. Dit betekent dat volgende aanroepen naar deze functie een interne cache gebruiken om vooraf geautoriseerde resource op te lossen. Het overgaan van **vals** voor deze parameter zal het interne geheime voorgeheugen onbruikbaar maken, resulterend in een servervraag telkens als **checkPreauthorisedResources** API wordt geroepen.

**Toegelaten Callbacks:** [ preauthorisedResources () ](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[ terug naar bovenkant ](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Beschrijving:** wint informatie terug die als meta-gegevens door de bibliotheek van Inschakelen van de Toegang wordt blootgesteld.

Er zijn twee typen metagegevens:

- **Statische** (het teken van de Authentificatie TTL, het tekenTTL van de Token van de Vergunning, en identiteitskaart van het Apparaat)
- **Metagegevens van de Gebruiker** (Dit omvat gebruiker specifieke informatie die van MVPD tot het apparaat van de gebruiker tijdens de Authentificatie en/of de stromen van de Vergunning wordt overgegaan)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](#UserMetadata)

**Parameters:**

- *sleutel*: Een identiteitskaart die de gevraagde meta-gegevens specificeert:
   - Als de sleutel `"TTL_AUTHN",` is, wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.

   - Als de sleutel `"TTL_AUTHZ"` is en de params een serie die middelidentiteitskaart als koord bevat, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.

   - Als de toets `"DEVICEID"` is, wordt de query uitgevoerd om de huidige apparaat-id te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.

   - Wanneer de sleutel uit de volgende lijst met gebruikermetagegevenstypen bestaat, wordt een JSON-object met de bijbehorende gebruikersmetagegevens naar de callback-functie [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) verzonden:

   - `"zip"` - Postcode

   - `"encryptedZip"` - Gecodeerde postcode

   - `"householdID"` - Huishoudelijke id. Als een MVPD geen subaccounts ondersteunt, is dit dezelfde als de gebruikersnaam.

   - `"maxRating"` - Maximale ouderlijke classificatie voor de gebruiker

   - `"userID"` - De gebruikers-id. Als een MVPD subaccounts ondersteunt en de gebruiker niet de hoofdaccount is, is de gebruikersnaam anders dan de huishoudelijke id.

   - `"channelID"` - De lijst met kanalen die de gebruiker mag bekijken

   - `"is_hoh"` - Markering die aangeeft of een gebruiker het hoofd van een huishouden is

   - `"encryptedZip"` - Gecodeerde postcode

   - `"typeID"` - Markering die aangeeft of de gebruikersaccount een primaire/secundaire account is

   - `"primaryOID"` - Huishoudelijke identificatie

   - `"postalCode"` - Vergelijkbaar met postcode

   - `"acctID"` - Account-id

   - `"acctParentID"` - Bovenliggende id van account

  **Nota**: De daadwerkelijke Meta-gegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat een MVPD ter beschikking stelt.  Zie [ Metagegevens van de Gebruiker ](#UserMetadata) voor de huidige lijst van beschikbare Metagegevens van de Gebruiker.


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


**teweeggebrachte callbacks:** [ setMetadataStatus () ](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Terug naar boven](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Beschrijving:** Vraag deze functie wanneer de gebruiker een MVPD van uw leverancier-selectie UI heeft geselecteerd om de leveranciersselectie naar Toegelaten te verzenden of deze functie met een ongeldige parameter te roepen voor het geval de gebruiker uw leverancier-selectie UI zonder een leverancier te selecteren verwierp.

**Callbacks
teweeggebracht:**[ setAuthentcationStatus () ](#setauthenticationstatusisauthenticated-errorcode), [ sendTrackingData () ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Terug naar boven](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Beschrijving:** wint de resultaten van de selectie van de klant in de leverancier-selectie dialoog terug. Dit kan op elk ogenblik na de eerste authentificatiecontrole worden gebruikt.

Deze functie is asynchroon en retourneert het resultaat naar uw callback-functie `selectedProvider()` .

- **MVPD** momenteel geselecteerde MVPD, of ongeldig als geen MVPD werd geselecteerd.
- **AE_State** het resultaat van authentificatie voor de huidige klant één van &quot;Nieuwe Gebruiker&quot;, &quot;Gebruiker niet voor authentiek verklaard&quot;, of &quot;Gebruiker voor authentiek verklaard&quot;

**teweeggebrachte callbacks:** [ selectedProvider () ](#getselectedprovider-getselectedprovider)

</br>

[Terug naar boven](#top)

</br>

## afmelden {#logout}

**Beschrijving:** logt uit de huidige klant, die alle authentificatie en vergunningsinformatie voor die gebruiker ontruimt. Verwijdert alle authN en authZ tokens van het systeem van de klant.

**Toegelaten Callbacks:** [ setAuthentcationStatus () ](#setauthenticationstatusisauthenticated-errorcode)
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

**Beschrijving:** teweeggebracht wanneer Toegangsbeheer initialisering heeft voltooid en klaar is om verzoeken te ontvangen. Implementeer deze callback om te weten wanneer u de communicatie met de API van Enabler van de Toegang kunt beginnen.
</br>

[Terug naar boven](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Beschrijving:** voer deze callback uit om de configuratieinformatie en de lijst van MVPD te ontvangen.

**Parameters:**

- *configXML*: xml voorwerp die de configuratie voor de huidige VERZOEKER met inbegrip van de lijst van MVPD houden.


**teweeggebracht door:** [ setRequestor () ](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Terug naar boven](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Beschrijving:** voer deze callback uit om uw eigen douane leverancier-selectie UI aan te halen. Het dialoogvenster moet de weergavenaam (en het optionele logo) gebruiken om de keuze van de klant te bepalen. Wanneer de klant een keus heeft gemaakt en de dialoog verworpen, verzend bijbehorende identiteitskaart voor de gekozen leverancier in de vraag aan *setSelectedProvider ()*.

**Parameters:**

- *leveranciers* - een serie van Voorwerpen die gevraagde MVPDs vertegenwoordigen:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**teweeggebracht door:** [ getAuthentication () ](#getauthenticationredirecturl-getauthenticationredirecturl), [ getAuthorization () ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[ Terug naar bovenkant ](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Beschrijving:** voer deze callback uit als de gebruiker een MVPD selecteerde die iFrame vereist waarin om zijn authentificatie login pagina UI te tonen.

**teweeggebracht door:**&#x200B;[ setSelectedProvider () ](#setselectedproviderproviderid-setselectedprovider)

</br> [ Terug naar bovenkant ](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Beschrijving:** voer deze callback uit om de authentificatiestatus (1=voor authentiek verklaard of 0=niet voor authentiek verklaard) en een beschrijvend foutenbericht te ontvangen als om het even welke fout terwijl het proberen om de authentificatiestatus (leeg koord op succesvolle voltooiing van de controle) voorkwam.

>[!NOTE]
> 
>Als u het huidige, [ Geavanceerde Fout die ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) systeem meldt, kunt u de parameter negeren errorCode naar deze functie wordt verzonden die.  Nochtans, is de isAuthenticated vlag nog van gebruik voor het volgen van de authentificatiestatus van een gebruiker in de machtigingsstroom


**Parameters:**

- *isAuthenticated* - verstrekt authentificatiestatus: 1 (voor authentiek verklaard) of 0 (niet voor authentiek verklaard).
- *errorCode* - Om het even welke fout die bij het bepalen van authentificatiestatus voorkwam. Een lege tekenreeks als deze geen is.


**teweeggebracht door:** [ checkAuthentication () ](#checkauthn-checkauthn), [ getAuthentication () ](#getauthenticationredirecturl-getauthenticationredirecturl), [ checkAuthorization () ](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Terug naar boven](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Het apparatentype en het werkende systeem worden afgeleid door het gebruik van een openbare bibliotheek van Java (<http://java.net/projects/user-agent-utils>) en het koord van de gebruikersagent. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.

**Beschrijving:** voer deze callback uit om volgende gegevens te ontvangen wanneer de specifieke gebeurtenissen voorkomen. U kunt dit bijvoorbeeld gebruiken om bij te houden hoeveel gebruikers zich met dezelfde referenties hebben aangemeld. Tekstspatiëring kan momenteel niet worden geconfigureerd. Met Adobe Pass-verificatie 1.6 rapporteert `sendTrackingData()` ook informatie over het apparaat, de Access Enabler-client en het type besturingssysteem. De callback van `sendTrackingData()` blijft compatibel met oudere versies.

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

| mvpdSelection | De gebruiker heeft een MVPD geselecteerd in een dialoogvenster voor het selecteren van leveranciers. |
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


**teweeggebracht door:** [ checkAuthentication () ](#checkauthn-checkauthn), [ getAuthentication () ](#getauthenticationredirecturl-getauthenticationredirecturl), [ checkAuthorization () ](#checkauthorizationinresourceid-checkauthorizationinresourceid), [ getAuthorization () ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Terug naar boven](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Beschrijving:** voer deze callback uit om het korte-levende media teken (inToken) en identiteitskaart van het middel (inRequestedResourceID) te ontvangen waarvoor een vergunningsverzoek of een controle-vergunning verzoek werd gemaakt en met succes voltooid.

**teweeggebracht door:** [ checkAuthorization () ](#checkAuthZ), [ getAuthorization () ](#getAuthZ)
</br>

[Terug naar boven](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailErrorMessage) {#token-request-failed-error-msg}

**Beschrijving:** voer deze callback uit die moet worden gesignaleerd wanneer een vergunning of een controle-vergunning verzoek is ontbroken. Kan optioneel door een MVPD worden gebruikt om een aangepast bericht weer te geven dat door de programmeur moet worden weergegeven.

>[!IMPORTANT]
>
>Deze callback-functie maakt deel uit van het oudere, originele Adobe Pass Authentication-foutrapportagesysteem. Het wordt behouden voor achterwaartse verenigbaarheid, maar het is niet noodzakelijk om deze functie bij allen te gebruiken als u uw eigen callbacks gebruikend het huidige, Geavanceerde systeem van de Rapportering van de Fout hebt uitgevoerd. Het nieuwere systeem voor foutenrapportering biedt gedetailleerdere informatie over waarom een vergunning (of andere verrichting) ontbrak, samen met voorgestelde cursussen voor elk type van fout of waarschuwing.

**Parameters:**

- *inRequestedResourceID* - een koord dat identiteitskaart van het Middel verstrekt die op het vergunningsverzoek werd gebruikt.
- *inRequestErrorCode* - een koord dat de de foutencode van de Authentificatie van Adobe Pass toont, die op de reden voor de mislukking wijst; de mogelijke waarden zijn &quot;Gebruiker niet Voor authentiek verklaarde Fout&quot;en &quot;Gebruiker niet Toegelaten Fout&quot;; voor meer details, zie &quot;Callback foutencodes&quot;hieronder.
- *inRequestDetailErrorMessage* - een extra beschrijvend koord geschikt voor vertoning. Als dit beschrijvende koord om het even welke reden niet beschikbaar is, verzendt de Authentificatie van Adobe Pass een leeg koord **(&quot;&quot;&quot;)**.  Dit kan door MVPD worden gebruikt om de berichten van de douanefout of verkoop-verwante berichten over te gaan. Als een abonnee bijvoorbeeld geen autorisatie voor een resource wordt verleend, kan de MVPD reageren met een `*inRequestDetailedErrorMessage*` zoals: **&quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik \*hier\* als u het pakket wilt bijwerken.&quot;** Het bericht wordt door de Authentificatie van Adobe Pass door deze callback tot de plaats van de Programmer overgegaan. De programmeur heeft dan de optie om het te tonen of te negeren. Adobe Pass-verificatie kan `*inRequestDetailedErrorMessage*` ook gebruiken om de programmeur op de hoogte te stellen van de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, **&quot;Een netwerkfout kwam voor toen het communiceren met de de vergunningsdienst van de leverancier&quot;.**



**teweeggebracht door:** [ checkAuthorization () ](#checkauthorizationinresourceid-checkauthorizationinresourceid), [ getAuthorization () ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Terug naar boven](#top)

</br>


## preauthorisedResources(authorisedResources) {#preauthorizedResources(authorizedResources)}

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die de erkende middelenlijst levert die na een vraag aan `checkPreauthorizedResources()` is teruggekeerd.

**Parameters:**

- *authorisedResources*: De lijst van erkende middelen.

**teweeggebracht door:** [ checkPreauthorisedResources () ](#checkPreauthRes)
</br>

[Terug naar boven](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de meta-gegevens levert die via een `getMetadata()` vraag worden gevraagd.

**Meer Informatie:** [ Metagegevens van de Gebruiker ](#userMetadata)

**Parameters:**

- *sleutel (Koord)*: De sleutel van de meta-gegevens waarvoor het verzoek werd gemaakt.
- *gecodeerd (Boolean)*: Een vlag die erop wijst of de &quot;waarde&quot;al dan niet wordt gecodeerd. Als dit &quot;waar&quot;is dan zal de &quot;waarde&quot;eigenlijk een Gecodeerde vertegenwoordiging van het Web JSON van de daadwerkelijke waarde zijn.
- *gegevens (Voorwerp JSON)*: Een Voorwerp JSON met de vertegenwoordiging van de meta-gegevens.Voor eenvoudige verzoeken (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), is het resultaat een Koord (die TTL van de Authentificatie, van de Vergunning TTL of identiteitskaart van het Apparaat vertegenwoordigen). In het geval van een verzoek van Metagegevens van de Gebruiker, kan het resultaat een primitief of JSON voorwerp zijn die de meta-gegevens lading vertegenwoordigen. De daadwerkelijke structuur van objecten met JSON-gebruikersmetagegevens is vergelijkbaar met het volgende:

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


**teweeggebracht door:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[ Terug naar bovenkant ](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Beschrijving:** voer deze callback uit om momenteel geselecteerde MVPD en het resultaat van authentificatie van huidige gebruiker te ontvangen die in de `result` parameter wordt verpakt. De parameter `result` is een object met de volgende eigenschappen:

- **MVPD** momenteel geselecteerde MVPD, of ongeldig als geen MVPD werd geselecteerd.
- **AE \_State** het resultaat van authentificatie voor de huidige gebruiker, één van &quot;Nieuwe Gebruiker&quot;, &quot;Gebruiker niet voor authentiek verklaard&quot;, of &quot;Gebruiker voor authentiek verklaard

**teweeggebracht door:** [ getSelectedProvider () ](#getSelProv)

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
