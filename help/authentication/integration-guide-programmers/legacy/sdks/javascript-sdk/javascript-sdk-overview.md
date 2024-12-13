---
title: JavaScript SDK - Overzicht
description: JavaScript SDK - Overzicht
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---

# (Verouderd) JavaScript SDK - Overzicht {#javascript-sdk-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding

De Adobe adviseert hoogst dat u aan recentste JS v4.x van de bibliotheek AccessEnabler migreert.

De integratie van Adobe Pass Authentication JavaScript biedt programmeurs een tv-oplossing overal in de vertrouwde ontwikkelomgeving van JS-webtoepassingen. De belangrijkste componenten van de integratie zijn uw &quot;high-level&quot;toepassing (gebruikersinteractie, videopresentatie), en de Adobe-geleverde &quot;laag-niveau&quot;bibliotheek AccessEnabler die uw ingang aan de toestemmingsstromen verstrekt, en communicatie met de servers van de Authentificatie van Adobe Pass behandelt.

De algemene de machtigingsstroom van de Authentificatie van Adobe Pass wordt behandeld in [ Stroom van het Entitlement van de Programmer ](/help/authentication/integration-guide-programmers/entitlement-flow.md), en de Cookbook van de Integratie van JavaScript begeleidt u door de implementatie. De volgende secties verstrekken beschrijvingen en steekproeven specifiek voor de integratie JavaScript AccessEnabler.

>[!IMPORTANT]
>
>In dit document wordt de implementatie voor een desktopweboplossing beschreven. De JavaScript-bibliotheek wordt niet ondersteund op mobiele platforms (bijvoorbeeld Safari op iOS, Chrome op Android). Gebruik onze native SDK&#39;s als u een mobiel platform (iOS, Android, Windows) als doel wilt instellen.

## Het dialoogvenster MVPD-selectie maken {#creating-the-mvpd-selection-dialog}

Een gebruiker kan zich alleen aanmelden bij de MVPD en vervolgens worden geverifieerd als de pagina of speler de gebruiker een manier biedt om zijn of haar MVPD te identificeren. Er is een standaardversie van een dialoogvenster voor MVPD-selectie beschikbaar voor ontwikkeling. Voor productiegebruik moet u uw eigen MVPD-kiezer implementeren.

Als u reeds weet wie de leverancier van de klant is, kunt u [ MVPD programmatically plaatsen ](/help/authentication/home.md), zonder gebruikersinteractie. De techniek is hetzelfde, maar passeert de stap om het dialoogvenster Provider aan te roepen en de klant te vragen zijn of haar MVPD te selecteren.

## De serviceprovider weergeven {#displaying-the-service-provider}

Het volgende codevoorbeeld toont aan hoe te om de dienstverlener voor de huidige klant te ontdekken en te tonen:

**HTML** - Deze pagina voegt een sectie aan de pagina toe die de gekozen leverancier van de klant toont, als zij reeds het programma worden geopend:

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** Dit dossier van JavaScript vraagt Toegelaten voor de huidige leverancier als de gebruiker reeds het programma wordt geopend, en toont het resultaat in de paginasectie die voor het wordt gereserveerd. Het implementeert ook een MVPD-selectordialoogvenster:

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## Afmelden {#logout}

Roep `logout()` aan om het logout-proces te starten. Deze methode gebruikt geen argumenten. Het logout de huidige gebruiker, die alle authentificatie en vergunningsinformatie voor die gebruiker ontruimt en alle tokens AuthN en AuthZ van het lokale systeem schrapt.

In sommige gevallen is de speler niet verantwoordelijk voor het afhandelen van gebruikersaanmeldingen:



- **wanneer logout van een plaats in werking wordt gesteld die niet met de Authentificatie van Adobe Pass is geïntegreerd.** In dit geval kan de MVPD de service Single Logout voor Adobe Pass-verificatie aanroepen via een omleiding van de browser. (Het aanroepen van SLO via een backchannel-aanroep wordt momenteel niet ondersteund.)

>[!NOTE]
>
>Als de gebruiker de computer lang genoeg inactief laat om de tokens te laten verlopen, kan hij of zij nog steeds terugkeren naar zijn of haar sessie en met succes het afmelden starten. De Authentificatie van Adobe Pass zorgt ervoor dat alle tokens worden geschrapt en deelt MVPD mee om hun zitting te schrappen, eveneens.

De volgende JavaScript-code demonstreert het afmelden (verifiëren) van een momenteel geverifieerde gebruiker:

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
