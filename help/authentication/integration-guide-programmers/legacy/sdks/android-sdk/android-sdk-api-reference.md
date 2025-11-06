---
title: Android SDK API-naslag
description: Android SDK API-naslag
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# (Verouderd) Android SDK API-naslag {#android-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#intro}

In dit document worden de methoden en callbacks beschreven die door de Android SDK for Adobe Pass Authentication worden weergegeven en die worden ondersteund met Adobe Pass Authentication 1.7 en hoger. De hier beschreven methodes en callback functies worden bepaald in de AccessEnabler.h en EntitlementDelegate.h kopbaldossiers.

Gelieve te verwijzen naar [ https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library ](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) voor de recentste Android AccessEnabler SDK.


**Nota:** het team van de Authentificatie van Adobe Pass moedigt u aan om slechts de Authentificatie van Adobe Pass *openbare* APIs te gebruiken:

- Openbare APIs is beschikbaar *en volledig getest* op alle gesteunde cliënttypes. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(s) heeft.</span>
- Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Nochtans, voor *niet* - openbare APIs, behouden wij het recht om hun handtekening op om het even welk toekomstig punt te veranderen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

## ANDROID API {#api}

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

**Beschrijving:** Instantieert het voorwerp van Inschakelen van de Toegang. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| **openbare statische** AccessEnabler **getInstance** (Context appContext, String softwareStatement, String redirectUrl) <br>        **werpt** AccessEnablerException <br><br>**openbare statische** AccessEnabler getInstance (Context appContext, String env_url, de softwareverklaring van het Koord, RedirectUrl) <br>**werpt** AccessEnablerException |

**Beschikbaarheid:** v3.1.2+

**Parameters:**

- *appContext*: de toepassingscontext van Android.
- env\_url: voor tests met de Adobe-testomgeving kunt u env\_url instellen op &quot;sp.auth-staging.adobe.com&quot;

**Afgekeurd:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Beschrijving:** vestigt de identiteit van de programmeur. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: het milieu van de Versie/van de Productie van Adobe.

Als een waarde voor de *urls* parameter wordt verstrekt, richt de resulterende netwerkvraag alle URLs die in de *wordt verstrekt urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst onthoudt de Access Enabler de URL van de bijbehorende serviceprovider. Alle volgende machtigingsaanvragen worden doorgestuurd naar de URL die is gekoppeld aan de serviceprovider die tijdens de configuratiefase aan de doel-MVPD is gekoppeld.

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+


**Parameters:**

- *requestID*: Unieke identiteitskaart verbonden aan de Programmer. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.

- *signedRequestorID*: Een exemplaar van de aanvrageridentiteitskaart die digitaal met uw privé sleutel wordt ondertekend. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)--> .

- *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Met deze array kunt u eindpunten opgeven voor verificatie- en verificatieservices die door Adobe worden geleverd (verschillende instanties kunnen worden gebruikt voor foutopsporingsdoeleinden). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Daarbij bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD is gekoppeld aan de snelste serviceprovider, dat wil zeggen de provider die eerst heeft gereageerd en die die MVPD ondersteunt.

**teweeggebrachte callbacks:** `setRequestorComplete()`

Vervangen:

     openbare void setRequestor (String requestId, String signedRequestorId) 
    
     openbare void setRequestor (String requestId, String signedRequestorId, ArrayList&lt;String> urls) 

[Terug naar Android API...](#api)

### setRequestorComplete {#setRequestorComplete}

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die uw toepassing informeert dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

| Callback: configuratie aanvrager voltooid |
| --- |
| java public void setRequestorComplete(int status) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan één van de volgende waarden nemen:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt

**teweeggebracht door:** `setRequestor()`

[Terug naar Android API...](#api)

### setOptions {#setOptions}

**Beschrijving:** vormt globale opties van SDK. Het keurt a **Kaart \&lt;Koord, Koord \>** als argument goed. De waarden van de kaart worden samen met elke netwerkaanroep die de SDK maakt, aan de server doorgegeven.

De waarden worden doorgegeven aan de server, onafhankelijk van de huidige flow (verificatie/autorisatie). Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.

| API-aanroep: setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String>-opties) |

**Beschikbaarheid:** v1.9.2+

**Parameters:**

- *opties*: Een Kaart&lt;Koord, Koord> die globale opties van SDK bevatten. Momenteel zijn de volgende opties beschikbaar:
   - **applicationProfile** - het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   - **ap_vi** - identiteitskaart van Experience Cloud (bezoekerID). Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   - **ap_ai** - identiteitskaart van Advertising
   - **device_info** - de informatie van de Cliënt zoals hier beschreven: [ die de verbinding en toepassing van het het informatieapparaat van de cliënt overgaan ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Terug naar boven...](#apis)


### checkAuthentication {#checkAuthN}

**Beschrijving:** controleert de authentificatiestatus. Het doet dit door naar een geldig authentificatietoken in de lokale symbolische opslagruimte te zoeken. Deze methode voert geen netwerkvraag uit en wij adviseren roepend het op de belangrijkste draad. Deze wordt door de toepassing gebruikt om de verificatiestatus van de gebruiker te controleren en de gebruikersinterface dienovereenkomstig bij te werken (de gebruikersinterface voor aanmelding/aanmelding wordt dus bijgewerkt). De authentificatiestatus wordt meegedeeld aan de toepassing via [*setAuthenticationStatus ()*](#setAuthNStatus) callback.

Als een MVPD de functie &#39;Verificatie per aanvrager&#39; ondersteunt, kunnen meerdere verificatietokens op een apparaat worden opgeslagen.  Voor details op deze eigenschap, zie de [ sectie van de Richtlijnen van het 0} Caching {in het Technische Overzicht van Android.](#$caching)

| API-aanroep: verificatiestatus controleren |
| --- |
| public void checkAuthentication() |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus()`

[Terug naar Android API...](#api)


### getAuthentication {#getAuthN}

**Beschrijving:** begint het volledige authentificatiewerkschema. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

- Als de laatste authentificatiepoging succesvol was, wordt de selectiefase van MVPD overgeslagen en [*navigateToUrl ()*](#navigagteToUrl) callback wordt teweeggebracht. De toepassing gebruikt deze callback om de controle te concretiseren WebView die de gebruiker met de MVPD login pagina voorstelt.
- Als de laatste authentificatiepoging niet succesvol was of als de gebruiker uitdrukkelijk het programma opende, [*displayProviderDialog ()*](#displayProviderDialog) callback wordt teweeggebracht. Deze callback wordt door uw toepassing gebruikt om de gebruikersinterface van de MVPD-selectie weer te geven. Ook wordt uw app vereist om de authentificatiestroom te hervatten door de bibliotheek van Inschakelen van de Toegang over de selectie van MVPD van de gebruiker via [ te informeren setSelectedProvider () ](#setSelectedProvider) methode.

Aangezien de gebruikersgegevens op de MVPD-aanmeldingspagina worden gecontroleerd, moet uw toepassing de meervoudige omleidingsbewerkingen controleren die plaatsvinden terwijl de gebruiker op de MVPD-aanmeldingspagina wordt geverifieerd. Wanneer de correcte geloofsbrieven zijn ingegaan, wordt de controle WebView opnieuw gericht aan een douane URL die door de {*constante wordt bepaald 0} AccessEnabler.ADOBEPASS\_REDIRECT\_URL.* Deze URL is niet bedoeld om door WebView te worden geladen. De toepassing moet deze URL onderscheppen en deze gebeurtenis interpreteren als een signaal dat de aanmeldingsfase is voltooid. Het zou dan controle aan Toegang moeten overhandigen Enabler om de authentificatiestroom te voltooien (door *te roepen getAuthenticationToken ()* methode).

Als een MVPD de functie &#39;Verificatie per aanvrager&#39; ondersteunt, kunnen meerdere verificatietokens worden opgeslagen op een apparaat (één per programmeur).  Voor details op deze eigenschap, zie de [ sectie van de Richtlijnen van het 0} Caching {in het Technische Overzicht van Android.](#$caching)

Tot slot wordt de authentificatiestatus meegedeeld aan de toepassing via *setAuthenticationStatus ()* callback.



| API-aanroep: initieert de verificatiestroom |
| --- |
| public void getAuthentication() |

**Beschikbaarheid:** v1.0+

| API-aanroep: initieert de verificatiestroom |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *forceAuthn*: Een vlag die specificeert als de authentificatiestroom zou moeten zijn begonnen, ongeacht als de gebruiker reeds voor authentiek verklaard of niet is.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Terug naar Android API...](#api)

### displayProviderDialog {#displayProviderDialog}

**Omschrijving** Callback die door Toegangsactivering wordt teweeggebracht om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om de gewenste MVPD te selecteren. De callback bevat een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de gebruikersinterface van de selectie correct samen te stellen (zoals de URL die het MVPD-logo aanwijst, een vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door *te roepen setSelectedProvider ()* en het over te gaan identiteitskaart van MVPD die aan de selectie van de gebruiker beantwoordt.

>[!NOTE]
>
> De verificatiestroom afbreken
> </br></br>
> Houd er rekening mee dat dit een punt is waarop de gebruiker op de knop Terug kan drukken. Dit is gelijk aan het afbreken van de verificatiestroom. In zulk een scenario, wordt uw toepassing vereist om de `setSelectedProvider()` methode te roepen, die *ongeldig* als parameter overgaat, om Toegang te geven toelaat de kans om zijn authentificatiestatus-machine terug te stellen.

| Callback: de gebruikersinterface van de MVPD-selectie weergeven |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Beschikbaarheid:** v1.0+

**Parameters**:

- *mvpds*: Lijst van de voorwerpen van MVPD die op MVPD betrekking hebbende informatie houden die de toepassing kan gebruiken om de elementen van de MVPD selectie UI te bouwen.

**teweeggebracht door:** `getAuthentication(), getAuthorization()`

[Terug naar Android API...](#api)


### setSelectedProvider {#setSelectedProvider}

**Beschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie van MVPD van de gebruiker op de hoogte te brengen. De toepassing kan deze methode gebruiken om de dienstverlener te selecteren of te veranderen die voor authentificatie wordt gebruikt.

Als de geselecteerde MVPD een TempPass-MVPD is, wordt deze automatisch geverifieerd met die MVPD zonder dat getAuthentication() achteraf moet worden aangeroepen.

Houd er rekening mee dat dit niet mogelijk is voor de Tijdelijke controle voor speciale acties waarbij extra parameters worden gegeven aan de methode getAuthentication().

Wanneer het overgaan van *ongeldig* als parameter, veronderstelt de Toegang dat de gebruiker de authentificatiestroom (d.w.z. op de &quot;Achterknoop&quot;gedrukt) heeft geannuleerd, en antwoordt door de authentificatiestatus-machine terug te stellen en door *te roepen setAuthenticationStatus ()* callback met de `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` foutencode.

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Terug naar Android API...](#api)


### navigateToUrl {#navigagteToUrl}

**Afgekeurd:** Beginnend met Android SDK 3.0, navigateToUrl wordt gebruikt slechts als het Lusje van de Douane van Chrome niet op het apparaat aanwezig is

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de toepassing meedeelt dat de gebruiker met de login van MVPD pagina moet worden voorgesteld om zijn geloofsbrieven in te gaan. De Toegangsfunctie geeft als parameter de URL van de MVPD-aanmeldingspagina door. Uw toepassing wordt vereist om een controle te concretiseren WebView en het te leiden aan dit URL. Ook, wordt de toepassing vereist om URLs te controleren die door de controle WebView wordt geladen en onderschepping de omleidingsverrichting richtend die douane URL door de `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` constante wordt bepaald. Op deze gebeurtenis, wordt de toepassing vereist om of de controle te sluiten of te verbergen WebView en de controle terug naar de bibliotheek van Inschakelen van de Toegang terug te winnen door *te roepen getAuthenticationToken ()* methode. De Toegangsmanager voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver terug te winnen en het lokaal in de symbolische opslag op te slaan.

>[!WARNING]
>
> **Aborting de authentificatiestroom** <br> gelieve te merken dat dit een punt is waar de gebruiker de capaciteit heeft om de &quot;Achterknoop&quot;te drukken, die aan het aborteren van de authentificatiestroom gelijkwaardig is. In zulk een scenario, wordt uw toepassing vereist om _te roepen setSelectedProvider ()_ methode die _ongeldig_ als parameter overgaat en een kans geeft aan Toegang toe om zijn authentificatiestatus-machine terug te stellen.

| Callback: MVPD-aanmeldingspagina weergeven |
| --- |
| public void navigateToUrl(String url) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *url*: URL die aan de MVPD login pagina richt

**teweeggebracht door:** `getAuthentication(), setSelectedProvider()`

[Terug naar Android API...](#api)


### getAuthenticationToken {#getAuthNToken}

**Afgekeurd:** Beginnend met Android SDK 3.0, aangezien het Lusje van de Douane van Chrome voor authentificatie wordt gebruikt, wordt deze methode niet meer gebruikt van de toepassing.

**Beschrijving:** voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken. Deze methode moet alleen door uw toepassing worden aangeroepen als reactie op een gebeurtenis waarbij het WebView-besturingselement dat de MVPD-aanmeldingspagina host, wordt omgeleid naar de aangepaste URL die door de `AccessEnabler.ADOBEPASS_REDIRECT_URL` -constante wordt gedefinieerd.

| API-aanroep: het verificatietoken ophalen |
| --- |
| public void getAuthenticationToken() |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *koekjes*: De koekjes die op het doeldomein (zie de demotoepassing in SDK voor een verwijzingsimplementatie) worden geplaatst.

**teweeggebrachte callbacks:** `setAuthenticationStatus()`, `sendTrackingData()`

[Terug naar Android API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die informeert
de toepassing van de status van de verificatiestroom. Er zijn veel
plaatsen waar deze stroom kan ontbreken, of als resultaat van gebruiker
interactie of als gevolg van andere onvoorziene scenario&#39;s (bv. netwerk
verbindingsproblemen, enz.). Deze callback informeert de toepassing van
de successtatus/mislukkingsstatus van de verificatiestroom, maar ook
indien nodig aanvullende informatie over de oorzaak van de fout te verstrekken.

| Callback: rapport de status van de authentificatiestroom |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan één van de volgende waarden nemen:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
- *code*: De reden van de mislukking. Als *status* `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` is, dan *code* een leeg koord (d.w.z., dat door de `AccessEnablerConstants.USER_AUTHENTICATED` constante wordt bepaald) is. In het geval van een fout kan deze parameter een van de volgende waarden hebben:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. In antwoord op *checkAuthentication ()* methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler heeft de authentificatiestatus-machine na de upper-layer toepassing teruggesteld *ongeldig* tot `setSelectedProvider()` wordt overgegaan om de authentificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals netwerkonbeschikbaarheid of omdat de gebruiker de verificatiestroom expliciet heeft geannuleerd.

**teweeggebracht door:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Terug naar Android API...](#api)


### checkPreauthorisedResources {#checkPreauth}

>**Afgekeurd:** Beginnend met Android SDK 3.6, preauthorize API vervangt checkPreauthorisedResources, verstrekkend uitgebreide foutencodes.

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om te bepalen als de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Beschikbaarheid:** v1.3+

**Parameters:** de `resources` parameter is een serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization()` . Dit houdt in dat er een waarde moet worden overeengekomen tussen de programmeur en de MVPD of een RSS-fragment voor media.

**callback teweeggebracht:** `preauthorizedResources()`

[Terug naar Android API...](#api)


### checkPreauthorisedResources {#checkPreauth2}

**Afgekeurd:** Beginnend met Android SDK 3.6, preauthorize API vervangt checkPreauthorisedResources, verstrekkend uitgebreide foutencodes.

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om te bepalen als de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Beschikbaarheid:** v3.1+

**Parameters:** de `resources` parameter is een serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization()` . Dit houdt in dat er een waarde moet worden overeengekomen tussen de programmeur en de MVPD of een RSS-fragment voor media.

De parameter `cache` geeft aan of preautorisatiereactie in de cache kan worden gebruikt of niet. Standaard is de cache true, de SDK retourneert een eerder in de cache opgeslagen reactie, indien beschikbaar.

**callback teweeggebracht:** `preauthorizedResources()`

[Terug naar Android API...](#api)

### preauthorisedResources {#preauthResources}

**Afgekeurd:** Beginnend met Android SDK 3.6, preauthorize API vervangt checkPreauthorisedResources, verstrekkend uitgebreide foutencodes. preauthorisedResources callback zal niet op nieuwe API worden geroepen.


**Beschrijving:** Callback die door checkPreauthorisedResources () wordt teweeggebracht. Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

| API-aanroep: stel de momenteel geselecteerde provider in |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Beschikbaarheid:** v1.3+

**Parameters:** de `resources` parameter is een serie van middelen waarvoor de gebruiker reeds gemachtigd is om te bekijken.

**teweeggebracht door:** `checkPreauthorizedResources()`

[Terug naar Android API...](#api)

### <span id="checkAuthZ"></span> checkAuthorization

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstatus te controleren. Het begint door de authentificatiestatus eerst te controleren. Als niet voor authentiek verklaard, wordt *setTokenRequestFailed ()* callback teweeggebracht, en de methode weggaat. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie details op *getAuthorization ()* methode.

| API-aanroep: autorisatiestatus controleren |
| --- |
| public void checkAuthorization(String resourceId) |

**Beschikbaarheid:** v1.0+

| API-aanroep: autorisatiestatus controleren |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *resourceId*: Identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Terug naar Android API...](#api)


### <span id="getAuthZ"></span> getAuthorization

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstroom in werking te stellen. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat Access Enabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het korte-levende media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. *setToken ()* callback wordt teweeggebracht en het korte media teken wordt geleverd als parameter aan de toepassing. Als om het even welke reden, de vergunning ontbreekt, *tokenRequestFailed ()* callback wordt teweeggebracht en de foutencode en de details worden verstrekt.

| API-aanroep: de machtigingsstroom starten |
| --- |
| public void getAuthorization(String resourceId) |

**Beschikbaarheid:** v1.0+

| API-aanroep: de machtigingsstroom starten |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Beschikbaarheid:** v1.8+

**Parameters:**

- *resourceId*: Identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Extra callbacks teweeggebrachte** <br> Deze methode kan de volgende callbacks (als de authentificatiestroom ook) teweegbrengen: *setAuthenticationStatus ()*, *displayProviderDialog ()*, *navigateToUrl ()*

**NOTA: Gelieve te gebruiken checkAuthorization () in plaats van getAuthorization () wanneer mogelijk. De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit tot een ingewikkelde implementatie aan de kant van de Programmer zou kunnen leiden.**

[Terug naar Android API...](#api)


### setToken {#setToken}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die uw toepassing informeert dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.

| Callback: autorisatiestroom is voltooid |
| --- |
| public void setToken(String-token, String resourceId) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *teken*: Het korte-levende media teken
- *resourceId*: Het middel waarvoor de vergunning werd verkregen

**teweeggebracht door:** `checkAuthorization()`, `getAuthorization()`


[Terug naar Android API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

| Callback: autorisatiestroom mislukt |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: Het middel waarvoor de vergunning werd verkregen
- *errorCode*: De code van de fout verbonden aan het mislukkingsscenario. Mogelijke waarden:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - De gebruiker kan geen autorisatie voor de opgegeven resource uitvoeren
- *errorDescription*: De extra details over het mislukkingsscenario. Als dit beschrijvende koord om het even welke reden niet beschikbaar is, verzendt de Authentificatie van Adobe Pass een leeg koord **(&quot;&quot;&quot;)**.

  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopberichten door te geven. Als een abonnee bijvoorbeeld geen toestemming voor een resource krijgt, kan de MVPD een bericht verzenden zoals: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik hier als u het pakket wilt bijwerken.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier.&quot;

**teweeggebracht door:** `checkAuthorization(), getAuthorization()`

[Terug naar Android API...](#api)

### afmelden {#logout}

**Beschrijving:** gebruik deze methode om de logout stroom in werking te stellen. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Dientengevolge, kan deze stroom niet met een eenvoudige HTTP- verzoek worden voltooid dat door de bibliotheek van Inschakelen van de Toegang wordt uitgegeven. Een Chrome Custom Tabs wordt door de SDK gebruikt om de HTTP-omleidingsbewerkingen uit te voeren. Deze stroom is zichtbaar voor de gebruiker en wordt gesloten wanneer deze is voltooid

| API-aanroep: de afmeldingsstroom starten |
| --- |
| public void logout() |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:**

- `navigateToUrl()` voor SDK-versie ouder dan 3.0
- `setAuthenticationStatus()` voor SDK-versie > 3.0


[Terug naar Android API...](#api)


### getSelectedProvider {#getSelectedProvider}

**Beschrijving:** gebruik deze methode om de momenteel geselecteerde leverancier te bepalen.

| API-aanroep: de momenteel geselecteerde MVPD bepalen |
| --- |
| public void getSelectedProvider() |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `selectedProvider()`

[Terug naar Android API...](#api)


### <span id="selectedProvider"></span> selectedProvider

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

| Callback: informatie over de momenteel geselecteerde MVPD |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Beschikbaarheid:** v1.0+

**Parameters:**

- *mvpd*: Voorwerp dat informatie over momenteel geselecteerde MVPD bevat

**teweeggebracht door:** `getSelectedProvider()`

[Terug naar Android API...](#api)


### getMetadata {#getMetadata}

**Beschrijving:** gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek van Inschakelen van de Toegang wordt blootgesteld. De toepassing heeft toegang tot deze informatie door een samengesteld object MetadataKey op te geven.

| API-aanroep: de AccessEnabler vragen naar metagegevens |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Beschikbaarheid:** v1.0+

Er zijn twee soorten meta-gegevens beschikbaar aan Programmeurs:

- Statische metagegevens (verificatietoken TTL, machtigingstoken-TTL en apparaat-id)
- Metagegevens van gebruikers (gebruikersspecifieke informatie, zoals gebruikersnaam en postcode; doorgegeven van een MVPD naar het apparaat van een gebruiker tijdens de verificatie- en/of autorisatiestromen)

**Parameters:**

- *metadataKey*: Een gegevensstructuur die een sleutel en args variabele inkapselt, met de volgende betekenis:
   - Als de sleutel `METADATA_KEY_USER_META` is en args een SerializableNameValuePair voorwerp met naam = `METADATA_ARG_USER_META` en waarde = `[metadata_name]` bevat, dan wordt de vraag gemaakt voor gebruikersmeta-gegevens. De huidige lijst met beschikbare metagegevenstypen voor gebruikers:
      - `zip` - Postcode

      - `householdID` - Huishoudelijke id. Als een MVPD geen subaccounts ondersteunt, is dit hetzelfde als `userID` .

      - `maxRating` - Maximale ouderlijke classificatie voor de gebruiker

      - `userID` - De gebruikers-id. Als een MVPD subaccounts ondersteunt en de gebruiker niet de hoofdaccount is, zal `userID` anders zijn dan `householdID` .

      - `channelID` - Een lijst met kanalen die de gebruiker mag bekijken
   - Als de toets `METADATA_KEY_DEVICE_ID` is, wordt de query uitgevoerd om de huidige apparaat-id te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs dienen contact op te nemen met Adobe voor informatie over activering en kosten.
   - Als de sleutel `METADATA_KEY_TTL_AUTHZ` is en args een SerializableNameValuePair voorwerp met naam = `METADATA_ARG_RESOURCE_ID` en waarde = `[resource_id]` bevat, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan de gespecificeerde middel.
   - Als de sleutel `METADATA_KEY_TTL_AUTHN` is, wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.



>[!NOTE]
>
>Voor SDK 3.4.0 zijn de constanten : `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` beschikbaar op com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>De werkelijke gebruikersmetagegevens die beschikbaar zijn voor een programmeur, zijn afhankelijk van wat een MVPD beschikbaar stelt.  Deze lijst wordt verder uitgebreid naarmate nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**teweeggebrachte callbacks:** [`setMetadataStatus()`](#setMetadaStatus)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[Terug naar Android API...](#api)

### setMetadataStatus {#setMetadaStatus}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de meta-gegevens levert die via a *worden gevraagd getMetadata ()* vraag.

| Callback: resultaat van verzoek om metagegevens op te halen |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *sleutel*: Het voorwerp MetadataKey dat de sleutel bevat waarvoor de meta-gegevenswaarde en bijbehorende parameters wordt gevraagd (zie duotoepassing voor een verwijzingsimplementatie).
- *resultaat*: Een samengesteld voorwerp dat de gevraagde meta-gegevens bevat. Het object heeft de volgende velden:
   - *simpleResult*: een Koord dat de meta-gegevenswaarde vertegenwoordigt toen het verzoek voor Authentificatie TTL, Vergunning TTL, of identiteitskaart van het Apparaat werd gemaakt. Deze waarde is null als de aanvraag is ingediend voor metagegevens van gebruiker.

   - *userMetadataResult*: een Voorwerp dat de vertegenwoordiging Java van een nuttige lading van de Metagegevens van de Gebruiker JSON bevat.\
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

**de daadwerkelijke structuur van de voorwerpen van gebruikersmeta-gegevens is gelijkaardig aan het volgende:**

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

- *gecodeerd*: De waarde van Boole die specificeert of de teruggewonnen meta-gegevens al dan niet gecodeerd zijn. Deze parameter is alleen van belang voor verzoeken van metagegevens van gebruikers. Deze parameter heeft geen betekenis voor statische metagegevens (bijvoorbeeld TTL voor verificatie) die altijd ongecodeerd worden ontvangen. Als deze parameter aan Waar wordt geplaatst, dan is het aan programmeur om de niet gecodeerde waarde van de Meta-gegevens van de Gebruiker te verkrijgen door een decryptie van RSA uit te voeren gebruikend whitelingende privé sleutel (de zelfde privé sleutel die voor het ondertekenen van aanvrager identiteitskaart in de [`setRequestor`](#setRequestor) vraag wordt gebruikt).

**teweeggebracht door:** [`getMetadata()`](#getMetadata)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[Terug naar Android API...](#api)


### getVersion {#getVersion}

**Beschrijving:** Deze methode kan worden gebruikt om de versie van de bibliotheek terug te winnen AccessEnabler.

| API-aanroep: get AccessEnabled-versie |
| --- |
| ```public static String getVersion()``` |


[Terug naar Android API...](#api)

</br>

## Gebeurtenissen bijhouden {#tracking}

Toegangsactivering activeert een extra callback die niet noodzakelijk met de machtigingsstromen verwant is. Het uitvoeren van de gebeurtenis-volgende callback functie genoemd *sendTrackingData ()* is facultatief, maar het laat de toepassing toe om specifieke gebeurtenissen te volgen en statistieken zoals aantal succesvolle/ontbroken authentificatie/toestemmingspogingen te compileren. Hieronder is de specificatie voor *sendTrackingData ()* callback:


### sendTrackingData {#sendTrackingData}

**Beschrijving:** Callback die door Toegangsbeheer wordt teweeggebracht die aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/de mislukking van authentificatie/vergunningsstromen signaleren. Het apparaattype, het clienttype Access Enabler en het besturingssysteem worden ook gerapporteerd door sendTrackingData().

>[!WARNING]
>
> Het apparatentype en het werkende systeem worden afgeleid door het gebruik van een openbare bibliotheek van Java ([ http://java.net/projects/user-agent-utils ](http://java.net/projects/user-agent-utils)) en het koord van de gebruikersagent. Houd er rekening mee dat deze informatie alleen wordt verstrekt als een grove manier om operationele meetgegevens in apparaatcategorieën op te delen, maar dat Adobe geen verantwoordelijkheid kan nemen voor onjuiste resultaten. Gebruik de nieuwe functionaliteit.


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

- *gebeurtenis*: de gebeurtenis die wordt gevolgd. Er zijn drie mogelijke gebeurtenistypen voor bijhouden:
   - **authenticationDetection:** om het even welke tijd een verzoek van het toestemmingstoken terugkeert (gebeurtenistype is `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** om het even welke tijd een authentificatiecontrole voorkomt (gebeurtenistype is `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** wanneer de gebruiker een MVPD in de de selectievorm van MVPD selecteert (gebeurtenistype is `EVENT_MVPD_SELECTION`)
- *gegevens*: extra gegevens die aan de gemelde gebeurtenis worden geassocieerd. Deze gegevens worden gepresenteerd in de vorm van een lijst met waarden.

Na zijn instructies om de waarden in de *gegevens* te interpreteren
array:

- Voor gebeurtenistype *`EVENT_AUTHN_DETECTION`:*
   - **0** - of het symbolische verzoek succesvol (waar/vals) was en als hierboven waar is:
   - **1** - het koord van identiteitskaart van MVPD
   - **2** - GUID (md5 hashed)
   - **3** - Symbolisch reeds in geheim voorgeheugen (waar/vals)
   - **4** - het type van Apparaat
   - **5** - Toegang toelaten cliënttype
   - **6** - Het type van werkend systeem

- Voor gebeurtenistype `EVENT_AUTHZ_DETECTION`
   - **0** - of het symbolische verzoek succesvol (waar/vals) was en als succesvol:
   - **1** - identiteitskaart van MVPD
   - **2** - GUID (md5 hashed)
   - **3** - Symbolisch reeds in geheim voorgeheugen (waar/vals)
   - **4** - Fout
   - **5** - Details
   - **6** - het type van Apparaat
   - **7** - Toegang toelaten cliënttype
   - **8** - Het type van werkend systeem

- Voor gebeurtenistype `EVENT_MVPD_SELECTION`
   - **0** - identiteitskaart van momenteel geselecteerde MVPD
   - **1** - het type van Apparaat
   - **2** - Toegang toelaten cliënttype
   - **3** - Het type van werkend systeem

**teweeggebracht door:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Terug naar Android API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
