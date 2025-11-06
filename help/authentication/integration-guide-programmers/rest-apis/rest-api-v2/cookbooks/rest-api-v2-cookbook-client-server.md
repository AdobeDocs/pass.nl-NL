---
title: REST API V2 Cookbook (client-naar-server)
description: REST API V2 Cookbook (client-naar-server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# REST API V2 Cookbook (client-naar-server) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

Het document is voorgenomen voor ontwikkelaars die [ de Authentificatie van Adobe Pass REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in hun het stromen toepassingen integreren die een cliënt-aan-Server (C2S) architectuur hebben.

## Vereisten {#prerequisites}

Voor termijnen en definities, verwijs naar de [ REST API V2 Verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) documentatie.

Voor verplichte vereisten en geadviseerde praktijken, verwijs naar de [ REST API V2 Checklist ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) documentatie.

Voor vaak gestelde vragen, verwijs naar [ REST API V2 FAQs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) documentatie.

## A. Registratiefase {#registration-phase}

Het doel van de registratiefase is de streamingtoepassing te registreren met Adobe Pass Authentication via het DCR-proces (Dynamic Client Registration).

Het dynamische proces van de Registratie van de Cliënt (DCR) vereist de het stromen toepassing om een paar cliëntgeloofsbrieven te verkrijgen en een toegangstoken als einddoel van de Fase van de Registratie terug te winnen.

De registratiefase is verplicht, maar de streamingtoepassing kan deze fase overslaan als deze een in cache geplaatst paar clientgegevens en een toegangstoken heeft die nog geldig zijn.

+++Verwante artikelen

**APIs:**

* [Client-referenties ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Toegangstoken ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Stromen:**

* [Dynamic Client Registration Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQs:**

* [Veelgestelde vragen over de registratiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Stap 1: De toepassing registreren {#step-1-register-your-application}

* **wint cliëntgeloofsbrieven terug:** de het stromen toepassing wint cliëntgeloofsbrieven door het [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) eindpunt te roepen.

   * De streamingtoepassing moet de clientgegevens opslaan en deze onbeperkt gebruiken wanneer een toegangstoken moet worden opgehaald.


* **wint toegangstoken terug:** de het stromen toepassing wint toegangstoken terug door het [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) eindpunt te roepen.

   * De streamingtoepassing moet het toegangstoken opslaan en gebruiken tot het verloopt, het vervolgens verwijderen en een nieuw token verkrijgen.

## B. Verificatiefase {#authentication-phase}

Het doel van de authentificatiefase is de het stromen toepassing de capaciteit te verstrekken om de identiteit van de gebruiker te verifiëren en informatie van gebruikersmeta-gegevens te verkrijgen.

De verificatiefase fungeert als een noodzakelijke stap voor de fase voorafgaand aan autorisatie of de machtigingsfase wanneer de streamingtoepassing inhoud moet afspelen.

+++Verwante artikelen

**APIs**

* [Verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Verificatiesessie ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Verificatie uitvoeren in gebruikersagent](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Profielen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profiel ophalen voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profiel ophalen voor specifieke code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Stromen**

* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**FAQs**

* [Veelgestelde vragen over de verificatiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Stap 2: Controleren op bestaande geverifieerde profielen {#step-2-check-for-existing-authenticated-profiles}

* **wint profielen terug:** de het stromen toepassing controleert bestaande profielen door het [**/api/v2/ {serviceProvider} /profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) eindpunt te roepen.


* **Scenario 1:** er zijn bestaande profielen, kan de het stromen toepassing aan de [ VoorkeurFase ](#preauthorization-phase) of [ Fase van de Toestemming ](#authorization-phase) te werk gaan.


* **Scenario 2:** Er zijn geen bestaande profielen, kan de het stromen toepassing aan de volgende stap te werk gaan [ de gebruiker ](#step-3-authenticate-the-user) voor authentiek verklaren.


* **Scenario 3:** Er zijn geen bestaande profielen, kan de het stromen toepassing te werk gaan om de gebruiker van tijdelijke toegang door de [ TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) eigenschap te voorzien.

   * Dit scenario is buiten het werkingsgebied van dit document, verwijs naar de [ Tijdelijke documentatie van de Stromen van de Toegang ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) voor meer informatie.

### Stap 3: De gebruiker verifiëren {#step-3-authenticate-the-user}

* **wint configuratie terug:** de het stromen toepassing wint de lijst van beschikbare MVPDs door het [**/api/v2/ {serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) eindpunt te roepen.

   * De het stromen toepassing kan een douane filtrerend mechanisme uitvoeren om de lijst van MVPDs van de configuratiereactie te verfijnen, die slechts de voorgenomen leveranciers toont terwijl het verbergen van anderen (b.v., MVPDs in ontwikkeling, test MVPDs, TempPass). Op deze manier weet u zeker dat gebruikers een curatieve selectie krijgen wanneer ze hun tv-provider kiezen.


* **creeer authentificatiesessie:** de het stromen toepassing stelt een authentificatiesessie door het [**/api/v2/ {serviceProvider} in werking te roepen/zittingen**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) eindpunt.


* **Scenario 1:** de het stromen toepassing kan browser of webmening openen, daarom moet het de authentificatie `url` laden.

   * De gebruiker verzendt zijn gebruikersnaam en wachtwoord binnen de MVPD aanmeldingspagina. Bij geslaagde verificatie wordt een succespagina weergegeven met de uiteindelijke omleiding.


* **Scenario 2:** de het stromen toepassing kan geen browser openen, daarom moet het de authentificatie `code` tonen. Een afzonderlijke webtoepassing is vereist om de gebruiker te vragen de `code` in te voeren, de verificatie `url` samen te stellen en open: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) .

   * De gebruiker verzendt zijn gebruikersnaam en wachtwoord binnen de MVPD aanmeldingspagina. Bij geslaagde verificatie wordt een succespagina weergegeven met de uiteindelijke omleiding.

### Stap 4: Controleren op geverifieerde profielen {#step-4-check-for-authenticated-profiles}

* **wint profiel voor specifieke code terug:** de het stromen toepassing moet een opiniepeilingsmechanisme uitvoeren gebruikend `code` om te controleren of werd het profiel met succes geproduceerd en bewaard door [**/api/v2/ {serviceProvider} /profiles/code/ {code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) eindpunt te roepen.

   * De het stromen toepassing moet **beginnen het opiniepeilings** mechanisme onder de volgende voorwaarden:

      * **Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd:** de primaire (het stromen) toepassing zou moeten beginnen opiniepeilend wanneer de gebruiker de definitieve bestemmingspagina bereikt, nadat de browser component URL laadt die voor de `redirectUrl` parameter in het [ wordt gespecificeerd Sessions ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) eindpuntverzoek.

      * **Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd:** de primaire (het stromen) toepassing zou moeten beginnen opiniepeilend zodra de gebruiker het authentificatieproces-recht na het ontvangen van de [ 3} eindpuntreactie van Zittingen {en het tonen van de authentificatiecode aan de gebruiker in werking stelt.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)

   * De het stromen toepassing moet **het opiniepeilings** mechanisme onder de volgende voorwaarden tegenhouden:

      * **Succesvolle authentificatie:** de het profielinformatie van de gebruiker wordt met succes teruggewonnen, bevestigend hun authentificatiestatus. Op dit moment is opiniepeiling niet langer nodig.

      * **de zitting van de Authentificatie en de codereduur:** de authentificatiesessie en de code verlopen, zoals die door `notAfter` wordt vermeld timestamp (b.v., 30 minuten) in de [ 4} eindpuntreactie van Zittingen {. ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) Als dit gebeurt, moet de gebruiker het authentificatieproces opnieuw beginnen, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

      * **Nieuwe geproduceerde authentificatiecode:** als de gebruiker om een nieuwe authentificatiecode op het primaire (scherm) apparaat verzoekt, is de bestaande zitting niet meer geldig, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

   * De het stromen toepassing moet **de opiniepeilende** mechanismefrequentie onder de volgende voorwaarden vormen:

      * **Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd:** de primaire (het stromen) toepassing zou elke 3-5 seconden of meer moeten opiniepeilen.

      * **Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd:** de primaire (het stromen) toepassing zou elke 3-5 seconden of meer moeten opiniepeilen.

   * De streamingtoepassing moet delen van de profielgegevens van de gebruiker in een permanente opslag in cache plaatsen om onnodige aanvragen te voorkomen en de gebruikerservaring te verbeteren.

## C. (Facultatief) Fase van voorafgaande toestemming {#preauthorization-phase}

Het doel van de fase voorafgaand aan de autorisatie is om de streamingtoepassing de mogelijkheid te bieden een subset van bronnen uit de catalogus te presenteren waartoe de gebruiker toegang zou hebben.

De fase voorafgaand aan autorisatie kan de gebruikerservaring verbeteren wanneer de gebruiker de streaming toepassing voor het eerst opent of naar een nieuwe sectie navigeert.

De fase voorafgaand aan autorisatie is niet verplicht. De streamingtoepassing kan deze fase overslaan als deze een catalogus met bronnen wil presenteren zonder deze eerst te filteren op basis van de machtiging van de gebruiker.

+++Verwante artikelen

**APIs**

* [Toestemmingsbesluiten ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Stromen**

* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQs**

* [Veelgestelde vragen over de preautorisatiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Stap 5: Controleren op vooraf geautoriseerde bronnen {#step-5-check-for-preauthorized-resources}

* **wint pre-vergunningsbesluiten terug:** de het stromen toepassing wint pre-vergunningsbesluiten voor een lijst van middelen door het [**/api/v2/ {serviceProvider}/decisions/preAuthze/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) eindpunt te roepen.

   * De streamingtoepassing is niet vereist voor het opslaan van beslissingen voorafgaand aan de autorisatie in permanente opslag. Nochtans, wordt het geadviseerd om vergunningsbesluiten in het voorgeheugen op te slaan om de gebruikerservaring te verbeteren. Dit helpt onnodige vraag naar middelen vermijden die reeds vooraf zijn geautoriseerd, die latentie verminderen en prestaties verbeteren.

   * De het stromen toepassing kan de reden voor een ontkend pre-vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van Besluiten pre-goedkeurt eindpunt. Deze gegevens verschaffen insight de specifieke reden waarom het verzoek om voorafgaande toestemming is geweigerd, zodat de gebruiker op de hoogte wordt gebracht van de gebruikerservaring of de benodigde afhandeling in de toepassing wordt geactiveerd. Ervoor zorgen dat elk nieuw mechanisme dat wordt toegepast voor het ophalen van beslissingen vóór toelating, niet in een eindeloze lus resulteert als het besluit vóór toelating wordt geweigerd. U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

   * De streamingtoepassing kan een voorafgaande beslissing voor een beperkt aantal bronnen verkrijgen in één API-aanvraag, meestal maximaal 5, als gevolg van voorwaarden die door de MVPD&#39;s worden opgelegd. Dit maximumaantal middelen kan worden bekeken en veranderd na het akkoord gaan met MVPDs door het Dashboard van Adobe Pass [ TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.


## D. Vergunningsfase {#authorization-phase}

Het doel van de machtigingsfase is om de streamingtoepassing de mogelijkheid te bieden bronnen af te spelen die de gebruiker vraagt nadat hij zijn rechten met de MVPD heeft gevalideerd.

De autorisatiefase is verplicht. De streamingtoepassing kan deze fase niet overslaan als deze bronnen wil afspelen die de gebruiker vraagt, omdat de gebruiker eerst met de MVPD moet controleren of de gebruiker gerechtigd is voordat de stream wordt vrijgegeven.

+++Verwante artikelen

**APIs**

* [Toestemmingsbesluiten ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Stromen**

* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQs**

* [Veelgestelde vragen over de machtigingsfase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Stap 6: Controleren op geautoriseerde bronnen {#step-6-check-for-authorized-resources}

* **wint vergunningsbesluit terug:** de het stromen toepassing wint vergunningsbesluit voor een specifiek middel door het [**/api/v2/ {serviceProvider}/Decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) eindpunt te roepen.

   * De streamingtoepassing is niet vereist voor het opslaan van beslissingen over machtigingen in permanente opslag.

   * De het stromen toepassing kan de reden voor een ontkend vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van Besluiten machtigt eindpunt. Deze gegevens verschaffen insight de specifieke reden waarom de aanvraag voor een vergunning is afgewezen, zodat de gebruiker op de hoogte kan worden gebracht van de ervaring of een noodzakelijke afhandeling van de toepassing kan worden gestart. Ervoor zorgen dat een nieuw mechanisme dat wordt toegepast voor het ophalen van vergunningsbesluiten niet in een eindeloze lus resulteert als het vergunningsbesluit wordt geweigerd. U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

   * De streamingtoepassing is niet vereist om een verlopen mediatoken te vernieuwen terwijl de stream actief wordt afgespeeld. Als het media-token tijdens het afspelen verloopt, moet het zijn toegestaan dat de stream zonder onderbreking wordt voortgezet. Nochtans, moet de cliënt om een nieuw vergunningsbesluit verzoeken — en een nieuw media teken verkrijgen — de volgende tijd de gebruiker probeert om een middel te spelen.

   * De streamingtoepassing kan een machtigingsbesluit voor een beperkt aantal bronnen verkrijgen in één API-aanvraag, meestal maximaal 1, als gevolg van voorwaarden die door MVPD&#39;s worden opgelegd.

## E. Afmeldingsfase {#logout-phase}

Het doel van de afmeldingsfase is om de streamingtoepassing de mogelijkheid te bieden het geverifieerde profiel van de gebruiker binnen de Adobe Pass-verificatie op verzoek te beëindigen.

De afmeldingsfase is verplicht. De streamingtoepassing moet de gebruiker de mogelijkheid bieden zich af te melden.

+++Verwante artikelen

**APIs**

* [Afmelden starten voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Stromen**

* [Basisuitlogingsstroom uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQs**

* [Veelgestelde vragen over de afmeldingsfase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Stap 7: Afmelden {#step-7-logout}

* **stelt Adobe Pass logout in werking:** de het stromen toepassing stelt de logout stroom door het [**in werking/api/v2/ {serviceProvider} /logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) eindpunt te roepen.

   * De streamingtoepassing moet de instructies volgen die in de kenmerken `actionName` en `actionType` van de eindpuntreactie bij de aanmelding zijn opgegeven om ervoor te zorgen dat het logout-proces correct is voltooid.

      * Als het kenmerk `actionType` in het antwoord is ingesteld op &quot;interactive&quot;:

         * **Scenario 1:** de het stromen toepassing kan browser of webmening openen, daarom moet het logout `url` laden.

         * **Scenario 2:** de het stromen toepassing kan geen browser openen, daarom kan het logout proces worden tegengehouden aangezien de zitting van MVPD niet in het geheime voorgeheugen van een het stromen apparatenbrowser werd voortgeduurd.
