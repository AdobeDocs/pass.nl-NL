---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 563e0b17de3be9290a661242355b4835b8c386e1
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

Het doel van dit kookboekdocument is best practices voor het implementeren van Adobe Pass Authentication met behulp van REST API V2 in een server-naar-server architectuur in detail te beschrijven. Het verstrekt basisvereisten, geleidelijke stroomimplementatie en algemene overwegingen voor productiemilieu&#39;s en verrichting.

## Componenten {#components}

In een werkende server-aan-server oplossing, zijn de volgende componenten betrokken:

| Type | Component | Beschrijving |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Streaming apparaat | Streaming-app | De programmeertoepassing die op het het stromen apparaat van de gebruiker verblijft en voor authentiek verklaarde video speelt. |
|                           | \[Optioneel\] AuthN-module | Als het Streamen Apparaat een Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Module AuthN verantwoordelijk voor het voor authentiek verklaren van de gebruiker op MVPD IdP. |
| \[Optioneel\] AuthN-apparaat | AuthN App | Als het Streaming Apparaat geen Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Toepassing AuthN een Toepassing van het Web van de Programmer die van het apparaat van een afzonderlijke gebruiker gebruikend Webbrowser wordt betreden. |
| Programmeringsinfrastructuur | Programmeringsservice | Een service die het streamingapparaat koppelt aan de Adobe Pass-service om verificatie- en autorisatiebeslissingen te verkrijgen. |
| Adobe-infrastructuur | Adobe Pass Service | De dienst die met de Dienst MVPD IdP en AuthZ integreert en authentificatie en vergunningsbesluiten verstrekt. |
| MVPD-infrastructuur | MVPD IdP | Een eindpunt MVPD dat op referentie-gebaseerde authentificatiedienst verleent om de identiteit van hun gebruiker te bevestigen. |
|                           | MVPD AuthZ Service | Een eindpunt MVPD dat vergunningsbesluiten verstrekt die op de abonnementen van de gebruiker, ouderlijke controles, enz. worden gebaseerd. |

De extra termijnen die in de stroom worden gebruikt worden bepaald in de [ Verklarende woordenlijst ](/help/authentication/glossary.md).

Het volgende diagram illustreert de volledige flow:

![ REST API V2 Cookbook (server-aan-Server) ](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Stappen om REST API V2 in server-aan-server architectuur uit te voeren {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Als u Adobe Pass REST API V2 wilt implementeren, moet u de onderstaande stappen in fasen uitvoeren.

## 0. Vereisten {#prerequisites}

In Server-aan-Server implementaties, moet de Streaming App en de Dienst van Programmer een protocol voor de Dienst van Programmer vestigen om te kunnen:

* Identificeer de Streaming App op unieke wijze op het apparaat.
* Doe namens de Streaming App en communiceer met Adobe Pass Service.
* Verzamel en bewaar informatie over de Streaming App en het apparaat zoals IP adres, bronhaven, apparateninformatie om het tot Adobe Pass over te gaan.
* Retourneer beslissingen en instructies aan de Streaming App.

Parameters zoals apparaat-id, client-id, clientgeheim (zoals hieronder gedefinieerd) kunnen worden opgeslagen in Streaming App of Programmer Service.

## A. Registratiefase {#registration-phase}

### Stap 1: De toepassing registreren {#step-1-register-your-application}

De toepassing kan Adobe Pass REST API V2 alleen aanroepen als hiervoor een toegangstoken is vereist op basis van de API-beveiligingslaag.

Voor Server-aan-Server implementaties, kan de Dienst van de Programmer namens een toepassingsinstantie registreren, toch moeten de cliëntgeloofsbrieven (cliënt identiteitskaart en cliëntgeheim) waarden voor elk het stromen apparaat worden verkregen.

Om het toegangstoken te krijgen, kan de Dienst van de Programmer namens een het stromen App handelen en moet stappen volgen zoals die in de [ Dynamische documentatie van de Registratie van de Cliënt ](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) worden beschreven.

## B. Verificatiefase {#authentication-phase}

### Stap 2: Controleren op bestaande geverifieerde profielen {#step-2-check-for-existing-authenticated-profiles}

De dienst van de Programmer controleert namens Streaming App voor bestaande voor authentiek verklaarde profielen: `/api/v2/{serviceProvider}/profiles` ([ wint voor authentiek verklaarde profielen ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) terug).

* Als er geen profiel is gevonden en Streaming toepassing een TempPass-flow implementeert
   * Volg documentatie op hoe te om [ Tijdelijke toegangsstromen uit te voeren ](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Als er geen profiel wordt gevonden, implementeert Streaming-toepassing een verificatiestroom
   * <b> Stap 2.a:</b> de Dienst van de Programmer wint de lijst van MVPDs beschikbaar voor serviceProvider terug: <b>/api/v2/{serviceProvider}/configuration </b><br>
([ wint lijst van beschikbare MVPDs ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) terug)
   * De Programmeringsdienst kan filteren op de lijst van MVPD&#39;s uitvoeren en alleen MVPD&#39;s weergeven die zijn bedoeld terwijl andere worden verborgen (TempPass, test MVPD&#39;s, MVPD&#39;s die in ontwikkeling zijn, enz.)
   * De dienst van de Programmer zou een gefiltreerde MVPD lijst voor de Streaming App aan vertoningsplukker moeten terugkeren, selecteert de Gebruiker MVPD
   * Met MVPD die van het Stromen App wordt geselecteerd, leidt de Dienst van de Programmer tot een zitting: <b>/api/v2/{serviceProvider}/zittingen </b><br>
([ creeer authentificatiesessie ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)) <br>
      * Er wordt een CODE en URL geretourneerd die voor verificatie moeten worden gebruikt
      * Als een profiel wordt gevonden, kan de Dienst van de Programmer aan <a href="#preauthorization-phase"> C te werk gaan. Voorbereidende fase </a>
   * De programmeerservice moet de CODE en de URL naar de streamingtoepassing retourneren

### Stap 3: De gebruiker verifiëren {#step-3-authenticate-the-user}

Een webtoepassing voor het tweede scherm of een browser gebruiken:

* Optie 1. Streaming App kan een browser of webweergave openen, de voor verificatie te gebruiken URL laden en de gebruiker landt op de MVPD-aanmeldingspagina waar gegevens moeten worden ingediend
   * Gebruiker voert login/wachtwoord in, definitieve omleiding toont een succespagina
* Optie 2. Streaming app kan geen browser openen en alleen de CODE weergeven. <b> een afzonderlijke Webtoepassing, AuthN_APP, moet worden ontwikkeld </b> om de gebruiker te vragen om CODE in te gaan, URL te bouwen en te openen: <b>/api/v2/authenticate/{serviceProvider} {CODE} </b>
   * Gebruiker voert login/wachtwoord in, definitieve omleiding toont een succespagina

### Stap 4: Controleren op geverifieerde profielen {#step-4-check-for-authenticated-profiles}

De dienst van de Programmer controleert authentificatie met MVPD om in Browser of Tweede Scherm te voltooien

* Opiniepeiling om de 15 seconden wordt aanbevolen op <b> /api/v2/{serviceProvider} /profiles/{mvpd} </b><br>
([ wint voor authentiek verklaarde profielen voor specifieke MVPD ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) terug)
   * Als de selectie MVPD niet in de Streaming toepassing wordt gemaakt aangezien de plukker MVPD in de Tweede toepassing van het Scherm wordt voorgesteld, zou de opiniepeiling met CODE <b>/api/v2/ {serviceProvider} /profiles/code/ {CODE} moeten gebeuren </b><br>
([ wint voor authentiek verklaarde profielen voor specifieke CODE ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) terug)
* De opiniepeiling zou niet 30 minuten moeten overschrijden, als 30 minuten worden bereikt, en de Streaming Toepassing nog actief is, moet een nieuwe zitting in werking worden gesteld en een nieuwe CODE en URL zullen zijn teruggekeerd
* Wanneer de authentificatie volledig is, is de terugkeer 200 met voor authentiek verklaard profiel
* De dienst van de Programmer kan aan <a href="#preauthorization-phase"> C te werk gaan. Voorbereidende fase </a>

## C. Pretoelatingsfase {#preauthorization-phase}

### Stap 5: Controleren op vooraf geautoriseerde bronnen {#step-5-check-for-preauthorized-resources}

Met een geldig authentificatieprofiel voor een gebruiker, heeft de Dienst van de Programmer de mogelijkheid om de toegang tot de beschikbare video&#39;s te controleren en de lijst tot de Streaming Toepassing over te gaan om te tonen.

* De stap is optioneel en wordt uitgevoerd als de toepassing de bronnen wil uitfilteren die niet beschikbaar zijn in het geverifieerde gebruikerspakket.
* Aanroep van <b> /api/v2/{serviceProvider} /decisions/preauthorize/{mvpd} </b><br>
([ verkrijg pre-vergunningsbesluit gebruikend specifieke MVPD ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Vergunningsfase {#authorization-phase}

### Stap 6: Controleren op geautoriseerde bronnen {#step-6-check-for-authorized-resources}

Streaming App bereidt zich voor op het afspelen van een video/middel/bron die door de gebruiker is geselecteerd.

* Stap is nodig voor elke start van het afspelen
* Streaming App geeft deze informatie door aan de Programmer Service
* De dienst van de Programmer namens de Streaming App, roep <b>/api/v2/{serviceProvider}/Decision/authorize/{mvpd} </b><br>
([ ontvang vergunningsbesluit gebruikend specifieke MVPD ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * Decision = &#39;Permit&#39;, geeft Programmer Service de Streaming App de opdracht om te beginnen met streamen
   * Decision = &#39;Deny&#39;, geeft Programmer Service de Streaming App de opdracht de gebruiker te laten weten dat deze geen toegang heeft tot die video
   * tijdens het proces kan de Programmeringsservice andere bedrijfsregels evalueren en de juiste beslissing teruggeven aan de Streaming-app

## E. Afmeldingsfase {#logout-phase}

### Stap 7: Afmelden {#step-7-logout}

Streaming app: gebruiker wil zich afmelden bij de MVPD

* Streaming App informeert de Programmer Service dat deze zich moet afmelden bij de MVPD voor deze specifieke app.
* De Programmeringsservice kan de informatie die deze over de geverifieerde gebruiker opslaat opschonen
* De vraag van de Dienst van de Programmer <b>/api/v2/{serviceProvider}/logout/{mvpd} </b><br>
([ initieert logout voor specifieke MVPD ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Als de reactie actionType=&#39;interactive&#39; en url aanwezig is, zal de Programmer Service aan de Streaming App url terugkeren
* Op basis van bestaande mogelijkheden opent de streamingtoepassing mogelijk de URL in de browser (meestal dezelfde als voor verificatie)
* Als de Streaming-app geen browser heeft of een andere instantie is dan de Streaming-app bij verificatie, kan de stroom worden gestopt omdat de MVPD-sessie niet in de cache van de browser werd herhaald.

## Omgevingen en functionele vereisten{#environments}

Een programmeur moet ten minste twee omgevingen maken: een voor productie en een of meer voor staging.

### Productie

De productieomgeving moet in hoge mate beschikbaar zijn en op passende wijze worden geschaald voor grote of onverwachte pieken (bv. live sport, baanbrekend nieuws).

De Adobe Pass Service wordt uitgevoerd op meerdere datacenters die geografisch over de hele VS zijn verspreid. Om de beste reactietijd (d.w.z. de laagste latentie) van de dienst van Adobe Pass te bereiken, zou de programmeur ook een gelijkaardige geografisch verspreide de dienstinfrastructuur moeten creëren.

De dienst van de Programmer zou het DNS geheime voorgeheugen tot maximaal 30 s moeten beperken voor het geval de Adobe verkeer moet terugleiden. Dit kan zich voordoen als een datacenter niet meer beschikbaar is.

De programmeur zou de openbare IP waaier van het productiemilieu moeten verstrekken. Deze zullen in een toegestane lijst van IPs in de infrastructuur van Adobe Pass voor toegang worden ingegaan en door het Fair API gebruiksbeleid van Adobe worden beheerd.

### Staging

De het opvoeren omgeving kan minimaal zijn, maar zou alle systeemcomponenten en bedrijfslogica moeten omvatten. Het moet op dezelfde wijze functioneren als de productie en het moet mogelijk maken om emissies buiten de productie te testen. In het ideale geval kan de testomgeving worden verbonden met de Adobe Pass-testomgevingen voor gebruik door de programmeur en door Adobe wanneer dat nodig is, zodat we u kunnen helpen bij het testen en oplossen van problemen.

### Functionele vereisten

De programmeerdienst moet nauwkeurige informatie doorgeven over de identificatie van het apparaat waarvoor hij de stromen uitvoert. Bovendien, moet de dienst van de Programmer IP van het apparaat overgaan waarvoor zij de stromen (in x-door:sturen-voor kopbal) samen met de haven van de verbindingsbron (op het gebied van apparateninfo) uitvoeren:

De programmeerservice moet gegevens en opmaak verzenden die vereist zijn voor afzonderlijke MVPD&#39;s of geïntegreerde apps (bijvoorbeeld apparaat-IP, bronpoort, apparaatinformatie, MRSS, optionele gegevens zoals ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->

De dienst van de Programmeur moet authentificatieprofiel en besluitvorming geldigheid eerbiedigen wanneer het in het voorgeheugen onderbrengen en de authentificaties of de besluiten ongeldig maken wanneer op de hoogte gebracht.

De Programmeringsservice moet certificaten onderhouden die worden gedeeld met de Adobe (voor gecodeerde gebruikersmetagegevens).

## Gerelateerde informatie {#related}

* [REST API V2 Reference](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
