---
title: Veelgestelde vragen over REST API V2
description: Veelgestelde vragen over REST API V2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# Veelgestelde vragen over REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document biedt een uitgebreid overzicht van de antwoorden op veelgestelde vragen over de Adobe Pass Authentication REST API V2-toepassing.

Voor meer informatie over REST API V2 algemeen, zie de [ REST API V2- Overzicht ](./rest-api-v2-overview.md) documentatie.

## Algemene veelgestelde vragen {#general-faqs}

Begin met deze sectie als u aan een toepassing werkt die REST API V2 moet integreren, of het een nieuwe toepassing of bestaande is die van [ REST API V1 ](#migrate-rest-api-v1-to-rest-api-v2) of [ SDK ](#migrate-sdk-to-rest-api-v2) migreert.

Zie ook de volgende secties voor meer informatie over migratiedetails en -stappen.

+++Veelgestelde vragen over de registratiefase

### 1. Wat is het doel van de registratiefase? {#registration-phase-faq1}

Het doel van de Fase van de Registratie is de cliënttoepassing tegen de Authentificatie van Adobe Pass door het [ Dynamische Registratie van de Cliënt (DCR) te registreren ](./rest-api-v2-glossary.md#dcr) proces.

Het dynamische proces van de Registratie van de Cliënt (DCR) vereist de cliënttoepassing om een paar cliëntgeloofsbrieven te verkrijgen en een toegangstoken als einddoel van de Fase van de Registratie terug te winnen.

Voor meer informatie, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md) documentatie.

### 2. Is de registratiefase verplicht? {#registration-phase-faq2}

De fase van de Registratie is verplicht, maar de cliënttoepassing kan deze fase overslaan als het een caching paar cliëntgeloofsbrieven en een toegangstoken heeft die nog geldig zijn.

### 3. Wat is een softwarestatement en hoe lang is het geldig? {#registration-phase-faq3}

De softwareverklaring is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#software-statement) documentatie wordt bepaald.

De softwareverklaring bestaat uit een Token van het Web JSON (JWT) die van het Dashboard van Adobe Pass [ TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass kan worden geproduceerd en worden gedownload handelend namens u.

De software-instructie is geldig voor een onbeperkt tijdsbestek, maar u kunt desgewenst een Adobe Pass-verificatievertegenwoordiger vragen deze te allen tijde in te trekken.

De clienttoepassing moet de softwareinstructie opslaan en gebruiken wanneer deze clientreferenties moet ophalen.

Voor meer details, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md) documentatie.

### 4. Hoe een softwareinstructie te genereren en te downloaden? {#registration-phase-faq4}

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [ Gids van de Gebruiker van de Kanalen van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) of [ de Gids van de Gebruiker van de Programmering van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) documentatie.

### 5. Wat gebeurt er als een softwareinstructie wordt ingetrokken? {#registration-phase-faq5}

Wanneer de softwareverklaring wordt ingetrokken, is er één belangrijk gevolg om te overwegen:

* De cliënttoepassingen die de ingetrokken softwareverklaring gebruiken zullen niet meer door de [ betitelingsstromen ](./rest-api-v2-glossary.md#entitlement) kunnen gaan, betekenend dat de gebruikers zullen worden geblokkeerd van het spelen van inhoud.

### 6. Wat zijn clientreferenties en hoe lang zijn deze geldig? {#registration-phase-faq6}

De cliëntgeloofsbrieven zijn een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#client-credentials) documentatie wordt bepaald.

De cliëntgeloofsbrieven bestaan uit een cliënt herkenningsteken en cliënt geheim paar dat van het eindpunt van het Register van de Cliënt kan worden teruggewonnen.

De clientgegevens zijn geldig voor een onbeperkt tijdsbestek.

De cliënttoepassing moet de cliëntgeloofsbrieven voor onbepaalde tijd opslaan en hen gebruiken wanneer het moeten een toegangstoken terugwinnen.

Voor meer informatie, verwijs naar [ terugwinnen cliëntgeloofsbrieven ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) documentatie.

### 7. Hoe te om cliëntgeloofsbrieven te beheren? {#registration-phase-faq7}

Wij adviseren de cliënttoepassing om een uniek paar cliëntgeloofsbrieven voor elke instantie van de gebruikerstoepassing in het geval van zowel cliënt-aan-server als server-aan-server integratie met de Authentificatie van Adobe Pass te beheren.

De cliënttoepassing moet de cliëntgeloofsbrieven voor onbepaalde tijd opslaan en hen gebruiken wanneer het moeten een toegangstoken terugwinnen.

### 8. Wat gebeurt er als clientgegevens in de cache verloren gaan? {#registration-phase-faq8}

Wanneer de caching cliëntgeloofsbrieven worden verloren, zijn er drie belangrijke te overwegen gevolgen:

* De cliënttoepassing moet een nieuw paar cliëntgeloofsbrieven verkrijgen.
* De cliënttoepassing moet een nieuw toegangstoken verkrijgen gebruikend het nieuwe paar cliëntgeloofsbrieven.
* De clienttoepassing moet de gebruiker vragen opnieuw te verifiëren, omdat de clienttoepassing de toegang tot de eerder verkregen geverifieerde profielen verliest.

### 9. Wat is een toegangstoken en hoe lang is het geldig? {#registration-phase-faq9}

Het toegangstoken is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#access-token) documentatie wordt bepaald.

Het toegangstoken bestaat uit het teken van de a [ drager ](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) dat van het Symbolische eindpunt van de Cliënt kan worden teruggewonnen.

Het toegangstoken is geldig voor een beperkt en kort tijdsbestek dat op het ogenblik van uitgifte wordt gespecificeerd.

De cliënttoepassing moet het toegangstoken opslaan en het gebruiken tot het verloopt wanneer het richten van REST API V2.

De cliënttoepassing moet een nieuw toegangstoken verkrijgen alvorens huidige verloopt om onbevoegde verzoeken te verhinderen.

Voor meer informatie, verwijs naar [ ontvang toegangstoken ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.

### 10. Hoe kan de cliënttoepassing een toegangstoken verfrissen? {#registration-phase-faq10}

De cliënttoepassing moet een toegangstoken de zelfde manier verfrissen zoals het terugwinnen van een nieuw toegangstoken, maar het gebruiken van caching cliëntgeloofsbrieven.

De cliënttoepassing moet niet opnieuw registreren om een toegangstoken te verfrissen, in plaats daarvan moet het de opgeslagen cliëntgeloofsbrieven gebruiken, anders zouden de gebruikers moeten opnieuw voor authentiek verklaren.

Voor meer informatie, verwijs naar [ ontvang toegangstoken ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.

+++

+++Veelgestelde vragen over de configuratiefase

### 1. Wat is het doel van de fase van de Configuratie? {#configuration-phase-faq1}

Het doel van de Fase van de Configuratie is de cliënttoepassing de lijst van MVPDs te verstrekken waarmee het samen met configuratiedetails actief die door de Authentificatie van Adobe Pass voor elke MVPD wordt bewaard.

De configuratiefase fungeert als een noodzakelijke stap voor de verificatiefase wanneer de clienttoepassing de gebruiker moet vragen zijn of haar tv-provider te selecteren.

### 2. Is de configuratiefase verplicht? {#configuration-phase-faq2}

De fase van de Configuratie is niet verplicht, kan de cliënttoepassing deze fase in de volgende scenario&#39;s overslaan:

* De gebruiker is al geverifieerd.
* De gebruiker krijgt tijdelijk toegang via de functie TempPass voor basis- of promotiedoeleinden.
* De gebruikersauthentificatie is verlopen, maar de cliënttoepassing heeft eerder geselecteerde MVPD als gebruikerservaring gemotiveerd keus in het voorgeheugen ondergebracht, en zet enkel de gebruiker ertoe aan om te bevestigen dat zij nog een abonnee van die MVPD zijn.

### 3. Wat is een configuratie en hoe lang is deze geldig? {#configuration-phase-faq3}

De configuratie is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#configuration) documentatie wordt bepaald.

De configuratie bestaat uit een lijst van MVPDs die van het eindpunt van de Configuratie kan worden teruggewonnen.

De cliënttoepassing kan de configuratie gebruiken om een component UI voor te stellen genoemd de &quot;Plukker&quot;wanneer het vereisen van de gebruiker om hun MVPD te selecteren.

De cliënttoepassing zou de configuratie moeten verfrissen alvorens de gebruiker door de Fase van de Authentificatie gaat.

Voor meer informatie, verwijs naar [ terugwinnen configuratie ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) documentatie.

### 4. Kan de clienttoepassing zijn eigen lijst met MVPD&#39;s beheren? {#configuration-phase-faq4}

De clienttoepassing kan zijn eigen lijst met MVPD&#39;s beheren, maar het wordt aangeraden de configuratie te gebruiken die door Adobe Pass Authentication wordt geleverd om ervoor te zorgen dat de lijst up-to-date en accuraat is.

De cliënttoepassing zou een [ fout ](/help/authentication/enhanced-error-codes.md) van de Authentificatie van Adobe Pass REST API V2 ontvangen als de gebruiker selecteerde MVPD geen actieve integratie met de gespecificeerde [ dienstverlener ](./rest-api-v2-glossary.md#service-provider) heeft.

### 5. Kan de clienttoepassing de lijst met MVPD&#39;s filteren? {#configuration-phase-faq5}

De cliënttoepassing kan de lijst van MVPDs filtreren die in de configuratiereactie wordt verstrekt door een douanemechanisme uit te voeren dat op eigen bedrijfslogica en vereisten zoals gebruikersplaats of gebruikersgeschiedenis van vorige selectie wordt gebaseerd.

### 6. Wat gebeurt er als de integratie met een MVPD is uitgeschakeld en als inactief is gemarkeerd? {#configuration-phase-faq6}

Wanneer de integratie met een MVPD gehandicapt is en als inactief wordt gemerkt, dan wordt MVPD verwijderd uit de lijst van MVPDs die in verdere configuratiereacties wordt verstrekt en er zijn twee belangrijke gevolgen om te overwegen:

* De niet voor authentiek verklaarde gebruikers van dat MVPD zullen niet meer de Fase van de Authentificatie kunnen voltooien gebruikend dat MVPD.
* De voor authentiek verklaarde gebruikers van dat MVPD zullen niet meer de Preauthentication, Authorization, of Logout Fases kunnen voltooien gebruikend dat MVPD.

De cliënttoepassing zou een [ fout ](/help/authentication/enhanced-error-codes.md) van de Authentificatie van Adobe Pass REST API V2 ontvangen als de gebruiker selecteerde MVPD niet meer een actieve integratie met de gespecificeerde [ dienstverlener ](./rest-api-v2-glossary.md#service-provider) heeft.

### 7. Wat gebeurt als de integratie met een MVPD terug en duidelijk als actief wordt toegelaten? {#configuration-phase-faq7}

Wanneer de integratie met een MVPD terug en duidelijk als actief wordt toegelaten, dan is MVPD inbegrepen terug in de lijst van MVPDs die in verdere configuratiereacties wordt verstrekt en er zijn twee belangrijke te overwegen gevolgen:

* De niet voor authentiek verklaarde gebruikers van dat MVPD zullen de Fase van de Authentificatie opnieuw kunnen voltooien gebruikend dat MVPD.
* De voor authentiek verklaarde gebruikers van dat MVPD zullen opnieuw de Voorkeur, de Vergunning, of Logout Fases kunnen voltooien gebruikend dat MVPD.

### 8. Hoe kan de integratie met een MVPD worden in- of uitgeschakeld? {#configuration-phase-faq8}

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

+++Veelgestelde vragen over de verificatiefase

### 1. Wat is het doel van de authentificatiefase? {#authentication-phase-faq1}

Het doel van de authentificatiefase is de cliënttoepassing de capaciteit te verstrekken om de identiteit van de gebruiker te verifiëren en informatie van gebruikersmeta-gegevens te verkrijgen.

De verificatiefase fungeert als een noodzakelijke stap voor de fase voorafgaand aan autorisatie of de machtigingsfase wanneer de clienttoepassing inhoud moet afspelen.

### 2. Hoe kan de clienttoepassing weten of de gebruiker al is geverifieerd? {#authentication-phase-faq2}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om te verifiëren of een gebruiker reeds voor authentiek wordt verklaard en de informatie van het terugkeerprofiel terugkeert:

* [API voor eindpunt van profielen](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. Hoe kan de clienttoepassing de metagegevens van de gebruiker ophalen? {#authentication-phase-faq3}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om [ informatie van gebruikersmeta-gegevens ](/help/authentication/user-metadata-feature.md) als deel van de profielinformatie terug te keren:

* [API voor eindpunt van profielen](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. Wat is een verificatiesessie en hoe lang is deze geldig? {#authentication-phase-faq4}

De authentificatiesessie is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#session) documentatie wordt bepaald.

De authentificatiesessie slaat informatie over het in werking gestelde authentificatieproces op dat van het eindpunt van Zittingen kan worden teruggewonnen.

De verificatiesessie is geldig gedurende een beperkte en korte periode die op het moment van uitgifte is opgegeven. Deze periode geeft aan hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat de gebruiker de flow opnieuw moet starten.

De cliënttoepassing kan de reactie van de authentificatiesessie gebruiken om te weten hoe te met het authentificatieproces te werk te gaan. Let op: er zijn gevallen waarin de gebruiker niet verplicht is om te verifiëren, zoals het verlenen van tijdelijke toegang, gedegradeerde toegang, of wanneer de gebruiker reeds voor authentiek wordt verklaard.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. Wat is een authentificatiecode en hoe lang is het geldig? {#authentication-phase-faq5}

De authentificatiecode is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#code) documentatie wordt bepaald.

De authentificatiecode slaat een unieke waarde op die wordt geproduceerd wanneer een gebruiker het authentificatieproces in werking stelt, en identificeert uniek de authentificatiesessie van een gebruiker tot het proces volledig is of tot de authentificatiesessie verloopt.

De verificatiecode is geldig gedurende een beperkt en kort tijdsbestek dat is opgegeven op het moment dat de verificatiesessie wordt gestart. Hiermee wordt aangegeven hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat de gebruiker de flow opnieuw moet starten.

De cliënttoepassing kan de authentificatiecode gebruiken om de gebruiker toe te staan om het authentificatieproces of op het zelfde apparaat of op tweede te voltooien of te hervatten, aangezien de authentificatiesessie niet verstreek.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. Hoe kan de cliënttoepassing weten als de gebruiker een geldige authentificatiecode typte en dat de authentificatiesessie nog niet verliep? {#authentication-phase-faq6}

De cliënttoepassing kan de authentificatiecode bevestigen die door de gebruiker in een secundaire (scherm) toepassing wordt getypt door een verzoek naar het eindpunt van Zittingen te verzenden verantwoordelijk om de informatie van de authentificatiesessie terug te winnen verbonden aan de authentificatiecode.

De cliënttoepassing zou een [ fout ](/help/authentication/enhanced-error-codes.md) ontvangen als de verstrekte authentificatiecode verkeerd zou worden getypt of in het geval dat de authentificatiesessie zou verlopen.

Voor meer informatie, verwijs naar [ terugwinnen authentificatiesessie ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) documentatie.

### 7. Wat is een profiel en hoe lang is het geldig? {#authentication-phase-faq7}

Het profiel is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#profile) documentatie wordt bepaald.

Het profiel slaat informatie over de de authentificatierealiteit van de gebruiker, meta-gegevensinformatie en veel meer op die van het eindpunt van Profielen kunnen worden teruggewonnen.

De clienttoepassing kan het profiel gebruiken om de verificatiestatus van de gebruiker te kennen, toegang te krijgen tot metagegevens van de gebruiker of de methode te zoeken die wordt gebruikt voor verificatie.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor eindpunt van profielen](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Het profiel is geldig gedurende een beperkt tijdsbestek dat is opgegeven wanneer hierom wordt gevraagd. Dit geeft aan hoe lang de gebruiker een geldige verificatie heeft voordat de verificatiefase opnieuw moet worden doorlopen.

Dit beperkte die tijdkader als authentificatie (authN) [ wordt bekend TTL ](./rest-api-v2-glossary.md#ttl) kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

+++Veelgestelde vragen over voorafgaande goedkeuring

### 1. Wat is het doel van de fase van voorafgaande toestemming? {#preauthorization-phase-faq1}

Het doel van de fase van voorafgaande toestemming is de clienttoepassing de mogelijkheid te bieden een subset van bronnen uit de catalogus te presenteren waartoe de gebruiker toegang zou hebben.

De fase voorafgaand aan autorisatie kan de gebruikerservaring verbeteren wanneer de gebruiker de cliënttoepassing voor het eerst opent of aan een nieuwe sectie navigeert.

### 2. Is de voorafgaande toelatingsfase verplicht? {#preauthorization-phase-faq2}

De fase voorafgaand aan autorisatie is niet verplicht. De clienttoepassing kan deze fase overslaan als deze een catalogus met bronnen wil presenteren zonder deze eerst te filteren op basis van de machtiging van de gebruiker.

### 3. Wat is een voorafgaande beslissing? {#preauthorization-phase-faq3}

Voortoestemming is een termijn die in de [ 1} documentatie van de Verklarende woordenlijst {wordt bepaald, terwijl de besluitvormingstermijn ook in de [ Verklarende woordenlijst ](./rest-api-v2-glossary.md#decision) kan worden gevonden.](./rest-api-v2-glossary.md#preauthorization)

In het voorafgaande besluit wordt informatie over het onderzoeksresultaat van het MVPD-proces vóór de goedkeuring opgeslagen die kan worden opgehaald uit het eindpunt van het besluit vooraf machtigen.

De clienttoepassing kan de voorafgaande autorisatiebeslissingen gebruiken om een subset van bronnen weer te geven waartoe de gebruiker toegang heeft (informatieve) beslissingen van de tv-provider.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor beslissingen vóór autorisatie ophalen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. Waarom ontbreekt het aan een media-token in het besluit tot voorafgaande toestemming? {#preauthorization-phase-faq4}

In het besluit tot voorafgaande toestemming ontbreekt een media-token omdat de fase van voorafgaande toestemming niet mag worden gebruikt om bronnen af te spelen, aangezien dat het doel is van de fase van autorisatie.

### 5. Wat is een bron en welke indelingen worden ondersteund? {#preauthorization-phase-faq5}

Het middel is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar [ het Identificeren van Beschermde Middelen ](/help/authentication/identify-protected-resources.md) documentatie.

### 6. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een besluit tot voorafgaande toestemming verkrijgen? {#preauthorization-phase-faq6}

De clienttoepassing kan een voorafgaande autorisatiebeslissing voor een beperkt aantal bronnen verkrijgen in één API-aanvraag, meestal maximaal 5, als gevolg van voorwaarden die door de MVPD&#39;s worden opgelegd.

Dit maximumaantal middelen kan worden bekeken en veranderd na het akkoord gaan met MVPDs door het Dashboard van Adobe Pass [ TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

+++Veelgestelde vragen over de machtigingsfase

### 1. Wat is het doel van de machtigingsfase? {#authorization-phase-faq1}

Het doel van de vergunningsfase is de cliënttoepassing het vermogen te verstrekken om middelen te spelen de gebruikersverzoeken na het bevestigen van hun rechten met MVPD.

### 2. Is de machtigingsfase verplicht? {#authorization-phase-faq2}

De vergunningsfase is verplicht, kan de cliënttoepassing niet deze fase overslaan als het middelen de gebruikersverzoeken wil spelen, aangezien het vereist om met MVPD te verifiëren dat de gebruiker alvorens de stroom vrij te geven gerechtigd is.

### 3. Wat is een vergunningsbesluit en hoe lang is het geldig? {#authorization-phase-faq3}

De vergunning is een termijn die in de [ 1} documentatie van de Verklarende woordenlijst {wordt bepaald, terwijl de besluitvormingstermijn ook in de [ Verklarende woordenlijst ](./rest-api-v2-glossary.md#decision) kan worden gevonden.](./rest-api-v2-glossary.md#authorization)

Het vergunningsbesluit slaat informatie over het MVPD onderzoeksresultaat van het vergunningsproces op dat van de Besluiten kan worden teruggewonnen goedkeurt eindpunt.

De clienttoepassing kan het machtigingsbesluit met een media-token gebruiken om de bronstream af te spelen voor het geval de gebruiker toegang zou krijgen tot de stream als het besluit van de tv-provider (gezaghebbend) dat besluit zou bevatten.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor goedkeuringsbeslissingen ophalen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Het vergunningsbesluit is geldig voor een beperkte en korte tijd die op het ogenblik van afgifte wordt gespecificeerd, die op de hoeveelheid tijd wijst het door de Authentificatie van Adobe Pass zal worden in het voorgeheugen ondergebracht alvorens te vereisen om MVPD opnieuw te vragen.

Dit beperkte die tijdkader als vergunning (authZ) [ wordt bekend TTL ](./rest-api-v2-glossary.md#ttl) kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

### 4. Wat is een media-token en hoe lang is het geldig? {#authorization-phase-faq4}

Het media teken is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#media-token) documentatie wordt bepaald.

Het media token bestaat uit een ondertekende tekenreeks die in duidelijke tekst wordt verzonden en die kan worden opgehaald uit het eindpunt Autoriseren van beslissingen.

Voor meer informatie, verwijs naar [ Integrerend de Symbolische documentatie van de Verificateur van Media ](/help/authentication/media-token-verifier-int.md).

Het media teken is geldig voor een beperkte en korte tijd die op het ogenblik van kwestie wordt gespecificeerd, die op de hoeveelheid tijd wijst het door de cliënttoepassing moet worden gebruikt alvorens te vereisen om Besluiten te vragen geef opnieuw eindpunt toe.

De clienttoepassing kan het media-token gebruiken om een bronstream af te spelen voor het geval dat de gebruiker toegang zou krijgen tot de stream als de beslissing van de tv-provider (gezaghebbend).

Raadpleeg de volgende documenten voor meer informatie:

* [API voor goedkeuringsbeslissingen ophalen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. Wat is een bron en welke indelingen worden ondersteund? {#authorization-phase-faq5}

Het middel is een termijn die in de [ verklarende woordenlijst ](./rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar [ het Identificeren van Beschermde Middelen ](/help/authentication/identify-protected-resources.md) documentatie.

### 6. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een vergunningsbesluit verkrijgen? {#authorization-phase-faq6}

De cliënttoepassing kan een vergunningsbesluit voor een beperkt aantal middelen in één enkel API verzoek, gewoonlijk tot 1, wegens voorwaarden verkrijgen die door MVPDs worden opgelegd.

+++

+++Veelgestelde vragen over de afmeldingsfase

### 1. Wat is het doel van de afmeldingsfase? {#logout-phase-faq1}

Het doel van de afmeldingsfase is om de clienttoepassing de mogelijkheid te bieden het geverifieerde profiel van de gebruiker binnen de Adobe Pass-verificatie op verzoek te beëindigen.

+++

+++Veelgestelde vragen over kopteksten

### 1. Hoe kan de waarde voor de machtigingsheader worden berekend? {#headers-faq1}

De ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) verzoekkopbal van de Vergunning 0} {bevat het `Bearer` toegangstoken dat door de cliënttoepassing wordt vereist om tot Adobe Pass beschermde APIs toegang te hebben.[

De headerwaarde voor autorisatie moet worden verkregen bij Adobe Pass-verificatie tijdens de registratiefase.

Raadpleeg de volgende documenten voor meer informatie:

* [Overzicht van dynamische clientregistratie](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Client credentials-API ophalen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [API voor toegangstoken ophalen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamic Client Registration Flow](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om het `Bearer` toegangstoken te verkrijgen als voorheen.

### 2. Hoe kan ik de waarde voor de kop van de AP-Device-Identifier berekenen? {#headers-faq2}

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) verzoekkopbal bevat het stromen apparatenherkenningsteken aangezien het door de cliënttoepassing werd gecreeerd.

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) kopbaldocumentatie verstrekt sommige voorbeelden van hoe te om de waarde voor verschillende platforms gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de apparaat-id te berekenen als voorheen.

### 3. Hoe kan ik de waarde voor de X-Device-Info header berekenen? {#headers-faq3}

De [ x-apparaat-Info ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) verzoekkopbal bevat de cliëntinformatie (apparaat, verbinding en toepassing) met betrekking tot het daadwerkelijke het stromen apparaat.

De [ x-apparaat-Info ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) kopbaldocumentatie verstrekt sommige voorbeelden van hoe te om de waarde voor verschillende platforms gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de apparaatinformatie te berekenen zoals voorheen.

+++

## Veelgestelde vragen over migratie {#migration-faqs}

Ga verder met deze sectie als u werkt aan een toepassing die een bestaande toepassing moet migreren naar REST API V2.

++ Algemene veelgestelde vragen over migratie

### 1. Moet ik een nieuwe clienttoepassing implementeren die naar REST API V2 is gemigreerd voor alle gebruikers tegelijk? {#migration-faq1}

Nee.

De clienttoepassing is niet verplicht een nieuwe versie uit te voeren waarin de REST API V2 wordt geïntegreerd voor alle gebruikers tegelijk.

De Authentificatie van Adobe Pass zal oudere versies van de cliënttoepassing blijven die REST API V1 of SDK integreren tot eind 2025.

### 2. Moet ik een nieuwe clienttoepassing implementeren die is gemigreerd naar REST API V2 voor alle API&#39;s en die tegelijkertijd stromen? {#migration-faq2}

Ja.

De clienttoepassing is vereist om een nieuwe versie uit te voeren die de REST API V2 integreert voor alle Adobe Pass Authentication-API&#39;s en die tegelijkertijd stroomt.

In het geval van de &quot;tweede het schermauthentificatie&quot;stroom, wordt de cliënttoepassing vereist om een nieuwe versie uit te voeren die REST API V2 voor zowel de [ primaire ](./rest-api-v2-glossary.md#primary-application) als [ secundaire ](./rest-api-v2-glossary.md#secondary-application) toepassingen gelijktijdig integreert.

Adobe Pass Authentication biedt geen ondersteuning voor &#39;hybride&#39; implementaties die zowel REST API V2 als REST API V1/SDK tussen API&#39;s en stromen integreren.

### 3. Zal de gebruikersverificatie behouden blijven bij het bijwerken naar een nieuwe clienttoepassing die is gemigreerd naar REST API V2? {#migration-faq3}

Nee.

De gebruikersverificatie die is verkregen in oudere clienttoepassingsversies waarin de REST API V1 of SDK is geïntegreerd, blijft niet behouden.

Daarom moet de gebruiker opnieuw worden geverifieerd in de nieuwe clienttoepassing die is gemigreerd naar REST API V2.

### 4. Kan de clienttoepassing de bestaande geregistreerde toepassingen (softwareinstructies) gebruiken? {#migration-faq4}

De clienttoepassing kan de bestaande geregistreerde toepassingen (softwareinstructies) niet opnieuw gebruiken. Daarom moet de toepassing een nieuwe geregistreerde toepassing (softwareinstructies) genereren en downloaden die speciaal is bedoeld voor het gebruik van REST API V2.

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](./rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [ Gids van de Gebruiker van de Kanalen van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) of [ de Gids van de Gebruiker van de Programmering van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) documentatie.

Voor het ogenblik zult u een vertegenwoordiger van de Authentificatie van Adobe Pass moeten vragen om het gebruik van REST API V2 voor uw nieuwe geregistreerde toepassingen (softwareverklaringen) toe te laten, tot het Dashboard van Adobe Pass [ TVE ](./rest-api-v2-glossary.md#tve-dashboard) zal worden bijgewerkt om het zelf-beheer van deze verrichting toe te staan.

Als u onderscheid wilt maken tussen de geregistreerde toepassingen (softwareinstructies) die worden gebruikt in clienttoepassingen die REST API V2 gebruiken, moet u een specifiek achtervoegsel toevoegen aan de geregistreerde toepassingsnaam, zoals &quot;RESTV2&quot;.

### 5. Kan de clienttoepassing de bestaande aangepaste schema&#39;s gebruiken? {#migration-faq5}

De cliënttoepassing kan de bestaande douaneregelingen opnieuw gebruiken die door het Dashboard van Adobe Pass [ worden geproduceerd TVE ](./rest-api-v2-glossary.md#tve-dashboard).

Voor meer details, verwijs naar de [ Gids van de Gebruiker van de Kanalen van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) of [ de Gids van de Gebruiker van de Programmering van het Dashboard van TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) documentatie.

### 6. Zijn de verbeterde foutcodes standaard ingeschakeld in REST API V2? {#migration-faq6}

Ja.

De clienttoepassingen die REST API V2 integreren, profiteren van de verbeterde functie voor foutcodes die standaard is ingeschakeld.

Voor meer details, verwijs naar de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migratie van REST API V1 naar REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Ga verder met deze subsectie als u werkt aan een toepassing die van REST API V1 naar REST API V2 moet migreren.

+++Veelgestelde vragen over de registratiefase

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de registratiefase? {#registration-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er geen wijzigingen op hoog niveau met betrekking tot de registratiefase.

De cliënttoepassing kan de zelfde stroom blijven gebruiken om tegen de Authentificatie van Adobe Pass door het [ Dynamische proces van de Registratie van de Cliënt (DCR) te registreren ](./rest-api-v2-glossary.md#dcr) en een toegangstoken te verkrijgen.

Raadpleeg de volgende documenten voor meer informatie:

* [Overzicht van dynamische clientregistratie](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Client credentials-API ophalen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [API voor toegangstoken ophalen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamic Client Registration Flow](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++Veelgestelde vragen over de configuratiefase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de configuratiefase? {#configuration-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ GET <br/> /api/v1/config/{serviceProvider} ](/help/authentication/provide-mvpd-list.md) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Veelgestelde vragen over de verificatiefase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de verificatiefase? {#authentication-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ POST <br/> /reggie/v1/{serviceProvider}/regcode ](/help/authentication/registration-code-request.md) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| MVPD-verificatie hervatten op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiate (MVPD) authentificatie | [ GET <br/> /api/v1/authenticate ](/help/authentication/initiate-authentication.md) | [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ GET <br/> /api/v1/checkauthn (eerste scherm) ](/help/authentication/check-authentication-token.md) <br/> [ GET <br/> /api/v1/checkauthn (tweede scherm) ](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Verificatietoken van gebruiker ophalen (profiel) | [ GET <br/> /api/v1/tokens/authn ](/help/authentication/retrieve-authentication-token.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ GET <br/> /api/v1/tokens/usermetadata ](/help/authentication/user-metadata.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over voorafgaande goedkeuring

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de fase voorafgaand aan de autorisatie? {#preauthorization-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ GET <br/> /api/v1/preauthorize (eerste scherm) ](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [ GET <br/> /api/v1/preauthorize (tweede scherm) ](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over de machtigingsfase

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de machtigingsfase? {#authorization-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MVPD-autorisatie starten | [ GET <br/> /api/v1/authorize ](/help/authentication/initiate-authorization.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Token van autorisatie ophalen (machtigingsbesluit) | [ GET <br/> /api/v1/tokens/authz ](/help/authentication/retrieve-authorization-token.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Token voor korte autorisatie ophalen (mediatoken) | [ GET <br/> /api/v1/tokens/media ](/help/authentication/obtain-short-media-token.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over de afmeldingsfase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de afmeldingsfase? {#logout-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ GET <br/> /api/v1/logout ](/help/authentication/initiate-logout.md) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migreren van SDK naar REST API V2 {#migrate-sdk-to-rest-api-v2}

Ga verder met deze subsectie als u werkt aan een toepassing die van SDK naar REST API V2 moet migreren.

+++Veelgestelde vragen over de registratiefase

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de registratiefase? {#registration-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over de configuratiefase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de configuratiefase? {#configuration-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/android-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Veelgestelde vragen over de verificatiefase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de verificatiefase? {#authentication-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiate (MVPD) authentificatie | [ AccessEnabler.getAuthentication ](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/javascript-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiate (MVPD) authentificatie | [ AccessEnabler.getAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ AccessEnabler.getAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| MVPD-verificatie hervatten op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiate (MVPD) authentificatie | [ AccessEnabler.getAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiate (MVPD) authentificatie | [ AccessEnabler.getAuthentication ](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/android-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/android-sdk-api-reference.md#getMetadata) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ AccessEnabler.getAuthentication ](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| MVPD-verificatie hervatten op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiate (MVPD) authentificatie | [ AccessEnabler.getAuthentication ](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profielen ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider} /profiles/code/{code} ](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over voorafgaande goedkeuring

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de fase voorafgaand aan de autorisatie? {#preauthorization-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [ AccessEnabler.preauthorize ](/help/authentication/preauthorize-api-javascript-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [ AccessEnabler.preauthorize ](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [ AccessEnabler.preauthorize ](/help/authentication/preauthorize-api-android-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over de machtigingsfase

#### 1. Welke API-migraties op hoog niveau zijn vereist voor de machtigingsfase? {#authorization-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>MVPD-autorisatie starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Veelgestelde vragen over de afmeldingsfase

#### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de afmeldingsfase? {#logout-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

##### AccessEnabler JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/javascript-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/iostvos-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/android-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
