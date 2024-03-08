---
title: iOS/tvOS API-naslaggids
description: iOS/tvOS API-naslaggids
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# iOS/tvOS SDK API-naslaggids {#iostvos-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Op deze pagina worden de methoden en callback-functies beschreven die door de native iOS/tvOS-client voor Adobe Pass-verificatie worden aangeboden. De hier beschreven methoden en callback-functies worden gedefinieerd in de `AccessEnabler.h` en `EntitlementDelegate.h` headerbestanden; u vindt ze hier in de iOS AccessEnabler SDK: `[SDK directory]/AccessEnabler/headers/api/`


Bijbehorende documentatie:

* Voor een beschrijving van de standaard Adobe Pass Authentication Entitlement flow raadpleegt u [Entitlement Flow](/help/authentication/entitlement-flow.md).
* Voor een geleidelijke analyse van hoe te om de de authentificatierechtenstroom van Adobe Pass uit te voeren gebruikend dit API, zie [iOS Integration Cookbook](/help/authentication/iostvos-sdk-cookbook.md).
* Voor de nieuwste iOS AccessEnabler SDK raadpleegt u [iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe moedigt u aan alleen de Adobe Pass-verificatie te gebruiken *publiek* API&#39;s:
>
>* Openbare API&#39;s zijn beschikbaar en volledig getest op alle ondersteunde clienttypen. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(n) heeft.
>* Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Voor niet-openbare API&#39;s behouden we ons echter het recht voor om hun handtekening op elk toekomstig moment te wijzigen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

</br>

## API-naslag {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instantieert het AccessEnabler-object.

* **[VEROUDERD]** [init](#init) - Instantieert het AccessEnabler-object.

* [`setOptions:options:`](#setOptions) - Hiermee configureert u algemene SDK-opties, zoals profiel of bezoeker-id.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - De identiteit van de programmeur wordt vastgesteld.

* **[VEROUDERD]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - De identiteit van de programmeur wordt vastgesteld.

* **[VEROUDERD]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Hiermee wordt de identiteit van de programmeur vastgesteld.

* [`setRequestorComplete:`](#setReqComplete) - Meldt aan uw toepassing dat de configuratiefase volledig is.

* [`checkAuthentication`](#checkAuthN) - Controleert de verificatiestatus van de huidige gebruiker.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Start de volledige verificatieworkflow.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) - Start de volledige verificatieworkflow.

* [`displayProviderDialog:`](#dispProvDialog) - Informeert uw toepassing om de aangewezen elementen UI voor de gebruiker te concretiseren om een MVPD te selecteren.

* [`setSelectedProvider:`](#setSelProv) - Informeert AccessEnabler over de MVPD-selectie van de gebruiker.

* [`navigateToUrl:`](#nav2url) - Meldt dat de gebruiker de MVPD-aanmeldingspagina moet zien.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Meldt uw toepassing dat de gebruiker van de MVPD login pagina, gebruikend SFSafariViewController moet worden voorgesteld

* [`handleExternalURL:url`](#handleExternalURL) - Voltooit de authentificatie/logout stroom.

* **[VEROUDERD]** [`getAuthenticationToken`](#getAuthNToken) - Verzoekt het authentificatietoken van de achterste deelserver.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informeert uw toepassing over de status van de verificatiestroom.

* [`checkPreauthorizedResources:`](#checkPreauth) - Hiermee wordt bepaald of de gebruiker al gemachtigd is om specifieke beveiligde bronnen weer te geven.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Hiermee wordt bepaald of de gebruiker al gemachtigd is om specifieke beveiligde bronnen weer te geven.

* [`preauthorizedResources:`](#preauthResources) - Biedt een lijst met bronnen die de gebruiker al mag bekijken.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Controleert de status van de huidige gebruiker.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Hiermee wordt de autorisatiestroom gestart.

* [`setToken:forResource:`](#setToken) - Meldt dat de autorisatiestroom is voltooid.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Meldt dat de autorisatiestroom is mislukt.

* [`logout`](#logout) - Hiermee wordt de afmeldingsstroom gestart.

* [`getSelectedProvider`](#getSelProv) - Hiermee bepaalt u de momenteel geselecteerde provider.

* [`selectedProvider:`](#selProv) - Hiermee wordt informatie over de momenteel geselecteerde MVPD aan uw toepassing geleverd.

* [`getMetadata:`](#getMeta) - Hiermee wordt informatie opgehaald die door de AccessEnabler-bibliotheek als metagegevens wordt weergegeven.

* [`presentTvProviderDialog:`](#presentTvDialog) - Meldt dat uw toepassing het Apple SSO-dialoogvenster weergeeft.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Meldt dat uw toepassing het Apple SSO-dialoogvenster verbergt.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - levert de door een [`getMetadata:`](#getMeta) vraag.

* [`sendTrackingData:forEventType:`](#sendTracking) - Informatie over het bijhouden van gegevens.

* [`MVPD`](#mvpd) - De klasse MVPD. [Bevat informatie over MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Instantieert het AccessEnabler-object. Er zou één enkele instantie AccessEnabler per toepassingsinstantie moeten zijn.

| **API-aanroep: Constructor iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Beschikbaarheid:** v3.0+

**Parameters:**

* **softwareStatement:** Een tekenreeks die de toepassing identificeert in het systeem van de Adobe. Ontdek hoe u een Software Statement kunt verkrijgen.

[Terug naar boven...](#apis)



### init - [VEROUDERD]{#init}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Instantieert het AccessEnabler-object. Er zou één enkele instantie AccessEnabler per toepassingsinstantie moeten zijn.

| API-aanroep: Constructor iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Beschikbaarheid:** v1.0+ **Tot:** v3.0

**Parameters:** Geen

[Terug naar boven...](#apis)

</br>

### setOptions:opties {#setOptions}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Vormt globale opties van SDK. Het keurt een NSDictionary als argument goed. De waarden van het woordenboek worden samen met elke netwerkaanroep die de SDK maakt, aan de server doorgegeven.

**Opmerking:** De waarden worden doorgegeven aan de server, onafhankelijk van de huidige flow (verificatie/autorisatie). Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.

| API-aanroep: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Beschikbaarheid:** v2.3.0+

**Parameters:**

* *opties*: Een NSDictionary met globale SDK-opties. Momenteel zijn de volgende opties beschikbaar:
   * **applicationProfile** - Het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   * **bezoekerID** - De Experience Cloud ID Service. Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   * **handleSVC** - Booleaanse waarde die aangeeft of de programmeur de SFSafariViewControllers zal afhandelen. Zie [Ondersteuning voor SFSafariViewController op iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) voor meer informatie .
      * Indien ingesteld op **false,** de SDK zal de eindgebruiker automatisch een SFSafariViewController voorleggen. De SDK navigeert verder naar de URL van de MVPD-aanmeldingspagina.
      * Indien ingesteld op **true,** de SDK **NOT** biedt de eindgebruiker automatisch een SFSafariViewController. De SDK wordt verder geactiveerd **navigate(toUrl:{url}, useSVC:YES)**.
* **apparaat\_info** - Clientgegevens zoals beschreven in [Clientgegevens doorgeven](/help/authentication/passing-client-information-device-connection-and-application.md).

[Terug naar boven...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Hiermee wordt de identiteit van de programmeur vastgesteld. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens, kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, setRequestor kan opnieuw worden geroepen wanneer de toepassing in de voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre token als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de `setRequestorComplete:` callback.

Als de `urls` parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: de Adobe RELEASE/productiemilieu.


Als een waarde is opgegeven voor de `urls` parameter, richt de resulterende netwerkvraag alle URLs die in `urls` parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Beschikbaarheid:** v3.0+

**Parameters:**

* *aanvragerID*: De unieke id die aan de programmeur is gekoppeld. Geef de unieke id die door Adobe aan uw site is toegewezen door wanneer u zich voor het eerst aanmeldt bij de Adobe Pass-verificatieservice.
* *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

>[!NOTE]
>
>Indien opgeroepen zonder de `serviceProviders` parameter, zal de bibliotheek de configuratie van de standaarddienstverlener terugwinnen (namelijk `https://sp.auth.adobe.com` voor het productieprofiel of `https://sp.auth-staging.adobe.com` voor het staging-profiel). Als de `serviceProviders` opgegeven, moet dit een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (namelijk de server met de kortste reactietijd neemt belangrijkheid).

**Callbacks geactiveerd:** [`setRequestorComplete:`](#setReqComplete)

[Terug naar boven...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [VEROUDERD] {#setReq}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Hiermee wordt de identiteit van de programmeur vastgesteld. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de `setRequestorComplete:` callback.

Als de `urls` parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: de Adobe RELEASE/productiemilieu.

Als een waarde is opgegeven voor de `urls` parameter, richt de resulterende netwerkvraag alle URLs die in `urls` parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Beschikbaarheid:** v1.0+ **Tot:** v3.0

| API-aanroep: configuratie aanvrager |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Beschikbaarheid:** v1.0+ **Tot:** v3.0

**Parameters:**

* *aanvragerID*: De unieke id die aan de programmeur is gekoppeld. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
* *signedRequestorID*: **Deze parameter bestaat in iOS AccessEnabler versie 1.2 en hoger.** Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

**Opmerkingen:** Indien opgeroepen zonder de `serviceProviders` parameter, zal de bibliotheek de configuratie van de standaarddienstverlener terugwinnen (namelijk`https://sp.auth.adobe.com` voor het productieprofiel of `https://sp.auth-staging.adobe.com` voor het staging-profiel). Als de `serviceProviders` opgegeven, moet dit een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (d.w.z., de server met de kortste reactietijd neemt belangrijkheid).

**Callbacks geactiveerd:** [`setRequestorComplete:`](#setReqComplete)


[Terug naar boven...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [VEROUDERD] {#setReq_tvos}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Hiermee wordt de identiteit van de programmeur vastgesteld. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Deze instelling mag slechts eenmaal worden uitgevoerd tijdens de levenscyclus van de toepassing.

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de `setRequestorComplete:` callback.

Als de `urls` parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: de Adobe RELEASE/productiemilieu.

Als een waarde is opgegeven voor de `urls` parameter, richt de resulterende netwerkvraag alle URLs die in `urls` parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: configuratie aanvrager</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Beschikbaarheid:** v2.0+ **Tot:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: configuratie aanvrager</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v2.0+ **Tot:** v3.0

**Parameters:**

* *aanvragerID*: De unieke id die aan de programmeur is gekoppeld. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
* *signedRequestorID*: **Deze parameter bestaat in iOS AccessEnabler versie 1.2 en hoger.** Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.
* geheim en publicKey: De geheime en openbare sleutel die wordt gebruikt om de tweede het schermvraag te ondertekenen. Zie voor meer informatie de [Clientloze documentatie](#create_dev).

Indien opgeroepen zonder de `serviceProviders` parameter, zal de bibliotheek de configuratie van de standaarddienstverlener terugwinnen (d.w.z., `https://sp.auth.adobe.com` voor het productieprofiel of https://sp.auth-staging.adobe.com voor het staging-profiel). Als de `serviceProviders` opgegeven, moet dit een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (d.w.z., de server met de kortste reactietijd neemt belangrijkheid).

**Callbacks geactiveerd:** [`setRequestorComplete:`](#setReqComplete)

[Terug naar boven...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die uw toepassing meedeelt dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: configuratie aanvrager voltooid</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Beschikbaarheid:** v1.0+

**Parameters**:

* *status*: kan een van de volgende waarden hebben:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
   * `ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt

**geactiveerd door:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Terug naar boven...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Controleert de authentificatiestatus van de huidige gebruiker.
Het doet dit door naar een geldig authentificatietoken in de lokale symbolische opslagruimte te zoeken. Deze methode voert geen netwerkvraag uit en wij adviseren roepend het op de belangrijkste draad.
Deze wordt door de toepassing gebruikt om de verificatiestatus van de gebruiker op te vragen en de gebruikersinterface dienovereenkomstig bij te werken (de gebruikersinterface voor aanmelding/aanmelding wordt dus bijgewerkt). De verificatiestatus wordt via de [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: verificatiestatus controleren</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Terug naar boven...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Start de volledige verificatieworkflow. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

* als de laatste verificatiepoging succesvol was, wordt de MVPD-selectiefase overgeslagen en wordt de [`navigateToUrl:`](#nav2url) callback wordt geactiveerd. De toepassing gebruikt deze callback om de controle te concretiseren WebView die de gebruiker met de MVPD login pagina voorstelt. **[OPMERKING: Vanaf Access Enabler 1.5 is deze functionaliteit niet beschikbaar vanwege een beperking in de SDK].**
* als de laatste verificatiepoging is mislukt of als de gebruiker zich expliciet heeft afgemeld, wordt [`displayProviderDialog:`](#dispProvDialog) callback wordt geactiveerd. Uw toepassing gebruikt deze callback om de MVPD selectie UI te tonen. Uw app is ook vereist om de verificatiestroom te hervatten door de AccessEnabler-bibliotheek via de [`setSelectedProvider:`](#setSelProv) methode.

Aangezien de geloofsbrieven van de gebruiker op de MVPD login pagina worden geverifieerd, wordt uw toepassing vereist om de veelvoudige omleidingsverrichtingen te controleren die plaatsvinden terwijl de gebruiker bij de MVPD login pagina voor authentiek verklaart. Wanneer de correcte geloofsbrieven zijn ingegaan, wordt de controle WebView opnieuw gericht aan een douane URL die door wordt bepaald `ADOBEPASS_REDIRECT_URL` constante. Deze URL is niet bedoeld om door WebView te worden geladen. De toepassing moet deze URL onderscheppen en deze gebeurtenis interpreteren als een signaal dat de aanmeldingsfase is voltooid. Het zou controle aan AccessEnabler dan moeten overdragen om de authentificatiestroom te voltooien (door [handleExternalURL](#handleExternalURL) methode).

Ten slotte wordt de authenticatiestatus via de [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Beschikbaarheid:** v1.9+

**Parameters:**

* *forceAuthn*: Een markering die aangeeft of de verificatiestroom moet worden gestart, ongeacht of de gebruiker al dan niet is geverifieerd.
* *data*: Een woordenboek dat bestaat uit sleutelwaardeparen die naar de betaaltelevisiebetaalservice moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Terug naar boven...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Start de volledige verificatieworkflow. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

* [presentTVProviderDialog()](#presentTvDialog) zal worden geroepen als de huidige aanvrager minstens één MVPD heeft die SSO steunt. Als geen MVPD SSO steunt, zal de klassieke authentificatiestroom beginnen en de filterparameter wordt genegeerd.
* Nadat de gebruiker de Apple SSO-stroom heeft voltooid [`dismissTvProviderDialog()`](#dismissTvDialog) wordt geactiveerd en het verificatieproces wordt voltooid.

Ten slotte wordt de authenticatiestatus via de [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.

**Beschikbaarheid:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**Parameters:**

* *forceAuthn*: Een markering die aangeeft of de verificatiestroom moet worden gestart, ongeacht of de gebruiker al dan niet is geverifieerd.
* *data*: Een woordenboek dat bestaat uit sleutelwaardeparen die naar de betaaltelevisiebetaalservice moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.
* filter: een woordenboek met twee lijsten met MVPD-id&#39;s die moeten worden weergegeven in het dialoogvenster Apple SSO. Om het even welke MVPD die SSO niet steunt zal worden genegeerd maar de orde zal worden geëerbiedigd. Het woordenboek moet twee toetsen hebben:
   * TV\_PROVIDERS: Een lijst met alle MVPDs die in de plukker zou moeten verschijnen
   * FEATURED\_TV\_PROVIDERS: Een lijst met alle MVPDs die zou moeten worden gemerkt zoals gespiegeld in de plukker. MVPD&#39;s in deze lijst moeten ook worden opgegeven in de lijst Tv\_PROVIDERS.

**Beschikbaarheid:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: initieert de verificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parameters:**

* *forceAuthn*: Een markering die aangeeft of de verificatiestroom moet worden gestart, ongeacht of de gebruiker al dan niet is geverifieerd.
* *data*: Een woordenboek dat bestaat uit sleutelwaardeparen die naar de betaaltelevisiebetaalservice moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.
* filter: Een lijst met MVPD-id&#39;s die moet worden weergegeven in het dialoogvenster Apple SSO. Om het even welke MVPD die SSO niet steunt zal worden genegeerd maar de orde zal worden geëerbiedigd.

**Callbacks geactiveerd:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Terug naar boven...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback teweeggebracht door AccessEnabler om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om gewenste MVPD te selecteren. De callback biedt een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de selectieinterface correct samen te stellen (zoals de URL die het logo van de MVPD aanwijst, de vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door te roepen `setSelectedProvider:` en geeft u de id van de MVPD door die overeenkomt met de selectie van de gebruiker.

**De verificatiestroom afbreken** - Dit is een punt waar de gebruiker de &quot;Achterknoop&quot;kan drukken, die aan het aborteren van de authentificatiestroom gelijkwaardig is. In dat scenario moet uw toepassing de [setSelectedProvider:](#setSelProv) methode, die ongeldig als parameter overgaat, om AccessEnabler de kans te geven om zijn authentificatiestatus-machine terug te stellen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: toon de selectie UI MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Beschikbaarheid:** v1.0+

**Parameters**:

* *mvpds*: lijst met MVPD-objecten met MVPD-gerelateerde informatie die de toepassing kan gebruiken om de UI-elementen voor de selectie van MVPD te maken.

**geactiveerd door:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Terug naar boven...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie MVPD van de gebruiker op de hoogte te brengen. De toepassing kan deze methode gebruiken om de dienstverlener te selecteren of te veranderen die voor authentificatie wordt gebruikt.

Als het geselecteerde MVPD een TempPass MVPD is zal het automatisch met die MVPD voor authentiek verklaren zonder het moeten getAuthentication () daarna roepen.

Houd er rekening mee dat dit niet mogelijk is voor de Tijdelijke controle voor speciale acties waarbij extra parameters worden gegeven aan de methode getAuthentication().

Bij het passeren *null* als parameter, veronderstelt de Toegang Enabler dat de gebruiker de authentificatiestroom (d.w.z. op de &quot;Achterknoop&quot;gedrukt) heeft geannuleerd, en antwoordt door de authentificatiestatus-machine opnieuw in te stellen en door de [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback met de `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` foutcode.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: stel de momenteel geselecteerde provider in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`,  [`navigateToUrl:`](#nav2url)

[Terug naar boven...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving:** Callback die door AccessEnabler wordt teweeggebracht om uw toepassing te verzoeken om een controlemechanisme UIWebView/WKWebView te concretiseren en URL te laden die in callback wordt verstrekt **`url`** parameter. De callback gaat de **`url`** parameter die URL van het authentificatieeindpunt of URL van het logout eindpunt vertegenwoordigt.

Als UIWebView/WKWebView` `de controller doorloopt verschillende omleidingen, moet uw toepassing de activiteit van de controller controleren en detecteren op welk moment een specifieke aangepaste URL wordt geladen die door de `ADOBEPASS_REDIRECT_URL `constante (d.w.z. `adobepass://ios.app`). Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet slechts door uw toepassing als signaal worden geïnterpreteerd dat de authentificatie of logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer de controller deze specifieke aangepaste URL laadt, moet uw toepassing UIWebView/WKWebView sluiten en AccessEnabler oproepen `handleExternalURL:url `API-methode.

**Opmerking:** Houd er rekening mee dat in het geval van de verificatiestroom dit een punt is waarop de gebruiker op de knop Terug kan drukken. Dit is gelijk aan het afbreken van de verificatiestroom. In een dergelijk scenario moet uw toepassing de [setSelectedProvider:](#setSelProv) methode doorgeven **`nil`** als parameter en het geven van een kans aan AccessEnabler om zijn authentificatiestatus-machine terug te stellen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: MVPD-aanmeldingspagina weergeven</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters**:

* *url*: de URL die naar de aanmeldingspagina van de MVPD verwijst

**geactiveerd door:** [setSelectedProvider:](#setSelProv)



[Terug naar boven...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving:** Callback die door AccessEnabler in plaats van wordt teweeggebracht `navigateToUrl:` callback voor het geval dat uw toepassing eerder handmatige behandeling van het Controlemechanisme van de Mening Safari (SVC) via [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) vraag, en slechts in het geval van MVPDs die Controlemechanisme van de Mening Safari (SVC) vereisen. Voor alle andere MVPDs `navigateToUrl:` callback wordt aangeroepen. Zie[Ondersteuning voor SFSafariViewController op iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) voor details op hoe het Controlemechanisme van de Mening Safari (SVC) zou moeten worden beheerd.

Vergelijkbaar met de `navigateToUrl:` callback `navigateToUrl:useSVC:` wordt teweeggebracht door AccessEnabler om uw toepassing te verzoeken om een `SFSafariViewController` en om URL te laden die in callback wordt verstrekt **`url`** parameter. De callback gaat de **`url`** parameter die URL van het authentificatieeindpunt of URL van het logout eindpunt vertegenwoordigt, en **`useSVC`** parameter die aangeeft dat de toepassing een `SFSafariViewController`.

Als de `SFSafariViewController` de controller doorloopt verschillende omleidingen, moet uw toepassing de activiteit van de controller controleren en detecteren op welk moment een specifieke aangepaste URL wordt geladen die door uw `application's custom scheme` (bv.****`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet slechts door uw toepassing als signaal worden geïnterpreteerd dat de authentificatie of logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer de controller deze specifieke aangepaste URL laadt, moet de toepassing het dialoogvenster `SFSafariViewController` en roepen AccessEnabler `handleExternalURL:url `API-methode.

**Opmerking:** Houd er rekening mee dat in het geval van de verificatiestroom dit een punt is waarop de gebruiker op de knop Terug kan drukken. Dit is gelijk aan het afbreken van de verificatiestroom. In een dergelijk scenario moet uw toepassing de [setSelectedProvider:](#setSelProv) methode doorgeven **`nil`** als parameter en het geven van een kans aan AccessEnabler om zijn authentificatiestatus-machine terug te stellen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: MVPD-aanmeldingspagina weergeven in SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:**v 3.2+

**Parameters**:

* *URL:* de URL die naar de aanmeldingspagina van de MVPD verwijst
* *useSVC:* of de url in SFSafariViewController zou moeten worden geladen.

**geactiveerd door:**[ setOptions:](#setOptions) voor [setSelectedProvider:](#setSelProv)

[Terug naar boven...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt aangeroepen door uw toepassing om de verificatie- of afmeldingsstroom te voltooien. Deze methode moet worden aangeroepen direct nadat de toepassing het moment detecteert waarop de `UIWebView/WKWebView or SFSafariViewController` controller wordt omgeleid naar een specifieke aangepaste URL. Als uw toepassing een `SFSafariViewController `de specifieke aangepaste URL wordt bepaald door uw `application's custom scheme` (bijvoorbeeld`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke aangepaste URL gedefinieerd door de `ADOBEPASS_REDIRECT_URL `constante (d.w.z. `adobepass://ios.app`).

In het geval van de authentificatiestroom voltooit AccessEnabler de stroom door het authentificatietoken van de achterste deelserver terug te winnen en het lokaal in de symbolische opslag op te slaan. AccessEnabler zal uw toepassing informeren dat de authentificatiestroom door te roepen volledig is `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> callback met statuscode 1, die op succes wijst. Als er een fout optreedt tijdens het uitvoeren van deze stappen, `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> callback wordt teweeggebracht met een statuscode van 0, die op authentificatiemislukking, evenals een overeenkomstige foutencode wijst.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: de verificatie- of uitlogingsstroom voltooien</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v3.0+

**Parameters:**

* *url*: De onderschepte URL van de ` UIWebView/WKWebView or SFSafariViewController ` besturingselement als tekenreeks.


**Callbacks geactiveerd:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Terug naar boven...](#apis)

</br>

#### getAuthenticationToken - [VEROUDERD] {#getAuthNToken}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken. Deze methode zou door uw toepassing slechts in antwoord op gebeurtenis moeten worden geroepen waar de controle WebView die de MVPD login pagina ontvangen opnieuw aan douane URL wordt gericht die door wordt bepaald `ADOBEPASS_REDIRECT_URL` constante.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: het verificatietoken ophalen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+ **Tot:** v3.0

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Terug naar boven...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die de toepassing van de status van de authentificatiestroom informeert. Er zijn vele plaatsen waar deze stroom kan ontbreken, of als gevolg van gebruikersinteractie of wegens andere onvoorziene scenario&#39;s (d.w.z., netwerkconnectiviteitsproblemen, enz.). Deze callback informeert de toepassing van de succes/mislukkingsstatus van de authentificatiestroom, terwijl ook het verstrekken van extra informatie over de mislukkingsreden, wanneer nodig.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: rapport de status van de authentificatiestroom</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Beschikbaarheid:** v1.0+

**Parameters**:

* *status*: kan een van de volgende waarden hebben:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   * `ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
* *code*: oorzaak van fout. Indien *status* is `ACCESS_ENABLER_STATUS_SUCCESS`vervolgens *code* is een lege tekenreeks (gedefinieerd door de `USER_AUTHENTICATED` constante). In het geval van een fout kan deze parameter een van de volgende waarden hebben:
   * `USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. Als reactie op de [checkAuthentication:](#checkAuthN) methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler heeft de authentificatiestatus-machine opnieuw ingesteld nadat de hogere laagtoepassing overgegaan *null* tot [`setSelectedProvider:`](#setSelProv) om de verificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals niet-beschikbaarheid van het netwerk of de gebruiker heeft de verificatiestroom expliciet geannuleerd.

**geactiveerd door:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Terug naar boven...](#apis)

</br>

### checkPreauthorisedResources: {#checkPreauth}


**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt door de toepassing gebruikt om te bepalen of de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is om informatie voor gebruik in het versieren van UI terug te winnen **(bijvoorbeeld Toegangsstatus met vergrendelings- en ontgrendelpictogrammen).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: stel de momenteel geselecteerde provider in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.3+

**Parameters:**

* *bronnen:* array van middelen waarvoor de autorisatie moet worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. De middel identiteitskaart is onderworpen aan de zelfde beperkingen zoals middelidentiteitskaart in de vraag, namelijk zou het een overeengekomen waarde moeten zijn die tussen Programmer en MVPD of een mediaRSS fragment wordt gevestigd.

**Callback geactiveerd:** [`preauthorizedResources:`](#preauthResources)

[Terug naar boven...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt door de toepassing gebruikt om te bepalen of de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen). De **cachegeheugen** parameter controleert of het interne geheime voorgeheugen voor het oplossen van middelen wordt gebruikt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: stel de momenteel geselecteerde provider in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Beschikbaarheid:** v3.1+



**Parameters:**

* *bronnen:* array van middelen waarvoor de autorisatie moet worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization:` de vraag, d.w.z., zou het een overeengekomen waarde moeten zijn die tussen de programmeur en MVPD of een mediaRSS fragment wordt gevestigd.
* *cache:* Boolean die opgeeft of de interne cache moet worden gebruikt voor het oplossen van bronnen. Als de waarde false is, wordt de cache overgeslagen, wat leidt tot serveraanroepen telkens wanneer deze API wordt aangeroepen.

**Callback geactiveerd:** [`preauthorizedResources:`](#preauthResources)

[Terug naar boven...](#apis)

</br>

### preauthorisedResources: {#preauthResources}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving:** Callback geactiveerd door `checkPreauthorizedResources:`. Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: stel de momenteel geselecteerde provider in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.3+

**Parameters:**

* `resources`: array met bronnen waarvoor de gebruiker al gemachtigd is om deze te bekijken.

**geactiveerd door:** [`checkPreauthorizedResources:`](#checkPreauth)



[Terug naar boven...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de status van de vergunning te controleren. Het begint door de authentificatiestatus eerst te controleren. Indien niet geverifieerd, wordt de [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) callback wordt geactiveerd en de methode wordt afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie de details over de [`getAuthorization:`](#getAuthZ) methode.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: autorisatiestatus controleren</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: autorisatiestatus controleren</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.9+

**Parameters:**

* *resource*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
* *data*: Een woordenboek dat bestaat uit sleutelwaardeparen die naar de betaaltelevisiebetaalservice moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Terug naar boven...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt gebruikt door de aanvraag om de vergunningsstroom in gang te zetten. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat AccessEnabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het kortstondige media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. De [`setToken:forResource:`](#setToken) callback wordt geactiveerd en het korte media-token wordt als een parameter aan de toepassing geleverd. Indien de vergunning om welke reden dan ook mislukt, [`tokenRequestFailed:forEventType:`](#tokenReqFailed) callback wordt geactiveerd en de foutcode/details worden opgegeven.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: de machtigingsstroom starten</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: de machtigingsstroom starten</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Beschikbaarheid:** v1.9+

**Parameters:**

* *resource*: De id van de bron waarvoor de gebruiker toestemming aanvraagt.
* *data*: Een woordenboek dat bestaat uit sleutelwaardeparen die naar de betaaltelevisiebetaalservice moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**Callbacks geactiveerd:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Extra callbacks geactiveerd:**\
Deze methode kan de volgende callbacks (als de authentificatiestroom ook in werking wordt gesteld) ook teweegbrengen: `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Gebruik `checkAuthorization:` / `checkAuthorization:withData:` in plaats van `getAuthorization:` / `getAuthorization:withData:` waar mogelijk. De `getAuthorization:` / `getAuthorization:withData:` de methode zal een volledige authentificatiestroom (als de gebruiker niet voor authentiek wordt verklaard) beginnen en dit tot een ingewikkelde implementatie aan de kant van de Programmer kan leiden.

[Terug naar boven...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die uw toepassing meedeelt dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: autorisatiestroom is voltooid</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters**:

* *token*: de korte-duur media-token
* *resource*: de bron waarvoor de vergunning is verleend

**geactiveerd door:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Terug naar boven...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: autorisatiestroom mislukt</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters**:

* *resource*: De bron waarvoor de vergunning is verkregen.
* *code*: De foutcode die aan het mislukkingsscenario is gekoppeld. Mogelijke waarden:
   * `USER_NOT_AUTHORIZED_ERROR` - de gebruiker geen autorisatie voor de opgegeven resource kon uitvoeren
* *beschrijving*: Aanvullende details over het mislukkingsscenario. Als deze beschrijvende tekenreeks om welke reden dan ook niet beschikbaar is, verzendt Adobe Pass Authentication een lege tekenreeks **(&quot;&quot;)**.\
  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopgerelateerde berichten door te geven. Bijvoorbeeld, als een abonnee vergunning voor een middel wordt ontkend, kon MVPD een bericht zoals verzenden: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Als u het pakket wilt bijwerken, klikt u op **hier**.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier&quot;.

**geactiveerd door:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Terug naar boven...](#apis)

</br>

### afmelden {#logout}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Deze methode wordt door uw toepassing aangeroepen om de afmeldingsstroom te starten. De logout is het resultaat van een reeks van HTTP omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Omdat deze stroom niet met een eenvoudig HTTP- verzoek kan worden voltooid dat door de bibliotheek AccessEnabler wordt uitgegeven, een `UIWebView/WKWebView or SFSafariViewController` controller moet worden geïnstantieerd om de HTTP-omleidingsbewerkingen te kunnen volgen.

De logout stroom verschilt van de authentificatiestroom in die zin dat de gebruiker niet wordt vereist om met interactie aan te gaan `UIWebView/WKWebView or SFSafariViewController`  op welke manier dan ook. Daarom adviseert de Adobe dat u de controle onzichtbaar (d.w.z. verborgen) tijdens het logout proces maakt.

Een patroon gelijkend op de authentificatiestroom wordt gebruikt. De iOS AccessEnabler activeert de `navigateToUrl:` callback of de `navigateToUrl:useSVC:` om een `UIWebView/WKWebView or SFSafariViewController` en om URL te laden die in callback wordt verstrekt `url` parameter. Dit is URL van het logout eindpunt op de achtergrondserver. Voor de tvOS AccessEnabler geldt dat `navigateToUrl:` callback of de `navigateToUrl:useSVC:` callback wordt aangeroepen.

Aangezien het door verscheidene omleidingen gaat, moet uw toepassing de activiteit van controleren `UIWebView/WKWebView or SFSafariViewController `en detecteert het moment waarop een specifieke aangepaste URL wordt geladen. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet door uw toepassing slechts als signaal worden geïnterpreteerd dat de logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer de controller deze specifieke aangepaste URL laadt, moet uw toepassing de controller sluiten en AccessEnabler oproepen `handleExternalURL:url `API-methode. Als uw toepassing een `SFSafariViewController `de specifieke aangepaste URL wordt bepaald door uw `application's custom scheme` (bijvoorbeeld`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke aangepaste URL gedefinieerd door de `ADOBEPASS_REDIRECT_URL `constante (d.w.z. `adobepass://ios.app`).

Uiteindelijk zal AccessEnabler het [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 0, die op succes van de logout stroom wijst.

**Opmerking:** Als de gebruiker het programma wordt geopend gebruikend Apple SSO zal de status VSA203 worden teweeggebracht. Als dit het geval is, zou de gebruiker moeten worden geïnstrueerd om zich van de systeemmontages ook te melden. Als u dit niet doet, wordt de verificatie opnieuw uitgevoerd wanneer de toepassing opnieuw wordt gestart.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: de afmeldingsstroom starten</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Terug naar boven...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Gebruik deze methode om de momenteel geselecteerde provider te bepalen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: bepaal de momenteel geselecteerde MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters:** Geen

**Callbacks geactiveerd:** [`selectedProvider:`](#selProv)

[Terug naar boven...](#apis)

</br>

### selectedProvider {#selProv}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: informatie over momenteel geselecteerde MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters**:

* *mvpd*: object met informatie over de momenteel geselecteerde MVPD

**geactiveerd door:** [`getSelectedProvider`](#getSelProv)

[Terug naar boven...](#apis)

</br>

### getMetadata: {#getMeta}

**Bestand:** AccessEnabler/headers/AccessEnabler.h

**Omschrijving:** Gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek AccessEnabler wordt blootgesteld. De toepassing heeft toegang tot deze gegevens door een woordenboekgebaseerd bestand op te geven *key* invoerparameter.

Er zijn twee soorten meta-gegevens beschikbaar aan Programmeurs:

* Statische metagegevens (verificatietoken TTL, machtigingstoken-TTL en apparaat-id)
* Metagegevens van gebruikers (gebruikersspecifieke informatie, zoals gebruikers-id, postcode; kan van een MVPD aan het apparaat van een gebruiker tijdens de Authentificatie en de Vergunning stromen worden overgegaan)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-aanroep: de AccessEnabler vragen naar metagegevens</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters:**

* *keyDictionary*: een woordenboekgegevensstructuur met de volgende indeling:
   * Als key is `METADATA_OPCODE_KEY` en waarde is `METADATA_AUTHENTICATION`, dan wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.
   * Als key is `METADATA_OPCODE_KEY` en waarde is `METADATA_AUTHORIZATION` **en**\
     key is `METADATA_RESOURCE_ID_KEY` en de waarde is een bepaalde middelidentiteitskaart, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.
   * Als key is `METADATA_OPCODE_KEY` en waarde is `METADATA_DEVICE_ID`, dan wordt de vraag gemaakt om huidige apparatenidentiteitskaart te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.
   * Als key is `METADATA_OPCODE_KEY` en waarde is `METADATA_USER_META` **en** key is `METADATA_USER_META_KEY` en value is de naam van de metagegevens en de query wordt uitgevoerd voor de metagegevens van de gebruiker. De lijst met beschikbare metagegevenstypen voor gebruikers:
      * `zip` - Lijst met postcodes
      * `householdID` - Huishoudelijke identificatiecode. Als een MVPD geen subaccounts ondersteunt, is dit gelijk aan `userID`.
      * `maxRating` - Een verzameling van maximale ouderlijke classificaties voor de gebruiker
      * `userID` - De gebruikersnaam. Als een MVPD subaccounts steunt, en de gebruiker is niet de belangrijkste rekening, `userID` verschilt van `householdID.`
      * `channelID` - Een lijst met kanalen die een gebruiker mag bekijken.

  >[!NOTE]
  >
  >De daadwerkelijke Metagegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD ter beschikking stelt. Deze lijst wordt uitgebreid wanneer nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**Callbacks geactiveerd:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Meer informatie:** [Metagegevens gebruiker](/help/authentication/user-metadata.md)

[Terug naar boven...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler na het roepen wordt teweeggebracht[getAuthentication()](#getAuthN) als de huidige aanvrager minstens één MVPD met steun SSO steunt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultaat van SSO-stromen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v2.0+

**Parameters**:

* viewController: vertegenwoordigt het Apple SSO-dialoogvenster. Deze viewController moet op het scherm worden getoond.

**geactiveerd door:** [`getAuthentication`](#getAuthN)

**Meer informatie:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Terug naar boven...](#apis)

</br>

### TVProviderDialog negeren {#dismissTvDialog}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht nadat de gebruiker de Dialoog van SSO van Apple sluit.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultaat van SSO-stromen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v2.0+

**Parameters**:

* viewController: vertegenwoordigt het Apple SSO-dialoogvenster. Deze viewController moet van het scherm worden verwijderd.

**geactiveerd door:** Handeling door gebruiker

**Meer informatie:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Terug naar boven...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die de meta-gegevens levert die via een [`getMetadata:`](#getMeta) vraag.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultaat van verzoek om metagegevens op te halen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Beschikbaarheid:** v1.0+

**Parameters**:

* *metagegevens*: De gevraagde metagegevens. Deze waarde is een `NSString` in het geval van statische metagegevens (TTL voor verificatie, TTL voor autorisatie, apparaat-id).  Het is een complex object wanneer u gebruikersspecifieke metagegevens aanvraagt. Dit complexe object is doorgaans de Objectieve C-representatie van een JSON-payload (bijvoorbeeld &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [150, 320]}&#39; wordt vertaald in doelstelling-C als NSDictionary (&quot;straat&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;)).   Voorbeeld van JSON-object voor metagegevens:

```JSON
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
```

* *gecodeerd*: Booleaanse waarde die aangeeft of de opgehaalde metagegevens zijn gecodeerd of niet. Deze parameter is alleen van belang voor verzoeken van metagegevens van gebruikers. Deze parameter heeft geen betekenis voor statische metagegevens (bijvoorbeeld TTL voor verificatie) die altijd ongecodeerd worden ontvangen. Als deze parameter aan waar wordt geplaatst, dan is het aan programmeur om de unencrypted waarde van de Meta-gegevens van de Gebruiker te verkrijgen door een decryptie van RSA uit te voeren gebruikend whitelingende privé sleutel (de zelfde privé sleutel die voor het ondertekenen van identiteitskaart van de Aanvrager in [`setRequestor:setSignedRequestorId:`](#setReq) en `setRequestor:setSignedRequestorId:serviceProviders: `oproepen).

* *key*: De sleutel die wordt gebruikt om de aanvraag voor het ophalen van metagegevens te formuleren.

* *arguments*: Dezelfde woordenlijst die is doorgegeven aan de map [`getMetadata:`](#getMeta) vraag. Dit wordt verstrekt om de toepassing toe te staan om de verzoeken met de reacties te passen.

**geactiveerd door:** [`getMetadata:`](#getMeta)

**Meer informatie:** [Metagegevens gebruiker](/help/authentication/user-metadata.md)


[Terug naar boven...](#apis)

</br>

### MVPD {#mvpd}

**Bestand:** AccessEnabler/headers/model/MVPD.h

**Beschrijving** Beschrijft het voorwerp MVPD. Kan worden gebruikt om informatie over de eigenschappen van MVPD te verkrijgen.

**Beschikbaarheid:** v1.0+ [boardingStatus eigenschap is beschikbaar in v2.2]

**Eigenschappen**:

* (NSString) identiteitskaart - identiteitskaart MVPD
* (NSString) displayName - de naam MVPD. [Dit moet worden gebruikt voor weergave in de kiezer]
* (NSString) logoURL - het MVPD logoadres.
* (BOOL) enablePlatformServices - Indien waar (true), ondersteunt de MVPD SSO-services zoals [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus - kan 3 waarden hebben:
   * nil - De MVPD ondersteunt Apple SSO niet.
   * PICKER - MVPD kan in de plukker van Apple verschijnen maar de authentificatiestroom wordt gedaan door Adobe.
   * ONDERSTEUND - MVPD wordt volledig gesteund door Apple en zal het teken van Apple gebruiken SSO.

[Terug naar boven...](#apis)

</br>

## Gebeurtenissen bijhouden {#tracking}

AccessEnabler teweegbrengt een extra callback teweeg die niet noodzakelijk met de machtigingsstromen verwant is. De uitvoering van [`sendTrackingData()`](#sendTracking) callback-functie is optioneel, maar de toepassing kan specifieke gebeurtenissen volgen en statistieken compileren, zoals het aantal geslaagde/mislukte verificatie-/autorisatiepogingen.

### sendTrackingData:forEventType: {#sendTracking}

**Bestand:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht signalerend aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/de mislukking van authentificatie/vergunningsstromen. Bij Adobe Pass-verificatie 1.6 worden het apparaattype, het AccessEnabler-clienttype en het besturingssysteem gerapporteerd door [`sendTrackingData()`](#sendTracking). De [`sendTrackingData()`](#sendTracking) callback blijft compatibel met oudere versies.

**Callback: gebeurtenissen bijhouden**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Beschikbaarheid:** v1.0+

**Opmerking:** Het apparaattype en besturingssysteem worden afgeleid via een openbare Java-bibliotheek (<http://java.net/projects/user-agent-utils>) en de userAgent-tekenreeks. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.

* Mogelijke waarden voor apparaattype:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Mogelijke waarden voor client type AccessEnabled:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parameters**:

* *event*: de code van de gebeurtenis die wordt bijgehouden. Er zijn drie mogelijke gebeurtenistypen voor bijhouden:
   * **authenticationDetection:** om het even welk tijd een verzoek van het toestemmingstoken terugkeert (gebeurtenis is `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** wanneer een verificatiecontrole plaatsvindt (gebeurtenis is `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** wanneer de gebruiker een MVPD in de MVPD selectievorm selecteert (gebeurtenis is `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: aanvullende gegevens die aan de gerapporteerde gebeurtenis zijn gekoppeld. Deze gegevens worden gepresenteerd in de vorm van een lijst met waarden.

**geactiveerd door:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instructies voor het interpreteren van de waarden in de *data* array:

* Voor trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Of de token-aanvraag succesvol was (true/false) en of deze succesvol was:
   * **1** - MVPD ID-tekenreeks
   * **2** - GUID (md5 hashed)
   * **3** - Token bevindt zich al in cache (true/false)
   * **4** - Type apparaat
   * **5** - Het type AccessEnabler-client
   * **6** - Type besturingssysteem

* Voor trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Of de token-aanvraag succesvol was (true/false) en of deze succesvol was:
   * **1** - MVPD-id
   * **2** - GUID (md5 hashed)
   * **3** - Token bevindt zich al in cache (true/false)
   * **4** - Fout
   * **5** - Details
   * **6** - Type apparaat
   * **7** - Het type AccessEnabler-client
   * **8** - Type besturingssysteem
* Voor trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID van de momenteel geselecteerde MVPD
   * **1** - Type apparaat
   * **2** - Het type AccessEnabler-client
   * **3** - Type besturingssysteem

</br>
