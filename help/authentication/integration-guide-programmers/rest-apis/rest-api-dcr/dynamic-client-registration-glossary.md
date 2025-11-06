---
title: Woordenlijst Dynamische clientregistratie (DCR)
description: Woordenlijst Dynamische clientregistratie (DCR)
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
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
> * [ REST API v2 Verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Verklarende termen {#glossary-terms}

### A {#a}

#### Toegangstoken {#access-token}

Het toegangstoken is een teken dat door de Authentificatie van Adobe Pass als resultaat van de [ Dynamische Registratie van de Cliënt (DCR) wordt geproduceerd ](#dcr) proces dat wordt bedoeld om toegang tot beschermde APIs te verzekeren.

### C {#c}

#### Client Credentials {#client-credentials}

De cliëntgeloofsbrieven zijn een reeks unieke waarden die tijdens het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces worden geproduceerd en moeten worden gebruikt om een [ toegangstoken ](#access-token) te verkrijgen.

#### Aangepast schema {#custom-scheme}

De douaneregeling is een unieke waarde die a [ van de Programmer ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) van verwijzingen voorziet toepassing die van het Dashboard van Adobe Pass [ kan worden geproduceerd en worden gedownload TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) en bedoeld om als definitieve omleiding in toepassingen te worden gebruikt die op de apparaten van iOS lopen.

### D {#d}

#### DCR {#dcr}

De Dynamische Registratie van de Cliënt (DCR) is een vergunningsmechanisme dat door [ wordt bepaald RFC 7591 ](https://datatracker.ietf.org/doc/html/rfc7591), en het is gebaseerd op het OAuth 2.0 vergunningskader dat door [ RFC 6749 ](https://datatracker.ietf.org/doc/html/rfc6749) wordt beschreven.

DCR wordt geleverd aan a [ Programmer ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) als dienst van de Authentificatie van Adobe Pass die toegang tot beschermde APIs verder kan toelaten.

Voor meer informatie, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

### R {#r}

#### Geregistreerde toepassing {#registered-application}

De geregistreerde toepassing is een concept van de Authentificatie van Adobe Pass dat informatie over de [ toepassing van de Programmer ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) opslaat die met het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces moet te werk gaan.

### S {#s}

#### Softwareinstructie {#software-statement}

De softwareverklaring is een Symbolisch van het Web JSON (JWT) dat van het Dashboard van Adobe Pass [ kan worden gedownload TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) en moet als deel van het [ Dynamische Registratie van de Cliënt (DCR) ](#dcr) proces worden gebruikt.
