---
title: Native API-naslaggids voor Amazon FireOS
description: Native API-naslaggids voor Amazon FireOS
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '3416'
ht-degree: 0%

---

# Native API-naslaggids voor Amazon FireOS {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Inleiding {#intro}

In dit document worden de methoden en callbacks beschreven die worden weergegeven door de Amazon FireOS SDK voor Adobe Pass-verificatie, ondersteund met Adobe Pass-verificatie. De hier beschreven methodes en callback functies worden bepaald in de AccessEnabler.h en EntitlementDelegate.h kopbaldossiers.

Zie <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> voor de nieuwste Amazon FireOS AccessEnabled SDK.

>[!NOTE]
>
>Het Adobe Pass-verificatieteam moedigt u aan alleen Adobe Pass-verificatie te gebruiken *publiek* API&#39;s:

- Openbare API&#39;s zijn beschikbaar *en volledig getest* op alle ondersteunde clienttypen. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(n) heeft.
- Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Voor *non*-public API&#39;s, behouden we ons het recht voor om hun handtekening op elk toekomstig moment te wijzigen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

## Amazon FireOS SDK API {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- [checkPreauthorisedResources](#checkPreauth)
- [preauthorisedResources](#preauthResources)
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [afmelden](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

</br>

### Factory.getInstance {#getInstance}

**Omschrijving:** Instantieert het Access Enabler-object. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Beschikbaarheid:** v3.0+


**Parameters:**

- *appContext*: Amazon Fire OS-toepassingscontext.
- softwareStatement
- redirectUrl : in het geval van FireOS wordt de parameterwaarde genegeerd en ingesteld op default : adobepass://android.app
- env_url: voor het testen met behulp van een testomgeving voor Adobe staging, kan env\_url worden ingesteld op &quot;sp.auth-staging.adobe.com&quot;

**Vervangen:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**Omschrijving:** Hiermee wordt de identiteit van de programmeur vastgesteld. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Deze instelling mag slechts eenmaal worden uitgevoerd tijdens de levenscyclus van de toepassing.

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: de milieu van de Versie van de Adobe/van de Productie.

Als een waarde is opgegeven voor de *urls* parameter, richt de resulterende netwerkvraag alle URLs die in *urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt Toegangsbeheer URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId)``` |


**Beschikbaarheid:** v3.0+


| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+


**Parameters:**

- *aanvragerID*: De unieke id die aan de programmeur is gekoppeld. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
- *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

**Callbacks geactiveerd:** `setRequestorComplete()`



**Vervangen:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Omschrijving:** Callback die door Toegankelijkheid wordt teweeggebracht die uw toepassing meedeelt dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

| Callback: configuratie aanvrager voltooid |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan een van de volgende waarden gebruiken:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt

**geactiveerd door:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Omschrijving:** Vormt globale opties van SDK. Het aanvaardt een **Kaart\&lt;string string=&quot;&quot;>** als argument. De waarden van de kaart zullen aan de server samen met elke netwerkvraag worden overgegaan die SDK maakt.

De waarden worden doorgegeven aan de server, onafhankelijk van de huidige flow (verificatie/autorisatie). Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.



| API-aanroep: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *opties*: Een kaart\&lt;string string=&quot;&quot;> met algemene SDK-opties. Momenteel zijn de volgende opties beschikbaar:
   - **applicationProfile** - Het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   - **ap\_vi** - De Experience Cloud ID Service. Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   - **apparaat\_info** - Apparaatgegevens zoals beschreven in **Cookbook met doorgegeven apparaatinformatie**

</br>

### checkAuthentication {#checkAuthN}

**Omschrijving:** Controleert de verificatiestatus. Het doet dit door naar een geldig authentificatietoken in de lokale symbolische opslagruimte te zoeken. Het roepen van deze methode voert geen netwerkvraag uit. Deze wordt door de toepassing gebruikt om de verificatiestatus van de gebruiker te controleren en de gebruikersinterface dienovereenkomstig bij te werken (de gebruikersinterface voor aanmelding/aanmelding wordt dus bijgewerkt). De verificatiestatus wordt via de [*setAuthenticationStatus()*](#setAuthNStatus) callback.

Als een MVPD de eigenschap &quot;Authentificatie per Aanvrager&quot;steunt, dan kunnen de veelvoudige authentificatietokens op een apparaat worden opgeslagen.

| API-aanroep: verificatiestatus controleren |
| --- |
| ```public void checkAuthentication()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Omschrijving:** Start de volledige verificatieworkflow. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

- Als de laatste authentificatiepoging succesvol was, wordt de MVPD selectiefase overgeslagen en een controle WebView zal de gebruiker met de MVPD login pagina voorstellen.
- Als de laatste verificatiepoging is mislukt of als de gebruiker zich expliciet heeft afgemeld, wordt [*displayProviderDialog()*](#displayProviderDialog) callback wordt geactiveerd. Uw toepassing gebruikt deze callback om de MVPD selectie UI te tonen. Uw app is ook vereist om de verificatiestroom te hervatten door de Access Enabler-bibliotheek te informeren over de MVPD-selectie van de gebruiker via de [setSelectedProvider()](#setSelectedProvider) methode.

Als een MVPD de eigenschap &quot;Authentificatie per Aanvrager&quot;steunt, dan kunnen de veelvoudige authentificatietokens op een apparaat (per Programmer) worden opgeslagen.

Ten slotte wordt de authenticatiestatus via de *setAuthenticationStatus()* callback.

| API-aanroep: initieert de verificatiestroom |
| --- |
| ```public void getAuthentication()``` |

**Beschikbaarheid:** v1.0+

| API-aanroep: initieert de verificatiestroom |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *forceAuthn*: Een markering die aangeeft of de verificatiestroom moet worden gestart, ongeacht of de gebruiker al dan niet is geverifieerd.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Beschrijving** Callback teweeggebracht door Toegangsactivering om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om gewenste MVPD te selecteren. De callback biedt een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de selectieinterface correct samen te stellen (zoals de URL die het logo van de MVPD aanwijst, de vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door te roepen *setSelectedProvider()* en geeft u de id van de MVPD door die overeenkomt met de selectie van de gebruiker.


| **Callback: toon de selectie UI MVPD** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Beschikbaarheid:** v1.0+

**Parameters**:

- *mvpds*: Lijst met MVPD-objecten die MVPD-gerelateerde informatie bevatten die de toepassing kan gebruiken om de UI-elementen voor de selectie van MVPD te maken.

**geactiveerd door:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Omschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie MVPD van de gebruiker op de hoogte te brengen. Bij het passeren *null* als parameter, toelaat van de Toegang opnieuw stelt huidige MVPD aan een ongeldige waarde in.

| **API-aanroep: stel de momenteel geselecteerde provider in** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Beschikbaarheid:**v 1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Omschrijving:** Callback die door Toegangsactivering op Android SDK wordt teweeggebracht. Deze moet worden genegeerd in Amazon FireOS SDK.

| **Callback: MVPD-aanmeldingspagina weergeven** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *url*: De URL die verwijst naar de aanmeldingspagina van de MVPD

**geactiveerd door:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Omschrijving:** Voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken.

| **API-aanroep: het verificatietoken ophalen** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *cookies*: Cookies die op het doeldomein zijn ingesteld (zie de demotoepassing in de SDK voor een voorbeeldimplementatie).

**Callbacks geactiveerd:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Omschrijving:** Callback die door Toegankelijkheid wordt teweeggebracht die de toepassing van de status van de authentificatie informeert. Er zijn vele plaatsen waar de authentificatiestroom kan ontbreken, of als resultaat van gebruikersinteractie of wegens andere onvoorziene scenario&#39;s (d.w.z. de problemen van de netwerkconnectiviteit, enz.). Deze callback informeert de toepassing van de succes/mislukkingsstatus van de authentificatie, terwijl ook het verstrekken van extra informatie over de mislukkingsreden, wanneer nodig.

Deze callback signaleert ook wanneer de logout stroom volledig is.

| **Callback: rapport de status van de authentificatiestroom** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan een van de volgende waarden gebruiken:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - afmelden
- *code*: Reden voor de voorgestelde status. Indien *status* is `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`vervolgens *code* is een lege tekenreeks (gedefinieerd door de `AccessEnabler.USER_AUTHENTICATED` constante). Als deze parameter niet wordt geverifieerd, kan deze een van de volgende waarden hebben:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. Als reactie op de *checkAuthentication()* methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler heeft de authentificatiestatus-machine opnieuw ingesteld nadat de hogere laagtoepassing overgegaan *null* tot `setSelectedProvider()` om de verificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals niet-beschikbaarheid van het netwerk of de gebruiker heeft de verificatiestroom expliciet geannuleerd.
   - `AccessEnabler.LOGOUT` - De gebruiker is niet geautoriseerd vanwege een afmeldingsactie.

**geactiveerd door:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorisedResources {#checkPreauth}

**Omschrijving:** Deze methode wordt door de toepassing gebruikt om te bepalen of de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| **API-aanroep: stel de momenteel geselecteerde provider in** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Beschikbaarheid:** v1.0+

**&lt;parameters: span=&quot;&quot; id=&quot;1&quot; translate=&quot;no&quot; /> De `resources` parameter is een array van bronnen waarvoor de autorisatie moet worden gecontroleerd .** Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, d.w.z., zou het een overeengekomen waarde moeten zijn die tussen de programmeur en MVPD of een mediaRSS fragment wordt gevestigd.

**Callback geactiveerd:** `preauthorizedResources()`

</br>

### preauthorisedResources {#preauthResources}

**Omschrijving:** Callback geactiveerd door checkPreauthorisedResources(). Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

| **API-aanroep: stel de momenteel geselecteerde provider in** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Beschikbaarheid:**v 1.0+

**Parameters:** De `resources` parameter is een array van bronnen waarvan de gebruiker al gemachtigd is om deze te bekijken.

**geactiveerd door:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de status van de vergunning te controleren. Het begint door de authentificatiestatus eerst te controleren. Indien niet geverifieerd, wordt de *setTokenRequestFailed()* callback wordt geactiveerd en de methode wordt afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie de details over de *getAuthorization()* methode.

| **API-aanroep: autorisatiestatus controleren** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Beschikbaarheid:** v1.0+

| **API-aanroep: autorisatiestatus controleren** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de vergunningsstroom in gang te zetten. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat Access Enabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het korte-levende media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. De *setToken()* callback wordt geactiveerd en het korte media-token wordt als een parameter aan de toepassing geleverd. Indien de vergunning om welke reden dan ook mislukt, *tokenRequestFailed()* callback wordt geactiveerd en de foutcode en details worden gegeven.

| **API-aanroep: de machtigingsstroom starten** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Beschikbaarheid:** v1.0+

| **API-aanroep: de machtigingsstroom starten** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Extra callbacks geactiveerd**  <br>Deze methode kan de volgende callbacks (als de authentificatiestroom ook in werking wordt gesteld) ook teweegbrengen: _setAuthenticationStatus()_, _displayProviderDialog()_ |

**NOTA: Gelieve te gebruiken checkAuthorization () in plaats van getAuthorization () wanneer mogelijk. De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit zou tot een ingewikkelde implementatie aan de kant van de Programmer kunnen leiden.**

</br>

### setToken {#setToken}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die uw toepassing meedeelt dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.

| **Callback: autorisatiestroom is voltooid** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Beschikbaarheid:**v 1.0+

**Parameters:**

- *token*: De token voor korte media
- *resourceId*: De bron waarvoor de vergunning is verleend

**geactiveerd door:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Omschrijving:** Callback teweeggebracht door Toegelaten van de Toegang die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

| **Callback: autorisatiestroom mislukt** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: De bron waarvoor de vergunning is verleend
- *errorCode*: Foutcode die aan het mislukkingsscenario is gekoppeld. Mogelijke waarden:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - De gebruiker kan geen autorisatie voor de opgegeven resource uitvoeren.
- *errorDescription*: Aanvullende details over het mislukkingsscenario. Als deze beschrijvende tekenreeks om welke reden dan ook niet beschikbaar is, verzendt Adobe Pass Authentication een lege tekenreeks >**(&quot;&quot;)**.  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopgerelateerde berichten door te geven. Bijvoorbeeld, als een abonnee vergunning voor een middel wordt ontkend, kon MVPD een bericht zoals verzenden: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik hier als u het pakket wilt bijwerken.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier.&quot;

**geactiveerd door:** `checkAuthorization(), getAuthorization()`

</br>

### afmelden {#logout}

**Omschrijving:** Gebruik deze methode om de logout-flow te starten. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd.

| **API-aanroep: de afmeldingsstroom starten** |
| --- |
| ```public void logout()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** Geen

</br>

### getSelectedProvider {#getSelectedProvider}

**Omschrijving:** Gebruik deze methode om de momenteel geselecteerde provider te bepalen.

| **API-aanroep: bepaal de momenteel geselecteerde MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

| **Callback: informatie over momenteel geselecteerde MVPD** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *mvpd*: Object met informatie over de momenteel geselecteerde MVPD

**geactiveerd door:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Omschrijving:** Gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek van Inschakelen van de Toegang wordt blootgesteld. De toepassing heeft toegang tot deze informatie door een samengesteld object MetadataKey op te geven.

| **API-aanroep: de AccessEnabler vragen naar metagegevens** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Beschikbaarheid:** v1.0+

Er zijn twee soorten meta-gegevens beschikbaar aan Programmeurs:

- Statische metagegevens (verificatietoken TTL, machtigingstoken-TTL en apparaat-id)
- Gebruikersmetagegevens (gebruikersspecifieke informatie, zoals gebruikersnaam en postcode; doorgegeven van een MVPD aan het apparaat van een gebruiker tijdens de verificatie- en/of autorisatiestromen)

**Parameters:**

- *metadataKey*: Een gegevensstructuur die een sleutel- en args-variabele omvat, met de volgende betekenis:
   - Als key is `METADATA_KEY_TTL_AUTHN` dan wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.
   - Als key is `METADATA_KEY_TTL_AUTHZ` en args bevat een SerializableNameValuePair-object met name = `METADATA_ARG_RESOURCE_ID` en value = `[resource_id]`, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.
   - Als key is `METADATA_KEY_DEVICE_ID` dan wordt de vraag gemaakt om huidige apparatenidentiteitskaart te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.
   - Als key is `METADATA_KEY_USER_META` en args bevat een SerializableNameValuePair-object met name = `METADATA_KEY_USER_META` en value = `[metadata_name]`, dan wordt de vraag gemaakt voor gebruikersmeta-gegevens. De huidige lijst met beschikbare metagegevenstypen voor gebruikers:
      - `zip` - Postcode
      - `householdID` - Huishoudelijke identificatiecode. Als een MVPD geen subaccounts steunt, zal dit identiek zijn aan `userID`.
      - `maxRating` - Maximale ouderlijke classificatie voor de gebruiker
      - `userID` - De gebruikersnaam. Als een MVPD subaccounts steunt, en de gebruiker is niet de belangrijkste rekening,
      - `channelID` - Een lijst met kanalen die de gebruiker mag bekijken

De daadwerkelijke Metagegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD ter beschikking stelt.  Deze lijst wordt verder uitgebreid naarmate nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**Callbacks geactiveerd:** [`setMetadataStatus()`](#setMetadaStatus)

**Meer informatie:** [Metagegevens gebruiker](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die de meta-gegevens levert die via wordt gevraagd *getMetadata()* vraag.

| **Callback: resultaat van verzoek om metagegevens op te halen** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *key*: Het object MetadataKey dat de sleutel bevat waarvoor een metagegevenswaarde wordt aangevraagd en de bijbehorende parameters (zie demotoepassing voor een referentie-implementatie).
- *resultaat*: Een samengesteld object met de gevraagde metagegevens. Het object heeft de volgende velden:
   - *simpleResult*: een tekenreeks die de metagegevenswaarde vertegenwoordigt op het moment dat de aanvraag is ingediend voor verificatie-TTL, verificatie-TTL of apparaat-id. Deze waarde is null als de aanvraag is ingediend voor metagegevens van gebruiker.

   - *userMetadataResult*: Een object dat de Java-representatie bevat van een payload naar een JSON-gebruikersmetagegevens. Bijvoorbeeld:

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
     ```

     wordt in Java vertaald als:

     ```java
     Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
     ```

     **De daadwerkelijke structuur van objecten met gebruikersmetagegevens is vergelijkbaar met het volgende:**

     ```json
     {
         updated: 1334243471,
         encrypted: ["encryptedProp"],
         data: {
             zip: ["12345", "34567"],
             maxRating: { 
                 "MPAA": "PG-13",
                 "VCHIP": "TV-Y", 
                 "URL": "http://exam.pl/e/manage/ratings"
             },
             householdID: "3456",
             userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
             channelID: ["channel-1", "channel-2"]
         }
     }
     ```


Deze waarde is null wanneer de aanvraag is ingediend voor eenvoudige metagegevens (verificatie-TTL, verificatie-TTL of apparaat-id).

- *gecodeerd*: Booleaanse waarde die opgeeft of de opgehaalde metagegevens zijn gecodeerd. Deze parameter is alleen van belang voor verzoeken van metagegevens van gebruikers. Deze parameter heeft geen betekenis voor statische metagegevens (bijvoorbeeld TTL voor verificatie) die altijd ongecodeerd worden ontvangen. Als deze parameter aan Waar wordt geplaatst, dan is het aan programmeur om de niet gecodeerde waarde van Metagegevens van de Gebruiker te verkrijgen door een decryptie van RSA uit te voeren gebruikend whitelingende privé sleutel (de zelfde privé sleutel die voor het ondertekenen van aanvrager identiteitskaart in wordt gebruikt [`setRequestor`](#setRequestor) vraag).

**geactiveerd door:** [`getMetadata()`](#getMetadata)

**Meer informatie:** [Metagegevens gebruiker](/help/authentication/user-metadata.md)

</br>

## getVersion {#getVersion}

**Omschrijving:** Gebruik deze methode om de huidige versie van AccessEnabler terug te winnen

| **API-aanroep: get AccessEnabled-versie** |
| --- |
| ```public static String getVersion()``` |

## Gebeurtenissen bijhouden {#tracking}

Toegangsactivering activeert een extra callback die niet noodzakelijk met de machtigingsstromen verwant is. Het uitvoeren van de genoemde gebeurtenis-volgende callback functie *sendTrackingData()* is optioneel, maar het stelt de toepassing in staat specifieke gebeurtenissen te volgen en statistieken te compileren, zoals het aantal succesvolle/mislukte verificatie-/autorisatiepogingen. Hieronder ziet u de specificatie voor de *sendTrackingData()* callback:

### sendTrackingData {#sendTrackingData}

**Omschrijving:** Callback teweeggebracht door Toegangstoegelaten die aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/het mislukken van authentificatie/vergunningsstromen signaleert. Het apparaattype, het clienttype Access Enabler en het besturingssysteem worden ook gerapporteerd door sendTrackingData().

>[!WARNING]
>
> Het apparaattype en het besturingssysteem worden afgeleid door het gebruik van een openbare Java-bibliotheek (http://java.net/projects/user-agent-utils) en de userAgent-tekenreeks. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.

- Mogelijke waarden voor apparaattype:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`

- Mogelijke waarden voor het clienttype Access Enabled:
   - `flash`
   - `html5`
   - `ios`
   - `tvos`
   - `android`
   - `firetv`

| Callback: gebeurtenissen bijhouden |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *event*: de gebeurtenis die wordt bijgehouden. Er zijn drie mogelijke gebeurtenistypen voor bijhouden:
   - **authenticationDetection:** om het even welk ogenblik keert een toestemmingstoken verzoek terug (gebeurtenistype is `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** wanneer een verificatiecontrole plaatsvindt (gebeurtenistype is `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** wanneer de gebruiker een MVPD in de MVPD selectievorm selecteert (gebeurtenistype is `EVENT_MVPD_SELECTION`)
- *data*: aanvullende gegevens die aan de gerapporteerde gebeurtenis zijn gekoppeld. Deze gegevens worden gepresenteerd in de vorm van een lijst met waarden.

Hieronder vindt u instructies voor het interpreteren van de waarden in de *data* array:

- Voor-gebeurtenistype *`EVENT_AUTHN_DETECTION`:*
   - **0** - Of de token-aanvraag succesvol was (true/false) en of het bovenstaande waar is:
   - **1** - MVPD ID-tekenreeks
   - **2** - GUID (md5 hashed)
   - **3** - Token bevindt zich al in cache (true/false)
   - **4** - Type apparaat
   - **5** - Toegangsbeheer voor clienttype
   - **6** - Type besturingssysteem

- Voor-gebeurtenistype `EVENT_AUTHZ_DETECTION`
   - **0** - Of de token-aanvraag succesvol was (true/false) en of deze succesvol was:
   - **1** - MVPD-id
   - **2** - GUID (md5 hashed)
   - **3** - Token bevindt zich al in cache (true/false)
   - **4** - Fout
   - **5** - Details
   - **6** - Type apparaat
   - **7** - Toegangsbeheer voor clienttype
   - **8** - Type besturingssysteem

- Voor-gebeurtenistype `EVENT_MVPD_SELECTION`
   - **0** - ID van de momenteel geselecteerde MVPD
   - **1** - Type apparaat
   - **2** - Toegangsbeheer voor clienttype
   - **3** - Type besturingssysteem

**geactiveerd door:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
