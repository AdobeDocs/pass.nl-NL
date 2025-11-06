---
title: De MVPD-aanmeldingspagina migreren van iFrame naar pop-up
description: De MVPD-aanmeldingspagina migreren van iFrame naar pop-up
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# (Verouderd) De MVPD-aanmeldingspagina migreren van iFrame naar pop-up {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Pop-up en iFrame {#popup-vs-iframe}

Sommige gebruikers hebben problemen met cookies van derden aangetroffen tijdens de iFrame-implementatie van een MVPD-aanmeldingspagina.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Het team van de Authentificatie van Adobe Pass **adviseert het uitvoeren van popup/nieuwe venster login pagina** eerder dan de iFrame versie op Firefox en Safari.  Als u echter een aanmeldingspagina voor Internet Explorer implementeert, kunnen er problemen optreden met de popup-implementatie. De IE-problemen worden veroorzaakt door het feit dat, nadat de gebruiker zich in het pop-upvenster met zijn MVPD heeft geverifieerd, de Adobe Pass-verificatie een bovenliggende pagina omleiden forceert. Dit wordt gezien als een pop-upblokkering door Internet Explorer. Het team van de Authentificatie van Adobe Pass **adviseert het uitvoeren van iFrame login voor Internet Explorer**.

De voorbeeldcode in deze technische notitie maakt gebruik van een hybride implementatie van zowel iFrame als popup: een iFrame openen in Internet Explorer en een pop-up openen in de andere browsers.

Aangezien er al een iFrame-implementatie bestaat, bevat het eerste deel van de technische notitie de code voor de iFrame-implementatie en het tweede deel de wijzigingen die nodig zijn om de popup-implementatie als standaard aan te passen.


## MVPD-kiezer met aanmeldingspagina in een iFrame {#mvpd-pickr-iframe}

In vorige codevoorbeelden werd een HTML-pagina weergegeven met de tag &lt;div>, waarin het iFrame samen met de knop Close iFrame moet worden gemaakt:

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Hier is de bijbehorende **JavaScript** code:

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## MVPD-kiezer met aanmeldingspagina in pop-upvenster {#mvpd-pickr-popup}

Aangezien wij geen **iFrame** meer zullen gebruiken, zal de code van HTML niet iFrame of de knoop bevatten om iFrame te sluiten. Div die vroeger iFrame bevatte - **mvpddiv** - zal voor het volgende worden gehouden en worden gebruikt:

* om de gebruiker te laten weten dat de MVPD-aanmeldingspagina al is geopend als de pop-upfocus niet meer aanwezig is
* om een koppeling te maken om de focus op de popup te herstellen

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

De lijst van MVPDs zal in div genoemd **plukker** als uitgezocht **- mvpdList** worden getoond.

Een nieuwe API callback zal worden gebruikt - **setConfig (configXML)**. De callback wordt geactiveerd na het aanroepen van de functie setRequestor(requestID). Deze callback keert de lijst van MVPDs terug die met aanvragerID eerder wordt geïntegreerd. In de callback methode, zal inkomende XML worden ontleed, en de lijst van MVPDs caching. De MVPD-kiezer wordt ook gemaakt, maar niet weergegeven.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Nadat de functie getAuthentication() of getAuthorization() is aangeroepen, wordt de callback displayProviderDialog() geactiveerd. Normaal, binnen deze callback, zou de lijst van MVPD gebouwd en getoond zijn geweest. Aangezien de MVPD-kiezer al is gemaakt, hoeft u deze alleen nog maar aan de gebruiker weer te geven.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Nadat de gebruiker een MVPD in de kiezer heeft geselecteerd, moet de pop-up worden gemaakt. Sommige browsers kunnen popup blokkeren als het met ongeveer :blank of met een pagina wordt gecreeerd die op een ander domein is - daarom wordt het geadviseerd om het met hostname van te openen waar AccessEnabler wordt geladen.

In de iFrame-implementatie werd het opnieuw instellen van de verificatiestroom uitgevoerd door de knop btnCloseIframe en de JavaScript-functie closeIframeAction(), maar nu is het niet meer mogelijk het iFrame te versieren. Zo, wordt het zelfde gedrag bereikt door te letten op wanneer popup (of door de gebruiker of door de authentificatiestroom te voltooien) wordt gesloten. Er is een codefragment toegevoegd dat ook nuttig is voor het geval de gebruiker de focus van de pop-up verliest:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Op createIFrame () callback zal **mvpddiv** div worden getoond.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* De voorbeeldcode bevat een hardcoded variabele voor de gebruikte requestID - &#39;REF&#39;, die moet worden vervangen door een echte id van de programmeur.
>* De voorbeeldcode wordt alleen correct uitgevoerd vanuit een zwevend domein dat is gekoppeld aan de gebruikte aanvrager-id.
>* Aangezien de gehele code kan worden gedownload, is de code in deze technische notitie afgebroken. Voor een volledige steekproef, zie **JS iFrame vs Pop-up Steekproef**.
>* De externe bibliotheken van JavaScript werden verbonden van [&#x200B; Google Ontvangen Diensten &#x200B;](https://developers.google.com/speed/libraries/).
