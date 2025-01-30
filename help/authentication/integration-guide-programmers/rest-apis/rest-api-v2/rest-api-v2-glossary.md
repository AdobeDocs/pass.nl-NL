---
title: Woordenlijst REST API V2
description: Woordenlijst REST API V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Woordenlijst REST API V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document bevat definities voor termen die worden gebruikt bij de integratie van de Adobe Pass Authentication REST API V2.

>[!MORELIKETHIS]
>
> * [ Dynamische Verklarende woordenlijst van de Registratie van de Cliënt (DCR) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Verklarende termen {#glossary-terms}

### A {#a}

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

De vergunning is een proces dat een gebruiker toestaat om tot beschermde inhoud ([ middel ](#resource)) van de catalogus van de a [ Programmer ](#programmer) toegang te hebben die op het bezeten [ MVPD ](#mvpd) abonnement wordt gebaseerd, na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd).

### C {#c}

#### Configuratie {#configuration}

De configuratie is een concept van de Authentificatie van Adobe Pass dat informatie over de [ Programmer ](#programmer) en [ de integratiemontages van MVPD ](#mvpd) opslaat en tijdens het [ authentificatie ](#authentication) proces kan worden gebruikt wanneer het vragen van de gebruiker om hun [ Tv- Leverancier ](#tv-provider) van een lijst van actieve integratie te selecteren.

### D {#d}

#### Besluit {#decision}

Het besluit is een concept van de Authentificatie van Adobe Pass dat informatie over de ](#mvpd) [ vergunning ](#authorization) of [ prepermission ](#preauthorization) procesonderzoek opslaat om gebruikerstoegang tot a [ te verlenen of te ontkennen programmeur ](#programmer) beschermde inhoud.[

#### Afbraak {#degradation}

De degradatie is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud zelfs toegang te hebben wanneer zijn [ MVPD ](#mvpd) een de dienstverstoring ervaart.

Voor meer informatie, verwijs naar de ](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) documentatie van de Eigenschap van 0} Afbraak.[

#### Apparaat-id {#device-id}

Apparaatidentiteitskaart is een uniek herkenningsteken gebonden aan het apparaat van de gebruiker en moet door de [ 1} toepassing van de Programmer {op alle fasen van de [ betitelings ](#entitlement) stroom worden verstrekt.](#programmer)

### E {#e}

#### Entitlement {#entitlement}

De bewering is een concept van de Authentificatie van Adobe Pass dat de beschikbare stromen en eigenschappen opneemt die een gebruiker door verschillende fasen gaan, om tot beschermde inhoud toegang te hebben, die zich van [ authentificatie ](#authentication), [ pre-autorisatie ](#preauthorization), [ vergunning ](#authorization), en tenslotte [ logout ](#logout) uitstrekken.

#### Verbeterde foutcode {#enhanced-error-code}

De uitgebreide foutcode is een Adobe Pass-verificatieconcept dat aanvullende informatie biedt over de fout die is opgetreden tijdens het verwerken van een aanvraag.

Voor meer informatie, verwijs naar de [ Verbeterde documentatie van de Codes van de Fout ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

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

Het media teken is een teken dat door de Authentificatie van Adobe Pass als resultaat van een vergunning [ wordt geproduceerd besluit ](#decision) wordt bedoeld om toegang tot beschermde inhoud te verlenen die.

Het media teken wordt overgegaan tot [ Programmer ](#programmer), die het dan bevestigt om de veiligheid van toegang voor dat [ middel ](#resource) te verzekeren.

Gelijk aan de vroegere termijn gebruikte korte toestemmingstoken.

#### Verificator mediatokens {#media-token-verifier}

De Symbolische Verifier van Media is een bibliotheek die door de Authentificatie van Adobe Pass wordt verdeeld die voor het verifiëren van de authenticiteit van a [ media teken ](#media-token) verantwoordelijk is.

Voor meer informatie, verwijs naar de [ Symbolische documentatie van de Verificateur van Media ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

De multichannel video programming distributor (MVPD) is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internetdiensten.

De MVPD wordt geïdentificeerd door een unieke waarde die tijdens het instapproces tussen de MVPD en de Adobe is gedefinieerd.

Synoniem met [ Tv- Leverancier ](#tv-provider) en [ Leverancier van de Identiteit ](#identity-provider).

### P {#p}

#### Partner {#partner}

De partner is een bedrijf dat de dienst of een kader aan a [ Programmer ](#programmer) verleent om één enkele sign-on gebruikerservaring toe te laten.

De partner wordt geïdentificeerd door een unieke waarde (bijvoorbeeld &quot;appel&quot;) die tijdens het aan boord nemen proces tussen de partner en de Adobe wordt bepaald.

#### Voorafgaande goedkeuring {#preauthorization}

De voorafgaande toestemming is een proces dat een gebruiker toestaat om een ondergroep van [ middelen ](#resource) van de catalogus van de a [ Programmer ](#programmer) voor te vertonen zij zouden gerechtigd zijn om, na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd) toegang te hebben.

Gelijk aan [ Preflight ](#preflight).

#### Preflight {#preflight}

Preflight is een proces dat een gebruiker toestaat om een ondergroep van [ middelen ](#resource) van de catalogus van de a [ Programmer ](#programmer) voor te vertonen zij zouden gerechtigd zijn om, na het bevestigen van de gebruikersrechten met [ MVPD ](#mvpd) toegang te hebben.

Synoniem met [ preAuthoring ](#preauthorization).

#### Primaire toepassing (programmeur) {#primary-application}

De primaire toepassing verwijst naar de toepassing van de a [ Programmer ](#programmer) die [ authentificatie ](#authentication) in werking stelt, maar die niet het proces kan voltooien door a [ gebruikersagent ](#user-agent) te gebruiken om aan de [ MVPD ](#mvpd) login pagina te navigeren.

#### Profiel {#profile}

Het profiel is een concept van de Authentificatie van Adobe Pass dat informatie over de datum en einddatum van het de authentificatiebegin van de gebruiker, de [ meta-gegevens van de gebruiker ](#user-metadata) samen met andere gebieden opslaat die op de methode wijzen om de authentificatie (b.v. &quot;regelmatig&quot;, &quot;gedegradeerd&quot;, &quot;tijdelijk&quot;, &quot;enig teken-op&quot;, enz.) te verkrijgen.

Gelijk aan de vroegere termijn gebruikte authentificatietoken.

#### Programmeur {#programmer}

De programmeur is een bedrijf dat via eigen kanalen (merken) op verschillende platforms inhoud aan consumenten levert.

De programmeur groepeert veelvoudige bezeten kanalen (merken) als [ dienstverleners ](#service-provider) in zijn integratie met de Authentificatie van Adobe Pass.

#### Proxy MVPD {#proxy-mvpd}

De volmacht MVPD is een bedrijf dat de identiteitsdiensten voor andere MVPDs verleent en direct met de Authentificatie van Adobe Pass integreert.

#### Proxied MVPD {#proxied-mvpd}

Proxied MVPD is een bedrijf dat geen directe integratie met de Authentificatie van Adobe Pass heeft, maar wordt geïntegreerd door a [ volmacht MVPD ](#proxy-mvpd).

#### Platformidentiteit {#platform-identity}

De platformidentiteit is een unieke die lading van het platformherkenningsteken door de dienst of een kader (bibliotheek) wordt geproduceerd verbindend aan het apparaat van de gebruiker en aan de [ Programmer ](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [ Enige sign-on gebruikend de stromen van de platformidentiteit ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

### R {#r}

#### Bron {#resource}

Het middel is een beschermde inhoud die een gebruiker probeert om tot van a [ de catalogus van de Programmer ](#programmer) toegang te hebben.

De bron wordt geïdentificeerd door een unieke waarde die is overeengekomen tussen de programmeur en de MVPD.

Voor meer informatie, verwijs naar de [ Beschermde documentatie van Middelen ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

### S {#s}

#### SAML {#saml}

De Taal van de Prijsverhoging van de Bevestiging van de Veiligheid (SAML) is een open norm voor het ruilen van authentificatie en vergunningsgegevens tussen partijen, in het bijzonder, tussen een [ identiteitsleverancier ](#identity-provider) en a [ dienstverlener ](#sp).

#### Secundaire toepassing (programmeur) {#secondary-application}

De secundaire toepassing verwijst naar de toepassing van de a [ Programmer ](#programmer) die het [ authentificatie ](#authentication) proces kan voltooien door a [ gebruikersagent ](#user-agent) te gebruiken om aan de [ MVPD ](#mvpd) login pagina te navigeren.

De secundaire toepassing kan worden uitgevoerd op hetzelfde apparaat als de primaire toepassing of op een ander (secundair) apparaat. In dat geval wordt de aanmeldingservaring een &#39;tweede schermverificatie&#39;-gebruikerservaring genoemd.

#### Servicetoken {#service-token}

Het de dienstteken is een uniek gebruikersherkenningsteken dat door de dienst of een kader (bibliotheek) wordt geproduceerd verbindend aan de gebruiker en aan de [ Programmer ](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [ Enige sign-on het gebruiken van de stromen van het de dienstteken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) documentatie.

#### Serviceprovider {#service-provider}

De dienstverlener is een kanaal (merk) dat door a [ Programmer ](#programmer) wordt bezeten.

De dienstverlener wordt geïdentificeerd door een unieke waarde die tijdens het aan boord nemen proces tussen de Programmer en de Adobe wordt bepaald.

Gelijk aan de voormalige gebruikte term aanvrager-id.

#### SLO {#slo}

Enige logout (SLO) is een proces dat een gebruiker toestaat om uit alle toepassingen af te melden die deel uitmaakten van [ enig teken-op (SSO) ](#sso).

#### SP {#sp}

De dienstverlener (SP) verwijst naar de rol die door de Authentificatie van Adobe Pass namens a [ Programmer ](#programmer) in een integratie met a [ MVPD ](#mvpd) wordt gespeeld.

#### SSO {#sso}

Enige sign-on (SSO) is een proces dat een gebruiker toestaat om eens voor authentiek te verklaren en toegang te krijgen tot veelvoudige ](#programmer) toepassingen van de Programmer [ zonder de behoefte om voor elk van hen voor authentiek te verklaren.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Basis TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een beperkte tijd toegang te hebben zonder de behoefte om met a [ MVPD ](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de [ BasisTempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass) documentatie.

#### Promotie voor TempPass {#temp-pass-promotional}

Promotional TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een maximumaantal middelen en een beperkte tijd toegang te hebben zonder de behoefte om met een [ MVPD ](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de [ BevorderingsTempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass) documentatie.

#### TTL {#ttl}

De tijd om te leven (TTL) is een waarde die op de hoeveelheid tijd wijst die een onderliggende entiteit geldig voor is.

TTL kan voor een [ toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), a [ profiel ](#profile), een toestemmings [ besluit ](#decision), of a [ media teken ](#media-token) worden vermeld.

#### TVE {#tve}

De TVE (TV Anywhere) is een industriesecche die consumenten toegang biedt tot hun favoriete tv-programma&#39;s, films en andere inhoud op meerdere apparaten, zoals smartphones, tablets, laptops en nog veel meer.

#### TVE-dashboard {#tve-dashboard}

Het dashboard van TV overal (TVE) is een hulpmiddel van de Authentificatie van Adobe Pass dat aan [ wordt verstrekt Programmeurs ](#programmer) om hun configuratie en gegevens te beheren.

Voor meer informatie, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) documentatie van de Gids van de Gebruiker van het Dashboard van 0} TVE.[

#### TV-provider {#tv-provider}

De tv-provider is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internet.

De tv-provider wordt geïdentificeerd door een unieke waarde die tijdens het instapproces tussen de tv-provider en de Adobe is gedefinieerd.

Synoniem met [ MVPD ](#mvpd) en [ Leverancier van de Identiteit ](#identity-provider).

### U {#u}

#### Gebruikersagent {#user-agent}

De gebruikersagent verwijst naar browser of gelijkaardige component (platform-specifiek) geschikt om het Web te navigeren en de [ MVPD ](#mvpd) login pagina terug te geven.

#### Gebruikersnaam {#user-id}

De gebruiker - identiteitskaart is een uniek herkenningsteken verbindend aan de gebruiker en komt van het [ MVPD ](#mvpd) authentificatieproces voort.

#### Metagegevens gebruiker {#user-metadata}

De gebruikersmeta-gegevens verwijst naar gebruiker-specifieke attributen (b.v., postcodes, ouderlijke classificaties, gebruiker IDs, enz.) die door [ MVPD ](#mvpd) worden gehandhaafd en door de Authentificatie van Adobe Pass als deel van a [ profiel ](#profile) worden verstrekt.

Voor meer informatie, verwijs naar de ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) documentatie van Meta-gegevens van de Gebruiker 0}.[

### V {#v}

#### VSA {#vsa}

De VideoRekening van de Abonnee (VSA) is een Apple ontwikkeld kader dat aan [ wordt verstrekt Programmer ](#programmer) om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar het [ Video Kader van de Rekening van de Abonnee ](https://developer.apple.com/documentation/videosubscriberaccount) en [ Enige sign-on gebruikend de documentatie van de partnerstromen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
