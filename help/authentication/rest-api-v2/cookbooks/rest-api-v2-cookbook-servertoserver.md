---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
source-git-commit: 87d4d95a3bf4ace68bc71ca700b09da14ee316f4
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>The content on this page is provided for information purposes only. Usage of this API requires a current license from Adobe. No unauthorized use is permitted.


## Overzicht {#overview}

Het doel van dit kookboekdocument is best practices voor het implementeren van Adobe Pass Authentication met behulp van REST API V2 in een server-naar-server architectuur in detail te beschrijven.  It provides basic requirements, step-by-step flow implementation and general considerations for production environments and operation.

>[!IMPORTANT]
>
> [](/help/authentication/throttling-mechanism.md)


## Components {#components}

In a working Server-to-Server solution the following components are involved:


| Type | Component | Description |
| --- | --- | --- |
| Streaming Device | Streaming App | The Programmer application that resides on the user&#39;s streaming device and plays authenticated video. |
| | \[Optional\] AuthN Module | if Streaming Device has a User Agent (i.e. Web Browser), the AuthN Module is resposnible for authenticating the user on the MVPD IdP. |
| \[Optional\] AuthN Device | AuthN App | if the Streaming Device does not have a User Agent (i.e. Web Browser), the AuthN Application is a Programmer web application that is accessed from a separte user&#39;s device using a web browser. |
| Programmeringsinfrastructuur | Programmeringsservice | Een service die het streamingapparaat koppelt aan de Adobe Pass-service om verificatie- en autorisatiebeslissingen te verkrijgen. |
| Adobe Infrastructure | Adobe Pass Service | De dienst die met de Dienst MVPD IdP en AuthZ integreert en authentificatie en vergunningsbesluiten verstrekt. |
| MVPD-infrastructuur | MVPD IdP | Een eindpunt MVPD dat op referentie-gebaseerde authentificatiedienst verleent om de identiteit van hun gebruiker te bevestigen. |
| | MVPD AuthZ Service | Een eindpunt MVPD dat vergunningsbesluiten verstrekt die op de abonnementen van de gebruiker, ouderlijke controles, enz. worden gebaseerd. |


[](/help/authentication/glossary.md)

In het volgende diagram wordt de volledige flow geïllustreerd:

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### Stappen om REST API V2 in server-aan-server architectuur uit te voeren {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Als u Adobe Pass REST API V2 wilt implementeren, moet u de onderstaande stappen in fasen uitvoeren.

## 0. Vereisten {#prerequisites}

In server-to-server implementations, the Streaming App and Programmer Service needs to establish a protocol, for Programmer Service to be able to :
* uniquely identify the Streaming App on the device
* act on behalf of the Streaming App and communicate with Adobe Pass Service
* collect and store information about the Streaming App and the device like IP address, source port, device information to pass it to Adobe Pass
* return decisions and instructions to the Streaming App

Parameters like device ID, client id, client secret (defined below) can be stored on either Streaming App or Programmer Service.

## A. Registration phase {#registration-phase}

### Step 1: Register your application {#step-1-register-your-application}

For the application to be able to call Adobe Pass REST API V2, it needs an access token required by the API security layer. For server-to-server implementations, the Programmer Service can register on behalf of an application instance.
The client id and client secret values needs to be obtained for each streaming device.

[](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Verificatiefase {#authentication-phase}

### Stap 2: Controleren op bestaande geverifieerde profielen {#step-2-check-for-existing-authenticated-profiles}

De Dienst van de Programmer controleert namens Streaming App voor bestaande geverifieerde profielen: `/api/v2/{serviceProvider}/profiles` ([ wint voor authentiek verklaarde profielen ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Als er geen profiel is gevonden en de streamingtoepassing een TempPass-flow implementeert
   * Volg documentatie op hoe te om [ Tijdelijke toegangsstromen uit te voeren ](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Als er geen profiel wordt gevonden, implementeert de streaming-toepassing een verificatieworkflow
   * <b></b><b></b><br>[](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
   * the Programmer Service may implement filtering on the list of MVPDs and display only MVPDs intended while hiding others (TempPass, test MVPDs, MVPDs under development, etc.)
   * the Programmer Service should return a filtered MVPD list for the Streaming App to display picker, User selects the MVPD
   * <b></b><br>[](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)<br>
      * a CODE and URL to use for authentication is returned
      * <a href="#preauthorization-phase"></a>
   * the Programmer Service should return the CODE and URL to the Streaming App

### Step 3: Authenticate the user {#step-3-authenticate-the-user}

Using a Browser or a Second Screen Web based application:

* Option 1. Streaming App can open a browser or webview, load the URL to authenticate and the user lands on MVPD login page where credentials needs to be submitted
   * gebruiker inlognaam/wachtwoord invoeren, laatste omleiding toont een succespagina
* Option 2. Streaming App can&#39;t open a browser and just display the CODE. <b></b><b></b>
   * user enter login/password, final redirect show a success page

### Step 4: Check for authenticated profiles {#step-4-check-for-authenticated-profiles}

The Programmer Service checks for authentication with MVPD to complete in Browser or Second Screen

* <b></b><br>[](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
   * <b></b><br>[](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* Polling should not exceed 30 minutes, in case 30 minutes are reached and the Streaming Application is still active, a new session needs to be initiated and a new CODE and URL will be returned
* When authentication is complete the return is 200 with authenticated profile
* <a href="#preauthorization-phase"></a>

## C. Preauthorization phase {#preauthorization-phase}

### Step 5: Check for preauthorized resources {#step-5-check-for-preauthorized-resources}

With a valid authentication profile for a user, the Programmer Service has the possibility to check the
access to the videos available and pass the list to the Streaming Application to display.

* Step is optional and executed if the application wants to filter our the resources not available in the authenticated user package
* <b></b><br>[](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## D. Authorization phase {#authorization-phase}

### Stap 6: Controleren op geautoriseerde bronnen {#step-6-check-for-authorized-resources}

Streaming App bereidt zich voor op het afspelen van een video/middel/bron die door de gebruiker is geselecteerd.

* Step is necessary for every play start
* Streaming App pass this information to the Programmer Service
* <b></b><br>[](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
   * Decision = &#39;Permit&#39; , instrueert de Programmer Service de Streaming App om te beginnen met streamen
   * Decision = &#39;Deny&#39;, instrueert de Programmer Service de Streaming App om de gebruiker te laten weten dat deze geen toegang heeft tot die video
   * tijdens het proces kan de Programmeringsservice andere bedrijfsregels evalueren en de juiste beslissing teruggeven aan de Streaming-app

## E. Afmeldingsfase {#logout-phase}

### Stap 7: Afmelden {#step-7-logout}

Streaming app: gebruiker wil zich afmelden bij de MVPD

* Streaming App informeert de Programmer Service dat deze zich moet afmelden bij de MVPD voor deze specifieke app.
* de Programmeringsservice kan de informatie die is opgeslagen over de geverifieerde gebruiker opschonen
* de vraag van de Dienst van de Programmer <b>/api/v2/{serviceProvider}/logout/{mvpd} </b><br>
([ begin logout voor specifieke MVPD ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Als de reactie actionType=&#39;interactive&#39; en url aanwezig is, keert de Programmer Service terug naar de Streaming App de url
* Op basis van de bestaande mogelijkheden kan de Streaming-app de URL openen in de browser (meestal dezelfde als de URL die wordt gebruikt voor verificatie)
* If the Streaming App does not have a browser or it is a different instance than the one at authentication, the flow can be stopped as the MVPD session was not persisted in the browser cache.

## Omgevingen en functionele vereisten{#environments}

Een programmeur moet ten minste twee omgevingen maken: een voor productie en een of meer voor staging.


### Production

The production environment should be highly available and scaled appropriately for large or unexpected spikes (e.g. live sports, breaking
news).



The Adobe Pass service runs on multiple data centers geographically dispersed throughout the US.  To achieve the best response time (i.e. lowest latency) from the Adobe Pass service, the Programmer should also create a similar geographically dispersed service
infrastructure.


De dienst van de Programmer zou het DNS geheime voorgeheugen tot maximaal 30 s moeten beperken voor het geval de Adobe verkeer moet terugleiden. Dit kan zich voordoen als een datacenter niet beschikbaar is.


De programmeur zou de openbare IP waaier van het productiemilieu moeten verstrekken. Deze zullen in een toegestane lijst van IPs in de infrastructuur van Adobe Pass voor toegang worden ingegaan en door het Fair API gebruiksbeleid van Adobe worden beheerd.

### Staging

De het opvoeren omgeving kan minimaal zijn, maar zou alle systeemcomponenten en bedrijfslogica moeten omvatten. Het moet op dezelfde manier werken als de productie en het mogelijk maken om introducties buiten de productie te testen. Idealiter kan de testomgeving worden verbonden met de Adobe Pass-testomgevingen voor gebruik door de programmeur en, indien nodig, door Adobe, zodat we kunnen helpen bij het testen en oplossen van problemen.

### Functionele vereisten

De Programmeringsdienst moet nauwkeurige identificatiegegevens doorgeven voor het apparaat waarvoor zij de stromen uitvoeren. Bovendien moet de dienst van de Programmer IP van het apparaat overgaan waarvoor zij de stromen (in x-door:sturen-voor kopbal) samen met de verbindingbronhaven (op het gebied van apparateninfo) uitvoeren:

De Programmeringsservice moet gegevens en opmaak verzenden die vereist zijn voor afzonderlijke MVPD&#39;s of geïntegreerde apps (bijvoorbeeld apparaat-IP, bronpoort, apparaatinformatie, MRSS, optionele gegevens zoals ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->

De Programmeringsservice moet het verificatieprofiel en de geldigheid van de beslissingen respecteren bij het in cache plaatsen en de verificatie of beslissingen ongeldig maken bij kennisgeving.

The Programmer Service must maintain certificates shared with Adobe (for encrypted user metadata).

## Related Information {#related}

* [REST API V2 Reference](/help/authentication/rest-api-v2/rest-api-v2-flows-overview.md)
