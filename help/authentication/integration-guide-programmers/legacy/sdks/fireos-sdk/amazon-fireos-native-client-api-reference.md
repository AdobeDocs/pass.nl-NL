---
title: Native API-naslaggids voor Amazon FireOS
description: Native API-naslaggids voor Amazon FireOS
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3451'
ht-degree: 0%

---

# (Verouderd) Referentie voor Amazon FireOS Native-client-API {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

## Inleiding {#intro}

In dit document worden de methoden en callbacks beschreven die worden weergegeven door de Amazon FireOS SDK for Adobe Pass Authentication, ondersteund door Adobe Pass Authentication. De hier beschreven methodes en callback functies worden bepaald in de AccessEnabler.h en EntitlementDelegate.h kopbaldossiers.

Raadpleeg <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> voor de nieuwste Amazon FireOS AccessEnabler SDK.

>[!NOTE]
>
>Het team van de Authentificatie van Adobe Pass moedigt u aan om slechts de Authentificatie van Adobe Pass *openbare* APIs te gebruiken:

- Openbare APIs is beschikbaar *en volledig getest* op alle gesteunde cliënttypes. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(n) heeft.
- Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Nochtans, voor *niet* - openbare APIs, behouden wij het recht om hun handtekening op om het even welk toekomstig punt te veranderen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

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

**Beschrijving:** Instantieert het voorwerp van Inschakelen van de Toegang. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Beschikbaarheid:** v3.0+


**Parameters:**

- *appContext*: De toepassingscontext van Amazon Fire OS.
- softwareStatement
- redirectUrl : in het geval van FireOS wordt de parameterwaarde genegeerd en ingesteld op default : adobepass://android.app
- env_url: voor het testen met behulp van een testomgeving voor Adobe staging, kan env\_url worden ingesteld op &quot;sp.auth-staging.adobe.com&quot;

**Afgekeurd:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**Beschrijving:** vestigt de identiteit van de programmeur. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Deze instelling mag slechts eenmaal worden uitgevoerd tijdens de levenscyclus van de toepassing.

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: het milieu van de Versie van de Adobe/van de Productie.

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
- *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Daarbij bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD is gekoppeld aan de snelste serviceprovider, dat wil zeggen de provider die eerst heeft gereageerd en die die MVPD ondersteunt.

**teweeggebrachte callbacks:** `setRequestorComplete()`



**Afgekeurd:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die uw toepassing informeert dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

| Callback: configuratie aanvrager voltooid |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan één van de volgende waarden nemen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - configuratie
de fase is voltooid
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - configuratie
fase mislukt

**teweeggebracht door:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Beschrijving:** vormt globale opties van SDK. Het keurt a **Kaart \&lt;Koord, Koord \>** als argument goed. De waarden van de kaart worden samen met elke netwerkaanroep die de SDK maakt, aan de server doorgegeven.

De waarden worden doorgegeven aan de server, onafhankelijk van de huidige flow (verificatie/autorisatie). Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.



| API-aanroep: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *opties*: Een Kaart \&lt;Koord, Koord \> die globale opties van SDK bevatten. Momenteel zijn de volgende opties beschikbaar:
   - **applicationProfile** - het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   - **ap \_vi** - de Dienst van identiteitskaart van het Experience Cloud Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   - **apparaat\_info** - de Informatie van het Apparaat zoals die in **wordt beschreven het overgaan van het koekje van apparateninformatie**

</br>

### checkAuthentication {#checkAuthN}

**Beschrijving:** controleert de authentificatiestatus. Het doet dit door naar een geldig authentificatietoken in de lokale symbolische opslagruimte te zoeken. Het roepen van deze methode voert geen netwerkvraag uit. Deze wordt door de toepassing gebruikt om de verificatiestatus van de gebruiker te controleren en de gebruikersinterface dienovereenkomstig bij te werken (de gebruikersinterface voor aanmelding/aanmelding wordt dus bijgewerkt). De authentificatiestatus wordt meegedeeld aan de toepassing via [*setAuthenticationStatus ()*](#setAuthNStatus) callback.

Als een MVPD de functie &#39;Verificatie per aanvrager&#39; ondersteunt, kunnen meerdere verificatietokens op een apparaat worden opgeslagen.

| API-aanroep: verificatiestatus controleren |
| --- |
| ```public void checkAuthentication()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Beschrijving:** begint het volledige authentificatiewerkschema. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

- Als de laatste authentificatiepoging succesvol was, wordt de selectiefase van MVPD overgeslagen en een controle WebView zal de gebruiker met de MVPD login pagina voorstellen.
- Als de laatste authentificatiepoging niet succesvol was of als de gebruiker uitdrukkelijk het programma opende, [*displayProviderDialog ()*](#displayProviderDialog) callback wordt teweeggebracht. Deze callback wordt door uw toepassing gebruikt om de gebruikersinterface van de MVPD-selectie weer te geven. Ook wordt uw app vereist om de authentificatiestroom te hervatten door de bibliotheek van Inschakelen van de Toegang over de selectie van MVPD van de gebruiker via [ te informeren setSelectedProvider () ](#setSelectedProvider) methode.

Als een MVPD de functie &#39;Verificatie per aanvrager&#39; ondersteunt, kunnen meerdere verificatietokens worden opgeslagen op een apparaat (één per programmeur).

Tot slot wordt de authentificatiestatus meegedeeld aan de toepassing via *setAuthenticationStatus ()* callback.

| API-aanroep: initieert de verificatiestroom |
| --- |
| ```public void getAuthentication()``` |

**Beschikbaarheid:** v1.0+

| API-aanroep: initieert de verificatiestroom |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *forceAuthn*: Een vlag die specificeert als de authentificatiestroom zou moeten zijn begonnen, ongeacht als de gebruiker reeds voor authentiek verklaard of niet is.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Omschrijving** Callback die door Toegangsactivering wordt teweeggebracht om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om de gewenste MVPD te selecteren. De callback bevat een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de gebruikersinterface van de selectie correct samen te stellen (zoals de URL die het MVPD-logo aanwijst, een vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door *te roepen setSelectedProvider ()* en het over te gaan identiteitskaart van MVPD die aan de selectie van de gebruiker beantwoordt.


| **Callback: toon de selectie UI van MVPD** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Beschikbaarheid:** v1.0+

**Parameters**:

- *mvpds*: Lijst van de voorwerpen van MVPD die op MVPD betrekking hebbende informatie houden die de toepassing kan gebruiken om de elementen van de MVPD selectie UI te bouwen.

**teweeggebracht door:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Beschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie van MVPD van de gebruiker op de hoogte te brengen. Wanneer het overgaan van *ongeldig* als parameter, stelt de Toegang toe huidige MVPD aan een ongeldige waarde terug.

| **API vraag: plaats de momenteel geselecteerde leverancier** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Beschikbaarheid:**v 1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Beschrijving:** Callback die door Toegelaten Toegang op Android SDK wordt teweeggebracht. Deze moet worden genegeerd op Amazon FireOS SDK.

| **Callback: login van de vertoning MVPD pagina** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *url*: URL die aan de MVPD login pagina richt

**teweeggebracht door:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Beschrijving:** voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken.

| **API vraag: wik het authentificatietoken** terug |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *koekjes*: De koekjes die op het doeldomein (zie de demotoepassing in SDK voor een verwijzingsimplementatie) worden geplaatst.

**teweeggebrachte callbacks:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de toepassing van het statuut van de authentificatie informeert. Er zijn vele plaatsen waar de authentificatiestroom kan ontbreken, of als resultaat van gebruikersinteractie of wegens andere onvoorziene scenario&#39;s (d.w.z. de problemen van de netwerkconnectiviteit, enz.). Deze callback informeert de toepassing van de succes/mislukkingsstatus van de authentificatie, terwijl ook het verstrekken van extra informatie over de mislukkingsreden, wanneer nodig.

Deze callback signaleert ook wanneer de logout stroom volledig is.

| **Callback: rapport het statuut van de authentificatiestroom** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *status*: Kan één van de volgende waarden nemen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - logout
- *code*: Reden voor voorgestelde status. Als *status* `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` is, dan *code* een leeg koord (d.w.z., dat door de `AccessEnabler.USER_AUTHENTICATED` constante wordt bepaald) is. Als deze parameter niet wordt geverifieerd, kan deze een van de volgende waarden hebben:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. In antwoord op *checkAuthentication ()* methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler heeft de authentificatiestatus-machine na de upper-layer toepassing teruggesteld *ongeldig* tot `setSelectedProvider()` wordt overgegaan om de authentificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals netwerkonbeschikbaarheid of omdat de gebruiker de verificatiestroom expliciet heeft geannuleerd.
   - `AccessEnabler.LOGOUT` - De gebruiker is niet geautoriseerd vanwege een afmeldingsactie.

**teweeggebracht door:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorisedResources {#checkPreauth}

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om te bepalen als de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen).

| **API vraag: plaats de momenteel geselecteerde leverancier** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Beschikbaarheid:** v1.0+

**&lt;Parameters:** de `resources` parameter is een serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization()` . Dit houdt in dat er een waarde moet worden overeengekomen tussen de programmeur en de MVPD of een RSS-fragment voor media.

**callback teweeggebracht:** `preauthorizedResources()`

</br>

### preauthorisedResources {#preauthResources}

**Beschrijving:** Callback die door checkPreauthorisedResources () wordt teweeggebracht. Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

| **API vraag: plaats de momenteel geselecteerde leverancier** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Beschikbaarheid:**v 1.0+

**Parameters:** de `resources` parameter is een serie van middelen waarvoor de gebruiker reeds gemachtigd is om te bekijken.

**teweeggebracht door:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstatus te controleren. Het begint door de authentificatiestatus eerst te controleren. Als niet voor authentiek verklaard, wordt *setTokenRequestFailed ()* callback teweeggebracht, en de methode weggaat. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie details op *getAuthorization ()* methode.

| **API vraag: de status van de controlevergunning** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Beschikbaarheid:** v1.0+

| **API vraag: de status van de controlevergunning** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: Identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstroom in werking te stellen. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat Access Enabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het korte-levende media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. *setToken ()* callback wordt teweeggebracht en het korte media teken wordt geleverd als parameter aan de toepassing. Als om het even welke reden, de vergunning ontbreekt, *tokenRequestFailed ()* callback wordt teweeggebracht en de foutencode en de details worden verstrekt.

| **API vraag: stel de vergunningsstroom** in werking |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Beschikbaarheid:** v1.0+

| **API vraag: stel de vergunningsstroom** in werking |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: Identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
- *gegevens*: Een Kaart die uit sleutel-waarde paren bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit mogelijk te maken zonder de SDK te wijzigen.

**teweeggebrachte callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Extra callbacks teweeggebrachte** <br> Deze methode kan de volgende callbacks (als de authentificatiestroom ook) teweegbrengen: _setAuthenticationStatus ()_, _displayProviderDialog ()_ |

**NOTA: Gelieve te gebruiken checkAuthorization () in plaats van getAuthorization () wanneer mogelijk. De getAuthorization () methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit tot een ingewikkelde implementatie aan de kant van de Programmer zou kunnen leiden.**

</br>

### setToken {#setToken}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die uw toepassing informeert dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.

| **Callback: met succes voltooide vergunningsstroom** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Beschikbaarheid:**v 1.0+

**Parameters:**

- *teken*: Het korte-levende media teken
- *resourceId*: Het middel waarvoor de vergunning werd verkregen

**teweeggebracht door:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

| **Callback: ontbroken vergunningsstroom** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *resourceId*: Het middel waarvoor de vergunning werd verkregen
- *errorCode*: De code van de fout verbonden aan het mislukkingsscenario. Mogelijke waarden:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - De gebruiker kan geen autorisatie voor de opgegeven resource uitvoeren
- *errorDescription*: De extra details over het mislukkingsscenario. Als dit beschrijvende koord om het even welke reden niet beschikbaar is, verzendt de Authentificatie van Adobe Pass een leeg koord > **(&quot;&quot;&quot;)**.  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopberichten door te geven. Als een abonnee bijvoorbeeld geen toestemming voor een resource krijgt, kan de MVPD een bericht verzenden zoals: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Klik hier als u het pakket wilt bijwerken.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier.&quot;

**teweeggebracht door:** `checkAuthorization(), getAuthorization()`

</br>

### afmelden {#logout}

**Beschrijving:** gebruik deze methode om de logout stroom in werking te stellen. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd.

| **API vraag: stel de logout stroom** in werking |
| --- |
| ```public void logout()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** niets

</br>

### getSelectedProvider {#getSelectedProvider}

**Beschrijving:** gebruik deze methode om de momenteel geselecteerde leverancier te bepalen.

| **API vraag: bepaal momenteel geselecteerde MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Beschikbaarheid:** v1.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Beschrijving:** Callback die door Toegangsactivering wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

| **Callback: informatie over momenteel geselecteerde MVPD** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *mvpd*: Voorwerp dat informatie over momenteel geselecteerde MVPD bevat

**teweeggebracht door:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Beschrijving:** gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek van Inschakelen van de Toegang wordt blootgesteld. De toepassing heeft toegang tot deze informatie door een samengesteld object MetadataKey op te geven.

| **API vraag: vraag AccessEnabled voor meta-gegevens** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Beschikbaarheid:** v1.0+

Er zijn twee soorten meta-gegevens beschikbaar aan Programmeurs:

- Statische metagegevens (verificatietoken TTL, machtigingstoken-TTL en apparaat-id)
- Metagegevens van gebruikers (gebruikersspecifieke informatie, zoals gebruikersnaam en postcode; doorgegeven van een MVPD naar het apparaat van een gebruiker tijdens de verificatie- en/of autorisatiestromen)

**Parameters:**

- *metadataKey*: Een gegevensstructuur die een sleutel en args variabele inkapselt, met de volgende betekenis:
   - Als de sleutel `METADATA_KEY_TTL_AUTHN` is, wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.
   - Als de sleutel `METADATA_KEY_TTL_AUTHZ` is en args een SerializableNameValuePair voorwerp met naam = `METADATA_ARG_RESOURCE_ID` en waarde = `[resource_id]` bevat, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan de gespecificeerde middel.
   - Als de toets `METADATA_KEY_DEVICE_ID` is, wordt de query uitgevoerd om de huidige apparaat-id te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.
   - Als de sleutel `METADATA_KEY_USER_META` is en args een SerializableNameValuePair voorwerp met naam = `METADATA_KEY_USER_META` en waarde = `[metadata_name]` bevat, dan wordt de vraag gemaakt voor gebruikersmeta-gegevens. De huidige lijst met beschikbare metagegevenstypen voor gebruikers:
      - `zip` - Postcode
      - `householdID` - Huishoudelijke id. Als een MVPD geen subaccounts ondersteunt, is dit hetzelfde als `userID` .
      - `maxRating` - Maximale ouderlijke classificatie voor de gebruiker
      - `userID` - De gebruikers-id. Als een MVPD subaccounts ondersteunt en de gebruiker niet het hoofdaccount is,
      - `channelID` - Een lijst met kanalen die de gebruiker mag bekijken

De werkelijke gebruikersmetagegevens die beschikbaar zijn voor een programmeur, zijn afhankelijk van wat een MVPD beschikbaar stelt.  Deze lijst wordt verder uitgebreid naarmate nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**teweeggebrachte callbacks:** [`setMetadataStatus()`](#setMetadaStatus)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Beschrijving:** Callback die door Toegangsmanager wordt teweeggebracht die de meta-gegevens levert die via a *worden gevraagd getMetadata ()* vraag.

| **Callback: resultaat van het verzoek van de meta-gegevensherwinning** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Beschikbaarheid:** v1.0+

**Parameters:**

- *sleutel*: Het voorwerp MetadataKey dat de sleutel bevat waarvoor de meta-gegevenswaarde en bijbehorende parameters wordt gevraagd (zie duotoepassing voor een verwijzingsimplementatie).
- *resultaat*: Een samengesteld voorwerp dat de gevraagde meta-gegevens bevat. Het object heeft de volgende velden:
   - *simpleResult*: een Koord dat de meta-gegevenswaarde vertegenwoordigt toen het verzoek voor Authentificatie TTL, Vergunning TTL, of identiteitskaart van het Apparaat werd gemaakt. Deze waarde is null als de aanvraag is ingediend voor metagegevens van gebruiker.

   - *userMetadataResult*: een Voorwerp dat de vertegenwoordiging Java van een nuttige lading van de Metagegevens van de Gebruiker JSON bevat. Bijvoorbeeld:

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

**Meer Informatie:** [ Metagegevens van de Gebruiker ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

</br>

## getVersion {#getVersion}

**Beschrijving:** gebruik deze methode om de huidige versie van AccessEnabler terug te winnen

| **API vraag: get versie AccessEnabler** |
| --- |
| ```public static String getVersion()``` |

## Gebeurtenissen bijhouden {#tracking}

Toegangsactivering activeert een extra callback die niet noodzakelijk met de machtigingsstromen verwant is. Het uitvoeren van de gebeurtenis-volgende callback functie genoemd *sendTrackingData ()* is facultatief, maar het laat de toepassing toe om specifieke gebeurtenissen te volgen en statistieken zoals aantal succesvolle/ontbroken authentificatie/toestemmingspogingen te compileren. Hieronder is de specificatie voor *sendTrackingData ()* callback:

### sendTrackingData {#sendTrackingData}

**Beschrijving:** Callback die door Toegangsbeheer wordt teweeggebracht die aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/de mislukking van authentificatie/vergunningsstromen signaleren. Het apparaattype, het clienttype Access Enabler en het besturingssysteem worden ook gerapporteerd door sendTrackingData().

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

- *gebeurtenis*: de gebeurtenis die wordt gevolgd. Er zijn drie mogelijke gebeurtenistypen voor bijhouden:
   - **authenticationDetection:** om het even welke tijd een verzoek van het toestemmingstoken terugkeert (gebeurtenistype is `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** om het even welke tijd een authentificatiecontrole voorkomt (gebeurtenistype is `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** wanneer de gebruiker een MVPD in de de selectievorm van MVPD selecteert (gebeurtenistype is `EVENT_MVPD_SELECTION`)
- *gegevens*: extra gegevens die aan de gemelde gebeurtenis worden geassocieerd. Deze gegevens worden gepresenteerd in de vorm van een lijst met waarden.

Na zijn instructies voor het interpreteren van de waarden in de *gegevens* serie:

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

**teweeggebracht door:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
