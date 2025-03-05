---
title: Veelgestelde vragen over REST API V2
description: Veelgestelde vragen over REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '9072'
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

### Veelgestelde vragen over de registratiefase {#registration-phase-faqs-general}

+++Veelgestelde vragen over de registratiefase

Verwijs naar de [ Dynamische Veelgestelde documentatie van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Veelgestelde vragen over de configuratiefase {#configuration-phase-faqs-general}

+++Veelgestelde vragen over de configuratiefase

#### 1. Wat is het doel van de fase van de Configuratie? {#configuration-phase-faq1}

Het doel van de configuratiefase is om de clienttoepassing een lijst met MVPD&#39;s te geven waarmee deze actief is geïntegreerd, samen met configuratiegegevens (bijvoorbeeld `id` , `displayName` , `logoUrl` , enz.) die zijn opgeslagen door Adobe Pass Authentication voor elke MVPD.

De configuratiefase fungeert als een noodzakelijke stap voor de verificatiefase wanneer de clienttoepassing de gebruiker moet vragen zijn of haar tv-provider te selecteren.

#### 2. Is de configuratiefase verplicht? {#configuration-phase-faq2}

De configuratiefase is niet verplicht. De clienttoepassing moet de configuratie alleen ophalen wanneer de gebruiker zijn MVPD moet selecteren om te worden geverifieerd of opnieuw te worden geverifieerd.

De cliënttoepassing kan deze fase in de volgende scenario&#39;s overslaan:

* De gebruiker is al geverifieerd.
* De gebruiker wordt aangeboden tijdelijke toegang door basis of promotionele [ ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) eigenschap TempPass.
* De gebruikersverificatie is verlopen, maar de clienttoepassing heeft de eerder geselecteerde MVPD in de cache geplaatst als een gebruikerservaring die de keuze motiveert. De gebruiker wordt alleen gevraagd te bevestigen dat hij of zij nog steeds een abonnee van die MVPD is.

#### 3. Wat is een configuratie en hoe lang is deze geldig? {#configuration-phase-faq3}

De configuratie is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) documentatie wordt bepaald.

De configuratie bevat een lijst van MVPDs die door de volgende attributen `id` wordt bepaald, `displayName`, `logoUrl`, enz., die van het [ 4} eindpunt van de Configuratie kan worden teruggewonnen.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

De clienttoepassing moet de configuratie alleen ophalen wanneer de gebruiker zijn MVPD moet selecteren om te worden geverifieerd of opnieuw te worden geverifieerd.

De clienttoepassing kan de configuratiereactie gebruiken om een UI-kiezer met beschikbare MVPD-opties weer te geven wanneer de gebruiker zijn tv-provider moet selecteren.

De clienttoepassing moet de door de gebruiker geselecteerde MVPD-id, zoals opgegeven door het MVPD-kenmerk configuration-level `id` , opslaan om door te gaan met de fasen Verificatie, Voortoelating, Autorisatie of Afmelden.

Voor meer informatie, verwijs naar [ terugwinnen configuratie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) documentatie.

#### 4. Moet de client-toepassing de informatie over de configuratiereactie in cache plaatsen in een permanente opslag? {#configuration-phase-faq4}

De clienttoepassing moet de configuratie alleen ophalen wanneer de gebruiker zijn MVPD moet selecteren om te worden geverifieerd of opnieuw te worden geverifieerd.

De cliënttoepassing zou de informatie van de configuratiereactie in een geheugenopslag in het voorgeheugen moeten opslaan om onnodige verzoeken te vermijden en de gebruikerservaring te verbeteren wanneer:

* De gebruiker is al geverifieerd.
* De gebruiker wordt aangeboden tijdelijke toegang door basis of promotionele [ ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) eigenschap TempPass.
* De gebruikersverificatie is verlopen, maar de clienttoepassing heeft de eerder geselecteerde MVPD in de cache geplaatst als een gebruikerservaring die de keuze motiveert. De gebruiker wordt alleen gevraagd te bevestigen dat hij of zij nog steeds een abonnee van die MVPD is.

#### 5. Kan de clienttoepassing zijn eigen lijst met MVPD&#39;s beheren? {#configuration-phase-faq5}

De clienttoepassing kan een eigen lijst met MVPD&#39;s beheren, maar de MVPD-id&#39;s moeten synchroon blijven met de Adobe Pass-verificatie. Daarom wordt aangeraden de configuratie te gebruiken die door Adobe Pass Authentication wordt geleverd om ervoor te zorgen dat de lijst up-to-date en accuraat is.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) van de Authentificatie van Adobe Pass REST API V2 ontvangen als het verstrekte herkenningsteken van MVPD ongeldig is of in het geval heeft het geen actieve integratie met de gespecificeerde [ dienstverlener ](rest-api-v2-glossary.md#service-provider).

#### 6. Kan de clienttoepassing de lijst met MVPD&#39;s filteren? {#configuration-phase-faq6}

De cliënttoepassing kan de lijst van MVPDs filtreren die in de configuratiereactie wordt verstrekt door een douanemechanisme uit te voeren dat op zijn eigen bedrijfslogica en vereisten zoals gebruikersplaats of gebruikersgeschiedenis van vorige selectie wordt gebaseerd.

De cliënttoepassing kan de lijst van [ TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPDs of MVPDs filtreren die hun integratie nog in ontwikkeling of het testen hebben.

#### 7. Wat gebeurt er als de integratie met een MVPD is uitgeschakeld en als inactief is gemarkeerd? {#configuration-phase-faq7}

Wanneer de integratie met een MVPD is uitgeschakeld en als inactief is gemarkeerd, wordt de MVPD verwijderd uit de lijst met MVPD&#39;s die in verdere configuratieantwoorden wordt geleverd. Er zijn twee belangrijke gevolgen die in overweging moeten worden genomen:

* De niet-geverifieerde gebruikers van die MVPD kunnen de verificatiefase niet meer voltooien met die MVPD.
* De geverifieerde gebruikers van die MVPD kunnen de fase van voorafgaande toestemming, autorisatie of afmelding met die MVPD niet meer voltooien.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) van de Authentificatie van Adobe Pass REST API V2 ontvangen als de gebruiker selecteerde MVPD geen actieve integratie met de gespecificeerde [ dienstverlener ](rest-api-v2-glossary.md#service-provider) heeft.

#### 8. Wat gebeurt er als de integratie met een MVPD weer ingeschakeld en gemarkeerd wordt als actief? {#configuration-phase-faq8}

Wanneer de integratie met een MVPD terug en duidelijk als actief wordt toegelaten, dan is MVPD inbegrepen terug in de lijst van MVPDs die in verdere configuratiereacties wordt verstrekt, en er zijn twee belangrijke gevolgen om te overwegen:

* De niet-geverifieerde gebruikers van die MVPD kunnen de verificatiefase opnieuw voltooien met behulp van die MVPD.
* De geautoriseerde gebruikers van die MVPD kunnen de fase van voorafgaande toestemming, autorisatie of afmelding met die MVPD opnieuw voltooien.

#### 9. Hoe kan de integratie met een MVPD worden in- of uitgeschakeld? {#configuration-phase-faq9}

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

+++

### Veelgestelde vragen over de verificatiefase {#authentication-phase-faqs-general}

+++Veelgestelde vragen over de verificatiefase

#### 1. Wat is het doel van de authentificatiefase? {#authentication-phase-faq1}

Het doel van de authentificatiefase is de cliënttoepassing de capaciteit te verstrekken om de identiteit van de gebruiker te verifiëren en informatie van gebruikersmeta-gegevens te verkrijgen.

De verificatiefase fungeert als een noodzakelijke stap voor de fase voorafgaand aan autorisatie of de machtigingsfase wanneer de clienttoepassing inhoud moet afspelen.

#### 2. Wat is een verificatiesessie en hoe lang is deze geldig? {#authentication-phase-faq2}

De authentificatiesessie is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) documentatie wordt bepaald.

De authentificatiesessie slaat informatie over het in werking gestelde authentificatieproces op dat van het eindpunt van Zittingen kan worden teruggewonnen.

De verificatiesessie is geldig gedurende een beperkt en kort tijdsbestek dat op het moment van uitgifte door de `notAfter` -tijdstempel wordt opgegeven. Dit geeft aan hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat hij of zij de flow opnieuw moet starten.

De cliënttoepassing kan de reactie van de authentificatiesessie gebruiken om te weten hoe te met het authentificatieproces te werk te gaan. Let op: er zijn gevallen waarin de gebruiker niet verplicht is om te verifiëren, zoals het verlenen van tijdelijke toegang, gedegradeerde toegang, of wanneer de gebruiker reeds voor authentiek wordt verklaard.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 3. Wat is een verificatiecode en hoe lang is deze geldig? {#authentication-phase-faq3}

De authentificatiecode is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) documentatie wordt bepaald.

De authentificatiecode slaat een unieke waarde op die wordt geproduceerd wanneer een gebruiker het authentificatieproces in werking stelt, en identificeert uniek de authentificatiesessie van een gebruiker tot het proces volledig is of tot de authentificatiesessie verloopt.

De verificatiecode is geldig gedurende een beperkt en kort tijdsbestek dat is opgegeven op het moment dat de verificatiesessie wordt gestart met de `notAfter` -tijdstempel, om aan te geven hoeveel tijd de gebruiker nodig heeft om het verificatieproces te voltooien voordat hij of zij de flow opnieuw moet starten.

De clienttoepassing kan de verificatiecode gebruiken om te controleren of de gebruiker de verificatie heeft voltooid en om de profielgegevens van de gebruiker op te halen, meestal via een opiniepeilingsmechanisme.

De cliënttoepassing kan de authentificatiecode ook gebruiken om de gebruiker toe te staan om het authentificatieproces of op het zelfde apparaat of op tweede (scherm) te voltooien of te hervatten, aangezien de authentificatiesessie niet verstreek.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor verificatiesessie maken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API voor verificatiesessie hervatten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 4. Hoe kan de clienttoepassing weten of de gebruiker een geldige verificatiecode heeft getypt en of de verificatiesessie nog niet is verlopen? {#authentication-phase-faq4}

De cliënttoepassing kan de authentificatiecode bevestigen die door de gebruiker in een secundaire (scherm) toepassing wordt getypt door een verzoek naar één van het eindpunt van Zittingen te verzenden verantwoordelijk om authentificatiesessie te hervatten of de informatie van de authentificatiesessie terug te winnen verbonden aan de authentificatiecode.

De cliënttoepassing zou een [ fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) ontvangen als de verstrekte authentificatiecode verkeerd werd getypt of in het geval dat de authentificatiesessie zou verlopen.

Voor meer informatie, verwijs naar [ hervat authentificatiesessie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) en [ ontvang authentificatiesessie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) documenten.

#### 5. Hoe kan de clienttoepassing weten of de gebruiker al is geverifieerd? {#authentication-phase-faq5}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om te verifiëren of een gebruiker reeds voor authentiek wordt verklaard en de informatie van het terugkeerprofiel terugkeert:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 6. Wat is een profiel en hoe lang is het geldig? {#authentication-phase-faq6}

Het profiel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) documentatie wordt bepaald.

Het profiel slaat informatie over de de authentificatierealiteit van de gebruiker, meta-gegevensinformatie en veel meer op die van het eindpunt van Profielen kunnen worden teruggewonnen.

De clienttoepassing kan het profiel gebruiken om de verificatiestatus van de gebruiker te kennen, toegang te krijgen tot metagegevens van de gebruiker, de methode te zoeken die wordt gebruikt voor verificatie of de entiteit die wordt gebruikt voor het opgeven van de identiteit.

Raadpleeg de volgende documenten voor meer informatie:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Het profiel is geldig gedurende een beperkt tijdsbestek dat is opgegeven wanneer het wordt opgevraagd door het tijdstempel van `notAfter` . Dit geeft aan hoeveel tijd de gebruiker een geldige verificatie heeft voordat deze de verificatiefase opnieuw moet doorlopen.

Dit beperkte die tijdkader als authentificatie (authN) [ wordt bekend TTL ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

#### 7. Moet de clienttoepassing de profielgegevens van de gebruiker in cache plaatsen in een permanente opslag? {#authentication-phase-faq7}

De clienttoepassing moet de profielgegevens van de gebruiker in een permanente opslag in cache plaatsen om onnodige verzoeken te voorkomen en de gebruikerservaring te verbeteren, rekening houdend met de volgende aspecten:

| Kenmerk | Gebruikerservaring |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `attributes` | De cliënttoepassing kan dit gebruiken om de gebruikerservaring te personaliseren die op verschillende [ wordt gebaseerd gebruikersmeta-gegevens ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) sleutels (b.v., `zip`, `maxRating`, enz.). |
| `mvpd` | De clienttoepassing kan dit gebruiken om de geselecteerde tv-provider van de gebruiker bij te houden.<br/><br/> wanneer het huidige gebruikersprofiel verloopt, kan de cliënttoepassing de onthouden selectie van MVPD gebruiken en enkel de gebruiker vragen om te bevestigen. |
| `notAfter` | De clienttoepassing kan dit gebruiken om de vervaldatum van het gebruikersprofiel bij te houden en het herverificatieproces te activeren wanneer dit verloopt, waarbij fouten tijdens de fasen voor voorafgaande toestemming of autorisatie worden vermeden.<br/><br/> de fout behandeling van de cliënttoepassing moet [ kunnen behandelen authenticated_profile_expired ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) foutencode, die erop wijst dat de cliënttoepassing de gebruiker vereist om opnieuw voor authentiek te verklaren. |

#### 8. Kan de clienttoepassing het profiel van de gebruiker uitbreiden zonder dat opnieuw verificatie vereist is? {#authentication-phase-faq8}

Nee.

Het gebruikersprofiel kan niet buiten zijn geldigheid zonder gebruikersinteractie worden uitgebreid, aangezien zijn vervaldatum door authentificatie TTL wordt bepaald die met MVPDs wordt gevestigd.

Daarom moet de clienttoepassing de gebruiker vragen opnieuw te verifiëren en te communiceren met de MVPD-aanmeldingspagina om het profiel op ons systeem te vernieuwen.

Nochtans, voor MVPDs die [ op huis-gebaseerde authentificatie ](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA) steunt, zal de gebruiker niet worden vereist om geloofsbrieven in te gaan.

#### 9. Wat zijn de gebruiksgevallen voor elk beschikbaar eindpunt van Profielen? {#authentication-phase-faq9}

De eindpunten van de Profielen van de basis worden ontworpen om cliënttoepassing het vermogen te verstrekken om de de authentificatiestatus van de gebruiker te kennen, tot de informatie van gebruikersmeta-gegevens toegang te hebben, de methode te vinden die wordt gebruikt om voor authentiek te verklaren of de entiteit die wordt gebruikt om identiteit te verstrekken.

Elk eindpunt past als volgt bij een specifiek geval van gebruik:

| API | Beschrijving | Hoofdletters gebruiken |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [ het eindpunt van Profielen API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Alle gebruikersprofielen ophalen. | **Gebruiker opent de cliënttoepassing voor het eerst**<br/><br/> In dit scenario, heeft de cliënttoepassing niet het geselecteerde herkenningsteken van MVPD van de gebruiker in het voorgeheugen onder wordt gebracht in blijvende opslag.<br/><br/> dientengevolge, zal het één enkel verzoek verzenden om alle beschikbare gebruikersprofielen terug te winnen. |
| [ eindpunt van Profielen voor specifieke MVPD API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Hiermee wordt het gebruikersprofiel opgehaald dat is gekoppeld aan een specifieke MVPD. | **de terugkeer van de Gebruiker aan de cliënttoepassing na het voor authentiek verklaren in een vorig bezoek**<br/><br/> In dit geval, moet de cliënttoepassing het eerder geselecteerde herkenningsteken van MVPD van de gebruiker hebben in blijvende opslag in het voorgeheugen onder wordt gebracht.<br/><br/> dientengevolge, zal het één enkel verzoek verzenden om het profiel van de gebruiker voor die specifieke MVPD terug te winnen. |
| [ eindpunt van Profielen voor specifieke (authentificatie) code API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Haal het gebruikersprofiel op dat is gekoppeld aan een specifieke verificatiecode. | **Gebruiker stelt het authentificatieproces**<br/><br/> in dit scenario in werking, moet de cliënttoepassing bepalen of de gebruiker met succes authentificatie heeft voltooid en hun profielinformatie terugwinnen.<br/><br/> dientengevolge, zal het een opiniepeilingsmechanisme beginnen om het profiel van de gebruiker terug te winnen verbonden aan de authentificatiecode. |

Voor meer details, verwijs naar de [ Basisprofielstroom die binnen primaire toepassing ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) wordt uitgevoerd en [ Basisprofielstroom die binnen secundaire toepassing ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md) documenten wordt uitgevoerd.

Het eindpunt van Profielen SSO dient een verschillend doel, het verstrekt de cliënttoepassing het vermogen om een gebruikersprofiel tot stand te brengen gebruikend de reactie van de partnerauthentificatie en het terug te winnen in één enkele, éénmalige verrichting.

| API | Beschrijving | Hoofdletters gebruiken |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [ Profielen SSO eindpunt API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Creeer en wik gebruikersprofiel gebruikend de reactie van de partnerauthentificatie. | **Gebruiker laat de toepassing toe om partner enig-sign-on te gebruiken om**<br/><br/> in dit scenario voor authentiek te verklaren, moet de cliënttoepassing een gebruikersprofiel tot stand brengen na het ontvangen van de reactie van de partnerauthentificatie en het in één enkele, eenmalige verrichting terugwinnen. |

Voor om het even welke verdere vragen, moeten de basis eindpunten van Profielen worden gebruikt om de de authentificatiestatus van de gebruiker te bepalen, tot de informatie van gebruikersmeta-gegevens toegang te hebben, de methode te vinden die wordt gebruikt om voor authentiek te verklaren of de entiteit die wordt gebruikt om identiteit te verstrekken.

Voor meer details, verwijs naar [ Enige sign-on gebruikend partnerstromen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) en [ Apple SSO Cookbook (REST API V2) ](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) documenten.

#### 10. Wat moet de clienttoepassing doen als de gebruiker meerdere MVPD-profielen heeft? {#authentication-phase-faq10}

Wanneer de gebruiker meerdere MVPD-profielen heeft, bepaalt de clienttoepassing de beste aanpak voor de afhandeling van dit scenario.

De clienttoepassing kan de gebruiker vragen het gewenste MVPD-profiel te selecteren of de selectie automatisch te maken, bijvoorbeeld door het eerste gebruikersprofiel in het antwoord te kiezen of het profiel met de langste geldigheidsperiode.

#### 11. Wat gebeurt er als gebruikersprofielen verlopen? {#authentication-phase-faq11}

Wanneer gebruikersprofielen verlopen, worden ze niet meer opgenomen in de reactie van het eindpunt van Profielen.

Als het eindpunt van Profielen een lege reactie van de profielkaart terugkeert, moet de cliënttoepassing een nieuwe authentificatiesessie creëren en de gebruiker ertoe aanzetten om opnieuw voor authentiek te verklaren.

Voor meer informatie, verwijs naar [ creeer authentificatiesessie API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) documentatie.

#### 12. Wanneer worden gebruikersprofielen ongeldig? {#authentication-phase-faq12}

Gebruikersprofielen worden in de volgende gevallen ongeldig:

* Wanneer de verificatie-TTL verloopt, zoals aangegeven door het tijdstempel van `notAfter` in de eindpuntreactie van Profielen.
* Wanneer de cliënttoepassing [ AP-apparaat-Identifier ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) kopbalwaarde verandert.
* Wanneer de cliënttoepassing de cliëntgeloofsbrieven bijwerkt die worden gebruikt om de [ kopbalwaarde van de Vergunning ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) terug te winnen.
* Wanneer de clienttoepassing de softwarefunctie die wordt gebruikt om clientreferenties op te halen of bijwerkt.

#### 13. Wanneer moet de clienttoepassing het opiniepeilingsmechanisme starten? {#authentication-phase-faq13}

Om de efficiëntie te waarborgen en onnodige verzoeken te vermijden, moet de clienttoepassing onder de volgende omstandigheden beginnen met het opiniepeilingsmechanisme:

**Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd**

De primaire (het stromen) toepassing zou moeten beginnen opiniepeilend wanneer de gebruiker de definitieve bestemmingspagina bereikt, nadat de browser component URL laadt die voor de `redirectUrl` parameter in het [ 2} eindpuntverzoek van Sessies {wordt gespecificeerd.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)

**Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd**

De primaire (het stromen) toepassing zou moeten beginnen opiniepeilend zodra de gebruiker het authentificatieproces-recht na het ontvangen van de ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) eindpuntreactie van de Zittingen [ in werking stelt en de authentificatiecode aan de gebruiker toont.

#### 14. Wanneer moet de clienttoepassing het opiniepeilingsmechanisme stoppen? {#authentication-phase-faq14}

Om de efficiëntie te waarborgen en onnodige verzoeken te vermijden, moet de clienttoepassing het opiniepeilingsmechanisme stopzetten onder de volgende omstandigheden:

**Succesvolle authentificatie**

De profielgegevens van de gebruiker worden opgehaald en hun verificatiestatus wordt bevestigd. Op dit moment is opiniepeiling niet langer nodig.

**de zitting van de Authentificatie en coderepering**

De authentificatiesessie en de code verlopen, zoals die door `notAfter` wordt vermeld timestamp in de [ 2} eindpuntreactie van Zittingen {. ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) Als dit gebeurt, moet de gebruiker het authentificatieproces opnieuw beginnen, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

**Nieuwe geproduceerde authentificatiecode**

Als de gebruiker om een nieuwe authentificatiecode op het primaire (scherm) apparaat verzoekt, is de bestaande zitting niet meer geldig, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.

#### 15. Welk interval tussen vraag zou de cliënttoepassing voor het opiniepeilingsmechanisme moeten gebruiken? {#authentication-phase-faq15}

Om efficiëntie te garanderen en onnodige verzoeken te voorkomen, moet de clienttoepassing de frequentie van het opiniepeilingsmechanisme onder de volgende omstandigheden configureren:

| **Authentificatie die binnen de primaire (scherm) toepassing wordt uitgevoerd** | **Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd** |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| De primaire (het stromen) toepassing zou om de 1-5 seconden moeten opiniepeilen. | De primaire (het stromen) toepassing zou om de 3-5 seconden moeten opiniepeilen. |

#### 16. Wat is het maximumaantal opiniepeilingsverzoeken dat de clienttoepassing kan verzenden? {#authentication-phase-faq16}

De cliënttoepassing moet de huidige grenzen naleven die door de Authentificatie van Adobe Pass [ worden bepaald die Mechanisme van de Omwenteling ](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits).

De fout behandeling van de cliënttoepassing moet [ 429 Te Vele de foutencode van Verzoeken ](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) kunnen behandelen, die erop wijst dat de cliënttoepassing het maximum toegestane aantal verzoeken heeft overschreden.

Voor meer details, verwijs naar het [ Throttling Mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

#### 17. Hoe kan de clienttoepassing de metagegevens van de gebruiker ophalen? {#authentication-phase-faq17}

De cliënttoepassing kan één van de volgende eindpunten vragen geschikt om [ informatie van gebruikersmeta-gegevens ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) als deel van de profielinformatie terug te keren:

* [API voor eindpunt van profielen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Het eindpunt van profielen voor specifieke MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Het eindpunt van profielen voor specifieke (authentificatie) code API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

De cliënttoepassing te hoeven niet om een afzonderlijk eindpunt te vragen om de de meta-gegevensinformatie van de gebruiker terug te winnen, aangezien het reeds in de profielinformatie inbegrepen is die wordt verkregen wanneer het verifiëren of de gebruiker voor authentiek wordt verklaard.

Raadpleeg de volgende documenten voor meer informatie:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 18. Hoe moet de clienttoepassing onbeheerde toegang beheren? {#authentication-phase-faq18}

Gezien uw organisatie van plan is om de [ degradatie ](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) eigenschap te gebruiken, moet de cliënttoepassing degraded toegangsstromen behandelen, die schetsen hoe REST API v2 eindpunten zich in dergelijke scenario&#39;s gedragen.

Voor meer details, verwijs naar [ Verminderde toegangsstromen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) documentatie.

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

#### 4. Moet de clienttoepassing de voorafgaande beslissingen in een permanente opslag in cache plaatsen? {#preauthorization-phase-faq4}

De clienttoepassing is niet verplicht om beslissingen vóór autorisatie op te slaan in permanente opslag. Nochtans, wordt het geadviseerd om vergunningsbesluiten in het voorgeheugen op te slaan om de gebruikerservaring te verbeteren. Dit helpt onnodige vraag aan de Besluiten te vermijden vooraf goedkeurt eindpunt voor middelen die reeds zijn toegestaan, verminderend latentie en verbeterend prestaties.

#### 5. Hoe kan de clienttoepassing bepalen waarom een besluit tot voorafgaande toestemming is geweigerd? {#preauthorization-phase-faq5}

De cliënttoepassing kan de reden voor een ontkend pre-vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van het Besluiten pre-goedkeurt eindpunt. Deze details geven inzicht in de specifieke reden waarom de aanvraag voor voorafgaande toestemming is afgewezen, waardoor de gebruikerservaring kan worden geïnformeerd of de benodigde afhandeling in de toepassing kan worden geactiveerd.

Ervoor zorgen dat elk nieuw mechanisme dat wordt toegepast voor het ophalen van beslissingen vóór toelating, niet in een eindeloze lus resulteert als het besluit vóór toelating wordt geweigerd.

U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

#### 6. Waarom ontbreekt het aan een media-token in het besluit tot voorafgaande toestemming? {#preauthorization-phase-faq6}

In het besluit tot voorafgaande toestemming ontbreekt een media-token omdat de fase van voorafgaande toestemming niet mag worden gebruikt om bronnen af te spelen, aangezien dat het doel is van de fase van autorisatie.

#### 7. Kan de machtigingsfase worden overgeslagen als er al een besluit tot voorafgaande goedkeuring bestaat? {#preauthorization-phase-faq7}

Nee.

De machtigingsfase kan niet worden overgeslagen, zelfs niet als er een besluit tot voorafgaande toestemming beschikbaar is. Voorafgaande beslissingen zijn alleen ter informatie en geven geen daadwerkelijke afspeelrechten. De fase voorafgaand aan de autorisatie is bedoeld als vroegtijdig advies, maar de fase van autorisatie is nog steeds vereist voordat inhoud wordt afgespeeld.

#### 8. Wat is een bron en welke indelingen worden ondersteund? {#preauthorization-phase-faq8}

Het middel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar de [ Beschermde documentatie van Middelen ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 9. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een besluit tot voorafgaande toestemming verkrijgen? {#preauthorization-phase-faq9}

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

#### 4. Moet de clienttoepassing de vergunningsbesluiten in een permanente opslag in cache plaatsen? {#authorization-phase-faq4}

De clienttoepassing is niet verplicht om machtigingsbesluiten op te slaan in permanente opslag.

#### 5. Hoe kan de clienttoepassing bepalen waarom een vergunningsbesluit is geweigerd? {#authorization-phase-faq5}

De cliënttoepassing kan de reden voor een ontkend vergunningsbesluit bepalen door de [ foutencode en het bericht ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) te inspecteren inbegrepen in de reactie van Besluiten machtigt eindpunt. Deze details geven inzicht in de specifieke reden waarom de aanvraag voor een vergunning is afgewezen, waardoor de gebruikerservaring kan worden geïnformeerd of een noodzakelijke afhandeling in de toepassing kan worden gestart.

Ervoor zorgen dat een nieuw mechanisme dat wordt toegepast voor het ophalen van vergunningsbesluiten niet in een eindeloze lus resulteert als het vergunningsbesluit wordt geweigerd.

U kunt overwegen om pogingen tot een redelijk aantal te beperken en weigeringen netjes af te handelen door duidelijke feedback aan de gebruiker te bekijken.

#### 6. Wat is een media-token en hoe lang is het geldig? {#authorization-phase-faq6}

Het media teken is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) documentatie wordt bepaald.

Het media token bestaat uit een ondertekende tekenreeks die in duidelijke tekst wordt verzonden en die kan worden opgehaald uit het eindpunt Autoriseren van beslissingen.

Voor meer informatie, verwijs naar de [ Symbolische documentatie van de Verificateur van Media ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

De mediatoken is geldig gedurende een beperkte en korte periode die op het moment van uitgifte is opgegeven. Deze periode geeft de tijdslimiet aan voordat deze door de clienttoepassing moet worden geverifieerd en gebruikt.

De clienttoepassing kan het media-token gebruiken om een bronstream af te spelen voor het geval dat de gebruiker toegang zou krijgen tot de stream als de beslissing van de tv-provider (gezaghebbend).

Raadpleeg de volgende documenten voor meer informatie:

* [API voor goedkeuringsbeslissingen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 7. Moet de clienttoepassing het media-token valideren voordat de bronstream wordt afgespeeld? {#authorization-phase-faq7}

Ja.

De clienttoepassing moet het mediatoken valideren voordat de bronstream wordt afgespeeld. Deze bevestiging zou moeten worden uitgevoerd gebruikend de [ Symbolische Verifier van Media ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Door `serializedToken` van de geretourneerde `token` te controleren, helpt de clienttoepassing onbevoegde toegang te voorkomen, zoals het rippen van streams, en zorgt deze ervoor dat alleen correct geautoriseerde gebruikers de inhoud kunnen afspelen.

#### 8. Moet de clienttoepassing een verlopen mediatken vernieuwen tijdens het afspelen? {#authorization-phase-faq8}

Nee.

De clienttoepassing is niet verplicht een verlopen mediatoken te vernieuwen terwijl de stream actief wordt afgespeeld. Als het media-token tijdens het afspelen verloopt, moet het zijn toegestaan dat de stream zonder onderbreking wordt voortgezet. Nochtans, moet de cliënt om een nieuw vergunningsbesluit — en een nieuw media teken verkrijgen — de volgende keer de gebruiker probeert om het zelfde middel te spelen.

#### 9. Wat is het doel van elk tijdstempelkenmerk in het vergunningsbesluit? {#authorization-phase-faq9}

Het vergunningsbesluit bevat verschillende tijdstempelkenmerken die een essentiële context bieden voor de geldigheidsperiode van de vergunning zelf en de bijbehorende media-token. Deze tijdstempels hebben verschillende doeleinden, afhankelijk van het feit of ze betrekking hebben op het vergunningsbesluit of het media-token.

**besluit-Niveau Tijdstempels**

Deze tijdstempels beschrijven de geldigheidsperiode van het algemene vergunningsbesluit:

| Kenmerk | Beschrijving | Notities |
|-------------|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Het tijdstip waarop het vergunningsbesluit is gegeven. | Dit is het begin van het geldigheidvenster van de autorisatie. |
| `notAfter` | Het tijdstip waarop het vergunningsbesluit verstrijkt. | De [ toestemmingstijd-aan-Levende (TTL) ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) bepaalt hoe lang de vergunning geldig blijft alvorens re-vergunning te vereisen. Over deze GVTO wordt onderhandeld met vertegenwoordigers van MVPD. |

**symbolisch-Niveau Tijdstempels**

Deze tijdstempels beschrijven de geldigheidsperiode van het media-token dat aan het vergunningsbesluit is gekoppeld:

| Kenmerk | Beschrijving | Notities |
|-------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Het tijdstip waarop het media-token werd uitgegeven. | Dit geeft aan wanneer het token geldig wordt voor afspelen. |
| `notAfter` | De tijd waarop het media-token verloopt. | De tokens van media hebben een opzettelijk korte levensduur (typisch 7 minuten) om misgebruikrisico&#39;s te minimaliseren en voor potentiële klokverschillen tussen de symbolisch-genererende server en de symbolisch-controlerende server rekening te houden. |

#### 10. Wat is een bron en welke indelingen worden ondersteund? {#authorization-phase-faq10}

Het middel is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) documentatie wordt bepaald.

De bron is een unieke id die is overeengekomen met MVPD&#39;s en die is gekoppeld aan inhoud die de clienttoepassing kan streamen.

De unieke resource-id kan twee indelingen hebben:

* Een eenvoudige tekenreeksindeling, zoals een unieke id voor een kanaal (merk).
* Een media RSS-indeling (MRSS) met aanvullende informatie, zoals de titel, classificaties en metagegevens voor ouderlijk toezicht.

Voor meer details, verwijs naar de [ Beschermde documentatie van Middelen ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 11. Voor hoeveel middelen kan de clienttoepassing tegelijkertijd een vergunningsbesluit verkrijgen? {#authorization-phase-faq11}

De cliënttoepassing kan een vergunningsbesluit voor een beperkt aantal middelen in één enkel API verzoek, gewoonlijk tot 1, wegens voorwaarden verkrijgen die door MVPDs worden opgelegd.

+++

### Veelgestelde vragen over de afmeldingsfase {#logout-phase-faqs-general}

+++Veelgestelde vragen over de afmeldingsfase

#### 1. Wat is het doel van de afmeldingsfase? {#logout-phase-faq1}

Het doel van de afmeldingsfase is om de clienttoepassing de mogelijkheid te bieden het geverifieerde profiel van de gebruiker binnen de Adobe Pass-verificatie op verzoek te beëindigen.

#### 2. Is de afmeldingsfase verplicht? {#logout-phase-faq2}

De afmeldingsfase is verplicht. De clienttoepassing moet de gebruiker de mogelijkheid bieden zich af te melden.

+++

### Veelgestelde vragen over kopteksten {#headers-faqs-general}

+++Veelgestelde vragen over kopteksten

#### 1. Hoe kan de waarde voor de machtigingsheader worden berekend? {#headers-faq1}

>[!IMPORTANT]
>
> Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de waarde van het toegangstoken van `Bearer` te verkrijgen als voorheen.

De ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) verzoekkopbal van de Vergunning 0} {bevat het `Bearer` toegangstoken dat door de cliënttoepassing wordt vereist om tot Adobe Pass beschermde APIs toegang te hebben.[

De headerwaarde voor autorisatie moet worden verkregen bij Adobe Pass-verificatie tijdens de registratiefase.

Raadpleeg de volgende documenten voor meer informatie:

* [Overzicht van dynamische clientregistratie](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Client credentials-API ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [API voor toegangstoken ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamic Client Registration Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### 2. Hoe kan ik de waarde voor de kop van de AP-Device-Identifier berekenen? {#headers-faq2}

>[!IMPORTANT]
>
> Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de waarde van de apparaat-id te berekenen zoals voorheen.

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) verzoekkopbal bevat het stromen apparatenherkenningsteken aangezien het door de cliënttoepassing werd gecreeerd.

De [ AP-Apparaat-Herkenningsteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) kopbaldocumentatie verstrekt voorbeelden voor belangrijke platforms van hoe te om de waarde gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

#### 3. Hoe kan ik de waarde voor de X-Device-Info header berekenen? {#headers-faq3}

>[!IMPORTANT]
>
> Als de clienttoepassing migreert van REST API V1 naar REST API V2, kan de clienttoepassing dezelfde methode blijven gebruiken om de waarde van de apparaatinformatie te berekenen als voorheen.

De [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) verzoekkopbal bevat de cliëntinformatie (apparaat, verbinding en toepassing) met betrekking tot het daadwerkelijke het stromen apparaat.

De [ x-apparaat-Info ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) kopbaldocumentatie verstrekt voorbeelden voor belangrijke platforms van hoe te om de waarde gegevens te verwerken, maar de cliënttoepassing kan verkiezen om een verschillende methode te gebruiken die op zijn eigen bedrijfslogica en vereisten wordt gebaseerd.

+++

### Diverse veelgestelde vragen {#misc-faqs-general}

+++Veelgestelde vragen

#### 1. Kan ik de REST API V2-verzoeken en -antwoorden verkennen en de API testen? {#misc-faq1}

Ja.

U kunt REST API V2 door onze specifieke [ Adobe Developer ](https://developer.adobe.com/adobe-pass/) website onderzoeken. De Adobe Developer-website biedt onbeperkte toegang tot:

* [ DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [ REST API V2 ](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Om met [ REST API V2 ](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) in wisselwerking te staan, moet u de [ 3} kopbal van de Vergunning {met a `Bearer` toegangstoken omvatten dat via [ wordt verkregen DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)

Voor het gebruiken van [ DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), wordt een softwareverklaring met REST API V2 werkingsgebied vereist. Voor meer details, verwijs naar het [ Dynamische document van Veelgestelde vragen van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### 2. Kan ik de REST API V2-verzoeken en -antwoorden verkennen met behulp van een API-ontwikkelingsprogramma met OpenAPI-ondersteuning? {#misc-faq2}

Ja.

U kunt OpenAPI specificatiedossiers voor [ DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) en [ REST API V2 ](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) van de [ Adobe Developer ](https://developer.adobe.com/adobe-pass/) website downloaden.

Als u de specificatiebestanden van OpenAPI wilt downloaden, klikt u op de downloadknoppen om de volgende bestanden op te slaan op uw lokale computer:

* [ DCR API JSON ](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [ REST API V2 JSON ](https://developer.adobe.com/adobe-pass/restApiV2.json)

U kunt deze bestanden vervolgens importeren in uw voorkeursprogramma voor API-ontwikkeling om de REST API V2-verzoeken en -antwoorden te verkennen en de API te testen.

#### 3. Kan ik nog steeds het bestaande API-testprogramma gebruiken dat zich bevindt op https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

Nee.

De clienttoepassingen die migreren naar REST API V2 moeten het nieuwe testprogramma gebruiken dat zich bevindt op https://developer.adobe.com/adobe-pass/. De Adobe Developer-website biedt onbeperkte toegang tot:

* [ DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [ REST API V2 ](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Om met [ REST API V2 ](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) in wisselwerking te staan, moet u de [ 3} kopbal van de Vergunning {met a `Bearer` toegangstoken omvatten dat via [ wordt verkregen DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)

Voor het gebruiken van [ DCR API ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), wordt een softwareverklaring met REST API V2 werkingsgebied vereist. Voor meer details, verwijs naar het [ Dynamische document van Veelgestelde vragen van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

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

De clienttoepassingen die migreren naar REST API V2 profiteren standaard automatisch van deze functie, waardoor gedetailleerdere en nauwkeurige foutinformatie wordt geboden.

Voor meer details, verwijs naar de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### 5. Vergen bestaande integraties configuratiewijzigingen wanneer wordt gemigreerd naar REST API V2? {#migration-faq5}

Nee.

De clienttoepassingen die migreren naar REST API V2, vereisen geen configuratiewijzigingen voor bestaande MVPD-integratie. Ook, zullen zij de zelfde herkenningstekens voor bestaande [ dienstverleners ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) en [ blijven gebruiken MVPDs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd).

Bovendien, blijven de protocollen die door de Authentificatie van Adobe Pass worden gebruikt om met eindpunten van MVPD te communiceren onveranderd.

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
