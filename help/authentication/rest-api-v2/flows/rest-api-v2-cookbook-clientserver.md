---
title: REST API V2 - Cookbook - stappen voor implementatie client/server
description: REST API V2 - Cookbook - stappen voor implementatie client/server
source-git-commit: 5ba888b35731ef64b3dd1878f2fa55083989f857
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# REST API V2 - Cookbook - stappen voor implementatie client/server {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

## Stappen om REST API V2 in cliënt zijtoepassingen uit te voeren {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Als u Adobe Pass REST API V2 wilt implementeren, moet u de onderstaande stappen in fasen uitvoeren.

## A. Registratiefase {#registration-phase}

### Stap 1: De toepassing registreren {#step-1-register-your-application}
De toepassing kan Adobe Pass REST API V2 alleen aanroepen als hiervoor een toegangstoken is vereist op basis van de API-beveiligingslaag.
Om het toegangstoken te krijgen, moet de toepassing stappen zoals beschreven volgen:
[ Dynamische Registratie van de Cliënt ](./dynamic-client-registration.md)

## B. Verificatiefase {#authentication-phase}

### Stap 2: Controleren op bestaande geverifieerde profielen {#step-2-check-for-existing-authenticated-profiles}
Streaming toepassing checks for existing authenticated profiles: <b> /api/v2/{serviceProvider}/profiles </b><br>
([ wint voor authentiek verklaarde profielen ](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md) terug)

* als er geen profiel is gevonden en Streaming-toepassing een TempPass-flow implementeert
   * volg documentatie over hoe te om [ Tijdelijke toegangsstromen uit te voeren ](./temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* als er geen profiel wordt gevonden, implementeert streaming toepassing een verificatiestroom
   * De het stromen toepassing wint de lijst van MVPDs beschikbaar voor serviceProvider terug: <b>/api/v2/{serviceProvider}/configuration </b><br>
([ wint lijst van beschikbare MVPDs ](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) terug)
   * Streaming-toepassing kan filteren toepassen op de lijst van MVPD&#39;s en alleen MVPD&#39;s weergeven die zijn bedoeld terwijl andere worden verborgen (TempPass, test MVPD&#39;s, MVPD&#39;s die in ontwikkeling zijn, enz.)
   * De gebruiker selecteert de weergavekiezer voor streaming toepassingen
   * De het stromen toepassing leidt tot een zitting: <b>/api/v2/{serviceProvider}/zittingen </b><br>
([ creeer authentificatiesessie ](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)) <br>
      * er wordt een CODE en URL geretourneerd die voor verificatie moeten worden gebruikt
      * als een profiel wordt gevonden, kan de Streaming toepassing aan <a href="#preauthorization-phase"> C te werk gaan. De fase van de voorafgaande vergunning </a> of <a href="#authorization-phase"> D. Autorisatiefase </a>

### Stap 3: De gebruiker verifiëren {#step-3-authenticate-the-user}
Gebruikend Browser of een Tweede het Web-based toepassing van het Scherm:

* Optie 1. Streaming toepassing kan een browser of webweergave openen, de URL voor verificatie laden en de gebruiker landt op de MVPD-aanmeldingspagina waar gegevens moeten worden ingediend
   * gebruiker inlognaam/wachtwoord invoeren, eindomleiding geeft een succespagina weer
* Optie 2. Streaming toepassing kan geen browser openen en alleen de CODE weergeven. <b> een afzonderlijke Webtoepassing moet worden ontwikkeld </b> om de gebruiker te vragen om CODE in te gaan, bouw en open URL: <b>/api/v2/authenticate/{serviceProvider} {CODE} </b>
   * gebruiker inlognaam/wachtwoord invoeren, eindomleiding geeft een succespagina weer

### Stap 4: Controleren op geverifieerde profielen {#step-4-check-for-authenticated-profiles}
Streaming toepassing controleert op verificatie met MVPD om te voltooien in browser of tweede scherm

* het opiniepeilen om de 15 seconden wordt geadviseerd op <b> /api/v2/{serviceProvider}/profiles/{mvpd} </b><br>
([ wint voor authentiek verklaarde profielen voor specifieke MVPD ](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) terug)
   * als de selectie MVPD niet in de Streaming toepassing wordt gemaakt aangezien de plukker MVPD in de Tweede toepassing van het Scherm wordt voorgesteld, zou de opiniepeiling met CODE <b>/api/v2/ {serviceProvider} /profiles/code/ {CODE} moeten gebeuren </b><br>
([ wint voor authentiek verklaarde profielen voor specifieke CODE ](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) terug)
* De opiniepeiling mag niet langer zijn dan 30 minuten, als 30 minuten zijn bereikt en de streamingtoepassing nog actief is, moet een nieuwe sessie worden gestart en moet een nieuwe CODE en URL worden geretourneerd
* wanneer de authentificatie volledig is is de terugkeer 200 met voor authentiek verklaard profiel
* De Streaming toepassing kan aan <a href="#preauthorization-phase"> C te werk gaan. De fase van de voorafgaande vergunning </a> of <a href="#authorization-phase"> D. Autorisatiefase </a>

## C. Pretoelatingsfase {#preauthorization-phase}

### Stap 5: Controleren op vooraf geautoriseerde bronnen {#step-5-check-for-preauthorized-resources}
Streaming toepassing bereidt zich voor op het weergeven van de video&#39;s die beschikbaar zijn voor de geverifieerde gebruiker en heeft de mogelijkheid om de
toegang tot deze bronnen.
* De stap is optioneel en wordt uitgevoerd als de toepassing de bronnen die niet beschikbaar zijn in het geverifieerde gebruikerspakket wil filteren
* oproep aan <b> /api/v2/{serviceProvider} /decisions/preauthorize/{mvpd} </b><br>
([ verkrijg pre-vergunningsbesluit gebruikend specifieke MVPD ](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))


## D. Vergunningsfase {#authorization-phase}

### Stap 6: Controleren op geautoriseerde bronnen {#step-6-check-for-authorized-resources}
Streaming toepassing bereidt zich voor op het afspelen van een video/middel/bron die door de gebruiker is geselecteerd.

* stap is nodig voor elke start van het afspelen
* call <b>/api/v2/{serviceProvider} /decisions/authorize/{mvpd} </b><br>
([ ontvang vergunningsbesluit gebruikend specifieke MVPD ](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * Decision = &#39;Permit&#39; , streamingapparaat start streaming
   * Decision = &#39;Deny&#39;, het streamingapparaat geeft de gebruiker te horen dat het geen toegang heeft tot die video

## E. Afmeldingsfase {#logout-phase}

### Stap 7: Afmelden {#step-7-logout}
Streaming apparaat: gebruiker wil zich afmelden bij de MVPD

* call <b>/api/v2/{serviceProvider}/logout/{mvpd} </b><br>
([ initieert logout voor specifieke MVPD ](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* als de reactie actionType=&#39;interactive&#39; en url aanwezig is, open url in Browser/Tweede Scherm om logout met MVPD te voltooien

