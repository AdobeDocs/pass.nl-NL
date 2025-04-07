---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2510'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

Het document is voorgenomen voor ontwikkelaars die [ de Authentificatie van Adobe Pass REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in hun het stromen toepassingen integreren die een Server-aan-Server (S2S) architectuur hebben.

## Vereisten {#prerequisites}

Voor termijnen en definities, verwijs naar de [ REST API V2 Verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) documentatie.

Voor verplichte vereisten en geadviseerde praktijken, verwijs naar de [ REST API V2 Checklist ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) documentatie.

Voor vaak gestelde vragen, verwijs naar [ REST API V2 FAQs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) documentatie.

### Componenten {#components}

Voordat u aan de slag gaat, dient u te weten wat de volgende componenten en termen zijn die in de workflow worden gebruikt:

| Type | Component | Beschrijving |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe-infrastructuur | Adobe Pass Service | Integreert met de Dienst van MVPD IdP en AuthZ om authentificatie en vergunningsbesluiten te verstrekken. |
| Programmeringsinfrastructuur | Programmeringsservice | Sluit het streamingapparaat aan op de Adobe Pass-service om geverifieerde profielen en autorisatiebeslissingen te verkrijgen. |
| MVPD-infrastructuur | MVPD IdP-service | Het eindpunt van MVPD verantwoordelijk voor op referentie-gebaseerde authentificatie, die de identiteit van de gebruiker bevestigt. |
|                           | MVPD AuthZ Service | Het eindpunt van MVPD dat vergunningsbesluiten bepaalt die op gebruikersabonnementen, ouderlijke controles, en andere machtigingsregels worden gebaseerd. |
| Streaming apparaat | Streaming-app | De toepassing van de Programmer die op het het stromen apparaat van de gebruiker loopt en voor authentiek verklaarde videoinhoud speelt. |
|                           | (Optioneel) AuthN-module | Als het streamingapparaat een gebruikersagent heeft (bijvoorbeeld een browser), handelt de AuthN-module gebruikersverificatie op de MVPD IdP af. |
| (Optioneel) AuthN-apparaat | AuthN App | Als het streamingapparaat geen gebruikersagent heeft (bijvoorbeeld een browser), is de AuthN-toepassing een programmeerwebtoepassing die via een webbrowser vanaf een afzonderlijk apparaat kan worden benaderd. |

### Vereisten {#requirements}

In Server-aan-Server (S2S) implementaties, moeten de Streaming App en de Dienst van de Programmer een protocol vestigen dat de Dienst van de Programmer toelaat:

* Communiceer met Adobe Pass Service namens de Streaming App.

* Verzamel en ga een uniek apparatenherkenningsteken voor het Streamen Apparaat zoals die door [ wordt vereist AP-Apparaat-Herkenningsteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) kopbal over.

* Verzamel en ga nauwkeurige het Stromen informatie van het Apparaat, met inbegrip van de bronhaven en apparaat-specifieke details, zoals die door [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) kopbal worden vereist.

* Verzamel en geef het IP-adres van het Streaming apparaat door zoals vereist door de X-Forwarded-For header.

* Sla parameters zoals apparaat-id, client-id en clientgeheim op in de Streaming-app of de Programmer-service.

* Gegevens opmaken en verzenden in overeenstemming met MVPD&#39;s en geïntegreerde apps, waaronder apparaat-IP, bronpoort, apparaatspecifieke informatie, MRSS en optionele id&#39;s zoals ECID.

* Certificaten onderhouden en veilig beheren die met Adobe worden gedeeld voor de verzending van gecodeerde metagegevens van gebruikers.

* Het verificatieprofiel en de geldigheid van het autorisatiebesluit respecteren wanneer caching, het waarborgen van authenticatie en autorisatiestatus ongeldig worden gemaakt wanneer kennisgeving wordt gedaan.

* Retourvergunningsbesluiten en relevante instructies voor de Streaming App.

### Omgevingen {#environments}

Zorg ervoor dat u ten minste twee omgevingen in stand houdt: Productie en Staging voordat u de workflow opent.

**Productie**

De productieomgeving moet hoog beschikbaar zijn en op de juiste wijze worden geschaald om grote of onverwachte verkeersopstoppingen, zoals die welke door live sportevenementen of het doorbreken van nieuws worden veroorzaakt, aan te kunnen.

* De Adobe Pass Service werkt in meerdere geografisch verspreide datacenters in de VS om de prestaties te optimaliseren en de latentie te minimaliseren.

   * De Programmeringsservice moet een vergelijkbare infrastructuurstrategie volgen, waarbij de responstijden voor lage latentie van Adobe Pass worden gewaarborgd.

* De programmeur moet de openbare IP waaier van zijn productiemilieu verstrekken.

   * Deze IPs zal aan een lijst van gewenste personen binnen de Infrastructuur van Adobe Pass worden toegevoegd.

* De dienst van de Programmer moet DNS caching tot maximaal 30 seconden beperken om voor dynamische het verpletteren toe te staan voor het geval Adobe verkeer moet omleiden omdat een gegevenscentrum niet beschikbaar wordt.

**het Opvoeren**

De het opvoeren omgeving kan minimaal zijn maar zou productie moeten weerspiegelen door alle kritieke systeemcomponenten en bedrijfslogica te omvatten.

* Het moet testversies mogelijk maken voordat het product in productie wordt genomen.

* Het moet operationeel vergelijkbaar blijven met de productie, zodat realistische tests mogelijk zijn.

* In het ideale geval moet de testomgeving worden verbonden met Adobe Pass-testomgevingen met:

   * Sta Programmeurs toe om tegen de Infrastructuur van Adobe te testen.

   * Schakel Adobe in als hulp bij het testen en oplossen van problemen.

## Workflow {#workflow}

Voer de onderstaande stappen uit zoals in het volgende diagram.

![ REST API V2 Cookbook (server-aan-Server) ](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2 Cookbook (server-aan-Server)*

## A. Registratiefase {#registration-phase}

Het doel van de registratiefase is de streamingtoepassing te registreren met Adobe Pass Authentication via het DCR-proces (Dynamic Client Registration).

Het dynamische proces van de Registratie van de Cliënt (DCR) vereist de het stromen toepassing om een paar cliëntgeloofsbrieven te verkrijgen en een toegangstoken als einddoel van de Fase van de Registratie terug te winnen.

De registratiefase is verplicht, maar de streamingtoepassing kan deze fase overslaan als deze een in cache geplaatst paar clientgegevens en een toegangstoken heeft die nog geldig zijn.

++ + verwante artikelen

API&#39;s:

* [Client-referenties ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Toegangstoken ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

Stromen:

* [Dynamic Client Registration Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Veelgestelde vragen:

* [Veelgestelde vragen over de registratiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Stap 1: De toepassing registreren {#step-1-register-your-application}

* Haal cliëntgeloofsbrieven terug: De Dienst van de Programmer wint cliëntgeloofsbrieven door het [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) eindpunt te roepen.

   * De Programmerservice of Programmerer App moet de clientgegevens opslaan en deze onbeperkt gebruiken wanneer u een toegangstoken moet ophalen.


* Haal toegangstoken terug: De Dienst van de Programmer wint toegangstoken door het [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) eindpunt te roepen.

   * De Dienst van de Programmer of App van de Programmer moet het toegangstoken opslaan en gebruiken tot het verloopt, dan verwerpen het en verkrijgen nieuwe.

## B. Verificatiefase {#authentication-phase}

Het doel van de authentificatiefase is de het stromen toepassing de capaciteit te verstrekken om de identiteit van de gebruiker te verifiëren en informatie van gebruikersmeta-gegevens te verkrijgen.

De verificatiefase fungeert als een noodzakelijke stap voor de fase voorafgaand aan autorisatie of de machtigingsfase wanneer de streamingtoepassing inhoud moet afspelen.

++ + verwante artikelen

API&#39;s

* [Verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Verificatiesessie ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Verificatie uitvoeren in gebruikersagent](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Profielen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profiel ophalen voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profiel ophalen voor specifieke code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Stromen

* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Veelgestelde vragen

* [Veelgestelde vragen over de verificatiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Stap 2: Controleren op bestaande geverifieerde profielen {#step-2-check-for-existing-authenticated-profiles}

* **wint profielen terug:** de Controle van de Dienst van de Programmer namens Streaming App voor bestaande profielen door het [**/api/v2/ {serviceProvider} /profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) eindpunt te roepen.


* **Scenario 1:** Er zijn bestaande profielen, kan de Dienst van de Programmer aan de [ Voorbereidende Fase ](#preauthorization-phase) of [ Fase van de Toestemming ](#authorization-phase) te werk gaan.


* **Scenario 2:** Er zijn geen bestaande profielen, kan de Dienst van de Programmer aan de volgende stap te werk gaan [ de gebruiker ](#step-3-authenticate-the-user) voor authentiek verklaren.


* **Scenario 3:** Er zijn geen bestaande profielen, kan de Dienst van de Programmer te werk gaan om de gebruiker van tijdelijke toegang door de [ TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) eigenschap te voorzien.

   * Dit scenario is buiten het werkingsgebied van dit document, verwijs naar de [ Tijdelijke documentatie van de Stromen van de Toegang ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) voor meer informatie.

### Stap 3: De gebruiker verifiëren {#step-3-authenticate-the-user}

* **wint configuratie terug:** De Dienst van de Programmer wint de lijst van beschikbare MVPDs door het [**/api/v2/ {serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) eindpunt te roepen.

   * De dienst van de Programmer kan een douane filtrerend mechanisme uitvoeren om de lijst van MVPDs van de configuratiereactie te verfijnen, zodat de Streaming App slechts de voorgenomen leveranciers toont terwijl het verbergen van anderen (b.v., MVPDs in ontwikkeling, test MVPDs, TempPass). Op deze manier weet u zeker dat gebruikers een curatieve selectie krijgen wanneer ze hun tv-provider kiezen.


* **creeer authentificatiesessie:** De Dienst van de Programmer stelt een authentificatiesessie door het [**/api/v2/ {serviceProvider} /sessies**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) eindpunt te roepen.

   * De programmeerservice moet de `code` en `url` retourneren naar de streamingtoepassing.


* **Scenario 1:** Streaming App kan browser of webmening openen, daarom moet het de authentificatie `url` laden.

   * De gebruiker verzendt zijn gebruikersnaam en wachtwoord binnen de MVPD aanmeldingspagina. Bij geslaagde verificatie wordt een succespagina weergegeven met de uiteindelijke omleiding.


* **Scenario 2:** Streaming App kan geen browser openen, daarom moet het de authentificatie `code` tonen. Een afzonderlijke Webtoepassing wordt vereist om de gebruiker ertoe aan te zetten om `code` in te gaan, de authentificatie `url` te construeren en te openen: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * De gebruiker verzendt zijn gebruikersnaam en wachtwoord binnen de MVPD aanmeldingspagina. Bij geslaagde verificatie wordt een succespagina weergegeven met de uiteindelijke omleiding.

### Stap 4: Controleren op geverifieerde profielen {#step-4-check-for-authenticated-profiles}

* **wint profiel voor specifieke code terug:** De Dienst van de Programmer moet een opiniepeilingsmechanisme uitvoeren gebruikend `code` om te controleren of werd het profiel met succes geproduceerd en bewaard door [**/api/v2/ {serviceProvider} /profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) eindpunt te roepen.

   * De dienst van de Programmer moet **het opiniepeilings** mechanisme onder de volgende voorwaarden beginnen:

      * **Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd:** De Dienst van de Programmer zou moeten beginnen te pollen wanneer de gebruiker de definitieve bestemmingspagina bereikt, nadat de browser component URL laadt die voor de `redirectUrl` parameter in het [ wordt gespecificeerd Sessions ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) eindpuntverzoek.

      * **Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd:** de toepassing van de Dienst van de Programmer zou moeten beginnen opiniepeilend zodra de gebruiker het authentificatieproces-recht na het ontvangen van de [ 3} eindpuntreactie van Zittingen {en het tonen van de authentificatiecode aan de gebruiker in werking stelt.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)

   * De dienst van de Programmer moet **het opiniepeilings** mechanisme onder de volgende voorwaarden tegenhouden:

      * **Succesvolle authentificatie:** de het profielinformatie van de gebruiker wordt met succes teruggewonnen, bevestigend hun authentificatiestatus. Op dit moment is opiniepeiling niet langer nodig.

      * **de zitting van de Authentificatie en de codereduur:** de authentificatiesessie en de code verlopen, zoals die door `notAfter` wordt vermeld timestamp (b.v., 30 minuten) in de [ 4} eindpuntreactie van Zittingen {. ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) Als dit gebeurt, moet de gebruiker het authentificatieproces opnieuw beginnen, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

      * **Nieuwe geproduceerde authentificatiecode:** als de gebruiker om een nieuwe authentificatiecode op het primaire (scherm) apparaat verzoekt, is de bestaande zitting niet meer geldig, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

   * De dienst van de Programmer moet **de opiniepeilings** mechanismefrequentie onder de volgende voorwaarden vormen:

      * **Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd:** de Dienst van de Programmer zou om de 3-5 seconden of meer moeten opiniepeilen.

      * **Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd:** de Dienst van de Programmer zou om de 3-5 seconden of meer moeten opiniepeilen.

   * De programmeerservice moet onderdelen van de profielgegevens van de gebruiker in een permanente opslag in cache plaatsen om onnodige verzoeken te voorkomen en de gebruikerservaring te verbeteren.

## C. (Facultatief) Fase van voorafgaande toestemming {#preauthorization-phase}

Het doel van de fase voorafgaand aan de autorisatie is om de streamingtoepassing de mogelijkheid te bieden een subset van bronnen uit de catalogus te presenteren waartoe de gebruiker toegang zou hebben.

De fase voorafgaand aan autorisatie kan de gebruikerservaring verbeteren wanneer de gebruiker de streaming toepassing voor het eerst opent of naar een nieuwe sectie navigeert.

De fase voorafgaand aan autorisatie is niet verplicht. De streamingtoepassing kan deze fase overslaan als deze een catalogus met bronnen wil presenteren zonder deze eerst te filteren op basis van de machtiging van de gebruiker.

++ + verwante artikelen

API&#39;s

* [Toestemmingsbesluiten ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Stromen

* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

Veelgestelde vragen

* [Veelgestelde vragen over de preautorisatiefase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Stap 5: Controleren op vooraf geautoriseerde bronnen {#step-5-check-for-preauthorized-resources}

* **wint pre-vergunningsbesluiten terug:** De Dienst van de Programmer wint pre-vergunningsbesluiten voor een lijst van middelen door [**/api/v2/ {serviceProvider}/decisions/preAuthze/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) eindpunt te roepen.

   * De Programmeringsdienst moet de lijst van vergunning overgaan en pre-vergunningsbesluiten aan de Streaming App ontkennen.

   * De Programmer Service is niet verplicht om beslissingen vóór de goedkeuring op te slaan in permanente opslag. Nochtans, wordt het geadviseerd om vergunningsbesluiten in het voorgeheugen op te slaan om de gebruikerservaring te verbeteren. Dit helpt onnodige vraag naar middelen vermijden die reeds vooraf zijn geautoriseerd, die latentie verminderen en prestaties verbeteren.

   * De dienst van de Programmeur kan de reden voor een ontkend pre-vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van Besluiten pre-goedkeurt eindpunt. Deze details geven inzicht in de specifieke reden waarom de aanvraag voor voorafgaande toestemming is afgewezen, waardoor de gebruikerservaring kan worden geïnformeerd of de benodigde afhandeling in de toepassing kan worden geactiveerd. Ervoor zorgen dat elk nieuw mechanisme dat wordt toegepast voor het ophalen van beslissingen vóór toelating, niet in een eindeloze lus resulteert als het besluit vóór toelating wordt geweigerd. U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

   * De Programmeringsdienst kan een voorafgaande vergunningsbesluit voor een beperkt aantal middelen in één enkel API verzoek, gewoonlijk tot 5, wegens voorwaarden verkrijgen die door MVPDs worden opgelegd. Dit maximumaantal middelen kan worden bekeken en veranderd na het akkoord gaan met MVPDs door het Dashboard van Adobe Pass [ TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

## D. Vergunningsfase {#authorization-phase}

Het doel van de machtigingsfase is om de streamingtoepassing de mogelijkheid te bieden bronnen af te spelen die de gebruiker vraagt nadat hij zijn rechten met de MVPD heeft gevalideerd.

De autorisatiefase is verplicht. De streamingtoepassing kan deze fase niet overslaan als deze bronnen wil afspelen die de gebruiker vraagt, omdat de gebruiker eerst met de MVPD moet controleren of de gebruiker gerechtigd is voordat de stream wordt vrijgegeven.

++ + verwante artikelen

API&#39;s

* [Toestemmingsbesluiten ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Stromen

* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Veelgestelde vragen

* [Veelgestelde vragen over de machtigingsfase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Stap 6: Controleren op geautoriseerde bronnen {#step-6-check-for-authorized-resources}

* **wint vergunningsbesluit terug:** De Dienst van de Programmer wint vergunningsbesluit voor een specifiek middel terug door de Streaming App door [**/api/v2/ {serviceProvider} /Decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) eindpunt te roepen.

   * De Programmer Service is niet verplicht om beslissingen over machtigingen in permanente opslag op te slaan.

   * De dienst van de Programmer kan de reden voor een ontkend vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van Besluiten machtigt eindpunt. Deze gegevens verschaffen insight de specifieke reden waarom de aanvraag voor een vergunning is afgewezen, zodat de gebruiker op de hoogte kan worden gebracht van de ervaring of de benodigde afhandeling in de Streaming App kan worden geactiveerd. Ervoor zorgen dat een nieuw mechanisme dat wordt toegepast voor het ophalen van vergunningsbesluiten niet in een eindeloze lus resulteert als het vergunningsbesluit wordt geweigerd. U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

   * De programmeerdienst kan andere bedrijfsregels evalueren en een passend vergunningsbesluit aan de Streaming App teruggeven.

   * De dienst van de Programmer wordt niet vereist om een verlopen media teken te verfrissen terwijl de stroom actief speelt. Als het media-token tijdens het afspelen verloopt, moet het zijn toegestaan dat de stream zonder onderbreking wordt voortgezet. Nochtans, moet de cliënt om een nieuw vergunningsbesluit verzoeken — en een nieuw media teken verkrijgen — de volgende tijd de gebruiker probeert om een middel te spelen.

   * De programmeerservice kan een vergunningsbesluit voor een beperkt aantal bronnen verkrijgen in één API-aanvraag, meestal maximaal 1, als gevolg van voorwaarden die door de MVPD&#39;s worden opgelegd.

## E. Afmeldingsfase {#logout-phase}

Het doel van de afmeldingsfase is om de streamingtoepassing de mogelijkheid te bieden het geverifieerde profiel van de gebruiker binnen de Adobe Pass-verificatie op verzoek te beëindigen.

De afmeldingsfase is verplicht. De streamingtoepassing moet de gebruiker de mogelijkheid bieden zich af te melden.

++ + verwante artikelen

API&#39;s

* [Afmelden starten voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

Stromen

* [Basisuitlogingsstroom uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

Veelgestelde vragen

* [Veelgestelde vragen over de afmeldingsfase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Stap 7: Afmelden {#step-7-logout}

* Initieer Adobe Pass logout: De Dienst van de Programmer stelt de logout stroom zoals gevraagd door de Streaming App in werking door het [/api/v2/ {serviceProvider}/logout/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) eindpunt te roepen.

   * De Programmeringsservice kan alle informatie opruimen die deze over de geverifieerde gebruiker opslaat.

   * De Programmeringsservice moet de instructies volgen die in de kenmerken `actionName` en `actionType` van de eindpuntreactie bij de aanmelding zijn opgegeven, om ervoor te zorgen dat het aflogproces correct is voltooid.

      * Als het kenmerk `actionType` in het antwoord is ingesteld op &quot;interactief&quot;, moet de Programmer Service de waarde van het kenmerk `url` retourneren naar de Streaming App.

         * **Scenario 1:** Streaming App kan browser of webmening openen, daarom moet het logout `url` laden.

         * **Scenario 2:** Streaming App kan geen browser openen, daarom kan het logout proces worden tegengehouden aangezien de zitting van MVPD niet in browser van het Streaming Apparaat geheim voorgeheugen werd gehouden.
