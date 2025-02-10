---
title: Veelgestelde vragen over REST API V2
description: Veelgestelde vragen over REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '6460'
ht-degree: 0%

---

# Veelgestelde vragen over REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document biedt een uitgebreid overzicht van de antwoorden op veelgestelde vragen over de Adobe Pass Authentication REST API V2-toepassing.

Voor meer informatie over REST API V2 algemeen, zie de [ REST API V2- Overzicht ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) documentatie.

## Algemene veelgestelde vragen {#general-faqs}

Begin met deze sectie als u aan een toepassing werkt die REST API V2 moet integreren, of het een nieuwe toepassing of bestaande is die van [ REST API V1 ](#migration-rest-api-v1-to-rest-api-v2) of [ SDK ](#migration-sdk-to-rest-api-v2) migreert.

Zie ook de volgende secties voor meer informatie over migratiedetails en -stappen.

>[!MORELIKETHIS]
>
> * [ Dynamische Veelgestelde vragen van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#general-faqs)

### Veelgestelde vragen over de registratiefase {#registration-phase-faqs-general}

+++Veelgestelde vragen over de registratiefase

Verwijs naar de [ Dynamische Veelgestelde documentatie van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Veelgestelde vragen over de configuratiefase {#configuration-phase-faqs-general}

+++Veelgestelde vragen over de configuratiefase

#### 1. Wat is het doel van de fase van de Configuratie? {#configuration-phase-faq1}

Het doel van de Fase van de Configuratie is de cliënttoepassing de lijst van MVPDs te verstrekken waarmee het samen met configuratiedetails actief wordt geïntegreerd die door de Authentificatie van Adobe Pass voor elke MVPD wordt bewaard.

De configuratiefase fungeert als een noodzakelijke stap voor de verificatiefase wanneer de clienttoepassing de gebruiker moet vragen zijn of haar tv-provider te selecteren.

#### 2. Is de configuratiefase verplicht? {#configuration-phase-faq2}

De fase van de Configuratie is niet verplicht, kan de cliënttoepassing deze fase in de volgende scenario&#39;s overslaan:

* De gebruiker is al geverifieerd.
* De gebruiker krijgt tijdelijk toegang via de functie TempPass voor basis- of promotiedoeleinden.
* De gebruikersverificatie is verlopen, maar de clienttoepassing heeft de eerder geselecteerde MVPD in de cache geplaatst als een gebruikerservaring die de keuze motiveert. De gebruiker wordt alleen gevraagd te bevestigen dat hij of zij nog steeds een abonnee van die MVPD is.

#### 3. Wat is een configuratie en hoe lang is deze geldig? {#configuration-phase-faq3}

De configuratie is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) documentatie wordt bepaald.

De configuratie bestaat uit een lijst van MVPDs die van het eindpunt van de Configuratie kan worden teruggewonnen.

De clienttoepassing kan de configuratie gebruiken om een UI-component met de naam &quot;Picker&quot; weer te geven wanneer de gebruiker zijn MVPD moet selecteren.

De cliënttoepassing zou de configuratie moeten verfrissen alvorens de gebruiker door de Fase van de Authentificatie gaat.

Voor meer informatie, verwijs naar [ terugwinnen configuratie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) documentatie.

#### 4. Kan de clienttoepassing zijn eigen lijst met MVPD&#39;s beheren? {#configuration-phase-faq4}

De clienttoepassing kan zijn eigen lijst met MVPD&#39;s beheren, maar het wordt aangeraden de configuratie te gebruiken die door Adobe Pass Authentication wordt geleverd om ervoor te zorgen dat de lijst up-to-date en accuraat is.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) van de Authentificatie van Adobe Pass REST API V2 ontvangen als de gebruiker selecteerde MVPD geen actieve integratie met de gespecificeerde [ dienstverlener ](rest-api-v2-glossary.md#service-provider) heeft.

#### 5. Kan de clienttoepassing de lijst met MVPD&#39;s filteren? {#configuration-phase-faq5}

De cliënttoepassing kan de lijst van MVPDs filtreren die in de configuratiereactie wordt verstrekt door een douanemechanisme uit te voeren dat op eigen bedrijfslogica en vereisten zoals gebruikersplaats of gebruikersgeschiedenis van vorige selectie wordt gebaseerd.

#### 6. Wat gebeurt er als de integratie met een MVPD is uitgeschakeld en als inactief is gemarkeerd? {#configuration-phase-faq6}

Wanneer de integratie met een MVPD is uitgeschakeld en als inactief is gemarkeerd, wordt de MVPD verwijderd uit de lijst met MVPD&#39;s die in verdere configuratieantwoorden is opgenomen en moet rekening worden gehouden met twee belangrijke gevolgen:

* De niet-geverifieerde gebruikers van die MVPD kunnen de verificatiefase niet meer voltooien met die MVPD.
* De geverifieerde gebruikers van die MVPD kunnen de fase van voorafgaande toestemming, autorisatie of afmelding met die MVPD niet meer voltooien.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) van de Authentificatie van Adobe Pass REST API V2 ontvangen als de gebruiker selecteerde MVPD geen actieve integratie met de gespecificeerde [ dienstverlener ](rest-api-v2-glossary.md#service-provider) heeft.

#### 7. Wat gebeurt er als de integratie met een MVPD weer ingeschakeld en gemarkeerd wordt als actief? {#configuration-phase-faq7}

Wanneer de integratie met een MVPD terug en duidelijk als actief wordt toegelaten, dan is MVPD inbegrepen terug in de lijst van MVPDs die in verdere configuratiereacties wordt verstrekt en er zijn twee belangrijke gevolgen om te overwegen:

* De niet-geverifieerde gebruikers van die MVPD kunnen de verificatiefase opnieuw voltooien met behulp van die MVPD.
* De geautoriseerde gebruikers van die MVPD kunnen de fase van voorafgaande toestemming, autorisatie of afmelding met die MVPD opnieuw voltooien.

#### 8. Hoe kan de integratie met een MVPD worden in- of uitgeschakeld? {#configuration-phase-faq8}

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

### Veelgestelde vragen over de verificatiefase {#authentication-phase-faqs-general}

+++Veelgestelde vragen over de verificatiefase

#### 1. Wat is het doel van de authentificatiefase? {#authentication-phase-faq1}

Het doel van de authentificatiefase is de cliënttoepassing de capaciteit te verstrekken om de identiteit van de gebruiker te verifiëren en informatie van gebruikersmeta-gegevens te verkrijgen.

De verificatiefase fungeert als een noodzakelijke stap voor de fase voorafgaand aan autorisatie of de machtigingsfase wanneer de clienttoepassing inhoud moet afspelen.

#### 2. Hoe kan de clienttoepassing weten of de gebruiker al is geverifieerd? {#authentication-phase-faq2}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om te verifiëren of een gebruiker reeds voor authentiek wordt verklaard en de informatie van het terugkeerprofiel terugkeert:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. Hoe kan de clienttoepassing de metagegevens van de gebruiker ophalen? {#authentication-phase-faq3}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om [ informatie van gebruikersmeta-gegevens ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) als deel van de profielinformatie terug te keren:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. Wat is een verificatiesessie en hoe lang is deze geldig? {#authentication-phase-faq4}

De authentificatiesessie is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) documentatie wordt bepaald.

De authentificatiesessie slaat informatie over het in werking gestelde authentificatieproces op dat van het eindpunt van Zittingen kan worden teruggewonnen.

De verificatiesessie is geldig gedurende een beperkte en korte periode die op het moment van uitgifte is opgegeven. Deze periode geeft aan hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat de gebruiker de flow opnieuw moet starten.

De cliënttoepassing kan de reactie van de authentificatiesessie gebruiken om te weten hoe te met het authentificatieproces te werk te gaan. Let op: er zijn gevallen waarin de gebruiker niet verplicht is om te verifiëren, zoals het verlenen van tijdelijke toegang, gedegradeerde toegang, of wanneer de gebruiker reeds voor authentiek wordt verklaard.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. Wat is een authentificatiecode en hoe lang is het geldig? {#authentication-phase-faq5}

De authentificatiecode is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) documentatie wordt bepaald.

De authentificatiecode slaat een unieke waarde op die wordt geproduceerd wanneer een gebruiker het authentificatieproces in werking stelt, en identificeert uniek de authentificatiesessie van een gebruiker tot het proces volledig is of tot de authentificatiesessie verloopt.

De verificatiecode is geldig gedurende een beperkt en kort tijdsbestek dat is opgegeven op het moment dat de verificatiesessie wordt gestart. Hiermee wordt aangegeven hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat de gebruiker de flow opnieuw moet starten.

De cliënttoepassing kan de authentificatiecode gebruiken om de gebruiker toe te staan om het authentificatieproces of op het zelfde apparaat of op tweede te voltooien of te hervatten, aangezien de authentificatiesessie niet verstreek.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. Hoe kan de cliënttoepassing weten als de gebruiker een geldige authentificatiecode typte en dat de authentificatiesessie nog niet verliep? {#authentication-phase-faq6}

De cliënttoepassing kan de authentificatiecode bevestigen die door de gebruiker in een secundaire (scherm) toepassing wordt getypt door een verzoek naar het eindpunt van Zittingen te verzenden verantwoordelijk om de informatie van de authentificatiesessie terug te winnen verbonden aan de authentificatiecode.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ontvangen als de verstrekte authentificatiecode verkeerd zou worden getypt of in het geval dat de authentificatiesessie zou verlopen.

Voor meer informatie, verwijs naar [ terugwinnen authentificatiesessie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) documentatie.

#### 7. Wat is een profiel en hoe lang is het geldig? {#authentication-phase-faq7}

Het profiel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) documentatie wordt bepaald.

Het profiel slaat informatie over de de authentificatierealiteit van de gebruiker, meta-gegevensinformatie en veel meer op die van het eindpunt van Profielen kunnen worden teruggewonnen.

De clienttoepassing kan het profiel gebruiken om de verificatiestatus van de gebruiker te kennen, toegang te krijgen tot metagegevens van de gebruiker of de methode te zoeken die wordt gebruikt voor verificatie.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Het profiel is geldig gedurende een beperkt tijdsbestek dat is opgegeven wanneer hierom wordt gevraagd. Dit geeft aan hoe lang de gebruiker een geldige verificatie heeft voordat de verificatiefase opnieuw moet worden doorlopen.

Dit beperkte die tijdkader als authentificatie (authN) [ wordt bekend TTL ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

### Veelgestelde vragen over de preautorisatiefase {#preauthorization-phase-faqs-general}

+++Veelgestelde vragen over voorafgaande goedkeuring

#### 1. Wat is het doel van de fase van voorafgaande toestemming? {#preauthorization-phase-faq1}

Het doel van de fase van voorafgaande toestemming is de clienttoepassing de mogelijkheid te bieden een subset van bronnen uit de catalogus te presenteren waartoe de gebruiker toegang zou hebben.

De fase voorafgaand aan autorisatie kan de gebruikerservaring verbeteren wanneer de gebruiker de cliënttoepassing voor het eerst opent of aan een nieuwe sectie navigeert.

#### 2. Is de voorafgaande toelatingsfase verplicht? {#preauthorization-phase-faq2}

De fase voorafgaand aan autorisatie is niet verplicht. De clienttoepassing kan deze fase overslaan als deze een catalogus met bronnen wil presenteren zonder deze eerst te filteren op basis van de machtiging van de gebruiker.

#### 3. Wat is een voorafgaande beslissing? {#preauthorization-phase-faq3}

Voortoestemming is een termijn die in de [ 1} documentatie van de Verklarende woordenlijst {wordt bepaald, terwijl de besluitvormingstermijn ook in de [ Verklarende woordenlijst ](rest-api-v2-glossary.md#decision) kan worden gevonden.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization)

In het voorafgaande goedkeuringsbesluit wordt informatie over het onderzoeksresultaat van het MVPD-proces voorafgaand aan de autorisatie opgeslagen die kan worden opgehaald uit het eindpunt van de Besluiten vooraf autoriseren.

De clienttoepassing kan de voorafgaande autorisatiebeslissingen gebruiken om een subset van bronnen weer te geven waartoe de gebruiker toegang heeft (informatieve) beslissingen van de tv-provider.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor beslissingen vóór autorisatie ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. Waarom ontbreekt het aan een media-token in het besluit tot voorafgaande toestemming? {#preauthorization-phase-faq4}

In het besluit tot voorafgaande toestemming ontbreekt een media-token omdat de fase van voorafgaande toestemming niet mag worden gebruikt om bronnen af te spelen, aangezien dat het doel is van de fase van autorisatie.

#### 5. Wat is een bron en welke indelingen worden ondersteund? {#preauthorization-phase-faq5}

Het middel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar de [ Beschermde documentatie van Middelen ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een besluit tot voorafgaande toestemming verkrijgen? {#preauthorization-phase-faq6}

De clienttoepassing kan een voorafgaande autorisatiebeslissing voor een beperkt aantal bronnen verkrijgen in één API-aanvraag, meestal maximaal 5, als gevolg van voorwaarden die door de MVPD&#39;s worden opgelegd.

Dit maximumaantal middelen kan worden bekeken en veranderd na het akkoord gaan met MVPDs door het Dashboard van Adobe Pass [ TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

### Veelgestelde vragen over de machtigingsfase {#authorization-phase-faqs-general}

+++Veelgestelde vragen over de machtigingsfase

#### 1. Wat is het doel van de machtigingsfase? {#authorization-phase-faq1}

Het doel van de machtigingsfase is om de clienttoepassing de mogelijkheid te bieden om bronnen af te spelen die de gebruiker vraagt nadat hij zijn rechten met de MVPD heeft gevalideerd.

#### 2. Is de machtigingsfase verplicht? {#authorization-phase-faq2}

De autorisatiefase is verplicht, de clienttoepassing kan deze fase niet overslaan als deze bronnen wil afspelen die de gebruiker vraagt, omdat deze eerst met de MVPD moet controleren of de gebruiker gerechtigd is voordat de stream wordt vrijgegeven.

#### 3. Wat is een vergunningsbesluit en hoe lang is het geldig? {#authorization-phase-faq3}

De vergunning is een termijn die in de [ 1} documentatie van de Verklarende woordenlijst {wordt bepaald, terwijl de besluitvormingstermijn ook in de [ Verklarende woordenlijst ](rest-api-v2-glossary.md#decision) kan worden gevonden.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization)

In het vergunningsbesluit wordt informatie over het onderzoeksresultaat van het MVPD-autorisatieproces opgeslagen die kan worden opgehaald uit het eindpunt van de Besluiten Autoriseren.

De clienttoepassing kan het machtigingsbesluit met een media-token gebruiken om de bronstream af te spelen voor het geval de gebruiker toegang zou krijgen tot de stream als het besluit van de tv-provider (gezaghebbend) dat besluit zou bevatten.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor goedkeuringsbeslissingen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Het vergunningsbesluit is geldig voor een beperkte en korte periode die op het ogenblik van afgifte wordt gespecificeerd, die op de hoeveelheid tijd wijst het door de Authentificatie van Adobe Pass zal worden in het voorgeheugen ondergebracht alvorens te vereisen om de MVPD opnieuw te vragen.

Dit beperkte die tijdkader als vergunning (authZ) [ wordt bekend TTL ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

#### 4. Wat is een media-token en hoe lang is het geldig? {#authorization-phase-faq4}

Het media teken is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) documentatie wordt bepaald.

Het media token bestaat uit een ondertekende tekenreeks die in duidelijke tekst wordt verzonden en die kan worden opgehaald uit het eindpunt Autoriseren van beslissingen.

Voor meer informatie, verwijs naar de [ Symbolische documentatie van de Verificateur van Media ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Het media teken is geldig voor een beperkte en korte tijd die op het ogenblik van kwestie wordt gespecificeerd, die op de hoeveelheid tijd wijst het door de cliënttoepassing moet worden gebruikt alvorens te vereisen om Besluiten te vragen geef opnieuw eindpunt toe.

De clienttoepassing kan het media-token gebruiken om een bronstream af te spelen voor het geval dat de gebruiker toegang zou krijgen tot de stream als de beslissing van de tv-provider (gezaghebbend).

Raadpleeg de volgende documenten voor meer informatie:

* [API voor goedkeuringsbeslissingen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. Wat is een bron en welke indelingen worden ondersteund? {#authorization-phase-faq5}

Het middel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar de [ Beschermde documentatie van Middelen ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een vergunningsbesluit verkrijgen? {#authorization-phase-faq6}

De cliënttoepassing kan een vergunningsbesluit voor een beperkt aantal middelen in één enkel API verzoek, gewoonlijk tot 1, wegens voorwaarden verkrijgen die door MVPDs worden opgelegd.

+++

### Veelgestelde vragen over de afmeldingsfase {#logout-phase-faqs-general}

+++Veelgestelde vragen over de afmeldingsfase

#### 1. Wat is het doel van de afmeldingsfase? {#logout-phase-faq1}

Het doel van de afmeldingsfase is om de clienttoepassing de mogelijkheid te bieden het geverifieerde profiel van de gebruiker binnen de Adobe Pass-verificatie op verzoek te beëindigen.

+++

### Veelgestelde vragen over kopteksten {#headers-faqs-general}

+++Veelgestelde vragen over kopteksten

#### 1. Hoe kan de waarde voor de machtigingsheader worden berekend? {#headers-faq1}

De ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) verzoekkopbal van de Vergunning 0} {bevat het `Bearer` toegangstoken dat door de cliënttoepassing wordt vereist om tot Adobe Pass beschermde APIs toegang te hebben.[

De headerwaarde voor autorisatie moet worden verkregen bij Adobe Pass-verificatie tijdens de registratiefase.

Raadpleeg de volgende documenten voor meer informatie:

* [Overzicht van dynamische clientregistratie](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Client credentials-API ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [API voor toegangstoken ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamic Client Registration Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om het `Bearer` toegangstoken te verkrijgen als voorheen.

#### 2. Hoe kan ik de waarde voor de kop van de AP-Device-Identifier berekenen? {#headers-faq2}

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) verzoekkopbal bevat het stromen apparatenherkenningsteken aangezien het door de cliënttoepassing werd gecreeerd.

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) kopbaldocumentatie verstrekt sommige voorbeelden van hoe te om de waarde voor verschillende platforms gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de apparaat-id te berekenen als voorheen.

#### 3. Hoe kan ik de waarde voor de X-Device-Info header berekenen? {#headers-faq3}

De [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) verzoekkopbal bevat de cliëntinformatie (apparaat, verbinding en toepassing) met betrekking tot het daadwerkelijke het stromen apparaat.

De [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) kopbaldocumentatie verstrekt sommige voorbeelden van hoe te om de waarde voor verschillende platforms gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de apparaatinformatie te berekenen zoals voorheen.

+++

## Veelgestelde vragen over migratie {#migration-faqs}

Ga verder met deze sectie als u werkt aan een toepassing die een bestaande toepassing moet migreren naar REST API V2.

>[!MORELIKETHIS]
>
> * [ Dynamische Veelgestelde vragen van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Algemene veelgestelde vragen over migratie {#general-migration-faqs}

++ Algemene veelgestelde vragen over migratie

#### 1. Moet ik een nieuwe clienttoepassing implementeren die naar REST API V2 is gemigreerd voor alle gebruikers tegelijk? {#migration-faq1}

Nee.

De clienttoepassing is niet verplicht een nieuwe versie uit te voeren waarin de REST API V2 wordt geïntegreerd voor alle gebruikers tegelijk.

Adobe Pass Authentication zal tot eind 2025 oudere versies van clienttoepassingen blijven ondersteunen waarin de REST API V1 of SDK zijn geïntegreerd.

#### 2. Moet ik een nieuwe clienttoepassing implementeren die is gemigreerd naar REST API V2 voor alle API&#39;s en die tegelijkertijd stromen? {#migration-faq2}

Ja.

De clienttoepassing is vereist om een nieuwe versie uit te voeren die de REST API V2 integreert voor alle Adobe Pass Authentication-API&#39;s en die tegelijkertijd stroomt.

In het geval van de &quot;tweede het schermauthentificatie&quot;stroom, wordt de cliënttoepassing vereist om een nieuwe versie uit te voeren die REST API V2 voor zowel de [ primaire ](rest-api-v2-glossary.md#primary-application) als [ secundaire ](rest-api-v2-glossary.md#secondary-application) toepassingen gelijktijdig integreert.

Adobe Pass Authentication biedt geen ondersteuning voor &#39;hybride&#39; implementaties waarbij zowel REST API V2 als REST API V1/SDK worden geïntegreerd tussen API&#39;s en stromen.

#### 3. Zal de gebruikersverificatie behouden blijven bij het bijwerken naar een nieuwe clienttoepassing die is gemigreerd naar REST API V2? {#migration-faq3}

Nee.

De gebruikersverificatie die is verkregen in oudere clienttoepassingsversies waarin de REST API V1 of SDK is geïntegreerd, blijft niet behouden.

Daarom moet de gebruiker opnieuw worden geverifieerd in de nieuwe clienttoepassing die is gemigreerd naar REST API V2.

#### 4. Zijn de verbeterde foutcodes standaard ingeschakeld in REST API V2? {#migration-faq4}

Ja.

De clienttoepassingen die REST API V2 integreren, profiteren van de verbeterde functie voor foutcodes die standaard is ingeschakeld.

Voor meer details, verwijs naar de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migratie van REST API V1 naar REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Ga verder met deze subsectie als u werkt aan een toepassing die van REST API V1 naar REST API V2 moet migreren.

#### Veelgestelde vragen over de registratiefase {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over de registratiefase

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de registratiefase? {#registration-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er geen wijzigingen op hoog niveau met betrekking tot de registratiefase.

De cliënttoepassing kan de zelfde stroom blijven gebruiken om tegen de Authentificatie van Adobe Pass door het [ Dynamische proces van de Registratie van de Cliënt (DCR) te registreren ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) en een toegangstoken te verkrijgen.

Raadpleeg de volgende documenten voor meer informatie:

* [Overzicht van dynamische clientregistratie](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Client credentials-API ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [API voor toegangstoken ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamic Client Registration Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Veelgestelde vragen over de configuratiefase {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over de configuratiefase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de configuratiefase? {#configuration-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ GET <br/> /api/v1/config/{serviceProvider} ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Veelgestelde vragen over de verificatiefase {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over de verificatiefase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de verificatiefase? {#authentication-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ POST <br/> /reggie/v1/{serviceProvider}/regcode ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /sessies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificatie hervatten (MVPD) op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) verificatie starten | [ GET <br/> /api/v1/authenticate ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ GET <br/> /api/v1/checkauthn (eerste scherm) ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [ GET <br/> /api/v1/checkauthn (tweede scherm) ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Verificatietoken van gebruiker ophalen (profiel) | [ GET <br/> /api/v1/tokens/authn ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ GET <br/> /api/v1/tokens/usermetadata ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de preautorisatiefase {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over voorafgaande goedkeuring

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de fase voorafgaand aan de autorisatie? {#preauthorization-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ GET <br/> /api/v1/preauthorize (eerste scherm) ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [ GET <br/> /api/v1/preauthorize (tweede scherm) ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de machtigingsfase {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over de machtigingsfase

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de machtigingsfase? {#authorization-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorisatie (MVPD) starten | [ GET <br/> /api/v1/authorize ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Token van autorisatie ophalen (machtigingsbesluit) | [ GET <br/> /api/v1/tokens/authz ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Token voor korte autorisatie ophalen (mediatoken) | [ GET <br/> /api/v1/tokens/media ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de afmeldingsfase {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Veelgestelde vragen over de afmeldingsfase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de afmeldingsfase? {#logout-phase-v1-to-v2-faq1}

Bij de migratie van REST API V1 naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabel worden vermeld:

| Toepassingsgebied | REST API V1 | REST API V2 | Opmerkingen |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ GET <br/> /api/v1/logout ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migratie van SDK naar REST API V2 {#migration-sdk-to-rest-api-v2}

Ga verder met deze subsectie als u werkt aan een toepassing die van SDK naar REST API V2 moet migreren.

#### Veelgestelde vragen over de registratiefase {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over de registratiefase

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de registratiefase? {#registration-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Complete Dynamic Client Registration (DCR) | Softwareinstructie leveren aan constructor | [ POST <br/> /o/client/register ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [ GET <br/> /o/client/token ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Dynamisch het Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ Dynamische Stroom van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de configuratiefase {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over de configuratiefase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de configuratiefase? {#configuration-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lijst met MVPD&#39;s met actieve integratie ophalen | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [ GET <br/> /api/v2/{serviceProvider}/configuration ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Veelgestelde vragen over de verificatiefase {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over de verificatiefase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de verificatiefase? {#authentication-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) verificatie starten | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabled iOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) verificatie starten | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /sessies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificatie hervatten (MVPD) op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) verificatie starten | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) verificatie starten | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registratiecode ophalen (verificatiecode) | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registratiecode controleren (verificatiecode) | [ GET <br/> /reggie/v1/{serviceProvider}/regcode/{code} ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [ GET <br/> /api/v2/{serviceProvider} /sessies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificatie hervatten (MVPD) op tweede scherm (toepassing) | [ GET <br/> /api/v1/authenticate ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [ POST <br/> /api/v2/{serviceProvider}/essies/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) verificatie starten | [ AccessEnabler.getAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [ AccessEnabler.setSelectedProvider ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [ POST <br/> /api/v2/{serviceProvider}/zittingen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ GET <br/> /api/v2/authenticate/{serviceProvider}/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis authentificatiestroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ Basis authentificatiestroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Gebruikersverificatiestatus controleren | [ AccessEnabler.checkAuthentication ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Metagegevens van gebruikers ophalen | [ AccessEnabler.getMetadata ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Voer een van de volgende handelingen uit: <br/><br/> [ GET <br/> /api/v2/{serviceProvider}/profiles ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [ GET <br/> /api/v2/{serviceProvider}/profiles/code/{code} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | De clienttoepassing kan de reactie van deze API&#39;s voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Gebruikersverificatiestatus controleren</li><li>Gebruikersprofiel ophalen</li><li>Metagegevens van gebruikers ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis die profielstroom binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ Basisprofielstroom die binnen secundaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de preautorisatiefase {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over voorafgaande goedkeuring

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de fase voorafgaand aan de autorisatie? {#preauthorization-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [ AccessEnabler.preauthorize ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [ AccessEnabler.preauthorize ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [ AccessEnabler.preauthorize ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Voorkeursmiddelen ophalen (voorafgaande autorisatiebesluiten) | [ AccessEnabler.checkPreauthorisedResources ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [ POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis pre-vergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de machtigingsfase {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over de machtigingsfase

##### 1. Welke API-migraties op hoog niveau zijn vereist voor de machtigingsfase? {#authorization-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Token voor korte autorisatie ophalen (mediatoken) | [ AccessEnabler.checkAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [ AccessEnabler.getAuthorization ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [ POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd} ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | De clienttoepassing kan de reactie van deze API voor meerdere doeleinden tegelijk gebruiken: <br/> <ul><li>Autorisatie (MVPD) starten</li><li>Goedkeuringsbesluit intrekken</li><li>Snel media-token ophalen</li></ul> <br/> Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basisvergunningsstroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Veelgestelde vragen over de afmeldingsfase {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Veelgestelde vragen over de afmeldingsfase

##### 1. Wat zijn de API-migraties op hoog niveau die vereist zijn voor de afmeldingsfase? {#logout-phase-sdk-to-v2-faq1}

Bij de migratie van SDK&#39;s naar REST API V2 zijn er belangrijke wijzigingen die in de volgende tabellen worden voorgesteld:

###### AccessEnabled JavaScript SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabled Android SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Toepassingsgebied | SDK | REST API V2 | Opmerkingen |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmelden starten | [ AccessEnabler.logout ](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [ GET <br/> /api/v2/{serviceProvider}/logout ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Raadpleeg de volgende documenten voor meer informatie: <br/> <ul><li>[ Basis logout stroom die binnen primaire toepassing wordt uitgevoerd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
