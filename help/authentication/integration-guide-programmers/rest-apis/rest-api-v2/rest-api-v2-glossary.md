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
> * [&#x200B; Dynamische Verklarende woordenlijst van de Registratie van de Cliënt (DCR) &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Verklarende termen {#glossary-terms}

### A {#a}

#### Verificatie {#authentication}

De authentificatie is een proces dat een gebruiker toestaat om hun identiteit aan a [&#x200B; Programmer &#x200B;](#programmer) te bewijzen, om toegang tot beschermde inhoud ([&#x200B; middel &#x200B;](#resource)) te krijgen, na het bevestigen van het gebruikersabonnement met [&#x200B; MVPD &#x200B;](#mvpd).

#### Verificatiecode {#code}

De authentificatiecode is een concept van de Authentificatie van Adobe Pass dat een unieke waarde opslaat die wordt geproduceerd wanneer een gebruiker het [&#x200B; authentificatie &#x200B;](#authentication) proces in werking stelt, en uniek de 2&rbrace; authentificatiesessie [&#128279;](#session) van een gebruiker identificeert tot het authentificatieproces volledig is.

De authentificatiecode kan door zowel a [&#x200B; Primaire (Programmer) Toepassing &#x200B;](#primary-application) of de Secundaire (Programmer) Toepassing van de a [&#x200B; worden gebruikt &#x200B;](#secondary-application) om het [&#x200B; authentificatie &#x200B;](#authentication) proces te voltooien, informatie over de [&#x200B; authentificatiesessie &#x200B;](#session) terug te winnen, of tot het gebruiker [&#x200B; profiel &#x200B;](#profile) toegang te hebben.

Gelijk aan de voormalige term gebruikte registratiecode.

#### Verificatiesessie {#session}

De authentificatiesessie is een concept van de Authentificatie van Adobe Pass dat informatie over het begonnen (of voortgezette) authentificatieproces van de gebruiker van de toepassing van de a [&#x200B; Programmer &#x200B;](#programmer) opslaat, en uniek geïdentificeerd door een [&#x200B; authentificatiecode &#x200B;](#code).

De authentificatiesessie kan op de [&#128279;](#programmer) toepassing van de 0&rbrace; Programmer &lbrace;ook wijzen om met het [&#x200B; toestemmings &#x200B;](#authorization) proces als volgende fase van de [&#x200B; recht &#x200B;](#entitlement) stroom te werk te gaan voor het geval de gebruiker reeds voor authentiek verklaard wordt.

#### Toestemming {#authorization}

De vergunning is een proces dat een gebruiker toestaat om tot beschermde inhoud ([&#x200B; middel &#x200B;](#resource)) van de catalogus van de a [&#x200B; Programmer &#x200B;](#programmer) toegang te hebben die op het bezeten [&#x200B; MVPD &#x200B;](#mvpd) abonnement wordt gebaseerd, na het bevestigen van de gebruikersrechten met [&#x200B; MVPD &#x200B;](#mvpd).

### C {#c}

#### Configuratie {#configuration}

De configuratie is een concept van de Authentificatie van Adobe Pass dat informatie over de [&#x200B; Programmer &#x200B;](#programmer) en [&#x200B; de integratiemontages van MVPD &#x200B;](#mvpd) opslaat en tijdens het [&#x200B; authentificatie &#x200B;](#authentication) proces kan worden gebruikt wanneer het vragen van de gebruiker om hun [&#x200B; Tv- Leverancier &#x200B;](#tv-provider) van een lijst van actieve integratie te selecteren.

### D {#d}

#### Besluit {#decision}

Het besluit is een concept van de Authentificatie van Adobe Pass dat informatie over de [&#128279;](#mvpd) [&#x200B; vergunning &#x200B;](#authorization) of [&#x200B; prepermission &#x200B;](#preauthorization) procesonderzoek opslaat om gebruikerstoegang tot a [&#x200B; te verlenen of te ontkennen programmeur &#x200B;](#programmer) beschermde inhoud.

#### Afbraak {#degradation}

De degradatie is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud zelfs toegang te hebben wanneer zijn [&#x200B; MVPD &#x200B;](#mvpd) een de dienstverstoring ervaart.

Voor meer informatie, verwijs naar de [&#128279;](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) documentatie van de Eigenschap van 0&rbrace; Afbraak.

#### Apparaat-id {#device-id}

Apparaatidentiteitskaart is een uniek herkenningsteken gebonden aan het apparaat van de gebruiker en moet door de [&#128279;](#programmer) 1&rbrace; toepassing van de Programmer &lbrace;op alle fasen van de [&#x200B; betitelings &#x200B;](#entitlement) stroom worden verstrekt.

### E {#e}

#### Entitlement {#entitlement}

De bewering is een concept van de Authentificatie van Adobe Pass dat de beschikbare stromen en eigenschappen opneemt die een gebruiker door verschillende fasen gaan, om tot beschermde inhoud toegang te hebben, die zich van [&#x200B; authentificatie &#x200B;](#authentication), [&#x200B; pre-autorisatie &#x200B;](#preauthorization), [&#x200B; vergunning &#x200B;](#authorization), en tenslotte [&#x200B; logout &#x200B;](#logout) uitstrekken.

#### Verbeterde foutcode {#enhanced-error-code}

De uitgebreide foutcode is een Adobe Pass-verificatieconcept dat aanvullende informatie biedt over de fout die is opgetreden tijdens het verwerken van een aanvraag.

Voor meer informatie, verwijs naar de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

De op huis-Gebaseerde Authentificatie (HBA) is het proces waarbij een consument automatisch toegang tot [&#x200B; TV overal (TVE) &#x200B;](#tve) inhoud op uitgezochte apparaten wordt verleend die met hun huisnetwerk worden verbonden, dat deel van de plaats binnen het abonnementscontract uitmaakt.

### I {#i}

#### Identiteitsprovider {#identity-provider}

De identiteitsleverancier is een bedrijf dat de identiteitsdiensten aan consumenten over kabel, satelliet, of op Internet-Gebaseerde diensten in de context van [&#x200B; TV overal (TVE) &#x200B;](#tve) verleent.

Synoniem met [&#x200B; MVPD &#x200B;](#mvpd) en [&#x200B; Tv- Leverancier &#x200B;](#tv-provider).

### L {#l}

#### Afmelden {#logout}

Logout is een proces dat een gebruiker toestaat om hun voor authentiek verklaarde [&#x200B; profiel &#x200B;](#profile) binnen de Authentificatie van Adobe Pass te eindigen en de [&#x200B; programmeur &#x200B;](#programmer) toepassing bij te werken om op de status van de gebruiker te wijzen.

### M {#m}

#### Mediumtoken {#media-token}

Het media teken is een teken dat door de Authentificatie van Adobe Pass als resultaat van een vergunning [&#x200B; wordt geproduceerd besluit &#x200B;](#decision) wordt bedoeld om toegang tot beschermde inhoud te verlenen die.

Het media teken wordt overgegaan tot [&#x200B; Programmer &#x200B;](#programmer), die het dan bevestigt om de veiligheid van toegang voor dat [&#x200B; middel &#x200B;](#resource) te verzekeren.

Gelijk aan de vroegere termijn gebruikte korte toestemmingstoken.

#### Verificator mediatokens {#media-token-verifier}

De Symbolische Verifier van Media is een bibliotheek die door de Authentificatie van Adobe Pass wordt verdeeld die voor het verifiëren van de authenticiteit van a [&#x200B; media teken &#x200B;](#media-token) verantwoordelijk is.

Voor meer informatie, verwijs naar de [&#x200B; Symbolische documentatie van de Verificateur van Media &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

De multichannel video programming distributor (MVPD) is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internetdiensten.

De MVPD wordt geïdentificeerd door een unieke waarde die tijdens het instapproces tussen de MVPD en de Adobe is gedefinieerd.

Synoniem met [&#x200B; Tv- Leverancier &#x200B;](#tv-provider) en [&#x200B; Leverancier van de Identiteit &#x200B;](#identity-provider).

### P {#p}

#### Partner {#partner}

De partner is een bedrijf dat de dienst of een kader aan a [&#x200B; Programmer &#x200B;](#programmer) verleent om één enkele sign-on gebruikerservaring toe te laten.

De partner wordt geïdentificeerd door een unieke waarde (bijvoorbeeld &quot;appel&quot;) die tijdens het aan boord nemen proces tussen de partner en de Adobe wordt bepaald.

#### Voorafgaande goedkeuring {#preauthorization}

De voorafgaande toestemming is een proces dat een gebruiker toestaat om een ondergroep van [&#x200B; middelen &#x200B;](#resource) van de catalogus van de a [&#x200B; Programmer &#x200B;](#programmer) voor te vertonen zij zouden gerechtigd zijn om, na het bevestigen van de gebruikersrechten met [&#x200B; MVPD &#x200B;](#mvpd) toegang te hebben.

Gelijk aan [&#x200B; Preflight &#x200B;](#preflight).

#### Preflight {#preflight}

Preflight is een proces dat een gebruiker toestaat om een ondergroep van [&#x200B; middelen &#x200B;](#resource) van de catalogus van de a [&#x200B; Programmer &#x200B;](#programmer) voor te vertonen zij zouden gerechtigd zijn om, na het bevestigen van de gebruikersrechten met [&#x200B; MVPD &#x200B;](#mvpd) toegang te hebben.

Synoniem met [&#x200B; preAuthoring &#x200B;](#preauthorization).

#### Primaire toepassing (programmeur) {#primary-application}

De primaire toepassing verwijst naar de toepassing van de a [&#x200B; Programmer &#x200B;](#programmer) die [&#x200B; authentificatie &#x200B;](#authentication) in werking stelt, maar die niet het proces kan voltooien door a [&#x200B; gebruikersagent &#x200B;](#user-agent) te gebruiken om aan de [&#x200B; MVPD &#x200B;](#mvpd) login pagina te navigeren.

#### Profiel {#profile}

Het profiel is een concept van de Authentificatie van Adobe Pass dat informatie over de datum en einddatum van het de authentificatiebegin van de gebruiker, de [&#x200B; meta-gegevens van de gebruiker &#x200B;](#user-metadata) samen met andere gebieden opslaat die op de methode wijzen om de authentificatie (b.v. &quot;regelmatig&quot;, &quot;gedegradeerd&quot;, &quot;tijdelijk&quot;, &quot;enig teken-op&quot;, enz.) te verkrijgen.

Gelijk aan de vroegere termijn gebruikte authentificatietoken.

#### Programmeur {#programmer}

De programmeur is een bedrijf dat via eigen kanalen (merken) op verschillende platforms inhoud aan consumenten levert.

De programmeur groepeert veelvoudige bezeten kanalen (merken) als [&#x200B; dienstverleners &#x200B;](#service-provider) in zijn integratie met de Authentificatie van Adobe Pass.

#### Proxy MVPD {#proxy-mvpd}

De volmacht MVPD is een bedrijf dat de identiteitsdiensten voor andere MVPDs verleent en direct met de Authentificatie van Adobe Pass integreert.

#### Proxied MVPD {#proxied-mvpd}

Proxied MVPD is een bedrijf dat geen directe integratie met de Authentificatie van Adobe Pass heeft, maar wordt geïntegreerd door a [&#x200B; volmacht MVPD &#x200B;](#proxy-mvpd).

#### Platformidentiteit {#platform-identity}

De platformidentiteit is een unieke die lading van het platformherkenningsteken door de dienst of een kader (bibliotheek) wordt geproduceerd verbindend aan het apparaat van de gebruiker en aan de [&#x200B; Programmer &#x200B;](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [&#x200B; Enige sign-on gebruikend de stromen van de platformidentiteit &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

### R {#r}

#### Bron {#resource}

Het middel is een beschermde inhoud die een gebruiker probeert om tot van a [&#x200B; de catalogus van de Programmer &#x200B;](#programmer) toegang te hebben.

De bron wordt geïdentificeerd door een unieke waarde die is overeengekomen tussen de programmeur en de MVPD.

Voor meer informatie, verwijs naar de [&#x200B; Beschermde documentatie van Middelen &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

### S {#s}

#### SAML {#saml}

De Taal van de Prijsverhoging van de Bevestiging van de Veiligheid (SAML) is een open norm voor het ruilen van authentificatie en vergunningsgegevens tussen partijen, in het bijzonder, tussen een [&#x200B; identiteitsleverancier &#x200B;](#identity-provider) en a [&#x200B; dienstverlener &#x200B;](#sp).

#### Secundaire toepassing (programmeur) {#secondary-application}

De secundaire toepassing verwijst naar de toepassing van de a [&#x200B; Programmer &#x200B;](#programmer) die het [&#x200B; authentificatie &#x200B;](#authentication) proces kan voltooien door a [&#x200B; gebruikersagent &#x200B;](#user-agent) te gebruiken om aan de [&#x200B; MVPD &#x200B;](#mvpd) login pagina te navigeren.

De secundaire toepassing kan worden uitgevoerd op hetzelfde apparaat als de primaire toepassing of op een ander (secundair) apparaat. In dat geval wordt de aanmeldingservaring een &#39;tweede schermverificatie&#39;-gebruikerservaring genoemd.

#### Servicetoken {#service-token}

Het de dienstteken is een uniek gebruikersherkenningsteken dat door de dienst of een kader (bibliotheek) wordt geproduceerd verbindend aan de gebruiker en aan de [&#x200B; Programmer &#x200B;](#programmer) wordt verstrekt om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar [&#x200B; Enige sign-on het gebruiken van de stromen van het de dienstteken &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) documentatie.

#### Serviceprovider {#service-provider}

De dienstverlener is een kanaal (merk) dat door a [&#x200B; Programmer &#x200B;](#programmer) wordt bezeten.

De dienstverlener wordt geïdentificeerd door een unieke waarde die tijdens het aan boord nemen proces tussen de Programmer en de Adobe wordt bepaald.

Gelijk aan de voormalige gebruikte term aanvrager-id.

#### SLO {#slo}

Enige logout (SLO) is een proces dat een gebruiker toestaat om uit alle toepassingen af te melden die deel uitmaakten van [&#x200B; enig teken-op (SSO) &#x200B;](#sso).

#### SP {#sp}

De dienstverlener (SP) verwijst naar de rol die door de Authentificatie van Adobe Pass namens a [&#x200B; Programmer &#x200B;](#programmer) in een integratie met a [&#x200B; MVPD &#x200B;](#mvpd) wordt gespeeld.

#### SSO {#sso}

Enige sign-on (SSO) is een proces dat een gebruiker toestaat om eens voor authentiek te verklaren en toegang te krijgen tot veelvoudige [&#128279;](#programmer) toepassingen van de Programmer  zonder de behoefte om voor elk van hen voor authentiek te verklaren.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Basis TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een beperkte tijd toegang te hebben zonder de behoefte om met a [&#x200B; MVPD &#x200B;](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de [&#x200B; BasisTempPass &#x200B;](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass) documentatie.

#### Promotie voor TempPass {#temp-pass-promotional}

Promotional TempPass is een eigenschap van de Authentificatie van Adobe Pass die een gebruiker toestaat om tot beschermde inhoud voor een maximumaantal middelen en een beperkte tijd toegang te hebben zonder de behoefte om met een [&#x200B; MVPD &#x200B;](#mvpd) voor authentiek te verklaren.

Voor meer informatie, verwijs naar de [&#x200B; BevorderingsTempPass &#x200B;](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass) documentatie.

#### TTL {#ttl}

De tijd om te leven (TTL) is een waarde die op de hoeveelheid tijd wijst die een onderliggende entiteit geldig voor is.

TTL kan voor een [&#x200B; toegangstoken &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), a [&#x200B; profiel &#x200B;](#profile), een toestemmings [&#x200B; besluit &#x200B;](#decision), of a [&#x200B; media teken &#x200B;](#media-token) worden vermeld.

#### TVE {#tve}

De TVE (TV Anywhere) is een industriesecche die consumenten toegang biedt tot hun favoriete tv-programma&#39;s, films en andere inhoud op meerdere apparaten, zoals smartphones, tablets, laptops en nog veel meer.

#### TVE-dashboard {#tve-dashboard}

Het dashboard van TV overal (TVE) is een hulpmiddel van de Authentificatie van Adobe Pass dat aan [&#x200B; wordt verstrekt Programmeurs &#x200B;](#programmer) om hun configuratie en gegevens te beheren.

Voor meer informatie, verwijs naar de [&#128279;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) documentatie van de Gids van de Gebruiker van het Dashboard van 0&rbrace; TVE.

#### TV-provider {#tv-provider}

De tv-provider is een bedrijf dat televisiediensten levert aan consumenten via kabel, satelliet of internet.

De tv-provider wordt geïdentificeerd door een unieke waarde die tijdens het instapproces tussen de tv-provider en de Adobe is gedefinieerd.

Synoniem met [&#x200B; MVPD &#x200B;](#mvpd) en [&#x200B; Leverancier van de Identiteit &#x200B;](#identity-provider).

### U {#u}

#### Gebruikersagent {#user-agent}

De gebruikersagent verwijst naar browser of gelijkaardige component (platform-specifiek) geschikt om het Web te navigeren en de [&#x200B; MVPD &#x200B;](#mvpd) login pagina terug te geven.

#### Gebruikersnaam {#user-id}

De gebruiker - identiteitskaart is een uniek herkenningsteken verbindend aan de gebruiker en komt van het [&#x200B; MVPD &#x200B;](#mvpd) authentificatieproces voort.

#### Metagegevens gebruiker {#user-metadata}

De gebruikersmeta-gegevens verwijst naar gebruiker-specifieke attributen (b.v., postcodes, ouderlijke classificaties, gebruiker IDs, enz.) die door [&#x200B; MVPD &#x200B;](#mvpd) worden gehandhaafd en door de Authentificatie van Adobe Pass als deel van a [&#x200B; profiel &#x200B;](#profile) worden verstrekt.

Voor meer informatie, verwijs naar de [&#128279;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) documentatie van Meta-gegevens van de Gebruiker 0&rbrace;.

### V {#v}

#### VSA {#vsa}

De VideoRekening van de Abonnee (VSA) is een Apple ontwikkeld kader dat aan [&#x200B; wordt verstrekt Programmer &#x200B;](#programmer) om één enkele sign-on gebruikerservaring toe te laten.

Voor meer informatie, verwijs naar het [&#x200B; Video Kader van de Rekening van de Abonnee &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) en [&#x200B; Enige sign-on gebruikend de documentatie van de partnerstromen &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
