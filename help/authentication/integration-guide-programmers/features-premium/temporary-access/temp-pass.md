---
title: Temperatuurcontrole
description: Temperatuurcontrole
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Temperatuurcontrole {#temp-pass}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht van functies {#tempass-featur-summary}

Met Temp Pass kunnen programmeurs tijdelijke toegang bieden tot hun beveiligde inhoud, voor gebruikers die geen accountgegevens bij een MVPD hebben.  De Controle van het Temperatuur omvat de volgende mogelijkheden:

* De Pas van de Temperatuur kan worden gevormd om tijdelijke toegang te verlenen om een verscheidenheid van scenario&#39;s, met inbegrip van het volgende te behandelen:
   * Een programmeur kan een dagelijkse, korte (bijvoorbeeld een voorvertoning van 10 minuten) van een van zijn sites aanbieden.
   * Een programmeur kan één lange presentatie (bijvoorbeeld vier uur) aanbieden van een belangrijk sportevenement zoals de Olympische Spelen of de Waanzin van NCAA March.
   * Een programmeur kan een combinatie van de vorige twee scenario&#39;s verstrekken; bijvoorbeeld, een eerste, langere kijkperiode één dag, die door een reeks korte periodes wordt gevolgd die dagelijks voor één of ander aantal verdere dagen herhalen.
* Programmeurs geven de duur (Tijd-aan-Levende, of TTL) van hun Pass van het Temperatuur aan.
* Temperatuurcontrole werkt per aanvrager.  Bijvoorbeeld, kon NBC een Pass van 4 uurTemp voor de aanvrager &quot;NBCOlymics&quot;opzetten.
* Programmeurs kunnen alle tokens herstellen die aan een bepaalde aanvrager zijn toegekend.  De &quot;tijdelijke MVPD&quot; die wordt gebruikt om de Temp-controle te implementeren, moet zijn geconfigureerd met &quot;Verificatie per aanvrager&quot; ingeschakeld.
* **de toegang van de Controle van het Temperatuur wordt verleend aan individuele gebruikers op specifieke apparaten**. Nadat de toegang van de Controle van het Temperatuur voor een gebruiker verloopt, zal die gebruiker geen tijdelijke toegang op het zelfde apparaat kunnen krijgen tot het de toestemmingstoken van die gebruiker verstreek van de server van de Authentificatie van Adobe Pass.


>[!NOTE]
>
>Temperatuurcontrole maakt deel uit van het Premium Workflow-pakket. Neem contact op met je Adobe Pass-verkoper als je deze functie wilt gebruiken.

## Details onderdeel {#tempass-featur-details}

* **hoe het Bekijken Tijd** wordt berekend - de hoeveelheid tijd dat een Pas geldig blijft van de Temperatuur correleert niet aan de hoeveelheid tijd een gebruiker het bekijken inhoud op de toepassing van de Programmer overspant.  Op het aanvankelijke gebruikersverzoek om vergunning via de Pas van de Temperatuur, wordt een vervaltijd berekend door de aanvankelijke huidige verzoektijd aan TTL toe te voegen die door de Programmer wordt gespecificeerd. Deze vervaltijd wordt gekoppeld aan de apparaat-id van de gebruiker en de aanvrager-id van de programmeur en wordt opgeslagen in de Adobe Pass-verificatiedatabase. Telkens wanneer de gebruiker probeert om tot inhoud toegang te hebben gebruikend de Pas van het Temperatuurpas van het zelfde apparaat, zal de Authentificatie van Adobe Pass de tijd van het serververzoek met de vervaltijd vergelijken verbonden aan het apparaat van de gebruiker identiteitskaart en de de verzoekidentiteitskaart van de Programmer. Als de tijd van het serververzoek minder dan de vervaltijd is, zal de vergunning worden verleend; anders, zal de vergunning worden ontkend.
* **Parameters van de Configuratie** - de volgende parameters van de Pas van de Temperatuur kunnen door een Programmer worden gespecificeerd om een regel van de Pas van de Temperatuur te creëren:
   * **Symbolische TTL** - de hoeveelheid tijd die een gebruiker wordt toegestaan te letten zonder het ondertekenen binnen aan MVPD. Deze keer is op de klok gebaseerd en verloopt of de gebruiker inhoud bekijkt of niet.
  >[!NOTE]
  >Aan een aanvraag-id kunnen niet meer dan één regel voor een tijdelijke controle zijn gekoppeld.
* **Authentificatie/Vergunning** - in de stroom van de Controle van het Temperatuur, specificeert u MVPD als &quot;Pas van het Temperatuur&quot;.  Adobe Pass-verificatie communiceert niet met een werkelijke MVPD in de Temp-controle-flow, zodat de &#39;Temp Pass&#39;-MVPD alle bronnen toestaat. Programmeurs kunnen een middel specificeren dat toegankelijk gebruikend de Pas van Temperatuur is net zoals zij voor de rest middelen op hun plaats doen. De bibliotheek van de Verificateur van Media kan worden gebruikt zoals gebruikelijk om het korte media token van de Pas van Temperatuur te verifiëren en middel controle vóór playback af te dwingen.
* **het Volgen Gegevens in de Stroom van de Pas van de Pas van de Temperatuur** - Twee punten betreffende het volgen van gegevens tijdens een de machtigingsstroom van de Pas van de Temperatuur:
   * De het Volgen identiteitskaart die van de Authentificatie van Adobe Pass aan uw **wordt overgegaan sendTrackingData ()** callback is een knoeiboel van identiteitskaart van het Apparaat.
   * Aangezien identiteitskaart van MVPD in de stroom van de Pas van de Temperatuur wordt gebruikt is &quot;Pas van Temperatuur&quot;, wordt die zelfde identiteitskaart van MVPD teruggegeven aan **sendTrackingData ()**. De meeste programmeurs zullen de metriek van de Pas van Temp waarschijnlijk anders willen behandelen dan daadwerkelijke metriek MVPD. Hiervoor is extra werk nodig in uw analytische implementatie.

In het volgende voorbeeld ziet u de Temperatuur-doorstroomsnelheid:

![ de stroom van de Pas van de Temperatuur ](../../../assets/temp-pass-flow.png)

*Cijfer: De stroom van de Controle van de Temperatuur*

## Tijdelijke controle implementeren {#implement-tempass}

Aan de Adobe Pass-verificatiezijde wordt Temp Pass geïmplementeerd met de toevoeging van een pseudo-MVPD met de naam &quot;TempPass&quot; aan de serverconfiguratie van de deelnemende programmeur.  Deze pseudo-MVPD fungeert als een echte MVPD die tijdelijk toegang verleent tot de beveiligde inhoud van de programmeur.

Aan de programmeerzijde wordt de Controle van Temp uitgevoerd als volgt voor de twee scenario&#39;s die MVPDs voor authentificatie gebruikt:

* **iFrame op de pagina van de Programmer**. Temperatuurcontrole werkt ongeacht het MVPD-verificatietype, maar voor het iFrame-scenario zijn aanvullende stappen vereist om de huidige verificatiestroom te annuleren en te verifiëren met Temperatuurcontrole. Deze stappen worden getoond in de [ Login iFrame ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) hieronder.
* **re-direct aan MVPD login pagina**. In het meer traditionele geval waarin de gebruikersinterface voor het activeren van de Temperatuur-controle wordt weergegeven voordat verificatie met een MVPD wordt gestart, zijn er geen speciale stappen die moeten worden ondernomen. Temperatuurcontrole moet worden behandeld als een normale MVPD.

De volgende punten zijn van toepassing op beide uitvoeringsscenario&#39;s:

* De &quot;Temperatuurcontrole&quot; mag alleen worden weergegeven in de MVPD-kiezer voor gebruikers die nog geen machtiging voor een tijdelijke controle hebben aangevraagd. Het blokkeren van de weergave van volgende aanvragen kan worden bereikt door een markering op cookies te plaatsen. Dit werkt zolang de gebruiker de browsercache niet wist. Als gebruikers hun browsercache wissen, wordt de opdracht Tijdelijke controle opnieuw weergegeven in de kiezer en kan de gebruiker deze opnieuw aanvragen. Toegang wordt alleen verleend als de &quot;Temperatuur Pass&quot;-tijd nog niet is verstreken.
* Wanneer een gebruiker toegang via de Volgorde van de Temperatuur vraagt, zal de Server van de Authentificatie van Adobe Pass zijn gebruikelijke vraag van de Prijsverhoging van de Taal van de Bevestiging van de Veiligheid (SAML) tijdens het authentificatieproces niet uitvoeren. In plaats daarvan, zal het authentificatieeindpunt succes terugkeren telkens als het wordt aangehaald terwijl de tekenen voor het apparaat geldig zijn.
* Wanneer een Pass van het Temperatuur verloopt, zal zijn gebruiker niet meer voor authentiek worden verklaard, omdat in de Stroom van het Pad van het Temperatuur het authentificatietoken en het toestemmingstoken de zelfde vervaldatum hebben. Om gebruikers uit te leggen dat hun tijdelijke controle is verlopen, moeten programmeurs de geselecteerde MVPD direct opvragen nadat ze `setRequestor()` hebben aangeroepen en vervolgens `checkAuthentication()` naar behoren aanroepen. In de callback van `setAuthenticationStatus()` kan een controle worden gemaakt om te bepalen als de autestatus 0 is, zodat als de geselecteerde MVPD &quot;TempPass&quot;was een bericht aan gebruikers kan worden voorgelegd dat hun zitting van de Controle van het Temperatuur is verlopen.
* Als een gebruiker het token Tijdelijke controle vóór het verlopen verwijdert, wordt met de daaropvolgende machtigingscontroles een token gegenereerd dat een TTL heeft die gelijk is aan de resterende tijd.
* Als de gebruiker het token Temperatuur-controle verwijdert nadat het is verlopen, worden de daaropvolgende machtigingscontroles uitgevoerd met de waarde &quot;Gebruiker niet geautoriseerd&quot;.

Zie de steekproeven in [ Code van de Steekproef ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) hieronder voor voorbeelden van hoe te om de implementatiedetails te coderen die in deze sectie worden beschreven.

## Voorbeeldcode {#tempass-sample-code}

In de volgende secties ziet u hoe u de API voor Adobe Pass-verificatie kunt aanroepen om de Temp-controle te implementeren:

* [iFrame-aanmeldingsvoorbeeld](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Voorbeeld van automatisch aanmelden](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### iFrame-aanmeldingsvoorbeeld {#iframe-login-sample}

In dit voorbeeld ziet u hoe u Temp Pass implementeert voor het geval waarin MVPD&#39;s iFrame-integratie ondersteunen:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Gebruiksgevallen bij aanmelding bij iFrame {#iframe-login-use-cases}

**om een Pass van het Temperatuur voor het eerst aan te vragen:**

1. Een gebruiker heeft toegang tot de pagina van de Programmer en klikt de login verbinding.
1. De MVPD-kiezer wordt geopend en de gebruiker kiest een MVPD in de lijst.
1. De iFrame voor verificatie wordt weergegeven. Dit iFrame bevat een koppeling Tijdelijke controle.
1. De gebruiker klikt op Tijdelijke controle, zodat de programmeur een vlag aan een cookie toevoegt om te voorkomen dat de gebruiker de koppeling Tijdelijke controle ziet bij volgende bezoeken aan de pagina.
1. De de authentificatieverzoek van de Volgorde van de Temp bereikt de servers van de Authentificatie van Adobe Pass, en zij produceren een authentificatietoken. De TTL is gelijk aan de periode die door de programmeur voor de Pass van Temperatuur wordt geplaatst.
1. De aanvraag voor een Temp-controle voor autorisatie bereikt de Adobe Pass-verificatieservers.
1. De Adobe Pass-verificatieservers extraheren het apparaat en de aanvrager-id&#39;s uit de aanvraag en slaan deze samen met de vervaltijd op in de database. De vervaltijd wordt berekend als: de aanvankelijke de verzoektijd van de Volgorde van de Temperatuur plus TTL (die door Programmer wordt gespecificeerd).
1. De Adobe Pass Authentication-servers genereren een verificatietoken.
1. De gebruiker heeft toegang tot de beveiligde inhoud.

**om een Pass van het Temperatuur opnieuw te verzoeken na een terugkerende gebruiker van de Pas van Temperatuur heeft de browser koekjes geschrapt:**

1. De gebruiker heeft toegang tot de pagina van de programmeur en klikt op de aanmeldingskoppeling.
1. De MVPD-kiezer wordt geopend en de gebruiker kiest een MVPD in de lijst.
1. De iFrame voor verificatie wordt weergegeven. Dit iFrame bevat een koppeling &#39;Tijdelijke controle&#39; (de gebruiker heeft het oorspronkelijke cookie verwijderd, zodat de programmeur niet weet of de gebruiker eerder op de koppeling &#39;Tijdelijke controle&#39; heeft geklikt).
1. De gebruiker klikt nogmaals op Tijdelijke controle, zodat de programmeur opnieuw een markering aan een cookie toevoegt, om te voorkomen dat de gebruiker de koppeling Tijdelijke controle ziet bij volgende bezoeken aan de pagina.
1. Het verificatieverzoek Temp Pass bereikt de Adobe Pass-verificatieservers, die een verificatietoken genereren. TTL is nu de resterende tijd voor de Pas van de Temperatuur (het verschil tussen de huidige tijd en de vervaltijd verbonden aan apparatenidentiteitskaart).
1. De aanvraag voor een Temp-controle voor autorisatie bereikt de Adobe Pass-verificatieservers.
1. De Adobe Pass Authentication-servers extraheren het apparaat en de aanvrager-id&#39;s uit de aanvraag en gebruiken deze om de vervaltijd op te halen uit de Adobe Pass Authentication-database. De huidige tijd wordt vergeleken met de vervaltijd.
1. Als de Temp-controle van de gebruiker niet is verlopen, genereren de Adobe Pass-verificatieservers een verificatietoken.
1. Als de Temperatuur-controle van de gebruiker niet is verlopen, kan de gebruiker toegang krijgen tot de beveiligde inhoud.

### Voorbeeld van automatisch aanmelden {#auto-login-sample}

In het volgende voorbeeld ziet u een geval waarbij een gebruiker automatisch wordt aangemeld bij TempPass wanneer een site wordt bezocht. De gebruiker kan op elk gewenst moment aanmelden bij een gewone MVPD en wordt gewaarschuwd als TempPass is verlopen:

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Meerdere sjablooncontroles gebruiken {#use-mult-tempass}

Bepaalde gebeurtenissen vereisen gefaseerde vrije toegang tot inhoud, zoals een eerste interval van vrije toegang (bijv. 4 uur), gevolgd door dagelijkse vrije toegang (bijv. 10 minuten op elke volgende dag).  Opdat een programmeur dit scenario uitvoert, moeten zij het met hun contact van de Adobe schikken om twee tijdelijke MVPDs voor de Programmer te vormen.

Voor dit voorbeeldscenario (een aanvankelijke 4 uren vrije zitting, die door dagelijkse 10 minuten vrije zittingen wordt gevolgd) vormt de Adobe een MVPD genoemd TempPass1 met een Tijd-aan-Levende (TTL) van 4 uren, en een TempPass2 met een TTL van 10 minuten voor de verdere periode.  Beide worden geassocieerd met identiteitskaart van de Aanvrager van de Programmer.

### Programmeringsuitvoering {#mult-tempass-prog-impl}

Nadat de Adobe de twee instanties TempPass vormt, zullen twee extra MVPDs (TempPass1 en TempPass2), in de lijst van MVPD van de Programmer verschijnen.  De programmeur moet de volgende stappen nemen om de veelvoudige tijdelijke overgangen uit te voeren:

1. Meld de gebruiker automatisch aan bij het eerste bezoek van de gebruiker aan de site met TempPass1. U kunt de Steekproef Autologin hierboven als uitgangspunt voor deze taak gebruiken.
1. Wanneer u vaststelt dat TempPass1 is verlopen, slaat u het feit (in een cookie/lokale opslag) op en geeft u de gebruiker uw standaard MVPD-kiezer weer. **zorg ervoor om uit TempPass1 en TempPass2 van die lijst** te filtreren.
1. Op elke volgende dag, als TempPass1 is verlopen, autologin die gebruiker met TempPass2.
1. Wanneer TempPass2 is verlopen, slaat u het feit (in een cookie/lokale opslag) voor de dag op en geeft u de gebruiker uw standaard MVPD-kiezer weer. Ook hier moet u TempPass1 en TempPass2 uit die lijst filteren.
1. Op elke nieuwe dag, bij 00:00 uren, stel alle tijdelijke overgangen voor TempPass2 terug, gebruikend het [ Web API van TempPass van het Terugstellen TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Programmerende nota:** de Authentificatie van Adobe Pass heeft geen ingebouwd mechanisme om het vrije stromen tegen te houden nadat de 10 minuten zijn overgegaan.  Het is aan Programmeurs om de toegang te beperken zodra TempPass2 verloopt. Om dit te bereiken, kunnen de programmeurs in hun plaatsen/apps een vraag &quot;checkAuthorization&quot;uitvoeren elke X minuten, waar X de tijdspanne is die de programmeur voor hun apps zinvol bepaalt.

## Alle sjablooncontroles opnieuw instellen {#reset-all-tempass}

Bepaalde bedrijfsregels vereisen een regelmatige zuivering van de Voldoende tems, of een ad-hocherziening van alle Temperaturen die voor een bepaalde Vraag-id en MVPD-id zijn uitgegeven. Deze functie ondersteunt bijvoorbeeld de volgende gebruiksgevallen:

* Een Temperatuurcontrole van 10 minuten per dag (de Temperatuurcontrole moet aan het begin van de dag opnieuw worden ingesteld)
* Een tijdelijke controle beschikbaar voor alle gebruikers tijdens het verbreken van het nieuws. (De Temp-controle moet voor alle apparaten opnieuw worden ingesteld zodra het nieuws op het punt staat.)
* Het meervoudige scenario van de Pas van de Temperatuur dat een combinatie van een eerste het bekijken periode van één of andere lengte, gevolgd door verdere dagperiodes van een andere lengte verstrekt.

Om alle Passen van Temperatuur terug te stellen, verstrekt de Authentificatie van Adobe Pass Programmeurs a *openbaar* Web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>De bovenstaande URL vervangt de vorige API voor opnieuw instellen. De oude API voor opnieuw instellen (v1) wordt niet meer ondersteund.

* **Protocol:** HTTPS
* **Gastheer:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Weg:** /reset-tempass/v2/reset
* **de parameters van de Vraag:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Kopballen:** ApiKey - 1232293681726481
* **Reactie:**
   * Succes - HTTP 204
   * Fout:
      * HTTP 400 voor een onjuiste aanvraag
      * HTTP 401 als de ApiKey niet is opgegeven
      * HTTP 403 als de ApiKey ongeldig is

Bijvoorbeeld:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Ondersteunde clients {#supp-clients}

Ondersteuning voor gereedschap Temperatuur doorgeven en herstellen per platform:

| Adobe Pass-verificatieclients | Temperatuurcontrole | Gereedschap herstellen |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabled | JA | JA |
| IOS voor native client | JA | JA |
| Systeemeigen clientbesturingssysteem | JA | JA |
| Android voor native client | JA | JA |
| Native Client FireTV | JA | JA |
| Clientloze API | JA | JA |

## Beperkingen en bekende problemen {#limitations}

In dit gedeelte worden de beperkingen beschreven die van toepassing zijn op de huidige implementatie van Temperatuurcontrole.

**JavaScript SDK**: steunt de functionaliteit van de de teppas van de terugstellingstempels van versie **3.X en hierboven**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
