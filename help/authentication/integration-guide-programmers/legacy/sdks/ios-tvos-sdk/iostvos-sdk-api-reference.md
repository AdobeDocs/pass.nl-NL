---
title: iOS/tvOS API-naslaggids
description: iOS/tvOS API-naslaggids
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# iOS/tvOS SDK API-naslaggids {#iostvos-sdk-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Op deze pagina worden de methoden en callback-functies beschreven die door de native iOS/tvOS-client voor Adobe Pass-verificatie worden aangeboden. De methoden en callback-functies die hier worden beschreven, worden gedefinieerd in de `AccessEnabler.h` - en `EntitlementDelegate.h` -headerbestanden. U vindt ze hier in de iOS AccessEnabler SDK: `[SDK directory]/AccessEnabler/headers/api/`


Bijbehorende documentatie:

* Voor een beschrijving van de basis de machtigingsstroom van de Authentificatie van Adobe Pass, zie [ de Stroom van de Entitlement ](/help/authentication/integration-guide-programmers/entitlement-flow.md).
* Voor een geleidelijke analyse van hoe te om Adobe Pass uit te voeren
de stroom van de authentificatierechten die deze API gebruiken, zie [ Cookbook van de Integratie van iOS ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Voor recentste iOS AccessEnabler SDK, zie [ de Inheemse Bibliotheek van Inschakelen van de Toegang van iOS ](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>De Adobe moedigt u aan om slechts de Authentificatie van Adobe Pass *openbare* APIs te gebruiken:
>
>* Openbare API&#39;s zijn beschikbaar en volledig getest op alle ondersteunde clienttypen. Voor om het even welke openbare eigenschap, zorgen wij ervoor dat elk cliënttype een overeenkomstige versie van de bijbehorende methode(n) heeft.
>* Openbare API&#39;s moeten zo stabiel mogelijk zijn, achterwaartse compatibiliteit ondersteunen en ervoor zorgen dat partnerintegratie niet wordt verbroken. Voor niet-openbare API&#39;s behouden we ons echter het recht voor om hun handtekening op elk toekomstig moment te wijzigen. Als u een bepaalde stroom tegenkomt die niet door een combinatie van de huidige openbare API van de Authentificatie van Adobe Pass vraag kan worden gesteund, is de beste benadering ons op de hoogte te brengen. Rekening houdend met uw behoeften, kunnen wij openbare APIs wijzigen en een stabiele oplossing verstrekken die zich voortzet.

</br>

## API-naslag {#apis}

* [ init ](#initWithSoftwareStatement):softwareStatement - Instantieert het voorwerp AccessEnabler.

* **[GEDEPRECEERDE]** [ init ](#init) - Instantieert het voorwerp AccessEnabler.

* [`setOptions:options:`](#setOptions) - Vormt globale opties van SDK zoals profiel of bezoekerID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3), [`setRequestor:requestorID:serviceProviders:`](#setReqV3) - vestigt de identiteit van de programmeur.

* **[VERVANGEN]** [`setRequestor:signedRequestorId:`](#setReq), [`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Vestigt de identiteit van de Programmer.

* **[VERVANGEN]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos) - Vestigt de identiteit van de programmeur.

* [`setRequestorComplete:`](#setReqComplete) - Meldt aan uw toepassing dat de configuratiefase volledig is.

* [`checkAuthentication`](#checkAuthN) - Controleert de verificatiestatus van de huidige gebruiker.

* [`getAuthentication`](#getAuthN) , [`getAuthentication:withData:`](#getAuthN) - De volledige verificatieworkflow wordt gestart.

* [`getAuthentication:filter`](#getAuthN_filter), [`getAuthentication:withData:`](#getAuthN)[ andFilter ](#getAuthN_filter) - begint het volledige authentificatiewerkschema.

* [`displayProviderDialog:`](#dispProvDialog) - Informeert uw toepassing om de juiste UI-elementen te instantiëren waarmee de gebruiker een MVPD kan selecteren.

* [`setSelectedProvider:`](#setSelProv) - Informeert AccessEnabler over de MVPD-selectie van de gebruiker.

* [`navigateToUrl:`](#nav2url) - Meldt dat de gebruiker de MVPD-aanmeldingspagina moet krijgen.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Meldt aan uw toepassing dat de gebruiker de MVPD login pagina, gebruikend SFSafariViewController moet worden voorgesteld

* [`handleExternalURL:url`](#handleExternalURL) - Voltooit de verificatie/logout-flow.

* **[VEROUDERD]** [`getAuthenticationToken`](#getAuthNToken) - Verzoekt het authentificatietoken van de achterste deelserver.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informeert uw toepassing over de status van de verificatiestroom.

* [`checkPreauthorizedResources:`](#checkPreauth) - Hiermee wordt bepaald of de gebruiker al gemachtigd is specifieke beveiligde bronnen weer te geven.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Hiermee wordt bepaald of de gebruiker al gemachtigd is specifieke beveiligde bronnen weer te geven.

* [`preauthorizedResources:`](#preauthResources) - Biedt een lijst met bronnen die de gebruiker al mag bekijken.

* [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ) - Controleert de machtigingsstatus van de huidige gebruiker.

* [`getAuthorization:`](#getAuthZ) , [`getAuthorization:withData:`](#getAuthZ) - Hiermee wordt de machtigingsstroom gestart.

* [`setToken:forResource:`](#setToken) - Meldt dat de autorisatiestroom is voltooid.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Meldt dat de autorisatiestroom is mislukt.

* [`logout`](#logout) - Hiermee wordt de afmeldingsstroom gestart.

* [`getSelectedProvider`](#getSelProv) - Hiermee bepaalt u de momenteel geselecteerde provider.

* [`selectedProvider:`](#selProv) - Levert informatie over de momenteel geselecteerde MVPD aan uw toepassing.

* [`getMetadata:`](#getMeta) - Hiermee wordt informatie opgehaald die als metagegevens door de AccessEnabler-bibliotheek wordt weergegeven.

* [`presentTvProviderDialog:`](#presentTvDialog) - Meldt dat uw toepassing het Apple SSO-dialoogvenster weergeeft.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Meldt dat uw toepassing het Apple SSO-dialoogvenster verbergt.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Biedt de metagegevens die zijn aangevraagd door een [`getMetadata:`](#getMeta) -aanroep.

* [`sendTrackingData:forEventType:`](#sendTracking) - Levert gegevens voor het bijhouden van gegevens.

* [`MVPD`](#mvpd) - De klasse MVPD. [ bevat informatie over MVPD ]

### init:softwareStatement {#initWithSoftwareStatement}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Instantieert het voorwerp AccessEnabler. Er zou één enkele instantie AccessEnabler per toepassingsinstantie moeten zijn.

| **API vraag: De aannemer van iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Beschikbaarheid:** v3.0+

**Parameters:**

* **softwareStatement:** een koord dat de toepassing in het systeem van de Adobe identificeert. Ontdek hoe u een Software Statement kunt verkrijgen.

[Terug naar boven...](#apis)



### init - [ VEROUDERD ]{#init}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Instantieert het voorwerp AccessEnabler. Er zou één enkele instantie AccessEnabler per toepassingsinstantie moeten zijn.

| API-aanroep: Constructor iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Beschikbaarheid:** v1.0+ **tot:** v3.0

**Parameters:** niets

[Terug naar boven...](#apis)

</br>

### setOptions:opties {#setOptions}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** vormt globale opties van SDK. Het keurt een NSDictionary als argument goed. De waarden van het woordenboek worden samen met elke netwerkaanroep die de SDK maakt, aan de server doorgegeven.

**Nota:** de waarden zullen aan de server onafhankelijk van de huidige stroom (authentificatie/vergunning) worden overgegaan. Als u de waarden wilt wijzigen, kunt u deze methode op elk gewenst moment aanroepen.

| API-aanroep: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Beschikbaarheid:** v2.3.0+

**Parameters:**

* *opties*: Een NSDictionary die globale opties van SDK bevat. Momenteel zijn de volgende opties beschikbaar:
   * **applicationProfile** - het kan worden gebruikt om serverconfiguraties te maken die op deze waarde worden gebaseerd.
   * **bezoekorID** - de Dienst van identiteitskaart van het Experience Cloud. Deze waarde kan later worden gebruikt voor geavanceerde analyserapporten.
   * **handleSVC** - Van Boole die als de programmeur SFSafariViewControllers zal behandelen. Gelieve te zien [ SFSafariViewController steun op iOS SDK 3.2+ ](/help/authentication/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) voor meer details.
      * Als de reeks aan **vals,** SDK automatisch de eindgebruiker met een SFSafariViewController zal voorstellen. De SDK navigeert verder naar de URL van de MVPD-aanmeldingspagina.
      * Als de reeks aan **waar,** SDK **NIET** automatisch de eindgebruiker met een SFSafariViewController zal voorstellen. SDK zal verder **navigeren (toUrl:{url}, useSVC:JA)**.
* **apparaat\_info** - de informatie van de Cliënt zoals die in [ wordt beschreven het overgaan cliëntinformatie ](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md).

[Terug naar boven...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** vestigt de identiteit van de programmeur. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens, kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, setRequestor kan opnieuw worden geroepen wanneer de toepassing in de voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre token als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback `setRequestorComplete:` aan uw toepassing gepresenteerd.

Als de parameter `urls` niet wordt gebruikt, richt de resulterende netwerkaanroep zich op de standaarddienstverlener URL: de Adobe RELEASE/productie milieu.


Als een waarde voor de parameter `urls` wordt opgegeven, richt de resulterende netwerkaanroep alle URL&#39;s die in de parameter `urls` worden opgegeven. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Beschikbaarheid:** v3.0+

**Parameters:**

* *requestID*: Unieke identiteitskaart verbonden aan de Programmer. Geef de unieke id die door Adobe aan uw site is toegewezen door wanneer u zich voor het eerst aanmeldt bij de Adobe Pass-verificatieservice.
* *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

>[!NOTE]
>
>Als deze wordt aangeroepen zonder de parameter `serviceProviders` , haalt de bibliotheek de configuratie op van het standaardservicebureau (dat wil zeggen `https://sp.auth.adobe.com` voor het productieprofiel of `https://sp.auth-staging.adobe.com` voor het staging-profiel). Als de parameter `serviceProviders` wordt opgegeven, moet deze een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (namelijk de server met de kortste reactietijd neemt belangrijkheid).

**teweeggebrachte callbacks:** [`setRequestorComplete:`](#setReqComplete)

[Terug naar boven...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [ VEROUDERD ] {#setReq}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** vestigt de identiteit van de programmeur. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback `setRequestorComplete:` aan uw toepassing gepresenteerd.

Als de parameter `urls` niet wordt gebruikt, richt de resulterende netwerkaanroep zich op de standaarddienstverlener URL: de Adobe RELEASE/productie milieu.

Als een waarde voor de parameter `urls` wordt opgegeven, richt de resulterende netwerkaanroep alle URL&#39;s die in de parameter `urls` worden opgegeven. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Beschikbaarheid:** v1.0+ **tot:** v3.0

| API-aanroep: configuratie aanvrager |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Beschikbaarheid:** v1.0+ **tot:** v3.0

**Parameters:**

* *requestID*: Unieke identiteitskaart verbonden aan de Programmer. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
* *signedRequestorID*: **Deze parameter bestaat in iOS AccessEnabler versies 1.2 en later.** Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)--> .
* *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

**Nota&#39;s:** Als geroepen zonder de `serviceProviders` parameter, zal de bibliotheek de configuratie van de standaarddienstverlener (namelijk `https://sp.auth.adobe.com` voor het productieprofiel of `https://sp.auth-staging.adobe.com` voor het opvoeren profiel) terugwinnen. Als de parameter `serviceProviders` wordt opgegeven, moet deze een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (d.w.z., de server met de kortste reactietijd neemt belangrijkheid).

**teweeggebrachte callbacks:** [`setRequestorComplete:`](#setReqComplete)


[Terug naar boven...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [ VEROUDERD ] {#setReq_tvos}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** vestigt de identiteit van de programmeur. Elke programmeur krijgt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Deze instelling mag slechts eenmaal worden uitgevoerd tijdens de levenscyclus van de toepassing.

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van Programmer in bijlage is. De serverreactie wordt gebruikt intern door de code AccessEnabler. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback `setRequestorComplete:` aan uw toepassing gepresenteerd.

Als de parameter `urls` niet wordt gebruikt, richt de resulterende netwerkaanroep zich op de standaarddienstverlener URL: de Adobe RELEASE/productie milieu.

Als een waarde voor de parameter `urls` wordt opgegeven, richt de resulterende netwerkaanroep alle URL&#39;s die in de parameter `urls` worden opgegeven. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt AccessEnabler URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.



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


**Beschikbaarheid:** v2.0+ **tot:** v3.0

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

**Beschikbaarheid:** v2.0+ **tot:** v3.0

**Parameters:**

* *requestID*: Unieke identiteitskaart verbonden aan de Programmer. Geef de unieke id die door Adobe aan uw site is toegewezen, door aan uw site wanneer u het eerst   geregistreerd bij de Adobe Pass Authentication-service.
* *signedRequestorID*: **Deze parameter bestaat in iOS AccessEnabler   versies 1.2 en hoger.** Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)--> .
* *urls*: Facultatieve parameter; door gebrek, de dienstverlener van Adobe   wordt gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.
* geheim en publicKey: De geheime en openbare sleutel die wordt gebruikt om de tweede het schermvraag te ondertekenen. Voor meer informatie zie de [ Clienteless documentatie ](#create_dev).

Als deze wordt aangeroepen zonder de parameter `serviceProviders` , haalt de bibliotheek de configuratie op van het standaardservicebureau (d.w.z. `https://sp.auth.adobe.com` voor het productieprofiel of https://sp.auth-staging.adobe.com voor het staging-profiel). Als de parameter `serviceProviders` wordt opgegeven, moet deze een array van URL&#39;s zijn. De configuratiegegevens worden opgehaald uit alle opgegeven eindpunten en worden samengevoegd. Als dubbele informatie in verschillende dienstverlener reacties bestaat, wordt het conflict opgelost ten gunste van de snelst antwoordende server (d.w.z., de server met de kortste reactietijd neemt belangrijkheid).

**teweeggebrachte callbacks:** [`setRequestorComplete:`](#setReqComplete)

[Terug naar boven...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht die uw toepassing informeert dat de configuratiefase volledig is. Dit is een signaal dat de app aanvragen voor machtigingen kan starten. De toepassing kan geen machtigingsaanvragen indienen totdat de configuratiefase is voltooid.

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

* *status*: kan één van de volgende waarden nemen:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - de configuratiefase is voltooid
   * `ACCESS_ENABLER_STATUS_ERROR` - configuratiefase mislukt

**teweeggebracht door:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Terug naar boven...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** controleert de authentificatiestatus van de huidige gebruiker.
Het doet dit door naar een geldig authentificatietoken in de lokale
opslagruimte voor token. Deze methode voert geen netwerkvraag uit en wij adviseren roepend het op de belangrijkste draad.
Het wordt gebruikt door de toepassing om de de authentificatiestatus van de gebruiker te vragen en
de gebruikersinterface dienovereenkomstig bijwerken (d.w.z. de gebruikersinterface voor aanmelden/afmelden bijwerken). De
de authenticatiestatus wordt aan de aanvraag meegedeeld via
de [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.


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

**Parameters:** niets

**teweeggebrachte callbacks:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Terug naar boven...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** begint het volledige authentificatiewerkschema. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

* als de laatste authentificatiepoging succesvol was, MVPD   de selectiefase wordt overgeslagen en   de callback van [`navigateToUrl:`](#nav2url) wordt geactiveerd. De   de toepassing gebruikt deze callback om de controle te concretiseren WebView die de gebruiker met de MVPD login pagina voorstelt. **[NOTA: Vanaf Toegang toelaten 1.5, is deze functionaliteit niet beschikbaar wegens een beperking in SDK ].**
* als de laatste verificatiepoging is mislukt of als de gebruiker zich expliciet heeft afgemeld, wordt de [`displayProviderDialog:`](#dispProvDialog) callback   geactiveerd. Uw toepassing gebruikt deze callback om de MVPD selectie UI te tonen. Uw app is ook vereist om de verificatiestroom te hervatten door de AccessEnabler-bibliotheek via de methode [`setSelectedProvider:`](#setSelProv) te informeren over de MVPD-selectie van de gebruiker.

Aangezien de geloofsbrieven van de gebruiker op de MVPD login pagina worden geverifieerd, wordt uw toepassing vereist om de veelvoudige omleidingsverrichtingen te controleren die plaatsvinden terwijl de gebruiker bij de MVPD login pagina voor authentiek verklaart. Wanneer de correcte geloofsbrieven zijn ingegaan, wordt de controle WebView opnieuw gericht aan een douane URL die door de `ADOBEPASS_REDIRECT_URL` constante wordt bepaald. Deze URL is niet bedoeld om door WebView te worden geladen. De toepassing moet deze URL onderscheppen en deze gebeurtenis interpreteren als een signaal dat de aanmeldingsfase is voltooid. Het zou dan controle aan AccessEnabler moeten overdragen om de authentificatiestroom (door de ](#handleExternalURL) methode te roepen 0} handleExternalURL) te voltooien.[

Tot slot wordt de authentificatiestatus meegedeeld aan de toepassing via [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.

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

* *forceAuthn*: Een vlag die specificeert als de authentificatiestroom zou moeten zijn begonnen, ongeacht als de gebruiker reeds voor authentiek verklaard of niet is.
* *gegevens*: Een woordenboek dat uit zeer belangrijk-waardeparen bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**teweeggebrachte callbacks:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Terug naar boven...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** begint het volledige authentificatiewerkschema. Het begint door de authentificatiestatus te controleren. Indien nog niet geverifieerd, wordt de verificatiestroom state-machine gestart:

* [ presentTvProviderDialog () ](#presentTvDialog) zal worden geroepen als de huidige aanvrager minstens één MVPD heeft die SSO steunt. Als geen MVPD SSO steunt, zal de klassieke authentificatiestroom beginnen en de filterparameter wordt genegeerd.
* Nadat de gebruiker de Apple SSO-stroom [`dismissTvProviderDialog()`](#dismissTvDialog) heeft voltooid, wordt deze geactiveerd en het verificatieproces is voltooid.

Tot slot wordt de authentificatiestatus meegedeeld aan de toepassing via [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.

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

* *forceAuthn*: Een vlag die specificeert als de authentificatiestroom zou moeten zijn begonnen, ongeacht als de gebruiker reeds voor authentiek verklaard of niet is.
* *gegevens*: Een woordenboek dat uit zeer belangrijk-waardeparen bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.
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

* *forceAuthn*: Een vlag die specificeert als de authentificatiestroom zou moeten zijn begonnen, ongeacht als de gebruiker reeds voor authentiek verklaard of niet is.
* *gegevens*: Een woordenboek dat uit zeer belangrijk-waardeparen bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.
* filter: Een lijst met MVPD-id&#39;s die moet worden weergegeven in het dialoogvenster Apple SSO. Om het even welke MVPD die SSO niet steunt zal worden genegeerd maar de orde zal worden geëerbiedigd.

**teweeggebrachte callbacks:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Terug naar boven...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht om de toepassing mee te delen dat de aangewezen elementen UI moeten worden geconcretiseerd om de gebruiker toe te staan om gewenste MVPD te selecteren. De callback biedt een lijst met MVPD-objecten met aanvullende informatie die kan helpen om het deelvenster met de selectieinterface correct samen te stellen (zoals de URL die het logo van de MVPD aanwijst, de vriendelijke weergavenaam, enz.)

Zodra de gebruiker gewenste MVPD heeft geselecteerd, wordt de upper-layer toepassing vereist om de authentificatiestroom te hervatten door `setSelectedProvider:` te roepen en het over te gaan identiteitskaart van MVPD die aan de selectie van de gebruiker beantwoordt.

**Aborting de authentificatiestroom** - dit is een punt waar de gebruiker de capaciteit heeft om de &quot;Achterknoop&quot;te drukken, die aan het aborteren van de authentificatiestroom gelijkwaardig is. In dat scenario, wordt uw toepassing vereist om [ setSelectedProvider te roepen:](#setSelProv) methode, die ongeldig als parameter overgaat, om AccessEnabler de kans te geven om zijn authentificatiestatus-machine terug te stellen.

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

* *mvpds*: lijst van voorwerpen MVPD die op MVPD betrekking hebbende informatie houden die de toepassing kan gebruiken om de elementen van de selectieUI te bouwen MVPD.

**teweeggebracht door:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Terug naar boven...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt geroepen door uw toepassing om Toegangsmanager van de selectie MVPD van de gebruiker op de hoogte te brengen. De toepassing kan deze methode gebruiken om de dienstverlener te selecteren of te veranderen die voor authentificatie wordt gebruikt.

Als het geselecteerde MVPD een TempPass MVPD is zal het automatisch met die MVPD voor authentiek verklaren zonder het moeten getAuthentication () daarna roepen.

Houd er rekening mee dat dit niet mogelijk is voor de Tijdelijke controle voor speciale acties waarbij extra parameters worden gegeven aan de methode getAuthentication().

Wanneer het overgaan van *ongeldig* als parameter, veronderstelt de Toegang dat de gebruiker de authentificatiestroom (d.w.z. op de &quot;Achterknoop&quot;gedrukt) heeft geannuleerd, en antwoordt door de authentificatiestatus-machine terug te stellen en door [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback met de `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` foutencode te roepen.

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

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus:errorCode:`, `sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Terug naar boven...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving:** Callback die door AccessEnabler wordt teweeggebracht om uw toepassing te verzoeken om een controlemechanisme UIWebView/WKWebView te concretiseren en URL te laden die in de 2} parameter van callback wordt verstrekt. **`url`** De callback gaat de **`url`** parameter over die URL van het authentificatieeindpunt of URL van het logout eindpunt vertegenwoordigt.

Aangezien het UIWebView/WKWebView ` ` controlemechanisme door verscheidene redirects gaat, moet uw toepassing de activiteit van het controlemechanisme controleren en het moment ontdekken wanneer het een specifieke douane URL laadt die door de `ADOBEPASS_REDIRECT_URL ` constante (d.w.z. `adobepass://ios.app` wordt bepaald). Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet slechts door uw toepassing als signaal worden geïnterpreteerd dat de authentificatie of logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing UIWebView/WKWebView sluiten en de 20} API methode roepen van AccessEnabler {.`handleExternalURL:url `

**Nota:** Gelieve te merken op dat in het geval van de authentificatiestroom dit een punt is waar de gebruiker de capaciteit heeft om de &quot;Achterknoop&quot;te drukken, die aan het aborteren van de authentificatiestroom gelijkwaardig is. In zulk een scenario, wordt uw toepassing vereist om [ setSelectedProvider te roepen:](#setSelProv) methode die **`nil`** als parameter overgaat en een kans geeft aan AccessEnabler om zijn authentificatiestatus-machine terug te stellen.

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

* *url*: URL die aan de MVPD login pagina richt

**teweeggebracht door:** [ setSelectedProvider:](#setSelProv)



[Terug naar boven...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving:** Callback die door AccessEnabler in plaats van `navigateToUrl:` callback in werking wordt gesteld voor het geval dat uw toepassing eerder handmatige behandeling van het Controlemechanisme van de Mening Safari (SVC) via [ setOptions (\ [&quot;handleSVC&quot;:waar&quot;\]) ](#setOptions), en slechts in het geval van MVPDs die Controlemechanisme van de Mening vereisen (SVC). Voor alle andere MVPD&#39;s wordt de callback `navigateToUrl:` aangeroepen. Gelieve te zien [ SFSafariViewController steun op iOS SDK 3.2+ ](/help/authentication/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) voor details op hoe het Controlemechanisme van de Mening Safari (SVC) zou moeten worden beheerd.

Net als bij de `navigateToUrl:` callback wordt `navigateToUrl:useSVC:` door de AccessEnabler geactiveerd om uw toepassing te vragen een `SFSafariViewController` -controller te instantiëren en de URL te laden die in de **`url`** -parameter van de callback is opgegeven. De callback geeft de parameter **`url`** door die de URL van het verificatieeindpunt of de URL van het logout-eindpunt vertegenwoordigt, en de parameter **`useSVC`** die aangeeft dat de toepassing een `SFSafariViewController` moet gebruiken.

Aangezien het `SFSafariViewController` controlemechanisme door verscheidene omleidingen gaat, moet uw toepassing de activiteit van het controlemechanisme controleren en het ogenblik ontdekken wanneer het een specifieke douane URL laadt die door uw `application's custom scheme` wordt bepaald (b.v.** ** `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet slechts door uw toepassing als signaal worden geïnterpreteerd dat de authentificatie of logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing `SFSafariViewController` sluiten en de 1} API methode van AccessEnabler roepen {.`handleExternalURL:url `

**Nota:** Gelieve te merken op dat in het geval van de authentificatiestroom dit een punt is waar de gebruiker de capaciteit heeft om de &quot;Achterknoop&quot;te drukken, die aan het aborteren van de authentificatiestroom gelijkwaardig is. In zulk een scenario, wordt uw toepassing vereist om [ setSelectedProvider te roepen:](#setSelProv) methode die **`nil`** als parameter overgaat en een kans geeft aan AccessEnabler om zijn authentificatiestatus-machine terug te stellen.

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

* *url:* URL die aan de MVPD login pagina richt
* *useSVC:* of url in SFSafariViewController zou moeten worden geladen.

**teweeggebracht door:**[ setOptions:](#setOptions) vóór [ setSelectedProvider:](#setSelProv)

[Terug naar boven...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt geroepen door uw toepassing om de authentificatie of logout stroom te voltooien. Deze methode moet worden aangeroepen direct nadat uw toepassing het moment detecteert waarop de `UIWebView/WKWebView or SFSafariViewController` -controller wordt omgeleid naar een specifieke aangepaste URL. In het geval dat uw toepassing a `SFSafariViewController ` controlemechanisme moet gebruiken wordt de specifieke douane URL bepaald door uw `application's custom scheme` (b.v. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke douane URL bepaald door de `ADOBEPASS_REDIRECT_URL ` constante (d.w.z. `adobepass://ios.app`).

In het geval van de authentificatiestroom voltooit AccessEnabler de stroom door het authentificatietoken van de achterste deelserver terug te winnen en het lokaal in de symbolische opslag op te slaan. AccessEnabler zal uw toepassing informeren dat de authentificatiestroom door `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> callback met een statuscode van 1 te roepen volledig is, erop wijzend succes. Als er tijdens de uitvoering van deze stappen een fout optreedt, wordt de callback van `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> geactiveerd met een statuscode 0 die een verificatiefout en een bijbehorende foutcode aangeeft.

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

* *url*: Onderschepte URL van de ` UIWebView/WKWebView or SFSafariViewController ` controle als koord.


**teweeggebrachte callbacks:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Terug naar boven...](#apis)

</br>

#### getAuthenticationToken - [ VEROUDERD ] {#getAuthNToken}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** voltooit de authentificatiestroom door het authentificatietoken van de achterste deelserver te verzoeken. Deze methode zou door uw toepassing slechts in antwoord op gebeurtenis moeten worden geroepen waar de controle WebView die de MVPD login pagina ontvangt aan douane URL wordt opnieuw gericht die door de `ADOBEPASS_REDIRECT_URL` constante wordt bepaald.


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

**Beschikbaarheid:** v1.0+ **tot:** v3.0

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Terug naar boven...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht die de toepassing van het statuut van de authentificatiestroom informeert. Er zijn vele plaatsen waar deze stroom kan ontbreken, of als gevolg van gebruikersinteractie of wegens andere onvoorziene scenario&#39;s (d.w.z., netwerkconnectiviteitsproblemen, enz.). Deze callback informeert de toepassing van de succes/mislukkingsstatus van de authentificatiestroom, terwijl ook het verstrekken van extra informatie over de mislukkingsreden, wanneer nodig.

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

* *status*: kan één van de volgende waarden nemen:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - de verificatiestroom is voltooid
   * `ACCESS_ENABLER_STATUS_ERROR` - verificatiestroom mislukt
* *code*: mislukkingsreden. Als *status* `ACCESS_ENABLER_STATUS_SUCCESS` is, dan *code* een leeg koord (d.w.z., dat door de `USER_AUTHENTICATED` constante wordt bepaald) is. In het geval van een fout kan deze parameter een van de volgende waarden hebben:
   * `USER_NOT_AUTHENTICATED_ERROR` - De gebruiker is niet geverifieerd. In antwoord op [ checkAuthentication:](#checkAuthN) methodevraag wanneer er geen geldig authentificatietoken in het lokale symbolische geheime voorgeheugen is.
   * `PROVIDER_NOT_SELECTED_ERROR` - De AccessEnabler heeft de       authentificatiestatus-machine na de hogere laagtoepassing       overgegaan *ongeldig* tot [`setSelectedProvider:`](#setSelProv) om de authentificatiestroom af te breken.  Waarschijnlijk heeft de gebruiker de verificatiestroom geannuleerd (druk op de knop &quot;Terug&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - De verificatiestroom is mislukt als gevolg van redenen zoals netwerkonbeschikbaarheid of omdat de gebruiker de verificatiestroom expliciet heeft geannuleerd.

**teweeggebracht door:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Terug naar boven...](#apis)

</br>

### checkPreauthorisedResources: {#checkPreauth}


**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om te bepalen als de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is informatie voor gebruik in het versieren van UI terug te winnen **(bijvoorbeeld, wijzend op toegangsstatus met slot en ontgrendelingspictogrammen).**

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

* *middelen:* serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. De     resource-id is onderworpen aan dezelfde beperkingen als de resource-id in de aanroep, dat wil zeggen dat deze een overeengekomen waarde moet zijn tussen de programmeur en de MVPD of een media-RSS-fragment.

**callback teweeggebracht:** [`preauthorizedResources:`](#preauthResources)

[Terug naar boven...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om te bepalen als de gebruiker reeds gemachtigd is om specifieke beschermde middelen te bekijken. Het primaire doel van deze methode is het ophalen van informatie voor gebruik in het versieren van de interface (bijvoorbeeld het aangeven van de toegangsstatus met vergrendelings- en ontgrendelpictogrammen). De **geheime voorgeheugen** parametercontroles of het interne geheime voorgeheugen voor het oplossen van middelen wordt gebruikt.

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

* *middelen:* serie van middelen waarvoor de vergunning zou moeten worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de aanroep van `getAuthorization:` , dat wil zeggen dat er een waarde moet worden overeengekomen die is vastgesteld tussen de programmeur en de MVPD of een RSS-fragment van media.
* *geheime voorgeheugen:* Van Boole die of specificeert om het interne geheime voorgeheugen voor het oplossen van middelen te gebruiken. Als de waarde false is, wordt de cache overgeslagen, wat leidt tot serveraanroepen telkens wanneer deze API wordt aangeroepen.

**callback teweeggebracht:** [`preauthorizedResources:`](#preauthResources)

[Terug naar boven...](#apis)

</br>

### preauthorisedResources: {#preauthResources}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving:** Callback die door `checkPreauthorizedResources:` wordt teweeggebracht. Verstrekt een lijst van middelen de gebruiker reeds aan mening wordt gemachtigd.

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

* `resources`: array met bronnen waarvoor de gebruiker al gemachtigd is om deze weer te geven.

**teweeggebracht door:** [`checkPreauthorizedResources:`](#checkPreauth)



[Terug naar boven...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstatus te controleren. Het begint door de authentificatiestatus eerst te controleren. Als deze niet is geverifieerd, wordt de callback van [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) geactiveerd en wordt de methode afgesloten. Als de gebruiker voor authentiek wordt verklaard, teweegbrengt het ook de vergunningsstroom. Zie details over de methode [`getAuthorization:`](#getAuthZ) .


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

* *middel*: identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
* *gegevens*: Een woordenboek dat uit zeer belangrijk-waardeparen bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**teweeggebrachte callbacks:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed), `setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Terug naar boven...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt gebruikt door de toepassing om de vergunningsstroom in werking te stellen. Als de gebruiker niet reeds voor authentiek wordt verklaard, stelt het ook de authentificatiestroom in werking. Als de gebruiker voor authentiek wordt verklaard, gaat AccessEnabler te werk om verzoeken om het toestemmingstoken (als geen geldig toestemmingstoken in het lokale symbolische geheime voorgeheugen) en voor het kortstondige media token uit te geven. Zodra het korte media token is verkregen, wordt de autorisatiestroom als volledig beschouwd. De callback van [`setToken:forResource:`](#setToken) wordt teweeggebracht en het korte media teken wordt geleverd als parameter aan de toepassing. Als de autorisatie om welke reden dan ook mislukt, wordt de callback van [`tokenRequestFailed:forEventType:`](#tokenReqFailed) geactiveerd en worden de foutcode/details weergegeven.

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

* *middel*: identiteitskaart van het middel waarvoor de gebruiker om toestemming verzoekt.
* *gegevens*: Een woordenboek dat uit zeer belangrijk-waardeparen bestaat die naar de de omslagdienst van betaaltelevisie moeten worden verzonden. Adobe kan deze gegevens gebruiken om toekomstige functionaliteit toe te laten zonder SDK te veranderen.

**teweeggebrachte callbacks:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**extra teweeggebrachte callbacks:**\
Deze methode kan ook de volgende callbacks activeren (als de verificatiestroom ook wordt gestart): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Gebruik waar mogelijk `checkAuthorization:` / `checkAuthorization:withData:` in plaats van `getAuthorization:` / `getAuthorization:withData:` . Met de methode `getAuthorization:` / `getAuthorization:withData:` wordt een volledige verificatiestroom gestart (als de gebruiker niet is geverifieerd). Dit kan leiden tot een gecompliceerde implementatie aan de kant van de programmeur.

[Terug naar boven...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht die uw toepassing meedeelt dat de vergunningsstroom met succes werd voltooid. Het media-token voor korte tijd wordt ook als een parameter geleverd.


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

* *teken*: het korte-levende media teken
* *middel*: het middel waarvoor de vergunning werd verkregen

**teweeggebracht door:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Terug naar boven...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht die de upper-layer toepassing informeert dat de vergunningsstroom ontbrak.

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

* *middel*: Het middel waarvoor de vergunning werd verkregen.
* *code*: De foutencode verbonden aan het mislukkingsscenario. Mogelijke waarden:
   * `USER_NOT_AUTHORIZED_ERROR` - de gebruiker kan geen autorisatie uitvoeren
voor de gegeven bron
* *beschrijving*: De extra details over het mislukkingsscenario. Als dit beschrijvende koord om het even welke reden niet beschikbaar is, verzendt de Authentificatie van Adobe Pass een leeg koord **(&quot;&quot;&quot;)**.\
  Deze tekenreeks kan door een MVPD worden gebruikt om aangepaste foutberichten of verkoopgerelateerde berichten door te geven. Bijvoorbeeld, als een abonnee vergunning voor een middel wordt ontkend, kon MVPD een bericht zoals verzenden: &quot;U hebt momenteel geen toegang tot dit kanaal in uw pakket. Als u uw pakket zou willen bevorderen klikt **hier**.&quot; Het bericht wordt overgegaan door de Authentificatie van Adobe Pass door deze callback aan Programmer, die de optie heeft om het te tonen of te negeren. Adobe Pass Authentication kan deze parameter ook gebruiken om meldingen te verzenden over de voorwaarde die tot een fout kan hebben geleid. Bijvoorbeeld, &quot;kwam een netwerkfout voor toen het communiceren met de de vergunningsdienst van de leverancier&quot;.

**teweeggebracht door:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Terug naar boven...](#apis)

</br>

### afmelden {#logout}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** Deze methode wordt geroepen door uw toepassing om de logout stroom in werking te stellen. De logout is het resultaat van een reeks van HTTP omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Omdat deze stroom niet kan worden voltooid met een eenvoudige HTTP-aanvraag die is uitgegeven door de AccessEnabler-bibliotheek, moet een `UIWebView/WKWebView or SFSafariViewController` -controller worden geïnstantieerd om de HTTP-omleidingsbewerkingen te kunnen volgen.

De logout-flow verschilt van de verificatiestroom omdat de gebruiker op geen enkele wijze met de `UIWebView/WKWebView or SFSafariViewController` -controller hoeft te communiceren. Daarom adviseert de Adobe dat u de controle onzichtbaar (d.w.z. verborgen) tijdens het logout proces maakt.

Een patroon gelijkend op de authentificatiestroom wordt gebruikt. De iOS AccessEnabler activeert de callback `navigateToUrl:` of de `navigateToUrl:useSVC:` om een controller `UIWebView/WKWebView or SFSafariViewController` te maken en de URL te laden die in de parameter `url` van de callback wordt opgegeven. Dit is URL van het logout eindpunt op de achtergrondserver. Voor tvOS AccessEnabler wordt noch de callback `navigateToUrl:` , noch de callback `navigateToUrl:useSVC:` aangeroepen.

Aangezien het door verscheidene omleidingen gaat, moet uw toepassing de activiteit van het `UIWebView/WKWebView or SFSafariViewController ` controlemechanisme controleren en het moment ontdekken wanneer het een specifieke douane URL laadt. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet door uw toepassing slechts als signaal worden geïnterpreteerd dat de logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing het controlemechanisme sluiten en de API methode van AccessEnabler `handleExternalURL:url ` roepen. In het geval dat uw toepassing a `SFSafariViewController ` controlemechanisme moet gebruiken wordt de specifieke douane URL bepaald door uw `application's custom scheme` (b.v. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke douane URL bepaald door de `ADOBEPASS_REDIRECT_URL ` constante (d.w.z. `adobepass://ios.app`).

Uiteindelijk zal AccessEnabler [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 0 roepen, die op succes van de logout stroom wijst.

**Nota:** als de gebruiker het programma wordt geopend gebruikend Apple SSO zal de status VSA203 worden teweeggebracht. Als dit het geval is, zou de gebruiker moeten worden geïnstrueerd om zich van de systeemmontages ook te melden. Als u dit niet doet, wordt de verificatie opnieuw uitgevoerd wanneer de toepassing opnieuw wordt gestart.


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

**Parameters:** niets

**teweeggebrachte callbacks:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Terug naar boven...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** gebruik deze methode om de momenteel geselecteerde leverancier te bepalen.

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

**Parameters:** niets

**teweeggebrachte callbacks:** [`selectedProvider:`](#selProv)

[Terug naar boven...](#apis)

</br>

### selectedProvider {#selProv}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht die informatie over momenteel geselecteerde MVPD aan de toepassing levert.

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

* *mvpd*: voorwerp dat informatie over momenteel geselecteerde MVPD bevat

**teweeggebracht door:** [`getSelectedProvider`](#getSelProv)

[Terug naar boven...](#apis)

</br>

### getMetadata: {#getMeta}

**Dossier:** AccessEnabler/headers/AccessEnabler.h

**Beschrijving:** gebruik deze methode om informatie terug te winnen die als meta-gegevens door de bibliotheek AccessEnabler wordt blootgesteld. De toepassing kan tot deze gegevens toegang hebben door een op woordenboek-gebaseerde *sleutel* inputparameter te verstrekken.

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

* *keyDictionary*: een structuur van woordenboekgegevens, met het volgende
indeling:
   * Als de sleutel `METADATA_OPCODE_KEY` is en de waarde `METADATA_AUTHENTICATION` is, dan wordt de vraag gemaakt om de vervaltijd van het authentificatietoken te verkrijgen.
   * Als de sleutel `METADATA_OPCODE_KEY` is en de waarde `METADATA_AUTHORIZATION` **is en**\
     key is `METADATA_RESOURCE_ID_KEY` en de waarde is een bepaalde middelID, dan wordt de vraag gemaakt om de vervaltijd van het toestemmingstoken te verkrijgen verbonden aan het gespecificeerde middel.
   * Als de toets `METADATA_OPCODE_KEY` is en de waarde `METADATA_DEVICE_ID` is, wordt de query uitgevoerd om de huidige apparaat-id te verkrijgen. Deze functie is standaard uitgeschakeld en programmeurs moeten contact opnemen met de Adobe voor informatie over de mogelijkheden en kosten.
   * Als de sleutel `METADATA_OPCODE_KEY` is en de waarde `METADATA_USER_META` **is en** de sleutel is `METADATA_USER_META_KEY` en de waarde is de naam van de meta-gegevens, dan wordt de vraag gemaakt voor gebruikersmeta-gegevens. De lijst met beschikbare metagegevenstypen voor gebruikers:
      * `zip` - Lijst met postcodes
      * `householdID` - Huishoudelijke id. Als een MVPD geen subaccounts ondersteunt, is dit gelijk aan `userID` .
      * `maxRating` - Een verzameling van maximale ouderlijke classificaties voor de gebruiker
      * `userID` - De gebruikers-id. Als een MVPD subaccounts ondersteunt en de gebruiker niet de hoofdaccount is, zal `userID` anders zijn dan `householdID.`
      * `channelID` - Een lijst met kanalen die een gebruiker mag bekijken.

  >[!NOTE]
  >
  >De daadwerkelijke Metagegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD ter beschikking stelt. Deze lijst wordt uitgebreid wanneer nieuwe metagegevens beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

**teweeggebrachte callbacks:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[Terug naar boven...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler na het roepen van [ wordt teweeggebracht getAuthentication () ](#getAuthN) als de huidige aanvrager minstens één MVPD met steun steunt SSO steunt.

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

**teweeggebracht door:** [`getAuthentication`](#getAuthN)

**Meer Informatie:** [ iOS/tvOS enig Teken ](#presentTvDialog)

[Terug naar boven...](#apis)

</br>

### TVProviderDialog negeren {#dismissTvDialog}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht nadat de gebruiker de Dialoog van Apple SSO sluit.

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

**teweeggebracht door:** actie van de Gebruiker

**Meer Informatie:** [ iOS/tvOS enig Teken ](#presentTvDialog)

[Terug naar boven...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Beschrijving** Callback die door AccessEnabler wordt teweeggebracht die de meta-gegevens levert die via een [`getMetadata:`](#getMeta) vraag worden gevraagd.

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

* *meta-gegevens*: De gevraagde meta-gegevens. Deze waarde is een `NSString` in het geval van statische metagegevens (TTL voor verificatie, TTL voor autorisatie, apparaat-id).  Het is een complex object wanneer u gebruikersspecifieke metagegevens aanvraagt. Dit complexe object is doorgaans de Objectief-C-representatie van een JSON-payload (bijvoorbeeld &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [ &quot;150&quot;, &quot;320&quot;]&#39; wordt in doelstelling-C vertaald als NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;15 0&quot;, &quot;320&quot;).   Voorbeeld van JSON-object voor metagegevens:

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

* *gecodeerd*: De waarde van Boole die specificeert als de teruggewonnen meta-gegevens al dan niet wordt gecodeerd. Deze parameter is alleen van belang voor verzoeken van metagegevens van gebruikers. Deze parameter heeft geen betekenis voor statische metagegevens (bijvoorbeeld TTL voor verificatie) die altijd ongecodeerd worden ontvangen. Als deze parameter aan waar wordt geplaatst, dan is het aan programmeur om de niet gecodeerde waarde van Metagegevens van de Gebruiker te verkrijgen door een decryptie van RSA uit te voeren gebruikend whitelingende privé sleutel (de zelfde privé sleutel die voor het ondertekenen van identiteitskaart van de Aanvrager in [`setRequestor:setSignedRequestorId:`](#setReq) en `setRequestor:setSignedRequestorId:serviceProviders: ` vraag wordt gebruikt).

* *sleutel*: De sleutel die wordt gebruikt om het verzoek van de meta-gegevensherwinning te formuleren.

* *argumenten*: Het zelfde woordenboek dat tot de [`getMetadata:`](#getMeta) vraag werd overgegaan. Dit wordt verstrekt om de toepassing toe te staan om de verzoeken met de reacties te passen.

**teweeggebracht door:** [`getMetadata:`](#getMeta)

**Meer Informatie:** [ Metagegevens van de Gebruiker ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Terug naar boven...](#apis)

</br>

### MVPD {#mvpd}

**Dossier:** AccessEnabler/headers/model/MVPD.h

**Beschrijving** beschrijft het voorwerp MVPD. Kan worden gebruikt om informatie over de eigenschappen van MVPD te verkrijgen.

**Beschikbaarheid:** v1.0+ [ boardingStatus bezit is beschikbaar van v2.2 ]

**Eigenschappen**:

* (NSString) identiteitskaart - identiteitskaart MVPD
* (NSString) displayName - de naam MVPD. [ dit zou moeten worden gebruikt aan vertoning in de plukker ]
* (NSString) logoURL - het MVPD logoadres.
* (BOOL) enablePlatformServices - als waar, steunt MVPD de diensten SSO zoals [ Apple SSO ](#presentTvDialog).
* (NSString) boardingStatus - kan 3 waarden hebben:
   * nil - De MVPD ondersteunt Apple SSO niet.
   * PICKER - MVPD kan in de plukker van Apple verschijnen maar de authentificatiestroom wordt gedaan door Adobe.
   * ONDERSTEUND - MVPD wordt volledig gesteund door Apple en zal het teken van Apple gebruiken SSO.

[Terug naar boven...](#apis)

</br>

## Gebeurtenissen bijhouden {#tracking}

AccessEnabler teweegbrengt een extra callback teweeg die niet noodzakelijk met de machtigingsstromen verwant is. Het implementeren van de callback-functie [`sendTrackingData()`](#sendTracking) is optioneel, maar de toepassing kan specifieke gebeurtenissen volgen en statistieken compileren, zoals het aantal geslaagde/mislukte verificatie-/autorisatiepogingen.

### sendTrackingData :forEventType: {#sendTracking}

**Dossier:** AccessEnabler/headers/EntitlementDelegate.h

**Omschrijving** Callback die door AccessEnabler wordt teweeggebracht signalerend aan de toepassing het voorkomen van diverse gebeurtenissen zoals de voltooiing/de mislukking van authentificatie/vergunningsstromen. Bij Adobe Pass-verificatie 1.6 worden het apparaattype, het AccessEnabler-clienttype en het besturingssysteem gerapporteerd door [`sendTrackingData()`](#sendTracking) . De callback van [`sendTrackingData()`](#sendTracking) blijft compatibel met oudere versies.

**Callback: het volgen gebeurtenissen**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Beschikbaarheid:** v1.0+

**Nota:** het apparatentype en het werkende systeem worden afgeleid door het gebruik van een openbare bibliotheek van Java (<http://java.net/projects/user-agent-utils>) en het koord van de gebruikersagent. Deze informatie wordt alleen verstrekt als een ruwe manier om operationele meetgegevens in apparatencategorieën op te delen, maar die Adobe kan geen verantwoordelijkheid voor onjuiste resultaten nemen. Gebruik de nieuwe functionaliteit.

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

* *gebeurtenis*: de code van de gebeurtenis die wordt gevolgd. Er zijn drie mogelijke gebeurtenistypen voor bijhouden:
   * **authenticationDetection:** om het even welke tijd een verzoek van het toestemmingstoken terugkeert (de gebeurtenis is `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** om het even welke tijd een authentificatiecontrole voorkomt (de gebeurtenis is `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** wanneer de gebruiker een MVPD in de MVPD selectievorm selecteert (de gebeurtenis is `TRACKING_GET_SELECTED_PROVIDER`)
* *gegevens*: extra gegevens die aan de gemelde gebeurtenis worden geassocieerd. Deze gegevens worden gepresenteerd in de vorm van een lijst met waarden.

**teweeggebracht door:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instructies voor het interpreteren van de waarden in de *gegevens* serie:

* Voor trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - of het symbolische verzoek succesvol (waar/vals) was en als succesvol:
   * **1** - MVPD identiteitskaart koord
   * **2** - GUID (md5 hashed)
   * **3** - Symbolisch reeds in geheim voorgeheugen (waar/vals)
   * **4** - het type van Apparaat
   * **5** - Het cliënttype van AccessEnabler
   * **6** - Het type van werkend systeem

* Voor trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - of het symbolische verzoek succesvol (waar/vals) was en als succesvol:
   * **1** - identiteitskaart MVPD
   * **2** - GUID (md5 hashed)
   * **3** - Symbolisch reeds in geheim voorgeheugen (waar/vals)
   * **4** - Fout
   * **5** - Details
   * **6** - het type van Apparaat
   * **7** - Het cliënttype van AccessEnabler
   * **8** - Het type van werkend systeem
* Voor trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - identiteitskaart van momenteel geselecteerde MVPD
   * **1** - het type van Apparaat
   * **2** - Het cliënttype van AccessEnabler
   * **3** - Het type van werkend systeem

</br>
