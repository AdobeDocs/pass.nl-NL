---
title: iOS/tvOS Cookbook
description: iOS/tvOS Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 0%

---

# iOS/tvOS SDK Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

In dit document worden de machtigingsworkflows beschreven die de toepassing op hoofdniveau van een programmeur kan implementeren via de API&#39;s die worden weergegeven door de iOS/tvOS AccessEnabler-bibliotheek.

De Adobe Pass-verificatieoplossing voor iOS/tvOS bestaat uiteindelijk uit twee domeinen:

* Het domein UI - dit is de bovenste toepassingslaag die UI uitvoert en de diensten gebruikt die door de bibliotheek AccessEnabler worden verleend om toegang tot beperkte inhoud te verlenen.

* Het domein AccessEnabler - dit is waar de machtigingswerkschema&#39;s in de vorm van worden uitgevoerd:

   * De vraag van het netwerk die aan de backendservers van de Adobe wordt gemaakt
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

## De service voor Experience Cloud-id configureren (bezoeker-id) {#visitorIDSetup}

Het vormen van [Experience Cloud-id](https://experienceleague.adobe.com/docs/id-service/using/home.html) waarde is belangrijk vanuit de [!DNL Analytics] standpunt. Eenmaal `visitorID` waarde wordt geplaatst, verzendt SDK deze informatie samen met elke netwerkvraag en [!DNL Adobe Pass] Deze gegevens worden verzameld door de verificatieserver. U kunt de analyses van de Adobe Pass Authentication-service koppelen aan andere analytische rapporten die u hebt van andere toepassingen of websites. Informatie over het instellen van bezoekerID vindt u [hier](#setOptions).

## Machtigingsstromen {#entitlement}

A.  [Vereisten](#prereqs) </br>
B.  [Stroom opstarten](#startup_flow) </br>
C.  [Verificatiestroom met Apple SSO](#authn_flow_wo_applesso)  </br>
D.  [Verificatiestroom met Apple SSO op iOS](#authn_flow_with_applesso) </br>
E.  [Verificatiestroom met Apple SSO op tvOS](#authn_flow_with_applesso_tvOS) </br>
F.  [Autorisatiestroom](#authz_flow) </br>
G.  [Media-stroom weergeven](#media_flow) </br>
H.  [Afmeldingsstroom zonder Apple SSO](#logout_flow_wo_AppleSSO) </br>
I.  [Afmeldingsstroom met Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Vereisten {#prereqs}

1. Maak uw callback-functies:
   * `setRequestorComplete()` </br>
   * geactiveerd door [setRequestor()](#$setReq), geeft succes of mislukking. </br>
   * Het succes wijst erop u met machtigingsvraag kunt te werk gaan.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * geactiveerd door [`getAuthentication()`](#$getAuthN) alleen als de gebruiker geen provider (MVPD) heeft geselecteerd en nog niet is geverifieerd. </br>
      * De `mvpds` parameter is een array van providers die beschikbaar zijn voor de gebruiker.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * geactiveerd door `checkAuthentication()` elke keer. </br>
      * geactiveerd door [`getAuthentication()`](#$getAuthN) alleen als de gebruiker al is geverifieerd en een provider heeft geselecteerd. </br>
      * De geretourneerde status is geslaagd of mislukt. De foutcode beschrijft het type fout.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * geactiveerd door [`getAuthentication()`](#$getAuthN) nadat de gebruiker een MVPD selecteert. De `url` parameter verstrekt de plaats van de MVPD login pagina.

   * `sendTrackingData(event, data)` </br>
      * geactiveerd door `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * De `event` parameter geeft aan welke machtigingsgebeurtenis heeft plaatsgevonden; de `data` parameter is een lijst van waarden met betrekking tot de gebeurtenis.

   * `setToken(token, resource)`

      * geactiveerd door [checkAuthorization()](#checkAuthZ) en [getAuthorization()](#$getAuthZ) na een geslaagde machtiging om een bron te bekijken.
      * De `token` parameter is het kortstondige media token; de `resource` parameter is de inhoud die de gebruiker mag bekijken.

   * `tokenRequestFailed(resource, code, description)` </br>
      * geactiveerd door [checkAuthorization()](#checkAuthZ) en [getAuthorization()](#$getAuthZ) na een niet-succesvolle toelating.
      * De `resource` parameter is de inhoud die de gebruiker probeerde te bekijken; de `code` parameter is de foutcode die aangeeft welk type fout is opgetreden; de `description` parameter beschrijft de fout die aan de foutencode wordt geassocieerd.

   * `selectedProvider(mvpd)` </br>
      * geactiveerd door [`getSelectedProvider()`](#getSelProv).
      * De `mvpd` parameter geeft informatie over de provider die door de gebruiker is geselecteerd.

   * `setMetadataStatus(metadata, key, arguments)`
      * geactiveerd door `getMetadata().`
      * De `metadata` parameter bevat de specifieke gegevens die u hebt aangevraagd; de `key` parameter is de sleutel die wordt gebruikt in de [getMetadata()](#getMeta) en de `arguments` parameter is hetzelfde woordenboek als dat waaraan is doorgegeven [getMetadata()](#getMeta).

   * [&quot;preauthorisedResources(authorisedResources)&quot;](#preauthResources)

      * geactiveerd door [`checkPreauthorizedResources()`](#checkPreauth).

      * De `authorizedResources` parameter geeft de bronnen weer die de gebruiker mag bekijken.

   * [` presentTvProviderDialog(viewController)`](#presentTvDialog)

      * geactiveerd door [getAuthentication()](#getAuthN) wanneer de huidige aanvrager minstens op MVPD steunt die steun SSO heeft.
      * De parameter viewController is de dialoog van Apple SSO en moet op het belangrijkste meningscontrolemechanisme worden voorgesteld.

   * [` dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Wordt geactiveerd door een gebruikershandeling (door &quot;Annuleren&quot; of &quot;Andere tv-providers&quot; te selecteren in het dialoogvenster Apple SSO).
      * De viewController parameter is de Dialoog van Apple SSO en moet van het belangrijkste meningscontrolemechanisme worden verworpen.

![](assets/iOS-flows.png)

### B. Startstroom {#startup_flow}

1. Start de toepassing op het hoogste niveau.</br>
1. Adobe Pass-verificatie starten </br>

   a. Bellen [`init`](#$init) om één enkel geval van de Authentificatie AccessEnabler van Adobe Pass te creëren.
   * **Afhankelijkheid:** Adobe Pass Authentication Native iOS/tvOS Library (AccessEnabler)

   b. Oproep `setRequestor()` om de identiteit van de programmeur vast te stellen; `requestorID` en (optioneel) een array met eindpunten voor Adobe Pass-verificatie. Voor tvOS zult u ook de openbare sleutel en het geheim moeten verstrekken. Zie [Clientloze documentatie](#create_dev) voor meer informatie.

   * **Afhankelijkheid:** Geldige Adobe Pass-verificatieaanvraag-id (werk dit samen met uw Adobe Pass-verificatieaccountmanager).

   * **Triggers:**
     [setRequestorComplete()](#$setReqComplete) callback.

   >[!NOTE]
   >
   >Er kunnen geen aanvragen voor een machtiging worden ingevuld totdat de identiteit van de aanvrager volledig is vastgesteld. Dit betekent in feite dat [`setRequestor()`](#$setReq)  nog steeds actief is, alle volgende aanvragen voor machtigingen. Bijvoorbeeld: [`checkAuthentication()`](#checkAuthN) zijn geblokkeerd.

   U hebt twee implementatieopties: wanneer de informatie van de aanvrager-identificatie naar de back-endserver is verzonden, kan de UI-toepassingslaag een van de volgende twee methoden kiezen: </br>

   1. Wacht op het in werking stellen van [`setRequestorComplete()`](#setReqComplete) callback (deel van de afgevaardigde AccessEnabler). Deze optie biedt de meeste zekerheid dat [`setRequestor()`](#$setReq) voltooid, dus wordt het aanbevolen voor de meeste implementaties.

   1. Doorgaan zonder te wachten op de activering van de [`setRequestorComplete()`](#setReqComplete) callback en begin met het afgeven van aanvragen voor machtigingen. Deze vraag (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorisedResource, getMetadata, logout) wordt een rij gevormd door de bibliotheek AccessEnabler, die de daadwerkelijke netwerkvraag na het [`setRequestor()`](#$setReq). Deze optie kan af en toe worden onderbroken als, bijvoorbeeld, de netwerkverbinding instabiel is.

1. Bellen `checkAuthentication()` om op een bestaande authentificatie te controleren zonder de volledige stroom van de Authentificatie in werking te stellen.  Als deze vraag slaagt, kunt u aan de stroom van de Vergunning direct te werk gaan. Zo niet, ga dan door naar de verificatiestroom.

   * **Afhankelijkheid:** Een geslaagde oproep tot [setRequestor()](#$setReq) (dit gebiedsdeel is ook op alle verdere vraag van toepassing).

   * **Triggers:** [setAuthenticationStatus()](#$setAuthNStatus) callback.


### C. Verificatiestroom zonder Apple SSO {#authn_flow_wo_applesso}

1. Bellen [`getAuthentication()`](#$getAuthN) om de verificatiestroom te starten of om te bevestigen dat de gebruiker al is geverifieerd.

   **Triggers:**

   * De [setAuthenticationStatus()](#$setAuthNStatus) callback, als de gebruiker reeds voor authentiek verklaard is. Ga in dit geval rechtstreeks door naar de [Autorisatiestroom](#authz_flow).

   * De [displayProviderDialog()](#$dispProvDialog) callback, als de gebruiker nog niet voor authentiek verklaard is.

1. De gebruiker de lijst met providers presenteren die is verzonden naar
   [`displayProviderDialog()`](#dispProvDialog).

1. Nadat de gebruiker een leverancier selecteert, verkrijg URL van MVPD van de gebruiker van `navigateToUrl:` of `navigateToUrl:useSVC:` callback en open een `UIWebView/WKWebView` of `SFSafariViewController` en stuurt die controller naar de URL.

1. Via de `UIWebView/WKWebView` of `SFSafariViewController` die in de vorige stap is geïnstantieerd, landt de gebruiker op de aanmeldingspagina van de MVPD en voert de aanmeldingsgegevens in. Er vinden verschillende omleidingsbewerkingen plaats binnen de controller.</br>

>[!NOTE]
>
>Op dit punt, heeft de gebruiker de kans om de authentificatiestroom te annuleren. Als dit voorkomt, is uw laag UI verantwoordelijk voor het informeren van AccessEnabler over deze gebeurtenis door te roepen [setSelectedProvider()](#setSelProv) with `null` als parameter. Dit staat AccessEnabler toe om het interne staat op te schonen en de Stroom van de Authentificatie terug te stellen.

1. Wanneer de gebruiker zich met succes heeft aangemeld, detecteert de toepassingslaag het laden van een specifieke aangepaste URL. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet door uw toepassing slechts als signaal worden geïnterpreteerd dat de authentificatiestroom heeft voltooid en dat het veilig is om het `UIWebView/WKWebView` of `SFSafariViewController` controller. In geval van een `SFSafariViewController`controller moet worden gebruikt. De specifieke aangepaste URL wordt gedefinieerd door de **`application's custom scheme`** (bijvoorbeeld`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke aangepaste URL gedefinieerd door de **`ADOBEPASS_REDIRECT_URL`** constant (d.w.z. `adobepass://ios.app`).

1. Sluit het UIWebView/WKWebView of SFSafariViewController controlemechanisme en vraag AccessEnabler `handleExternalURL:url` API methode, die AccessEnabler opdraagt om het authentificatietoken van de backendserver terug te winnen.

1. (Facultatief) Vraag [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker wordt gemachtigd om te bekijken. De `resources` parameter is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsinformatie van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback
   * **Uitvoeringspunt:** Na de voltooide verificatiestroom

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### D. Verificatiestroom met Apple SSO op iOS {#authn_flow_with_applesso}

1. Bellen [`getAuthentication()`](#$getAuthN) om de verificatiestroom te starten of om te bevestigen dat de gebruiker al is geverifieerd.
   **Triggers:**

   * De [presentTVProviderDialog()](#presentTvDialog) callback, als de gebruiker niet voor authentiek wordt verklaard en de huidige aanvrager minstens op MVPD heeft die SSO steunt. Als geen MVPDs SSO steunt, zal de klassieke authentificatiestroom worden gebruikt.

1. Nadat de gebruiker een leverancier selecteert zal de bibliotheek AccessEnabler een authentificatietoken van de informatie verkrijgen die door het kader van Apple VSA wordt verstrekt.

1. De [setAuthenticationStatus()](#setAuthNStatus) callback wordt geactiveerd. Op dit moment moet de gebruiker worden geverifieerd met Apple SSO.

1. [Optioneel] Bellen [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker wordt gemachtigd om te bekijken. De `resources` parameter is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsinformatie van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback
   * **Uitvoeringspunt:** Na de voltooide verificatiestroom

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### E. Verificatiestroom met Apple SSO op tvOS {#authn_flow_with_applesso_tvOS}

1. Bellen [`getAuthentication()`](#$getAuthN) om de verificatiestroom te starten of om te bevestigen dat de gebruiker al is geverifieerd.
   **Triggers:**
   * De [`presentTvProviderDialog()`](#presentTvDialog) callback, als de gebruiker niet voor authentiek wordt verklaard en de huidige aanvrager minstens op MVPD heeft die SSO steunt. Als geen MVPDs SSO steunt, zal de klassieke authentificatiestroom worden gebruikt.

1. Nadat de gebruiker een provider heeft geselecteerd, [`status()`](#status_callback_implementation) callback wordt aangeroepen. Er wordt een registratiecode opgegeven en de AccessEnabler-bibliotheek begint de server te pollen voor een geslaagde verificatie op het tweede scherm.

1. Als de opgegeven registratiecode is gebruikt voor verificatie op het tweede scherm, wordt de instelling [`setAuthenticatiosStatus()`](#setAuthNStatus) callback wordt geactiveerd. Op dit moment moet de gebruiker worden geverifieerd met Apple SSO.
1. [Optioneel] Bellen [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker wordt gemachtigd om te bekijken. De `resources` parameter is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker. U kunt onder andere de machtigingsinformatie van de MVPD van de gebruiker gebruiken om de gebruikersinterface te versieren (bijvoorbeeld vergrendelde/ontgrendelde symbolen naast beveiligde inhoud).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback

   * **Uitvoeringspunt:** Na de voltooide verificatiestroom
1. Ga door naar de machtigingsstroom als de verificatie is gelukt.

### F. Vergunningsstroom {#authz_flow}

1. Bellen [getAuthorization()](#$getAuthZ) de vergunningsstroom in gang te zetten.

   * **Afhankelijkheid:** Geldige ResourceID(s) overeengekomen met de MVPD(s).
   * Middel IDs zou het zelfde moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn. Voor informatie over Middel IDs, zie [Beveiligde bronnen identificeren](/help/authentication/identify-protected-resources.md)

1. Verificatie en autorisatie valideren.

   * Als de [getAuthorization()](#$getAuthZ) De vraag slaagt: De gebruiker heeft geldige tokens AuthN en AuthZ (de gebruiker wordt voor authentiek verklaard en gemachtigd om de gevraagde media te bekijken).

   * Indien [getAuthorization()](#$getAuthZ) ontbreekt: Onderzoek de geworpen uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
      * Als het een authentificatie (AuthN) fout was dan herstart de authentificatiestroom.
      * Als het een autorisatiefout (AuthZ) was, dan is de gebruiker niet geautoriseerd om de gevraagde media te bekijken en een soort foutbericht zou aan de gebruiker moeten worden getoond.
      * Als er een ander type fout is opgetreden (verbindingsfout, netwerkfout, enz.) geeft u vervolgens een geschikt foutbericht weer aan de gebruiker.

1. Valideer de token voor korte media.\
   Gebruik de Adobe Pass Authentication Media Token Verifier-bibliotheek om het kortstondige mediatoken te verifiëren dat door de [getAuthorization()](#$getAuthZ) oproep hierboven:

   * Als de validatie is gelukt: de gevraagde media afspelen voor de gebruiker.
   * Als de validatie mislukt: de AuthZ-token was ongeldig, wordt de mediaquery geweigerd en wordt een foutbericht weergegeven aan de gebruiker.


1. Ga terug naar de normale stroom van de toepassing.

### G. Media-stroom weergeven {#media_flow}

1. De gebruiker selecteert de media die u wilt weergeven.
1. Zijn de media beveiligd? Uw toepassing controleert of het geselecteerde medium is beveiligd:

   * Als de geselecteerde media is beveiligd, start uw toepassing de [Autorisatiestroom](#authz_flow) hierboven.

   * Als het geselecteerde medium niet is beveiligd, speelt u de media voor de gebruiker af.

### H. Logout Flow zonder Apple SSO {#logout_flow_wo_AppleSSO}

1. Bellen [`logout()`](#$logout) om de gebruiker af te melden. AccessEnabler ontruimt alle caching waarden en tekenen. Na het ontruimen van het geheime voorgeheugen, richt AccessEnabler een servervraag om de server-zijzittingen schoon te maken. Merk op dat aangezien de servervraag in SAML kon resulteren die aan IdP (dit staat voor de zittingsschoonmaak op de kant IdP toe) wordt omgeleid, deze vraag moet alle omleidingen volgen. Om deze reden, moet deze vraag binnen een controlemechanisme UIWebView/WKWebView of SFSafariViewController worden behandeld.

   a. Na het zelfde patroon zoals het authentificatiewerkschema, doet het domein AccessEnabler een verzoek aan de UI toepassingslaag, via `navigateToUrl:` of `navigateToUrl:useSVC:` callback, om een controlemechanisme te creëren UIWebView/WKWebView of SFSafariViewController en die opdracht te geven om URL te laden die in callback wordt verstrekt `url` parameter. Dit is URL van het logout eindpunt op de achtergrondserver.

   b. Uw toepassing moet de activiteit van de `UIWebView/WKWebView or SFSafariViewController` en detecteert het moment waarop een specifieke aangepaste URL wordt geladen, terwijl verschillende omleidingen worden doorlopen. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Deze mag alleen door uw toepassing worden geïnterpreteerd als een signaal dat de afmeldingsflow is voltooid en dat het veilig is om de `UIWebView/WKWebView` of `SFSafariViewController` controller. Wanneer de controller deze specifieke aangepaste URL laadt, moet de toepassing het dialoogvenster `UIWebView/WKWebView or SFSafariViewController` controlemechanisme en vraag AccessEnabler `handleExternalURL:url`API-methode. In geval van een `SFSafariViewController`controller moet worden gebruikt. De specifieke aangepaste URL wordt gedefinieerd door de **`application's custom scheme`** (bijvoorbeeld `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke aangepaste URL gedefinieerd door de **`ADOBEPASS_REDIRECT_URL`**  constant (d.w.z. `adobepass://ios.app`).

   >[!NOTE]
   >
   >De logout stroom verschilt van de authentificatiestroom in die zin dat de gebruiker niet wordt vereist om met UIWebView/WKWebView of SFSafariViewController op om het even welke manier in wisselwerking te staan. De toepassingslaag UI gebruikt UIWebView/WKWebView of SFSafariViewController om ervoor te zorgen dat alle omleidingen worden gevolgd. Daarom is het mogelijk (en aanbevolen) om de controller tijdens het aflogproces onzichtbaar te maken (dat wil zeggen verborgen).


### I. Afmeldingsstroom met Apple SSO {#logout_flow_with_AppleSSO}

1. Bellen [`logout()`](#$logout) om de gebruiker af te melden.
1. De [status()](#status_callback_implementation) callback zal met id VSA203 worden geroepen.
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
