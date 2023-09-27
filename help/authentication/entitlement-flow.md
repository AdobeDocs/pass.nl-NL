---
title: De machtigingsstroom van de programmeur
description: De machtigingsstroom van de programmeur
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---

# De machtigingsstroom van de programmeur {#prog-entitlement-flow}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

In dit document wordt de elementaire machtigingsstroom vanuit het perspectief van de programmeur beschreven.  Voor informatie over functies en gebruiksgevallen die verder gaan dan de hier besproken elementaire TVE-integratie, raadpleegt u [Gebruiksgevallen voor programmeerprogramma&#39;s](/help/authentication/programmer-use-cases.md).

De Authentificatie van Adobe Pass bemiddelen de machtigingsstroom tussen Programmers en MVPDs door veilige, verenigbare interfaces voor beide partijen te verstrekken.  Aan de kant van de programmeur biedt Adobe Pass Authentication twee algemene typen machtigingsinterface:

1. AccessEnabler - Een clientcomponent die een bibliotheek met API&#39;s voor toepassingen biedt op apparaten die webpagina&#39;s kunnen weergeven (bijvoorbeeld webapps, smartphone/tablet-apps).
2. Clientless API - RESTful-webservices voor apparaten die geen webpagina&#39;s kunnen weergeven (bijvoorbeeld set-top boxes, spelconsoles, smart TV&#39;s). De vereiste voor het weergeven van webpagina&#39;s vloeit voort uit de eis van het MVPD dat gebruikers zich op de website van het MVPD verifiëren.

Naast het platformneutrale overzicht dat hier wordt gepresenteerd, is er hier een API-specifiek overzicht zonder clips: documentatie voor de API zonder clips. AccessEnabler wordt native uitgevoerd op ondersteunde platforms (AS / JS op internet, doelstelling-C op iOS en Java op Android). De API&#39;s van AccessEnabler zijn consistent op alle ondersteunde platforms. Alle platforms die geen ondersteuning bieden voor AccessEnabler gebruiken dezelfde API zonder Clientless.

Voor beide typen interfaces zorgt Adobe Pass-verificatie veilig voor de overdracht van bevoegdheden tussen de toepassing van de programmeur en de MVPD van de gebruiker:

![](assets/prog-entitlement-flow.png)


*Afbeelding: Adobe Pass Authentication Ecosystem*

>[!IMPORTANT]
>
>Opmerking in het bovenstaande diagram: er is één onderdeel van de machtigingsstroom dat niet door Adobe Pass-verificatieservers wordt uitgevoerd: de MVPD-aanmelding. Gebruikers moeten zich aanmelden bij de aanmeldingspagina van hun MVPD. Vanwege deze vereiste moet de toepassing van de programmeur gebruikers op apparaten die geen webpagina&#39;s kunnen renderen, de opdracht geven om over te schakelen naar een apparaat voor web om zich aan te melden bij hun MVPD, waarna ze voor de rest van de machtigingsstroom terugkeren naar het oorspronkelijke apparaat.

## Entitlement Flow {#entitlement-flow}

Er zijn vier verschillende substromen die de basismachtigingsstroom vormen:

1. [Stroom opstarten](/help/authentication/entitlement-flow.md#startup)
1. [Verificatiestroom](/help/authentication/entitlement-flow.md#authentication)
1. [Autorisatiestroom](/help/authentication/entitlement-flow.md#authorization)
1. [Logout FLow](/help/authentication/entitlement-flow.md#logout)

Tijdens het eerste bezoek van een gebruiker aan de site van een programmeur gaat de machtigingsstroom verder in de bovenstaande volgorde. Nochtans, bij verdere bezoeken, afhankelijk van of de authentificatie en toestemmingstokens al dan niet zijn verlopen, of afhankelijk van het bekijken van beleid, zou een gebruiker slechts door één of twee van de substromen kunnen gaan.

### Stroom opstarten {#startup}

Vestigt de identiteit van de Programmer en het apparaat, voert initialisatietaken uit. Dit is een voorwaarde voor alle volgende machtigingsoproepen.

**AccessEnabled**

* **`setRequestor()`** - Hiermee wordt uw identiteit vastgesteld met de AccessEnalber en als extensie de Adobe Pass-verificatieservers. Deze aanroep is een voorloper van de rest van de machtigingsstroom. Bijvoorbeeld in JavaScript:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**Clientloze API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Afhankelijk van het platform zijn er mogelijk noodzakelijke taken die moeten worden voltooid voordat uw aanroepen van uw app worden geregistreerd. Zie de **Clientless API-documentatie** voor meer informatie. De Xbox-platforms vereisen bijvoorbeeld dat u de voorgeschreven beveiligingsstappen uitvoert voordat u regcode aanroept.

### Verificatiestroom {#authentication}

De succesvolle authentificatie produceert een teken AuthN verbonden aan het apparaat en de aanvrager. Succesvolle verificatie is een voorwaarde voor autorisatie.

**AccessEnabled**

* `checkAuthentication()` - Controleert of een geldig verificatietoken in de cache van een lokale token aanwezig is, zonder dat de volledige verificatiestroom wordt geactiveerd. Dit activeert de `setAuthenticationStatus()` callback-functie.
* `getAuthentication()` - Hiermee wordt de volledige verificatiestroom gestart. Als dit gelukt is, genereert Adobe Pass Authentication een AuthN-token en wordt deze op de client in cache opgeslagen. De gebruiker meldt zich op hun geselecteerde plaats MVPDs, die of in een iFrame, Popup venster, of een webmening wordt voorgesteld, afhankelijk van het platform. Dit activeert de displayProviderDialog().

**Clientloze API**

* `<FQDN>/.../checkauthn` - De webserviceversie van `checkAuthentication()` hierboven.
* `<FQDN>/.../config` - Hiermee wordt de lijst met MVPD&#39;s geretourneerd naar de app voor het tweede scherm.
* `<FQDN>/.../authenticate` - Hiermee wordt de verificatiestroom gestart vanuit de app voor het tweede scherm, waarbij gebruikers worden omgeleid naar hun geselecteerde MVPD voor aanmelding. Als dit gelukt is, genereert Adobe Pass Authentication een AuthN-token en slaat deze op de server op. De gebruiker keert terug naar het oorspronkelijke apparaat om de machtigingsstroom te voltooien.

Een token AuthN wordt als geldig beschouwd als de volgende twee punten waar zijn:

* De AuthN-token is niet verlopen
* MVPD verbonden aan het teken AuthN is op de lijst van toegestane MVPDs voor huidige identiteitskaart van de Aanvrager

#### Generic AccessEnabler Initial Authentication Workflow {#generic-ae-initial-authn-flow}

1. Uw app start de verificatieworkflow met een aanroep van `getAuthentication()`, die een geldig verificatietoken in de cache zoekt. Deze methode heeft een optionele `redirectURL` parameter; als u geen waarde opgeeft voor `redirectURL`, na een geslaagde verificatie wordt de gebruiker geretourneerd naar de URL waarvan de verificatie is geïnitialiseerd.
1. AccessEnabler bepaalt de huidige authentificatiestatus. Als de gebruiker momenteel voor authentiek wordt verklaard, roept AccessEnabler uw `setAuthenticationStatus()` callback functie, die een authentificatiestatus overgaat die op succes wijst.
1. Als de gebruiker niet voor authentiek wordt verklaard, gaat AccessEnabler de authentificatiestroom door te bepalen of de laatste authentificatiepoging van de gebruiker met bepaalde MVPD succesvol was. Als een MVPD-id in de cache is geplaatst EN de `canAuthenticate` markering is waar OF er is een MVPD geselecteerd met `setSelectedProvider()`wordt de gebruiker niet gevraagd het dialoogvenster MVPD-selectie te openen. De authentificatiestroom gaat verder gebruikend de caching waarde van MVPD (namelijk zelfde MVPD die tijdens de laatste succesvolle authentificatie werd gebruikt). Een netwerkvraag wordt gemaakt aan de backendserver, en de gebruiker wordt opnieuw gericht aan de MVPD login pagina.

1. Als er geen MVPD-id in de cache is opgeslagen en er geen MVPD is geselecteerd met `setSelectedProvider()` OF de `canAuthenticate` markering is ingesteld op false, de `displayProviderDialog()` callback wordt aangeroepen. Deze callback leidt uw app om UI tot stand te brengen die de gebruiker een lijst van MVPDs voorstelt om van te kiezen. Er wordt een array met MVPD-objecten geleverd, die de benodigde informatie bevat om de MVPD-kiezer te maken. Elk voorwerp MVPD beschrijft een entiteit MVPD, en bevat informatie zoals identiteitskaart van MVPD en URL waar het embleem MVPD kan worden gevonden.

1. Zodra een MVPD wordt geselecteerd, moet uw app AccessEnabler op de hoogte brengen van de keus van de gebruiker. Voor niet-Flash cliënten, zodra de gebruiker gewenste MVPD selecteert, informeert u AccessEnabler van de gebruikersselectie via een vraag aan `setSelectedProvider()` methode. Flash clients verzenden in plaats daarvan een gedeelde `MVPDEvent` van het type &quot;`mvpdSelection`&quot;, geeft u de geselecteerde provider door.

1. Wanneer AccessEnabler van de selectie van MVPD van de gebruiker wordt geïnformeerd, wordt een netwerkvraag gemaakt aan de backendserver, en de gebruiker wordt opnieuw gericht aan de MVPD login pagina.

1. Binnen het authentificatiewerkschema, communiceert AccessEnabler met de Authentificatie van Adobe Pass en geselecteerde MVPD om de geloofsbrieven van de gebruiker (gebruiker - identiteitskaart en wachtwoord) te vragen en hun identiteit te verifiëren. Terwijl sommige MVPDs aan hun eigen plaats voor login opnieuw richt, vereisen anderen u om hun login pagina binnen een iFrame te tonen. Uw pagina moet callback omvatten die tot een iFrame leidt, voor het geval de klant één van die MVPDs kiest.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Zodra de gebruiker met succes het programma heeft geopend, wint AccessEnabler het authentificatietoken terug en deelt uw app mee dat de authentificatiestroom volledig is. AccessEnabler roept de `setAuthenticationStatus()` callback met statuscode 1, die op succes wijst. Als er een fout optreedt tijdens het uitvoeren van deze stappen, `setAuthenticationStatus()` callback wordt teweeggebracht met een statuscode van 0, die op authentificatiemislukking, evenals een overeenkomstige foutencode wijst.

>[!IMPORTANT]
>Comcast is momenteel de enige MVPD die geen statische URL voor het logo biedt. Programmeurs moeten de nieuwste actuele logo&#39;s ophalen van [XFINITY Developer&#39;s portal](https://developers.xfinity.com/products/tv-everywhere).
>

### Autorisatiestroom {#authorization}

Autorisatie is een eerste vereiste voor het weergeven van beveiligde inhoud. Succesvolle autorisatie genereert een AuthZ-token, samen met een kortstondig Media Token dat voor beveiligingsdoeleinden aan de app van de programmeur wordt geleverd. Ter ondersteuning van de workflow voor machtigingen moet u de vereiste instellingen voor de aanvrager hebben uitgevoerd en de [Verificator mediatokens](/help/authentication/media-token-verifier-int.md). Met deze voltooide, kunt u vergunning in werking stellen.

Uw app start een autorisatie wanneer een gebruiker toegang tot een beveiligde bron aanvraagt. U geeft een bron-id door die de gewenste bron opgeeft (bijvoorbeeld een kanaal, aflevering, enz.). Uw app controleert eerst op een opgeslagen verificatietoken. Als er geen wordt gevonden, start u het verificatieproces.

**AccessEnabled**

* `checkAuthorization()` - Controleert de vergunning zonder de volledige vergunningsstroom in te leiden. Dit wordt vaak gebruikt om statusinformatie bij te werken die wordt weergegeven in de gebruikersinterface van de programmeerapp.

* `getAuthorization()` - Hiermee wordt de volledige autorisatiestroom gestart.

U verstrekt de volgende callback functies om de resultaten van de vergunningsvraag te behandelen:

* `setToken()` - Als de authentificatie eerder succesvol was en de vergunning slaagt, roept AccessEnabler uw `setToken()` callback functie, die het kortstondige media teken overgaat, die op een succesvolle conclusie aan de de machtigingsstroom van de Authentificatie van Adobe Pass wijst. (Voordat de gebruiker beveiligde inhoud mag bekijken, controleert de toepassing van de programmeur de geldigheid van het Media Token met behulp van de Media Token Verifier.

* `tokenRequestFailed()` - Als de gebruiker niet voor het gevraagde middel (of als de vraag om een andere reden ontbreekt) wordt geautoriseerd, roept AccessEnabler deze callback functie (plus uw eigen fout die functies meldt), die details over de mislukking overgaat.

**Clientloze API**

* `\<FQDN\>/.../authorize` - Hiermee wordt de volledige autorisatiestroom gestart.

#### Generic AccessEnabler Authorization Workflow {#generic-ae-authr-wf}

1. Verstrek een callback functie die uw toegewezen programmeur GUID van de Toegang registreert toelaat, gebruikend `setReqestor()`. Deze callback functie wordt geroepen wanneer AccessEnabler met succes is gedownload.

1. Bellen `getAuthorization()` wanneer een gebruiker toegang tot een beschermde bron aanvraagt. Gebruiken `getAuthorization()`, geeft u een bron-id door die de gewenste bron opgeeft (bijvoorbeeld een kanaal, aflevering, enz.). AccessEnabler zoekt naar een in de cache opgeslagen verificatietoken dat met het vergunningsverzoek moet worden doorgegeven. Als er geen wordt gevonden, wordt de verificatiestroom gestart.
1. Voorzie callback functies om de resultaten van vergunning te behandelen:

   * `setToken()` - Als de autorisatie is gelukt of als de gebruiker eerder geautoriseerd is, gaat de Access Enabler verder met het autorisatieproces door uw `setToken()` callback functie, die het kort-levende toestemmingstoken overgaat.

   * `tokenRequestFailed()` - Als de gebruiker niet voor het gevraagde middel wordt geautoriseerd (of als de vraag om een andere reden ontbreekt), roept AccessEnabler om het even welke fout rapporteringsfuncties aan u hebt geregistreerd, plus `tokenRequestFailed()` callback, die details over de mislukking overgaan.

### Afmeldingsstroom {#logout}

Wist tokens en andere gegevens verbonden aan de de machtigingsstroom van de huidige gebruiker.

**AccessEnabled**

* `logout()`

**Clientloze API**

* `\<FQDN\>/.../logout`

## Begrijp AccessEnabler gedrag {#ae-behavior}

Alle API-aanroepen van AccessEnabler zijn asynchroon (met één uitzondering vermeld in de API-referenties). U kunt API een willekeurig aantal tijden roepen, nochtans is er geen sterke garantie dat de acties die door de vraag teweeggebracht worden in de zelfde orde zullen worden voltooid dat de vraag werd gemaakt. (Een uitzondering hierop is de huidige runtime van de Flash Player; als u geen multi-threaded bent, worden aanroepen zeker *do* voltooid in de volgorde waarin ze worden aangeroepen.)

Om tussen reacties te onderscheiden, en reacties met vraag te kunnen paren, alle callbacks echo terug hun inputparameters. Dit omvat `setToken()` en`tokenRequestFailed()`, die uiteindelijk door `checkAuthorization()`. (Voor `checkAuthorization()` callbacks, wordt de gebruikte bron teruggekaatst.) Gebruikend uit deze eigenschap, kunt u onderscheiden welke reactie beantwoordt aan welke vraag. Als u deze functie wilt gebruiken, kunt u iets als volgt coderen:

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### Veelgestelde vragen over gedrag AE {#ae-beh-faq}

**Vraag. Wat gebeurt als ik een tweede vraag AccessEnabler maak alvorens de eerste vraag eindigt?**

De eerste vraag blijft uitvoeren aangezien de tweede vraag (asynchrone mededelingen) uitvoert.

**Vraag. Is er een maximumaantal gelijktijdige vraag die AccessEnabler kan steunen?**

Geen grens wordt uitdrukkelijk geplaatst in de code AccessEnabler, zodat bent u beperkt slechts door uw beschikbare systeemmiddelen, evenals door de capaciteit van MVPD.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
