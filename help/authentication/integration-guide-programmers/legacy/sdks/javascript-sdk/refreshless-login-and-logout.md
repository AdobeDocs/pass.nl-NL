---
title: Aanmelding en afmelding zonder vernieuwen
description: Aanmelding en afmelding zonder vernieuwen
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1762'
ht-degree: 0%

---

# (Verouderd) Aanmelding en afmelding zonder vernieuwen {#tefresh-less-login-and-logout}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

Voor webtoepassingen moet u rekening houden met enkele verschillende mogelijke scenario&#39;s voor het verifiëren en afmelden van gebruikers.  MVPDs vereist dat de gebruikers login op de Web-pagina van MVPD voor authentiek verklaren, met de volgende extra factoren die in spel komen:

- Sommige MVPD&#39;s vereisen een volledige omleiding van uw site naar de aanmeldingspagina
- Sommige MVPD&#39;s vereisen dat u een iFrame op uw site opent om de MVPD-aanmeldingspagina weer te geven
- Sommige browsers verwerken het iFrame-scenario niet goed, dus voor deze browsers is het beter om een popup-venster te gebruiken in plaats van het iFrame

Voorafgaand aan Adobe Pass-verificatie 2.7 zorgden al deze scenario&#39;s voor het verifiëren van een gebruiker voor een volledige pagina van de pagina van de programmeur. Voor 2.7 en volgende versies verbeterde het Adobe Pass-verificatieteam deze stromen, zodat de gebruiker geen pagina hoeft te vernieuwen in uw app tijdens het aanmelden en het afmelden.


## Gedetailleerde beschrijving {#detailed_description}

Laten we beginnen met een samenvatting van de oorspronkelijke verificatie- en logout-stromen, en dat vervolgens volgen met de verbeterde verificatie- en logout-stromen. Merk op dat de eerste vier secties normale MVPDs (niet-TempPass) richten, terwijl de laatste sectie de speciale implementatie beschrijft die voor TempPass moet worden toegepast:

- [Oorspronkelijke verificatiestroom](#orig_authn)
- [Oorspronkelijke afmeldingsstroom](#orig_logout)
- [Verbeterde verificatiestroom](#improved_authn)
- [Verbeterde afmeldingsstroom](#improved_logout)
- [TempPass Flow](#improved_temppass)

</br>

## Oorspronkelijke verificatie-/afmeldingsstromen {#orig_authn}

**Authentificatie**

De het Webcliënten van de Authentificatie van Adobe Pass hebben twee manieren om voor authentiek te verklaren, afhankelijk van de vereisten van MVPDs:

1. **Volledig-pagina Redirect -** Na de gebruiker selecteert een leverancier    (geconfigureerd met volledige paginaomleiding) van de MVPD-kiezer op de    De website van de programmeur, `setSelectedProvider(<mvpd>)` wordt aangehaald op AccessEnabler en de gebruiker wordt opnieuw gericht aan de MVPD login pagina. Nadat de gebruiker geldige geloofsbrieven verstrekt wordt hij opnieuw gericht terug naar de website van de Programmer. AccessEnabler wordt geïnitialiseerd en het authentificatietoken wordt teruggewonnen van de Authentificatie van Adobe Pass tijdens `setRequestor`.
1. **iFrame/Pop - PopVenster -** nadat de gebruiker een leverancier (die met iFrame wordt gevormd) selecteert, `setSelectedProvider(<mvpd>)` wordt aangehaald op AccessEnabler. Deze actie activeert de callback van `createIFrame(width, height)`, waarbij de programmeur op de hoogte wordt gesteld van het maken van een iFrame (of pop-up, afhankelijk van de browser/voorkeuren) met de naam `"mvpdframe"` en de opgegeven afmetingen. Nadat iFrame/popup wordt gecreeerd, laadt AccessEnabled de MVPD login pagina in iFrame/popup. De gebruiker verstrekt geldige geloofsbrieven en iFrame/popup wordt opnieuw gericht aan de Authentificatie van Adobe Pass, die een JS fragment terugkeert dat iFrame/popup sluit en de ouderpagina (de website van de Programmer) opnieuw laadt. Net als bij stroom 1 wordt het verificatietoken opgehaald tijdens `setRequestor` .

De `displayProviderDialog` callback (teweeggebracht door `getAuthentication`/ `getAuthorization`) keert een lijst van MVPDs en hun aangewezen montages terug. Met de eigenschap `iFrameRequired` van een MVPD kan de programmeur controleren of flow 1 of flow 2 moet worden geactiveerd. Merk op dat de programmeur extra actie (het creëren van een iFrame/popup) slechts voor stroom 2 moet ondernemen.

**annuleert Authentificatie**

Er is ook de situatie waarin de gebruiker uitdrukkelijk de authentificatiestroom door de login pagina te sluiten annuleert. Hier volgen de scenario&#39;s en de voorgestelde oplossing voor de programmeurs:

1. **Volledige pagina richt opnieuw -** wanneer de login pagina wordt gesloten, zal de gebruiker aan de website van de Programmer opnieuw moeten navigeren en de volledige stroom van het begin in werking stellen. In dit scenario is geen expliciete actie van de programmeur vereist.
1. **iFrame -** De programmeur wordt geadviseerd om iFrame binnen a `div` (of gelijkaardige component UI) te ontvangen die een Dichte knoop in bijlage aan het heeft. Wanneer de gebruiker op de knop Sluiten drukt, vernietigt de programmeur het iFrame samen met de bijbehorende UI en voert hij `setSelectedProvider(null)` uit. Deze vraag staat AccessEnabler toe om zijn interne staat te ontruimen en maakt het voor de gebruiker mogelijk om een verdere authentificatiestroom in werking te stellen. `setAuthenticationStatus` en `sendTrackingData(AUTHENTICATION_DETECTION...)` worden geactiveerd om een mislukte verificatiestroom aan te geven (zowel aan `getAuthentication` als `getAuthorization`).
1. **Popup -** sommige browsers kunnen niet nauwkeurig de gebeurtenis ontdekken van de vensterdichte, zodat moet een verschillende benadering hier worden genomen (in tegenstelling tot de iFrame stroom hierboven). De Adobe adviseert dat de programmeur een tijdopnemer initialiseert die periodiek het bestaan van login popup verifieert. Als het venster niet bestaat, kan de programmeur ervoor zorgen dat de gebruiker de login stroom manueel annuleerde, en de Programmeur kan met het roepen `setSelectedProvider(null)` te werk gaan. De teweeggebrachte callbacks zijn het zelfde als in stroom 2 hierboven.

</br>

## Oorspronkelijke afmeldingsstroom {#orig_logout}

De logout-API van de AccessEnabler wist de lokale status van de bibliotheek en laadt de MVPD-aanmeldings-URL in het huidige tabblad/venster. De browser navigeert naar het MVPD logout-eindpunt en nadat het proces is voltooid, wordt de gebruiker teruggeleid naar de website van de programmeur. De enige actie die namens de gebruiker wordt vereist is het drukken van de Logout knoop/verbinding en het in werking stellen van de stroom; geen gebruikersinteractie wordt vereist op het MVPD logout eindpunt.

**Oorspronkelijke Authentificatie/Logout Stroom met Pagina verfrist zich**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Verbeterde (Vernieuwen) verificatie {#improved_authn}

>[!NOTE]
>
>Voor de verbeterde aanmeldings- en aanmeldstromen zonder vernieuwing is het vereist dat de browser moderne HTML5-technologieën, waaronder webberichten, ondersteunt.

Zowel de hierboven beschreven verificatie- (aanmelding) als aanmeldstromen bieden een vergelijkbare gebruikerservaring door de hoofdpagina opnieuw te laden nadat elke flow is voltooid.  De huidige functie is bedoeld om de gebruikerservaring te verbeteren door aanmeldingsgegevens en aanmeldgegevens zonder vernieuwen (background) te bieden. De programmeur kan aanmeldingsgegevens voor de achtergrond in- en uitschakelen door twee Booleaanse markeringen (`backgroundLogin` en `backgroundLogout` ) door te geven aan de parameter `configInfo` van de `setRequestor` API. Standaard is aanmeldingsnaam/afmelding op de achtergrond uitgeschakeld (dit biedt compatibiliteit met de vorige implementatie).

**Voorbeeld:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Authentificatie**

De volgende punten beschrijven de overgang tussen de originele authentificatiestromen en de verbeterde stromen:

1. De omleiding van de volledige pagina wordt vervangen door een nieuw browsertabblad waarin de MVPD-aanmelding wordt uitgevoerd. De programmeur moet een nieuw tabblad (via `window.open` ) met de naam `mvpdwindow` maken wanneer de gebruiker een MVPD selecteert (met `iFrameRequired = false` ). De programmeur voert dan `setSelectedProvider(<mvpd>)` uit, toestaand AccessEnabler om MVPD login URL in het nieuwe lusje te laden. Nadat de gebruiker geldige geloofsbrieven verstrekt, zal de Authentificatie van Adobe Pass het lusje sluiten en een window.postMessage naar de website van de Programmer verzenden die aan AccessEnabler signaleert dat de authentificatiestroom heeft gebeëindigd. De volgende callbacks worden teweeggebracht:

   - Als de flow is gestart door `getAuthentication` : `setAuthenticationStatus` en `sendTrackingData(AUTHENTICATION_DETECTION...)` , worden geactiveerd om te signaleren dat de verificatie is gelukt of mislukt.

   - Als de flow is gestart door `getAuthorization` : `setToken/tokenRequestFailed` en `sendTrackingData(AUTHORIZATION_DETECTION...)` , worden geactiveerd om aan te geven dat de autorisatie is gelukt of mislukt.

1. De stroom van het iFrame-/pop-upvenster blijft meestal ongewijzigd, waarbij het verschil is dat de bovenliggende pagina niet opnieuw wordt geladen nadat de gebruiker geldige gegevens heeft verschaft. Het iFrame/popup zal automatisch sluiten na login en een `window.postMessage` wordt verzonden naar de ouderpagina, die AccessEnabler op de hoogte brengt dat de stroom heeft gebeëindigd. Dezelfde callbacks worden geactiveerd als in de vorige flow, **plus de volgende nieuwe callback:`destroyIFrame`** . Met de callback van `destroyIFrame` kan de programmeur eventuele aan iFrame gekoppelde/hulpcomponenten verwijderen, zoals UI-decoraties. Het bestaan van deze callback werd niet vereist in de oude authentificatiestroom omdat nadat login volledig was, de Authentificatie van Adobe Pass de pagina van de Programmer opnieuw zou laden, waarbij alle componenten UI op het werd vernietigd.

</br>

>[!IMPORTANT]
> 
>U moet het MVPD login iFrame of popup venster als direct kind van de pagina laden die de instantie AccessEnabler bevat. Als het MVPD-aanmeldings-iFrame of pop-upvenster twee of meer niveaus onder de pagina met de AccessEnabler-instantie bevat, kan de flow hangen. Als u bijvoorbeeld een iFrame hebt tussen de hoofdpagina en de MVPD iFrame (Pagina =\> iFrame =\> MVPD iFrame), zou de aanmeldstroom kunnen mislukken.

</br>

**annuleert Authentificatie**

Dit zijn de stromen voor het annuleren van authentificatie:

1. **Browser lusje -** aangezien het lusje fundamenteel een nieuw venster is, heeft het vangen van zijn dichte gebeurtenis de zelfde beperkingen zoals besproken in scenario 3 van de oude authentificatiestromen. Bovendien is de tijdopnemerbenadering hier niet mogelijk, omdat er geen manier is om tussen een lusje te onderscheiden dat manueel door de gebruiker werd gesloten en een lusje dat automatisch aan het eind van de login stroom werd gesloten. De oplossing hier is voor AccessEnabler om &quot;stil&quot;te blijven (geen callbacks worden teweeggebracht) wanneer de gebruiker de stroom annuleert. Bovendien hoeft de programmeur geen specifieke actie te ondernemen. De gebruiker zal een andere authentificatiestroom kunnen in werking stellen zonder de &quot;Veelvoudige Fout van Verzoeken van de Authentificatie&quot;te ontvangen (deze fout is gehandicapt in AccessEnabled voor achtergrondlogin).

1. **iFrame -** de programmeur kan de benadering kiezen die in scenario 2 van de oude authentificatiestromen wordt besproken (creeer omslag UI van iFrame en bijbehorende Dichte knoop die `setSelectedProvider(null)` teweegbrengt. Hoewel deze benadering niet meer een sterk vereiste is (de veelvoudige authentificatiestromen worden toegestaan voor achtergrondlogin, zoals die in scenario 1 hierboven wordt besproken), wordt het nog geadviseerd door Adobe.

1. **Popup -** dit is identiek aan Browser hierboven lusjestroom.

</br>

## Verbeterde afmeldingsstroom {#improved_logout}

De nieuwe logout-flow wordt uitgevoerd in een verborgen iFrame, waardoor de volledige paginareeiding wordt verwijderd.  Dit is mogelijk omdat de gebruiker geen specifieke actie hoeft uit te voeren op de MVPD-aanmeldpagina.

Nadat de logout-flow is voltooid, wordt het iFrame omgeleid naar een aangepast Adobe Pass-verificatieeindpunt. Dit zal een fragment van JS dienen dat `window.postMessage` aan de ouder uitvoert, die AccessEnabler op de hoogte brengt dat logout volledig is. De volgende callbacks worden getriggerd: `setAuthenticationStatus()` en `sendTrackingData(AUTHENTICATION_DETECTION ...)` , die aangeven dat de gebruiker niet langer geverifieerd is.

In de onderstaande afbeelding ziet u de workflow zonder vernieuwen waarmee een gebruiker zich kan aanmelden bij zijn of haar MVPD zonder de mainpage van uw toepassing te vernieuwen:

**Verbeterde (verfrissen-minder) Authentificatie/Logout Stroom**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass Flow {#improved_temppas}

Logbestand zonder vernieuwen volgt een andere aanpak voor MVPD&#39;s van het type TempPass.

Aangezien voor de TempPass-flow automatisch een venster moet worden gemaakt en gesloten zonder expliciete gebruikersinteractie, kan dit een probleem opleveren voor sommige browsers (popup blockers). Daarom voert AccessEnabler de login fase achter de scènes uit, zonder een Webcontainer te vereisen die door Programmer wordt gecreeerd.

Hier zijn de aspecten die de programmeur zich van bewust moet zijn wanneer het uitvoeren van TempPass voor login en logout verfrissen zich zonder:

- Voordat verificatie wordt gestart, moet het iFrame- of pop-upvenster alleen worden gemaakt voor MVPD&#39;s die niet onder TempPass vallen. De programmeur kan detecteren of een MVPD TempPass is door de eigenschap `tempPass` van het MVPD-object te lezen (geretourneerd door `setConfig()` / `displayProviderDialog()` ).

- De callback van `createIFrame()` moet een controle voor TempPass bevatten, en voert zijn logica slechts uit wanneer MVPD NIET TempPass is.

- De callback van `destroyIFrame()` moet een controle voor TempPass bevatten, en voert zijn logica slechts uit wanneer MVPD NIET TempPass is.

- De callbacks `setAuthenticationStatus()` en `sendTrackingData()` worden aangehaald nadat de authentificatie voltooit (precies zoals in de stroom verfrist-zonder voor normale MVPDs).

>[!NOTE]
>
>Deze stroom is alleen beschikbaar voor TempPass zonder vernieuwen. Voor de vernieuwingsstroom moet TempPass expliciet worden afgehandeld (wanneer TempPass iFrame / popup vereist)

</br>

Het volgende codevoorbeeld toont hoe te om een venster van MVPD op de website van een Programmer (zowel voor normale MVPDs als voor TempPass) te behandelen:

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
