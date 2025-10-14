---
title: Woordenlijst Dynamische clientregistratie (DCR)
description: Woordenlijst Dynamische clientregistratie (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Woordenlijst Dynamische clientregistratie (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document bevat definities voor termen die worden gebruikt bij het integreren van de DCR (Adobe Pass Authentication Dynamic Client Registration).

>[!MORELIKETHIS]
> 
> * [&#x200B; REST API v2 Verklarende woordenlijst &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Verklarende termen {#glossary-terms}

### A {#a}

#### Toegangstoken {#access-token}

Het toegangstoken is een teken dat door de Authentificatie van Adobe Pass als resultaat van de [&#x200B; Dynamische Registratie van de Cliënt (DCR) wordt geproduceerd &#x200B;](#dcr) proces dat wordt bedoeld om toegang tot beschermde APIs te verzekeren.

### C {#c}

#### Client Credentials {#client-credentials}

De cliëntgeloofsbrieven zijn een reeks unieke waarden die tijdens het [&#x200B; Dynamische Registratie van de Cliënt (DCR) &#x200B;](#dcr) proces worden geproduceerd en moeten worden gebruikt om een [&#x200B; toegangstoken &#x200B;](#access-token) te verkrijgen.

#### Aangepast schema {#custom-scheme}

De douaneregeling is een unieke waarde die a [&#x200B; van de Programmer &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) van verwijzingen voorziet toepassing die van het Dashboard van Adobe Pass [&#x200B; kan worden geproduceerd en worden gedownload TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) en bedoeld om als definitieve omleiding in toepassingen te worden gebruikt die op de apparaten van iOS lopen.

### D {#d}

#### DCR {#dcr}

De Dynamische Registratie van de Cliënt (DCR) is een vergunningsmechanisme dat door [&#x200B; wordt bepaald RFC 7591 &#x200B;](https://datatracker.ietf.org/doc/html/rfc7591), en het is gebaseerd op het OAuth 2.0 vergunningskader dat door [&#x200B; RFC 6749 &#x200B;](https://datatracker.ietf.org/doc/html/rfc6749) wordt beschreven.

DCR wordt geleverd aan a [&#x200B; Programmer &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) als dienst van de Authentificatie van Adobe Pass die toegang tot beschermde APIs verder kan toelaten.

Voor meer informatie, verwijs naar het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

### R {#r}

#### Geregistreerde toepassing {#registered-application}

De geregistreerde toepassing is een concept van de Authentificatie van Adobe Pass dat informatie over de [&#x200B; toepassing van de Programmer &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) opslaat die met het [&#x200B; Dynamische Registratie van de Cliënt (DCR) &#x200B;](#dcr) proces moet te werk gaan.

### S {#s}

#### Softwareinstructie {#software-statement}

De softwareverklaring is een Symbolisch van het Web JSON (JWT) dat van het Dashboard van Adobe Pass [&#x200B; kan worden gedownload TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) en moet als deel van het [&#x200B; Dynamische Registratie van de Cliënt (DCR) &#x200B;](#dcr) proces worden gebruikt.
