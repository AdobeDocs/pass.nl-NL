---
title: Woordenlijst REST API V2
description: Woordenlijst REST API V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 0%

---

# Woordenlijst REST API V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document verstrekt definities voor termijnen die worden gebruikt wanneer het integreren van de V2 documentatie van de Authentificatie van Adobe Pass REST API en dienst als opheffing aan onze erfenis [ Verklarende woordenlijst ](/help/authentication/glossary.md).

## Verklarende termen {#glossary-terms}

### A {#a}

#### Toegangstoken {#access-token}

Het toegangstoken is een teken dat door de Authentificatie van Adobe Pass als resultaat van de [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) wordt geproduceerd proces dat wordt bedoeld om toegang tot beschermde APIs te verzekeren.

#### Verificatie {#authentication}

De authentificatie is een proces dat een gebruiker toestaat om hun identiteit aan a [ Programmer ](#programmer) te bewijzen, om toegang tot beschermde inhoud ([ middel ](#resource)) te krijgen, na het bevestigen van het gebruikersabonnement met [ MVPD ](#mvpd).

#### Verificatiecode {#code}

De authentificatiecode is een concept van de Authentificatie van Adobe Pass dat een unieke waarde opslaat die wordt geproduceerd wanneer een gebruiker het [ authentificatie ](#authentication) proces in werking stelt, en uniek de 2} authentificatiesessie ](#session) van een gebruiker identificeert tot het authentificatieproces volledig is.[

De authentificatiecode kan door zowel a [ Primaire (Programmer) Toepassing ](#primary-application) of de Secundaire (Programmer) Toepassing van de a [ worden gebruikt ](#secondary-application) om het [ authentificatie ](#authentication) proces te voltooien, informatie over de [ authentificatiesessie ](#session) terug te winnen, of tot het gebruiker [ profiel ](#profile) toegang te hebben.

Gelijk aan de voormalige term gebruikte registratiecode.

#### Verificatiesessie {#session}

De authentificatiesessie is een concept van de Authentificatie van Adobe Pass dat informatie over het begonnen (of voortgezette) authentificatieproces van de gebruiker van de toepassing van de a [ Programmer ](#programmer) opslaat, en uniek geïdentificeerd door een [ authentificatiecode ](#code).

De authentificatiesessie kan op de ](#programmer) toepassing van de 0} Programmer {ook wijzen om met het [ toestemmings ](#authorization) proces als volgende fase van de [ recht ](#entitlement) stroom te werk te gaan voor het geval de gebruiker reeds voor authentiek verklaard wordt.[

#### Toestemming {#authorization}

De vergunning is een proces dat een gebruiker toestaat om tot beschermde inhoud ([ middel ](#resource)) van de catalogus van de a [ Programmer ](#programmer) toegang te hebben die op het bezeten [ wordt gebaseerd MVPD ](#mvpd), na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd).

### C {#c}

#### Client Credentials {#client-credentials}

De cliëntgeloofsbrieven zijn een reeks unieke waarden die tijdens het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces worden geproduceerd en moeten worden gebruikt om een [ toegangstoken ](#access-token) te verkrijgen.

#### Configuratie {#configuration}

De configuratie is een concept van de Authentificatie van Adobe Pass dat informatie over de [ Programmer ](#programmer) en [ 3} integratiemontages MVPD {opslaat en tijdens het [ authentificatie ](#authentication) proces kan worden gebruikt wanneer het vragen van de gebruiker om hun [ Tv- Leverancier ](#tv-provider) van een lijst van actieve integratie te selecteren.](#mvpd)

#### Aangepast schema {#custom-scheme}

De douaneregeling is een unieke waarde die a [ van de Programmer ](#programmer) van verwijzingen voorziet toepassing die van het Dashboard van Adobe Pass [ kan worden geproduceerd en worden gedownload TVE ](#tve-dashboard) en bedoeld om als definitieve omleiding in toepassingen te worden gebruikt die op de apparaten van iOS lopen.

### D {#d}

#### DCR {#dcr}

De Dynamische Registratie van de Cliënt (DCR) is een vergunningsmechanisme dat door [ wordt bepaald RFC 7591 ](https://datatracker.ietf.org/doc/html/rfc7591), en het is gebaseerd op het OAuth 2.0 vergunningskader dat door [ RFC 6749 ](https://datatracker.ietf.org/doc/html/rfc6749) wordt beschreven.

DCR wordt geleverd aan a [ Programmer ](#programmer) als dienst van de Authentificatie van Adobe Pass die toegang tot beschermde APIs verder kan toelaten.

Voor meer informatie, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/dcr-api/dynamic-client-registration-overview.md) documentatie.

#### Besluit {#decision}

Het besluit is een concept van de Authentificatie van Adobe Pass dat informatie over de ](#mvpd) [ vergunning ](#authorization) of [ prepermission ](#preauthorization) procesonderzoek opslaat om gebruikerstoegang tot a [ te verlenen of te ontkennen programmeur ](#programmer) beschermde inhoud.[

#### Afbraak {#degradation}

De degradatie is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud zelfs toegang te hebben wanneer zijn [ MVPD ](#mvpd) een de dienstverstoring ervaart.

Voor meer informatie, verwijs naar het [ Verslechterings API Overzicht ](/help/authentication/degradation-api-overview.md) documentatie.

### E {#e}

#### Entitlement {#entitlement}

De bewering is een concept van de Authentificatie van Adobe Pass dat de beschikbare stromen en eigenschappen opneemt die een gebruiker door verschillende fasen, om tot beschermde inhoud toegang te hebben gaan, die zich van [ authentificatie ](#authentication), [ pre-autorisatie ](#preauthorization), [ vergunning ](#authorization), en tenslotte [ logout ](#logout) uitstrekken.

#### Verbeterde foutcode {#enhanced-error-code}

De uitgebreide foutcode is een Adobe Pass-verificatieconcept dat aanvullende informatie biedt over de fout die is opgetreden tijdens het verwerken van een aanvraag.

Voor meer informatie, verwijs naar de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

De op huis-Gebaseerde Authentificatie (HBA) is het proces waarbij een consument automatisch toegang tot [ TV overal (TVE) ](#tve) inhoud op uitgezochte apparaten wordt verleend die met hun huisnetwerk worden verbonden, dat deel van de plaats binnen het abonnementscontract uitmaakt.

### I {#i}

#### Identiteitsprovider {#identity-provider}

De identiteitsleverancier is een bedrijf dat de identiteitsdiensten aan consumenten over kabel, satelliet, of op Internet-Gebaseerde diensten in de context van [ TV overal (TVE) ](#tve) verleent.

Synoniem met [ MVPD ](#mvpd) en [ Tv- Leverancier ](#tv-provider).

### L {#l}

#### Afmelden {#logout}

Logout is een proces dat een gebruiker toestaat om hun voor authentiek verklaarde [ profiel ](#profile) binnen de Authentificatie van Adobe Pass te eindigen en de [ programmeur ](#programmer) toepassing bij te werken om op de status van de gebruiker te wijzen.

### M {#m}

#### Mediumtoken {#media-token}

Het media teken is een teken dat door de Authentificatie van Adobe Pass als resultaat van een vergunning [ besluit ](#decision) wordt geproduceerd die toegang tot beschermde inhoud moet verlenen.

Het media teken wordt overgegaan tot [ Programmer ](#programmer), die het dan bevestigt om de veiligheid van toegang voor dat [ middel ](#resource) te verzekeren.

Gelijk aan de vroegere termijn gebruikte korte toestemmingstoken.

#### Verificator mediatokens {#media-token-verifier}

De Symbolische Verifier van Media is een bibliotheek die door de Authentificatie van Adobe Pass wordt verdeeld die voor het verifiëren van de authenticiteit van a [ media teken ](#media-token) verantwoordelijk is.

Voor meer informatie, verwijs naar [ Integrerend de Symbolische documentatie van de Verificateur van Media ](/help/authentication/media-token-verifier-int.md).

#### MVPD {#mvpd}

De multichannel video programming distributor (MVPD) is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internetdiensten.

MVPD wordt geïdentificeerd door een unieke waarde die tijdens het aan boord nemen proces tussen MVPD en Adobe wordt bepaald.

Synoniem met [ Tv- Leverancier ](#tv-provider) en [ Leverancier van de Identiteit ](#identity-provider).

### P {#p}

#### Partner {#partner}

De partner is een bedrijf dat de dienst of een kader aan a [ Programmer ](#programmer) verleent om één enkele sign-on gebruikerservaring toe te laten.

De partner wordt geïdentificeerd door een unieke waarde (bijvoorbeeld &quot;appel&quot;) die tijdens het aan boord nemen proces tussen de partner en de Adobe wordt bepaald.

#### Voorafgaande goedkeuring {#preauthorization}

De voorafgaande toestemming is een proces dat een gebruiker toestaat om een ondergroep van [ middelen ](#resource) van de catalogus van de a [ Programmer ](#programmer) voor te vertonen zij zouden gerechtigd zijn om tot toegang te hebben, na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd).

Gelijk aan [ Preflight ](#preflight).

#### Preflight {#preflight}

Preflight is een proces dat een gebruiker toestaat om een ondergroep van [ middelen ](#resource) van de catalogus van de a [ Programmer ](#programmer) voor te vertonen zij zouden gerechtigd zijn om, na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd) toegang te hebben.

Synoniem met [ preAuthoring ](#preauthorization).

#### Primaire toepassing (programmeur) {#primary-application}

De primaire toepassing verwijst naar de toepassing van de a [ Programmer ](#programmer) die [ authentificatie ](#authentication) in werking stelt, maar die niet het proces kan voltooien door a [ gebruikersagent ](#user-agent) te gebruiken om aan de [ MVPD ](#mvpd) login pagina te navigeren.

#### Profiel {#profile}

Het profiel is een concept van de Authentificatie van Adobe Pass dat informatie over de datum en einddatum van het de authentificatiebegin van de gebruiker, de [ meta-gegevens van de gebruiker ](#user-metadata) samen met andere gebieden opslaat die op de methode wijzen om de authentificatie (b.v. &quot;regelmatig&quot;, &quot;gedegradeerd&quot;, &quot;tijdelijk&quot;, &quot;enig teken-op&quot;, enz.) te verkrijgen.

Gelijk aan vroegere termijn gebruikte authentificatietoken.

#### Programmeur {#programmer}

De programmeur is een bedrijf dat via eigen kanalen (merken) op verschillende platforms inhoud aan consumenten levert.

De programmeur groepeert veelvoudige bezeten kanalen (merken) als [ dienstverleners ](#service-provider) in zijn integratie met de Authentificatie van Adobe Pass.

#### Proxy MVPD {#proxy-mvpd}

De volmacht MVPD is een bedrijf dat de identiteitsdiensten voor andere MVPDs verleent en direct met de Authentificatie van Adobe Pass integreert.

#### Proxied MVPD {#proxied-mvpd}

Proxied MVPD is een bedrijf dat geen directe integratie met de Authentificatie van Adobe Pass heeft, maar wordt geïntegreerd door a [ volmacht MVPD ](#proxy-mvpd).

#### Platformidentiteit {#platform-identity}

De platformidentiteit is een unieke lading van het platformherkenningsteken die door de dienst of het kader (bibliotheek) verbindend aan het apparaat van de gebruiker wordt geproduceerd en aan de [ Programmer ](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [ Enige sign-on gebruikend de stromen van de platformidentiteit ](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

### R {#r}

#### Geregistreerde toepassing {#registered-application}

De geregistreerde toepassing is een concept van de Authentificatie van Adobe Pass dat informatie over de [ toepassing van de Programmer ](#programmer) opslaat die met het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces moet te werk gaan.

#### Bron {#resource}

Het middel is een beschermde inhoud die een gebruiker probeert om tot van a [ de catalogus van de Programmer ](#programmer) toegang te hebben.

De bron wordt geïdentificeerd door een unieke waarde die is overeengekomen tussen de programmeur en de MVPD.

Voor meer informatie, verwijs naar [ het identificeren van Beschermde Middelen ](/help/authentication/identify-protected-resources.md) documentatie.

### S {#s}

#### SAML {#saml}

De Taal van de Prijsverhoging van de Bevestiging van de Veiligheid (SAML) is een open norm voor het ruilen van authentificatie en vergunningsgegevens tussen partijen, in het bijzonder, tussen een [ identiteitsleverancier ](#identity-provider) en a [ dienstverlener ](#sp).

#### Secundaire toepassing (programmeur) {#secondary-application}

De secundaire toepassing verwijst naar de toepassing van de a [ Programmer ](#programmer) die het [ authentificatie ](#authentication) proces kan voltooien door a [ gebruikersagent ](#user-agent) te gebruiken om aan de [ MVPD ](#mvpd) login pagina te navigeren.

De secundaire toepassing kan worden uitgevoerd op hetzelfde apparaat als de primaire toepassing of op een ander (secundair) apparaat. In dat geval wordt de aanmeldingservaring een &#39;tweede schermverificatie&#39;-gebruikerservaring genoemd.

#### Servicetoken {#service-token}

Het de dienstteken is een uniek gebruikersherkenningsteken dat door de dienst of een kader (bibliotheek) verbindend aan de gebruiker wordt geproduceerd en aan de [ Programmer ](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [ Enige sign-on het gebruiken van de stromen van het de dienstteken ](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) documentatie.

#### Serviceprovider {#service-provider}

De dienstverlener is een kanaal (merk) dat door a [ Programmer ](#programmer) wordt bezeten.

De dienstverlener wordt geïdentificeerd door een unieke waarde die tijdens het aan boord nemen proces tussen de Programmer en de Adobe wordt bepaald.

Synoniem met vroegere gebruikte termijn [ aanvrager identiteitskaart ](/help/authentication/glossary.md#requestor-id).

#### Softwareinstructie {#software-statement}

De softwareverklaring is een Symbolisch van het Web JSON (JWT) dat van het Dashboard van Adobe Pass [ kan worden gedownload TVE ](#tve-dashboard) en moet als deel van het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces worden gebruikt.

#### SLO {#slo}

Enige logout (SLO) is een proces dat een gebruiker toestaat om uit alle toepassingen af te melden die deel uitmaakten van [ enig teken-op (SSO) ](#sso).

#### SP {#sp}

De dienstverlener (SP) verwijst naar de rol die door de Authentificatie van Adobe Pass namens a [ Programmer ](#programmer) in een integratie met een [ MVPD ](#mvpd) wordt gespeeld.

#### SSO {#sso}

Enige sign-on (SSO) is een proces dat een gebruiker toestaat om eens voor authentiek te verklaren en toegang te krijgen tot veelvoudige ](#programmer) toepassingen van de Programmer [ zonder de behoefte om voor elk van hen voor authentiek te verklaren.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Basis TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een beperkte tijd toegang te hebben zonder de behoefte om met een [ MVPD ](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de ](/help/authentication/temp-pass.md) documentatie van de Pas van 0} Temperatuur {.[

#### Promotie voor TempPass {#temp-pass-promotional}

De promotionele TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een maximumaantal middelen en een beperkte tijd toegang te hebben zonder de behoefte om met een [ MVPD ](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de ](/help/authentication/promotional-temp-pass.md) documentatie van de Pas van de Bevordering van Temperatuur [.

#### TTL {#ttl}

De tijd om te leven (TTL) is een waarde die op de hoeveelheid tijd wijst die een onderliggende entiteit geldig voor is.

TTL kan voor een [ toegangstoken ](#access-token), a [ profiel ](#profile), een toestemmings [ besluit ](#decision), of a [ media teken ](#media-token) worden vermeld.

#### TVE {#tve}

De TVE (TV Anywhere) is een industriesecche die consumenten toegang biedt tot hun favoriete tv-programma&#39;s, films en andere inhoud op meerdere apparaten, zoals smartphones, tablets, laptops en nog veel meer.

#### TVE-dashboard {#tve-dashboard}

Het dashboard van TV overal (TVE) is een hulpmiddel van de Authentificatie van Adobe Pass dat aan [ Programmeurs ](#programmer) wordt verstrekt om hun configuratie en gegevens te beheren.

Voor meer informatie, verwijs naar de ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md) documentatie van de Gids van de Gebruiker van het Dashboard van 0} TVE.[

#### TV-provider {#tv-provider}

De tv-provider is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internet.

De tv-provider wordt geïdentificeerd door een unieke waarde die tijdens het instapproces tussen de tv-provider en de Adobe wordt gedefinieerd.

Synoniem met [ MVPD ](#mvpd) en [ Leverancier van de Identiteit ](#identity-provider).

### U {#u}

#### Gebruikersagent {#user-agent}

De gebruikersagent verwijst naar browser of gelijkaardige component (platform specifiek) geschikt om het Web te navigeren en de [ MVPD ](#mvpd) login pagina terug te geven.

#### Metagegevens gebruiker {#user-metadata}

De gebruikersmeta-gegevens verwijst naar gebruiker specifieke attributen (b.v., postcodes, ouderlijke classificaties, gebruiker IDs, enz.) die door [ MVPD ](#mvpd) worden gehandhaafd en door de Authentificatie van Adobe Pass als deel van a [ profiel ](#profile) worden verstrekt.

Voor meer informatie, verwijs naar de ](/help/authentication/user-metadata-feature.md) documentatie van Meta-gegevens van de Gebruiker 0}.[

### V {#v}

#### VSA {#vsa}

De VideoRekening van de Abonnee (VSA) is een Apple ontwikkeld kader dat aan [ wordt verstrekt Programmer ](#programmer) om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) en [ Enige sign-on gebruikend de documentatie van de partnerstromen ](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
