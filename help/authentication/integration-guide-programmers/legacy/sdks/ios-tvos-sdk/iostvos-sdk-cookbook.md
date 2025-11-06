---
title: iOS/tvOS Cookbook
description: iOS/tvOS Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '2424'
ht-degree: 0%

---

# (Verouderd) iOS/tvOS SDK Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#intro}

In dit document worden de machtigingsworkflows beschreven die de toepassing op hoofdniveau van een programmeur kan implementeren via de API&#39;s die worden weergegeven door de iOS/tvOS AccessEnabler-bibliotheek.

De Adobe Pass-verificatieoplossing voor iOS/tvOS bestaat uiteindelijk uit twee domeinen:

* Het domein UI - dit is de bovenste toepassingslaag die UI uitvoert en de diensten gebruikt die door de bibliotheek AccessEnabler worden verleend om toegang tot beperkte inhoud te verlenen.

* Het domein AccessEnabler - dit is waar de machtigingswerkschema&#39;s in de vorm van worden uitgevoerd:

   * Netwerkaanroepen naar Adobe-backendservers
   * Zakelijke en logische regels met betrekking tot de workflows voor verificatie en autorisatie
   * Beheer van diverse bronnen en verwerking van workflowstatus (zoals de tokencache)

Het doel van het domein AccessEnabler is alle ingewikkeldheid van de machtigingswerkschema&#39;s te verbergen, en aan de hogere laagtoepassing (door de bibliotheek AccessEnabler) een reeks eenvoudige machtigingsprimitieven te verstrekken waarmee u de machtigingswerkschema&#39;s uitvoert:

1. De identiteit van de aanvrager instellen
1. Verificatie aanvragen bij een bepaalde identiteitsprovider
1. Autorisatie voor een bepaalde bron controleren en ophalen
1. Afmelden
1. SSO van Apple stroomt door het VSA-kader van Apple te verfijnen

De het netwerkactiviteit van AccessEnabler vindt plaats in zijn eigen draad, zodat wordt de draad UI nooit geblokkeerd. Het communicatiekanaal in twee richtingen tussen de twee toepassingsdomeinen moet daarom een volledig asynchroon patroon volgen:

* De toepassingslaag UI verzendt berichten naar het domein AccessEnabler via de API vraag die door de bibliotheek AccessEnabler wordt blootgesteld.
* AccessEnabler antwoordt aan de laag UI door de callback methodes inbegrepen in het protocol AccessEnabler dat de laag UI met de bibliotheek AccessEnabler registreert.

## De Experience Cloud ID-service configureren (bezoeker-id) {#visitorIDSetup}

Het vormen van de [&#x200B; identiteitskaart van Experience Cloud &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=nl-NL) waarde is belangrijk van het [!DNL Analytics] standpunt. Wanneer een `visitorID` -waarde is ingesteld, verzendt de SDK deze informatie samen met elke netwerkaanroep en verzamelt de [!DNL Adobe Pass] -verificatieserver deze informatie. U kunt de analyses van de Adobe Pass Authentication-service koppelen aan andere analytische rapporten die u hebt van andere toepassingen of websites. De informatie over hoe te opstellings bezoekorID kan [&#x200B; hier &#x200B;](#setOptions) worden gevonden.

## Machtigingsstromen {#entitlement}

A. [&#x200B; Eerste vereisten &#x200B;](#prereqs) </br>
B. [&#x200B; Stroom van de Opstarten &#x200B;](#startup_flow) </br>
C. [&#x200B; de Stroom van de Authentificatie met Apple SSO &#x200B;](#authn_flow_wo_applesso) </br>
D. [&#x200B; de Stroom van de Authentificatie met Apple SSO op iOS &#x200B;](#authn_flow_with_applesso) </br>
E. [&#x200B; de Stroom van de Authentificatie met Apple SSO op tvOS &#x200B;](#authn_flow_with_applesso_tvOS) </br>
F. [&#x200B; de Stroom van de Vergunning &#x200B;](#authz_flow) </br>
G. [&#x200B; de Stroom van Media van de Mening &#x200B;](#media_flow) </br>
H. [&#x200B; Logout Stroom zonder Apple SSO &#x200B;](#logout_flow_wo_AppleSSO) </br>
I. [&#x200B; Logout Stroom met Apple SSO &#x200B;](#logout_flow_with_AppleSSO) </br>


### A. Vereisten {#prereqs}

1. Maak uw callback-functies:
   * `setRequestorComplete()` </br>
   * Teweeggebracht door [&#x200B; setRequest () &#x200B;](#$setReq), keert succes of mislukking terug. </br>
   * Het succes wijst erop u met machtigingsvraag kunt te werk gaan.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Wordt alleen geactiveerd door [`getAuthentication()`](#$getAuthN) als de gebruiker geen provider (MVPD) heeft geselecteerd en nog niet is geverifieerd. </br>
      * De parameter `mvpds` is een array van providers die beschikbaar zijn voor de gebruiker.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Wordt telkens geactiveerd door `checkAuthentication()` . </br>
      * Wordt alleen geactiveerd door [`getAuthentication()`](#$getAuthN) als de gebruiker al is geverifieerd en een provider heeft geselecteerd. </br>
      * De geretourneerde status is geslaagd of mislukt. De foutcode beschrijft het type fout.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Wordt geactiveerd door [`getAuthentication()`](#$getAuthN) nadat de gebruiker een MVPD selecteert. De parameter `url` biedt de locatie van de MVPD-aanmeldingspagina.

   * `sendTrackingData(event, data)` </br>
      * Wordt geactiveerd door `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()` .
      * De parameter `event` geeft aan welke gebeurtenis entitlement heeft plaatsgevonden. De parameter `data` is een lijst met waarden die betrekking hebben op de gebeurtenis.

   * `setToken(token, resource)`

      * Teweeggebracht door [&#x200B; checkAuthorization () &#x200B;](#checkAuthZ) en [&#x200B; getAuthorization () &#x200B;](#$getAuthZ) na een succesvolle vergunning om een middel te bekijken.
      * De parameter `token` is het kortstondige media-token; de parameter `resource` is de inhoud die de gebruiker mag bekijken.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Teweeggebracht door [&#x200B; checkAuthorization () &#x200B;](#checkAuthZ) en [&#x200B; getAuthorization () &#x200B;](#$getAuthZ) na een onsuccesvolle vergunning.
      * De parameter `resource` is de inhoud die de gebruiker probeerde te bekijken. De parameter `code` is de foutcode die aangeeft welk type fout is opgetreden. De parameter `description` beschrijft de fout die aan de foutcode is gekoppeld.

   * `selectedProvider(mvpd)` </br>
      * Wordt geactiveerd door [`getSelectedProvider()`](#getSelProv) .
      * De parameter `mvpd` biedt informatie over de provider die door de gebruiker is geselecteerd.

   * `setMetadataStatus(metadata, key, arguments)`
      * geactiveerd door `getMetadata().`
      * De `metadata` parameter verstrekt de specifieke gegevens u vroeg; de `key` parameter is de sleutel die in [&#x200B; wordt gebruikt getMetadata () &#x200B;](#getMeta) verzoek; en de `arguments` parameter is het zelfde woordenboek dat tot [&#x200B; getMetadata () &#x200B;](#getMeta) werd overgegaan.

   * [&quot;preauthorisedResources(authorisedResources)&quot;](#preauthResources)

      * Wordt geactiveerd door [`checkPreauthorizedResources()`](#checkPreauth) .

      * De parameter `authorizedResources` geeft de bronnen weer die de gebruiker
is geautoriseerd om te bekijken.

   * [` presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Teweeggebracht door [&#x200B; getAuthentication () &#x200B;](#getAuthN) wanneer de huidige aanvrager minstens op MVPD steunt die steun SSO heeft.
      * De parameter viewController is de dialoog van Apple SSO en moet op het belangrijkste meningscontrolemechanisme worden voorgesteld.

   * [` dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Wordt geactiveerd door een gebruikershandeling (door &quot;Annuleren&quot; of &quot;Andere tv-providers&quot; te selecteren in het dialoogvenster Apple SSO).
      * De viewController parameter is de Dialoog van Apple SSO en moet van het belangrijkste meningscontrolemechanisme worden verworpen.

![](../../../../assets/iOS-flows.png)

### B. Startstroom {#startup_flow}

1. Start de toepassing op het hoogste niveau.</br>
1. Adobe Pass-verificatie starten </br>

   a. Vraag [`init`](#$init) aan om één instantie van Adobe Pass Authentication AccessEnabler te maken.
   * **Afhankelijkheid:** Eigen iOS/tvOS Bibliotheek van de Authentificatie van Adobe Pass (AccessEnabler)

   b. Roep `setRequestor()` aan om de identiteit van de programmeur vast te stellen; geef `requestorID` van de programmeur en (optioneel) een array van eindpunten voor Adobe Pass-verificatie door. Voor tvOS zult u ook de openbare sleutel en het geheim moeten verstrekken. Zie [&#x200B; Clientless documentatie &#x200B;](#create_dev) voor details.

   * **Afhankelijkheid:** Geldige VraagID van de Authentificatie van Adobe Pass (het Werk met uw Rekening van de Authentificatie van Adobe Pass
Manager om dit te rangschikken).

   * **Trekkers:**
     [&#x200B; setRequestorComplete () &#x200B;](#$setReqComplete) callback.

   >[!NOTE]
   >
   >Er kunnen geen aanvragen voor een machtiging worden ingevuld totdat de identiteit van de aanvrager volledig is vastgesteld. Dit betekent in feite dat terwijl [`setRequestor()`](#$setReq) nog wordt uitgevoerd, alle volgende aanvragen voor machtigingen worden uitgevoerd. [`checkAuthentication()`](#checkAuthN) worden bijvoorbeeld geblokkeerd.

   U hebt twee implementatieopties: wanneer de informatie over de identificatie van de aanvrager naar de back-endserver is verzonden, kan de toepassingslaag van de gebruikersinterface een van de volgende twee methoden kiezen: </br>

   1. Wacht op het teweegbrengen van [`setRequestorComplete()`](#setReqComplete) callback (deel van de afgevaardigde AccessEnabler). Deze optie biedt de meeste zekerheid dat [`setRequestor()`](#$setReq) is voltooid en wordt daarom aangeraden voor de meeste implementaties.

   1. Ga verder zonder te wachten op het activeren van de callback van [`setRequestorComplete()`](#setReqComplete) en geef aanvragen voor machtigingen af. Deze vraag (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorisedResource, getMetadata, logout) wordt een rij gevormd door de bibliotheek AccessEnabler, die de daadwerkelijke netwerkvraag na [`setRequestor()`](#$setReq) zal maken. Deze optie kan af en toe worden onderbroken als, bijvoorbeeld, de netwerkverbinding instabiel is.

1. Roep `checkAuthentication()` aan om op een bestaande authentificatie te controleren zonder de volledige stroom van de Authentificatie in werking te stellen.  Als deze vraag slaagt, kunt u aan de stroom van de Vergunning direct te werk gaan. Zo niet, ga dan door naar de verificatiestroom.

   * **Afhankelijkheid:** Een succesvolle vraag aan [&#x200B; setRequestor () &#x200B;](#$setReq) (dit gebiedsdeel is eveneens op alle verdere vraag van toepassing).

   * **Trekkers:** [&#x200B; setAuthenticationStatus () &#x200B;](#$setAuthNStatus) callback.


### C. Verificatiestroom zonder Apple SSO {#authn_flow_wo_applesso}

1. Roep [`getAuthentication()`](#$getAuthN) aan om de verificatiestroom te starten of om bevestiging te krijgen dat de gebruiker al is
geverifieerd.

   **Trekkers:**

   * [&#x200B; setAuthenticationStatus () &#x200B;](#$setAuthNStatus) callback, als de gebruiker reeds voor authentiek verklaard is. In dit geval, ga direct aan de [&#x200B; Stroom van de Vergunning &#x200B;](#authz_flow) te werk.

   * [&#x200B; displayProviderDialog () &#x200B;](#$dispProvDialog) callback, als de gebruiker nog niet voor authentiek verklaard is.

1. De gebruiker de lijst met providers presenteren die is verzonden naar
   [`displayProviderDialog()`](#dispProvDialog).

1. Nadat de gebruiker een provider heeft geselecteerd, haalt u de URL van de MVPD van de gebruiker op via de callback `navigateToUrl:` of `navigateToUrl:useSVC:` en opent u een controller `UIWebView/WKWebView` of `SFSafariViewController` en stuurt u die controller naar de URL.

1. Via de `UIWebView/WKWebView` - of `SFSafariViewController` -code die in de vorige stap is geïnstantieerd, landt de gebruiker op de MVPD-aanmeldingspagina en voert deze de aanmeldingsgegevens in. Verscheidene omleidingsverrichtingen vinden binnen het controlemechanisme plaats.</br>

>[!NOTE]
>
>Op dit punt, heeft de gebruiker de kans om de authentificatiestroom te annuleren. Als dit voorkomt, is uw laag UI verantwoordelijk voor het informeren van AccessEnabler over deze gebeurtenis door [&#x200B; setSelectedProvider () te roepen &#x200B;](#setSelProv) met `null` als parameter. Dit staat AccessEnabler toe om het interne staat op te schonen en de Stroom van de Authentificatie terug te stellen.

1. Wanneer de gebruiker zich met succes heeft aangemeld, detecteert de toepassingslaag het laden van een specifieke aangepaste URL. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Deze mag alleen door uw toepassing worden geïnterpreteerd als een signaal dat de verificatiestroom is voltooid en dat het veilig is om de `UIWebView/WKWebView` - of `SFSafariViewController` -controller te sluiten. In het geval dat a `SFSafariViewController` controlemechanisme wordt vereist om worden gebruikt wordt de specifieke douane URL bepaald door **`application's custom scheme`** (b.v. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt dit specifieke douane URL bepaald door de **`ADOBEPASS_REDIRECT_URL`** constante (namelijk `adobepass://ios.app`).

1. Sluit het UIWebView/WKWebView of SFSafariViewController controlemechanisme en vraag de API methode van AccessEnabler `handleExternalURL:url`, die AccessEnabler opdraagt om het authentificatietoken van de backendserver terug te winnen.

1. (Optioneel) Roep [`checkPreauthorizedResources(resources)`](#$checkPreauth) aan om te controleren welke bronnen de gebruiker mag bekijken. De parameter `resources` is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsgegevens van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Trekkers:** [`preauthorizedResources()`](#preauthResources) callback
   * **het punt van de Uitvoering:** na de voltooide Stroom van de Authentificatie

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### D. Verificatiestroom met Apple SSO op iOS {#authn_flow_with_applesso}

1. Vraag [`getAuthentication()`](#$getAuthN) om de authentificatiestroom in werking te stellen, of bevestiging te krijgen dat de gebruiker reeds voor authentiek verklaard is.
   **Trekkers:**

   * [&#x200B; presentTvProviderDialog () &#x200B;](#presentTvDialog) callback, als de gebruiker niet voor authentiek wordt verklaard en de huidige aanvrager heeft minstens op MVPD die SSO steunt. Als geen MVPDs SSO steunt, zal de klassieke authentificatiestroom worden gebruikt.

1. Nadat de gebruiker een leverancier selecteert zal de bibliotheek AccessEnabler een authentificatietoken van de informatie verkrijgen die door het kader van Apple VSA wordt verstrekt.

1. [&#x200B; setAuthenticationStatus () &#x200B;](#setAuthNStatus) callback zal worden teweeggebracht. Op dit moment moet de gebruiker worden geverifieerd met Apple SSO.

1. [ Facultatieve ] Vraag [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker aan mening wordt gemachtigd. De parameter `resources` is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsgegevens van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Trekkers:** [`preauthorizedResources()`](#preauthResources) callback
   * **het punt van de Uitvoering:** na de voltooide Stroom van de Authentificatie

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### E. Verificatiestroom met Apple SSO op tvOS {#authn_flow_with_applesso_tvOS}

1. Roep [`getAuthentication()`](#$getAuthN) aan om de
verificatiestroom of om bevestiging te krijgen dat de gebruiker al
geverifieerd.
   **Trekkers:**
   * De callback van [`presentTvProviderDialog()`](#presentTvDialog) als de gebruiker niet voor authentiek wordt verklaard en de huidige aanvrager minstens op MVPD heeft die SSO steunt. Als geen MVPDs SSO steunt, zal de klassieke authentificatiestroom worden gebruikt.

1. Nadat de gebruiker een provider selecteert, wordt de callback van [`status()`](#status_callback_implementation) aangeroepen. Er wordt een registratiecode opgegeven en de AccessEnabler-bibliotheek begint de server te pollen voor een geslaagde verificatie op het tweede scherm.

1. Als de opgegeven registratiecode is gebruikt voor verificatie op het tweede scherm, wordt de callback van [`setAuthenticatiosStatus()`](#setAuthNStatus) geactiveerd. Op dit moment moet de gebruiker worden geverifieerd met Apple SSO.
1. [ Facultatieve ] Vraag [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker aan mening wordt gemachtigd. De parameter `resources` is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsgegevens van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Trekkers:** [`preauthorizedResources()`](#preauthResources) callback

   * **het punt van de Uitvoering:** na de voltooide Stroom van de Authentificatie
1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### F. Vergunningsstroom {#authz_flow}

1. Vraag [&#x200B; getAuthorization () &#x200B;](#$getAuthZ) om de vergunningsstroom in werking te stellen.

   * **Afhankelijkheid:** Geldige ResourceID(s) overeengekomen met MVPD(s).
   * Middel IDs zou het zelfde moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn. Voor informatie over Middel IDs, zie [&#x200B; Herkenningsteken van het Middel &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)

1. Verificatie en autorisatie valideren.

   * Als de [&#x200B; getAuthorization () &#x200B;](#$getAuthZ) vraag slaagt: De gebruiker heeft geldige tokens AuthN en AuthZ (de gebruiker wordt voor authentiek verklaard en gemachtigd om op de gevraagde media te letten).

   * Als [&#x200B; getAuthorization () &#x200B;](#$getAuthZ) ontbreekt: Onderzoek de geworpen uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
      * Als het een authentificatie (AuthN) fout was dan herstart de authentificatiestroom.
      * Als het een autorisatiefout (AuthZ) was, dan is de gebruiker niet geautoriseerd om de gevraagde media te bekijken en een soort foutbericht zou aan de gebruiker moeten worden getoond.
      * Als er een ander type fout is opgetreden (verbindingsfout, netwerkfout, enz.), geeft u een geschikt foutbericht weer voor de gebruiker.

1. Valideer de token voor korte media.\
   Gebruik de TokenVerificatiebibliotheek van de Media van de Authentificatie van Adobe Pass om het kortstondige media teken te verifiëren dat van [&#x200B; is teruggekeerd getAuthorization () &#x200B;](#$getAuthZ) hierboven vraag:

   * Als de validatie is gelukt: de gevraagde media afspelen voor de gebruiker.
   * Als de validatie mislukt: de AuthZ-token was ongeldig, wordt de mediaquery geweigerd en wordt een foutbericht weergegeven aan de gebruiker.


1. Ga terug naar de normale stroom van de toepassing.

### G. Media-stroom weergeven {#media_flow}

1. De gebruiker selecteert de media die u wilt weergeven.
1. Zijn de media beveiligd? Uw toepassing controleert of het geselecteerde medium is beveiligd:

   * Als de geselecteerde media beschermd is, begint uw toepassing de [&#x200B; Stroom van de Vergunning &#x200B;](#authz_flow) hierboven.

   * Als het geselecteerde medium niet is beveiligd, kunt u het medium afspelen voor
de gebruiker.

### H. Logout Flow zonder Apple SSO {#logout_flow_wo_AppleSSO}

1. Roep [`logout()`](#$logout) aan om de gebruiker af te melden. AccessEnabler ontruimt alle caching waarden en tekenen. Na het ontruimen van het geheime voorgeheugen, richt AccessEnabler een servervraag om de server-zijzittingen schoon te maken. Merk op dat aangezien de servervraag in SAML kon resulteren die aan IdP (dit staat voor de zittingsschoonmaak op de kant IdP toe) wordt omgeleid, deze vraag moet alle omleidingen volgen. Om deze reden, moet deze vraag binnen een controlemechanisme UIWebView/WKWebView of SFSafariViewController worden behandeld.

   a. Na het zelfde patroon als het authentificatiewerkschema, doet het domein AccessEnabler een verzoek aan de UI toepassingslaag, via `navigateToUrl:` of `navigateToUrl:useSVC:` callback, om een UIWebView/WKWebView of SFSafariViewController tot stand te brengen en die opdracht te geven om URL te laden die in de parameter van callback `url` wordt verstrekt. Dit is URL van het logout eindpunt op de achtergrondserver.

   b. Uw toepassing moet toezicht houden op de activiteit van de `UIWebView/WKWebView or SFSafariViewController` -controller en het moment detecteren waarop een specifieke aangepaste URL wordt geladen, aangezien deze meerdere omleidingen doorloopt. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Deze moet alleen door uw toepassing worden geïnterpreteerd als een signaal dat de afmeldingsflow is voltooid en dat het veilig is om de `UIWebView/WKWebView` - of `SFSafariViewController` -controller te sluiten. Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing het `UIWebView/WKWebView or SFSafariViewController` controlemechanisme sluiten en de 1&rbrace; API methode van AccessEnabler roepen `handleExternalURL:url`. Als een `SFSafariViewController` controlemechanisme moet worden gebruikt wordt de specifieke douane URL bepaald door **`application's custom scheme`** (bijvoorbeeld, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke douane URL bepaald door de **`ADOBEPASS_REDIRECT_URL`** constante (namelijk `adobepass://ios.app`).

   >[!NOTE]
   >
   >De logout stroom verschilt van de authentificatiestroom in die zin dat de gebruiker niet wordt vereist om met UIWebView/WKWebView of SFSafariViewController op om het even welke manier in wisselwerking te staan. De toepassingslaag UI gebruikt UIWebView/WKWebView of SFSafariViewController om ervoor te zorgen dat alle omleidingen worden gevolgd. Daarom is het mogelijk (en aanbevolen) om de controller tijdens het aflogproces onzichtbaar te maken (dat wil zeggen verborgen).


### I. Afmeldingsstroom met Apple SSO {#logout_flow_with_AppleSSO}

1. Roep [`logout()`](#$logout) aan om de gebruiker af te melden.
1. De [&#x200B; status () &#x200B;](#status_callback_implementation) callback zal met identiteitskaart VSA203 worden geroepen.
1. De gebruiker zou aan login van de systeemmontages ook moeten worden geïnstrueerd. Als u dit niet doet, wordt de verificatie opnieuw uitgevoerd wanneer de toepassing opnieuw wordt gestart.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
