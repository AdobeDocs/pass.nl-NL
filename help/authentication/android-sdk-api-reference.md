---
title: Referentie voor API van Android
description: Referentie voor API van Android
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '4526'
ht-degree: 0%

---

# Referentie voor API van Android {#android-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

In dit document worden de methoden en callbacks beschreven die door de Android-SDK voor Adobe Pass-verificatie worden weergegeven en die worden ondersteund met Adobe Pass-verificatie versie 1.7 en hoger. De hier beschreven methodes en callback functies worden bepaald in de AccessEnabler.h en EntitlementDelegate.h kopbaldossiers.

Zie [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) voor de nieuwste Android AccessEnabler SDK.


**Opmerking:** Het Adobe Pass-verificatieteam moedigt u aan alleen Adobe Pass-verificatie te gebruiken *publiek* API&#39;s:

- Openbare API&#39;s zijn beschikbaar *en volledig getest* op alle ondersteunde clienttypen. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(n) heeft.</span>
- Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Voor *non*-public API&#39;s, behouden we ons het recht voor om hun handtekening op elk toekomstig moment te wijzigen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

## Android-API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- preautoriseren
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

### Factory.getInstance {#getInstance}

**Omschrijving:** Instantieert het Access Enabler-object. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| **public static** AccessEnabled **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**throws** AccessEnablerException |

**Beschikbaarheid:** v3.1.2+

**Parameters:**

- *appContext*: Android-toepassingscontext.
- env\_url: voor het testen met de testomgeving van Adobe staging, kan env\_url worden ingesteld op &quot;sp.auth-staging.adobe.com&quot;

**Vervangen:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Omschrijving:** Hiermee wordt de identiteit van de programmeur vastgesteld. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

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

- *signedRequestorID*: Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

**Callbacks geactiveerd:** `setRequestorComplete()`

Vervangen:

    public void setRequestor(String requestId, String signedRequestorId)
    
    public void setRequestor (String requestId, String signedRequestId, ArrayList)&lt;string> URL)

[Terug naar de Android-API...](#api)

### setRequestorComplete {#setRequestorComplete}

**Omschrijving:** Callback die door Toegankelijkheid wordt teweeggebracht die uw toepassing meedeelt dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

| Callback: configuratie aanvrager voltooid |
| --- |
| java public void setRequestorComplete(int status) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan een van de volgende waarden gebruiken:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt

**geactiveerd door:** `setRequestor()`

[Terug naar de Android-API...](#api)

### setOptions {#setOptions}

**Omschrijving:** Vormt globale opties van SDK. Het aanvaardt een **Kaart\&lt;string string=&quot;&quot;>** als argument. De waarden van de kaart zullen aan de server samen met elke netwerkvraag worden overgegaan die SDK maakt.

De waarden worden doorgegeven aan de server, onafhankelijk van de huidige flow (verificatie/autorisatie). Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.

| API-aanroep: setOptions |
| --- |
| public void setOptions(HashMap)&lt;string string=&quot;&quot;> opties) |

**Beschikbaarheid:** v1.9.2+

**Parameters:**

- *opties*: Een kaart&lt;string string=&quot;&quot;> met algemene SDK-opties. Momenteel zijn de volgende opties beschikbaar:
   - **applicationProfile** - Het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   - **ap_vi** - De Experience Cloud-id (bezoeker-id). Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   - **ap_ai** - De advertentie-id
   - **device_info** - Clientgegevens zoals hier beschreven: [Apparaatverbinding en toepassing met clientinformatie doorgeven](/help/authentication/passing-client-information-device-connection-and-application.md).

[Terug naar boven...](#apis)


### checkAuthentication {#checkAuthN}

**Omschrijving:** Controleert de verificatiestatus. Het doet dit door naar een geldig authentificatietoken in de lokale symbolische opslagruimte te zoeken. Deze methode voert geen netwerkvraag uit en wij adviseren roepend het op de belangrijkste draad. Deze wordt door de toepassing gebruikt om de verificatiestatus van de gebruiker te controleren en de gebruikersinterface dienovereenkomstig bij te werken (de gebruikersinterface voor aanmelding/aanmelding wordt dus bijgewerkt). De verificatiestatus wordt via de [*setAuthenticationStatus()*](#setAuthNStatus) callback.

Als een MVPD de eigenschap &quot;Authentificatie per Aanvrager&quot;steunt, dan kunnen de veelvoudige authentificatietokens op een apparaat worden opgeslagen.  Voor meer informatie over deze functie raadpleegt u de [Richtlijnen voor caching](#$caching) in het technische overzicht van Android.

| API-aanroep: verificatiestatus controleren |
| --- |
| public void checkAuthentication() |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus()`

[Terug naar de Android-API...](#api)


### getAuthentication {#getAuthN}

**Omschrijving:** Start de volledige verificatieworkflow. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

- Als de laatste verificatiepoging succesvol was, wordt de MVPD-selectiefase overgeslagen en wordt de [*navigateToUrl()*](#navigagteToUrl) callback wordt geactiveerd. De toepassing gebruikt deze callback om de controle te concretiseren WebView die de gebruiker met de MVPD login pagina voorstelt.
- Als de laatste verificatiepoging is mislukt of als de gebruiker zich expliciet heeft afgemeld, wordt [*displayProviderDialog()*](#displayProviderDialog) callback wordt geactiveerd. Uw toepassing gebruikt deze callback om de MVPD selectie UI te tonen. Uw app is ook vereist om de verificatiestroom te hervatten door de Access Enabler-bibliotheek te informeren over de MVPD-selectie van de gebruiker via de [setSelectedProvider()](#setSelectedProvider) methode.

Aangezien de geloofsbrieven van de gebruiker op de MVPD login pagina worden geverifieerd, wordt uw toepassing vereist om de veelvoudige omleidingsverrichtingen te controleren die plaatsvinden terwijl de gebruiker bij de MVPD login pagina voor authentiek verklaart. Wanneer de correcte geloofsbrieven zijn ingegaan, wordt de controle WebView opnieuw gericht aan een douane URL die door wordt bepaald *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* constante. Deze URL is niet bedoeld om door WebView te worden geladen. De toepassing moet deze URL onderscheppen en deze gebeurtenis interpreteren als een signaal dat de aanmeldingsfase is voltooid. Het zou dan controle aan Toegang moeten overhandigen Enabler om de authentificatiestroom te voltooien (door *getAuthenticationToken()* methode).

Als een MVPD de eigenschap &quot;Authentificatie per Aanvrager&quot;steunt, dan kunnen de veelvoudige authentificatietokens op een apparaat (per Programmer) worden opgeslagen.  Voor meer informatie over deze functie raadpleegt u de [Richtlijnen voor caching](#$caching) in het technische overzicht van Android.

Ten slotte wordt de authenticatiestatus via de *setAuthenticationStatus()* callback.



| API-aanroep: initieert de verificatiestroom |
| --- |
| public void getAuthentication() |

**Beschikbaarheid:** v1.0+

| API-aanroep: initieert de verificatiestroom |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;string object=&quot;&quot;> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *forceAuthn*: Een markering die aangeeft of de verificatiestroom moet worden gestart, ongeacht of de gebruiker al dan niet is geverifieerd.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Terug naar de Android-API...](#api)

### displayProviderDialog {#displayProviderDialog}

**Beschrijving** Callback teweeggebracht door Toegangsactivering om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om gewenste MVPD te selecteren. De callback biedt een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de selectieinterface correct samen te stellen (zoals de URL die het logo van de MVPD aanwijst, de vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door te roepen *setSelectedProvider()* en geeft u de id van de MVPD door die overeenkomt met de selectie van de gebruiker.

>[!NOTE]
>
> De verificatiestroom afbreken
> </br></br>
> Houd er rekening mee dat dit een punt is waarop de gebruiker op de knop Terug kan drukken. Dit is gelijk aan het afbreken van de verificatiestroom. In een dergelijk scenario moet uw toepassing de `setSelectedProvider()` methode, doorgeven *null* als parameter, om Toegang toe te geven toelaat de kans om zijn authentificatiestatus-machine opnieuw in te stellen.

| Callback: toon de selectie UI MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Beschikbaarheid:** v1.0+

**Parameters**:

- *mvpds*: Lijst met MVPD-objecten die MVPD-gerelateerde informatie bevatten die de toepassing kan gebruiken om de UI-elementen voor de selectie van MVPD te maken.

**geactiveerd door:** `getAuthentication(), getAuthorization()`

[Terug naar de Android-API...](#api)


### setSelectedProvider {#setSelectedProvider}

**Omschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie MVPD van de gebruiker op de hoogte te brengen. De toepassing kan deze methode gebruiken om de dienstverlener te selecteren of te veranderen die voor authentificatie wordt gebruikt.

Als het geselecteerde MVPD een TempPass MVPD is zal het automatisch met die MVPD voor authentiek verklaren zonder het moeten getAuthentication () daarna roepen.

Houd er rekening mee dat dit niet mogelijk is voor de Tijdelijke controle voor speciale acties waarbij extra parameters worden gegeven aan de methode getAuthentication().

Bij het passeren *null* als parameter, veronderstelt de Toegang Enabler dat de gebruiker de authentificatiestroom (d.w.z. op de &quot;Achterknoop&quot;gedrukt) heeft geannuleerd, en antwoordt door de authentificatiestatus-machine opnieuw in te stellen en door de *setAuthenticationStatus()* callback met de `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` foutcode.

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Terug naar de Android-API...](#api)


### navigateToUrl {#navigagteToUrl}

**Vervangen:** Vanaf Android SDK 3.0 wordt navigateToUrl alleen gebruikt als Chrome Custom Tab niet aanwezig is op het apparaat

**Omschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de toepassing meedeelt dat de gebruiker van de MVPD login pagina moet worden voorgesteld om zijn geloofsbrieven in te gaan. Toegangsbeheer gaat als parameter URL van de MVPD login pagina over. Uw toepassing wordt vereist om een controle te concretiseren WebView en het te leiden aan dit URL. Ook, wordt de toepassing vereist om URLs te controleren die door de controle WebView wordt geladen en onderschepping de redirection verrichting gericht die douane URL door wordt bepaald `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` constante. Op deze gebeurtenis, wordt de toepassing vereist om of de controle te sluiten of te verbergen WebView en de controle terug naar de bibliotheek van Inschakelen van de Toegang terug te geven door te roepen *getAuthenticationToken()* methode. De Toegangsmanager voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver terug te winnen en het lokaal in de symbolische opslag op te slaan.

>[!WARNING]
>
> **De verificatiestroom afbreken**  <br>Houd er rekening mee dat dit een punt is waarop de gebruiker op de knop Terug kan drukken. Dit is gelijk aan het afbreken van de verificatiestroom. In een dergelijk scenario moet uw toepassing de _setSelectedProvider()_ methode doorgeven _null_ als de parameter en het geven van een kans aan Toegangs toe om zijn authentificatiestatus-machine terug te stellen.

| Callback: MVPD-aanmeldingspagina weergeven |
| --- |
| public void navigateToUrl(String url) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *url*: De URL die verwijst naar de aanmeldingspagina van de MVPD

**geactiveerd door:** `getAuthentication(), setSelectedProvider()`

[Terug naar de Android-API...](#api)


### getAuthenticationToken {#getAuthNToken}

**Vervangen:** Vanaf Android SDK 3.0 wordt deze methode niet meer gebruikt vanuit de toepassing, aangezien Chrome Custom Tab wordt gebruikt voor verificatie.

**Omschrijving:** Voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken. Deze methode zou door uw toepassing slechts in antwoord op gebeurtenis moeten worden geroepen waar de controle WebView die de MVPD login pagina ontvangen opnieuw aan douane URL wordt gericht die door wordt bepaald `AccessEnabler.ADOBEPASS_REDIRECT_URL` constante.

| API-aanroep: het verificatietoken ophalen |
| --- |
| public void getAuthenticationToken() |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *cookies*: Cookies die op het doeldomein zijn ingesteld (zie de demotoepassing in de SDK voor een voorbeeldimplementatie).

**Callbacks geactiveerd:** `setAuthenticationStatus()`, `sendTrackingData()`

[Terug naar de Android-API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Omschrijving:** Callback die door Toegankelijkheid wordt teweeggebracht die de toepassing van de status van de authentificatiestroom informeert. Er zijn vele plaatsen waar deze stroom kan ontbreken, of als gevolg van gebruikersinteractie of wegens andere onvoorziene scenario&#39;s (d.w.z. de problemen van de netwerkconnectiviteit, enz.). Deze callback informeert de toepassing van de succes/mislukkingsstatus van de authentificatiestroom, terwijl ook het verstrekken van extra informatie over de mislukkingsreden, wanneer nodig.

| Callback: rapport de status van de authentificatiestroom |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan een van de volgende waarden gebruiken:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
- *code*: Redenen voor mislukking. Indien *status* is `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`vervolgens *code* is een lege tekenreeks (gedefinieerd door de `AccessEnablerConstants.USER_AUTHENTICATED` constante). In het geval van een fout kan deze parameter een van de volgende waarden hebben:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. Als reactie op de *checkAuthentication()* methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler heeft de authentificatiestatus-machine opnieuw ingesteld nadat de hogere laagtoepassing overgegaan *null* tot `setSelectedProvider()` om de verificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals niet-beschikbaarheid van het netwerk of de gebruiker heeft de verificatiestroom expliciet geannuleerd.

**geactiveerd door:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Terug naar de Android-API...](#api)


### checkPreauthorisedResources {#checkPreauth}

>**Vervangen:** Vanaf Android SDK 3.6 vervangt de API voor preautoriseren checkPreauthorisedResources, die uitgebreide foutcodes biedt.

**Omschrijving:** Deze methode wordt door de toepassing gebruikt om te bepalen of de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Beschikbaarheid:** v1.3+

**Parameters:** De `resources` parameter is een array van bronnen waarvoor de autorisatie moet worden gecontroleerd . Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, d.w.z., zou het een overeengekomen waarde moeten zijn die tussen de programmeur en MVPD of een mediaRSS fragment wordt gevestigd.

**Callback geactiveerd:** `preauthorizedResources()`

[Terug naar de Android-API...](#api)


### checkPreauthorisedResources {#checkPreauth2}

**Vervangen:** Vanaf Android SDK 3.6 vervangt de API voor preautoriseren checkPreauthorisedResources, die uitgebreide foutcodes biedt.

**Omschrijving:** Deze methode wordt door de toepassing gebruikt om te bepalen of de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Beschikbaarheid:** v3.1+

**Parameters:** De `resources` parameter is een array van bronnen waarvoor de autorisatie moet worden gecontroleerd . Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, d.w.z., zou het een overeengekomen waarde moeten zijn die tussen de programmeur en MVPD of een mediaRSS fragment wordt gevestigd.

De `cache` parameter geeft aan of preautorisatierespons in de cache kunnen worden gebruikt. Standaard is de cache waar en retourneert de SDK een eerder in de cache geplaatste reactie, indien beschikbaar.

**Callback geactiveerd:** `preauthorizedResources()`

[Terug naar de Android-API...](#api)

### preauthorisedResources {#preauthResources}

**Vervangen:** Vanaf Android SDK 3.6 vervangt de API voor preautoriseren checkPreauthorisedResources, die uitgebreide foutcodes biedt. preauthorisedResources callback zal niet op nieuwe API worden geroepen.


**Omschrijving:** Callback geactiveerd door checkPreauthorisedResources(). Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Beschikbaarheid:** v1.3+

**Parameters:** De `resources` parameter is een array van bronnen waarvan de gebruiker al gemachtigd is om deze te bekijken.

**geactiveerd door:** `checkPreauthorizedResources()`

[Terug naar de Android-API...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de status van de vergunning te controleren. Het begint door de authentificatiestatus eerst te controleren. Indien niet geverifieerd, wordt de *setTokenRequestFailed()* callback wordt geactiveerd en de methode wordt afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie de details over de *getAuthorization()* methode.

| API-aanroep: autorisatiestatus controleren |
| --- |
| public void checkAuthorization(String resourceId) |

**Beschikbaarheid:** v1.0+

| API-aanroep: autorisatiestatus controleren |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *resourceId*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Terug naar de Android-API...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de vergunningsstroom in gang te zetten. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat Access Enabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het korte-levende media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. De *setToken()* callback wordt geactiveerd en het korte media-token wordt als een parameter aan de toepassing geleverd. Indien de vergunning om welke reden dan ook mislukt, *tokenRequestFailed()* callback wordt geactiveerd en de foutcode en details worden gegeven.

| API-aanroep: de machtigingsstroom starten |
| --- |
| public void getAuthorization(String resourceId) |

**Beschikbaarheid:** v1.0+

| API-aanroep: de machtigingsstroom starten |
| --- |
| public void getAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *resourceId*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
- *data*: Een kaart die bestaat uit sleutel-waardeparen die naar de betaaltelevisie-omslagdienst moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Extra callbacks geactiveerd**  <br> Deze methode kan de volgende callbacks (als de authentificatiestroom ook in werking wordt gesteld) ook teweegbrengen: *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**NOTA: Gelieve te gebruiken checkAuthorization () in plaats van getAuthorization () wanneer mogelijk. De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit zou tot een ingewikkelde implementatie aan de kant van de Programmer kunnen leiden.**

[Terug naar de Android-API...](#api)


### setToken {#setToken}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die uw toepassing meedeelt dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.

| Callback: autorisatiestroom is voltooid |
| --- |
| public void setToken(String-token, String resourceId) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *token*: De token voor korte media
- *resourceId*: De bron waarvoor de vergunning is verleend

**geactiveerd door:** `checkAuthorization()`, `getAuthorization()`


[Terug naar de Android-API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Omschrijving:** Callback teweeggebracht door Toegelaten van de Toegang die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

| Callback: autorisatiestroom mislukt |
| --- |
| public void tokenRequestFailed(String resourceId), <br>        String errorCode, String errorDescription) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: De bron waarvoor de vergunning is verleend
- *errorCode*: Foutcode die aan het mislukkingsscenario is gekoppeld. Mogelijke waarden:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - De gebruiker kan geen autorisatie voor de opgegeven resource uitvoeren.
- *errorDescription*: Aanvullende details over het mislukkingsscenario. Als deze beschrijvende tekenreeks om welke reden dan ook niet beschikbaar is, verzendt Adobe Pass Authentication een lege tekenreeks **(&quot;&quot;)**.

  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopgerelateerde berichten door te geven. Bijvoorbeeld, als een abonnee vergunning voor een middel wordt ontkend, kon MVPD een bericht zoals verzenden: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik hier als u het pakket wilt bijwerken.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier.&quot;

**geactiveerd door:** `checkAuthorization(), getAuthorization()`

[Terug naar de Android-API...](#api)

### afmelden {#logout}

**Omschrijving:** Gebruik deze methode om de logout-flow te starten. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Dientengevolge, kan deze stroom niet met een eenvoudige HTTP- verzoek worden voltooid dat door de bibliotheek van Inschakelen van de Toegang wordt uitgegeven. De SDK gebruikt aangepaste Chrome-tabs om de HTTP-omleidingsbewerkingen uit te voeren. Deze stroom is zichtbaar voor de gebruiker en wordt gesloten wanneer deze is voltooid

| API-aanroep: de afmeldingsstroom starten |
| --- |
| public void logout() |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:**

- `navigateToUrl()` voor SDK-versie ouder dan 3.0
- `setAuthenticationStatus()` voor SDK-versie > 3.0


[Terug naar de Android-API...](#api)


### getSelectedProvider {#getSelectedProvider}

**Omschrijving:** Gebruik deze methode om de momenteel geselecteerde provider te bepalen.

| API-aanroep: bepaal de momenteel geselecteerde MVPD |
| --- |
| public void getSelectedProvider() |

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `selectedProvider()`

[Terug naar de Android-API...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

| Callback: informatie over momenteel geselecteerde MVPD |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Beschikbaarheid:** v1.0+

**Parameters:**

- *mvpd*: Object met informatie over de momenteel geselecteerde MVPD

**geactiveerd door:** `getSelectedProvider()`

[Terug naar de Android-API...](#api)


### getMetadata {#getMetadata}

**Omschrijving:** Gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek van Inschakelen van de Toegang wordt blootgesteld. De toepassing heeft toegang tot deze informatie door een samengesteld object MetadataKey op te geven.

| API-aanroep: de AccessEnabler vragen naar metagegevens |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Beschikbaarheid:** v1.0+

Er zijn twee soorten meta-gegevens beschikbaar aan Programmeurs:

- Statische metagegevens (verificatietoken TTL, machtigingstoken-TTL en apparaat-id)
- Gebruikersmetagegevens (gebruikersspecifieke informatie, zoals gebruikersnaam en postcode; doorgegeven van een MVPD aan het apparaat van een gebruiker tijdens de verificatie- en/of autorisatiestromen)

**Parameters:**

- *metadataKey*: Een gegevensstructuur die een sleutel- en args-variabele omvat, met de volgende betekenis:
   - Als key is `METADATA_KEY_USER_META` en args bevat een SerializableNameValuePair-object met name = `METADATA_ARG_USER_META` en value = `[metadata_name]`, dan wordt de vraag gemaakt voor gebruikersmeta-gegevens. De huidige lijst met beschikbare metagegevenstypen voor gebruikers:
      - `zip` - Postcode

      - `householdID` - Huishoudelijke identificatiecode. Als een MVPD geen subaccounts steunt, zal dit identiek zijn aan `userID`.

      - `maxRating` - Maximale ouderlijke classificatie voor de gebruiker

      - `userID` - De gebruikersnaam. Als een MVPD subaccounts steunt, en de gebruiker is niet de belangrijkste rekening, `userID` verschilt van `householdID`.

      - `channelID` - Een lijst met kanalen die de gebruiker mag bekijken
   - Als key is `METADATA_KEY_DEVICE_ID` dan wordt de vraag gemaakt om huidige apparatenidentiteitskaart te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.
   - Als key is `METADATA_KEY_TTL_AUTHZ` en args bevat een SerializableNameValuePair-object met name = `METADATA_ARG_RESOURCE_ID` en value = `[resource_id]`, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.
   - Als key is `METADATA_KEY_TTL_AUTHN` dan wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.



>[!NOTE]
>
>Voor SDK 3.4.0, de constanten: `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` zijn beschikbaar op com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>De daadwerkelijke Metagegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD ter beschikking stelt.  Deze lijst wordt verder uitgebreid naarmate nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**Callbacks geactiveerd:** [`setMetadataStatus()`](#setMetadaStatus)

**Meer informatie:** [Metagegevens gebruiker](/help/authentication/user-metadata-feature.md)

[Terug naar de Android-API...](#api)

### setMetadataStatus {#setMetadaStatus}

**Omschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die de meta-gegevens levert die via wordt gevraagd *getMetadata()* vraag.

| Callback: resultaat van verzoek om metagegevens op te halen |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *key*: Het object MetadataKey dat de sleutel bevat waarvoor een metagegevenswaarde wordt aangevraagd en de bijbehorende parameters (zie demotoepassing voor een referentie-implementatie).
- *resultaat*: Een samengesteld object met de gevraagde metagegevens. Het object heeft de volgende velden:
   - *simpleResult*: een tekenreeks die de metagegevenswaarde vertegenwoordigt op het moment dat de aanvraag is ingediend voor verificatie-TTL, verificatie-TTL of apparaat-id. Deze waarde is null als de aanvraag is ingediend voor metagegevens van gebruiker.

   - *userMetadataResult*: Een object dat de Java-representatie bevat van een payload naar een JSON-gebruikersmetagegevens.\
     Bijvoorbeeld:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
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

**Meer informatie:** [Metagegevens gebruiker](/help/authentication/user-metadata-feature.md)


[Terug naar de Android-API...](#api)


### getVersion {#getVersion}

**Omschrijving:** Deze methode kan worden gebruikt om de versie van de bibliotheek terug te winnen AccessEnabler.

| API-aanroep: get AccessEnabled-versie |
| --- |
| ```public static String getVersion()``` |


[Terug naar de Android-API...](#api)

</br>

## Gebeurtenissen bijhouden {#tracking}

Toegangsactivering activeert een extra callback die niet noodzakelijk met de machtigingsstromen verwant is. Het uitvoeren van de genoemde gebeurtenis-volgende callback functie *sendTrackingData()* is optioneel, maar het stelt de toepassing in staat specifieke gebeurtenissen te volgen en statistieken te compileren, zoals het aantal succesvolle/mislukte verificatie-/autorisatiepogingen. Hieronder ziet u de specificatie voor de *sendTrackingData()* callback:


### sendTrackingData {#sendTrackingData}

**Omschrijving:** Callback teweeggebracht door Toegangstoegelaten die aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/het mislukken van authentificatie/vergunningsstromen signaleert. Het apparaattype, het clienttype Access Enabler en het besturingssysteem worden ook gerapporteerd door sendTrackingData().

>[!WARNING]
>
> Het apparaattype en besturingssysteem worden afgeleid via een openbare Java-bibliotheek ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) en de userAgent-tekenreeks. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.


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
   - `android`

</br>

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

Hieronder vindt u instructies voor het interpreteren van de waarden in de *data*
array:

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

**geactiveerd door:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Terug naar de Android-API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
