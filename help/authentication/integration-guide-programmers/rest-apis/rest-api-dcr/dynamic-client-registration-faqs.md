---
title: Veelgestelde vragen over dynamische clientregistratie
description: Veelgestelde vragen over dynamische clientregistratie
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Veelgestelde vragen over dynamische clientregistratie {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document biedt een uitgebreid overzicht van veelgestelde vragen over de toepassing van Adobe Pass Authentication Dynamic Client Registration (DCR).

Voor meer informatie over de Dynamische Registratie van de Cliënt (DCR) algemeen, zie het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

>[!MORELIKETHIS]
>
> * [ REST API v2 FAQs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## Algemene veelgestelde vragen {#general-faqs}

Begin met deze sectie als u aan een toepassing werkt die de Dynamische Registratie van de Cliënt (DCR) moet integreren, of het een nieuwe of bestaande toepassing is die van één van de vorige mechanismen migreert.

### Veelgestelde vragen over de registratiefase {#registration-phase-faqs-general}

+++Veelgestelde vragen over de registratiefase

#### 1. Wat is het doel van de registratiefase? {#registration-phase-faq1}

Het doel van de Fase van de Registratie is de cliënttoepassing tegen de Authentificatie van Adobe Pass door het [ Dynamische Registratie van de Cliënt (DCR) te registreren ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) proces.

Het dynamische proces van de Registratie van de Cliënt (DCR) vereist de cliënttoepassing om een paar cliëntgeloofsbrieven te verkrijgen en een toegangstoken als einddoel van de Fase van de Registratie terug te winnen.

Voor meer informatie, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

#### 2. Is de registratiefase verplicht? {#registration-phase-faq2}

De fase van de Registratie is verplicht, maar de cliënttoepassing kan deze fase overslaan als het een caching paar cliëntgeloofsbrieven en een toegangstoken heeft die nog geldig zijn.

#### 3. Wat is een softwarestatement en hoe lang is het geldig? {#registration-phase-faq3}

De softwareverklaring is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement) documentatie wordt bepaald.

De softwareverklaring bestaat uit een Token van het Web JSON (JWT) die van het Dashboard van Adobe Pass [ TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass kan worden geproduceerd en worden gedownload handelend namens u.

De software-instructie is geldig voor een onbeperkt tijdsbestek, maar u kunt desgewenst een Adobe Pass-verificatievertegenwoordiger vragen deze te allen tijde in te trekken.

De clienttoepassing moet de softwareinstructie opslaan en gebruiken wanneer deze clientreferenties moet ophalen.

Voor meer details, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

#### 4. Hoe een softwareinstructie te genereren en te downloaden? {#registration-phase-faq4}

Deze verrichting kan door het Dashboard van Adobe Pass [ worden voltooid TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [ Gids van de Gebruiker van de Kanalen van het Dashboard van TVE ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) of [ de Gids van de Gebruiker van de Programmering van het Dashboard van TVE ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) documentatie.

#### 5. Wat gebeurt er als een softwareinstructie wordt ingetrokken? {#registration-phase-faq5}

Wanneer de softwareverklaring wordt ingetrokken, is er één belangrijk gevolg om te overwegen:

* De cliënttoepassingen die de ingetrokken softwareverklaring gebruiken zullen niet meer door de [ betitelingsstromen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) kunnen gaan, betekenend dat de gebruikers zullen worden geblokkeerd van het spelen van inhoud.

#### 6. Wat zijn clientreferenties en hoe lang zijn deze geldig? {#registration-phase-faq6}

De cliëntgeloofsbrieven zijn een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials) documentatie wordt bepaald.

De cliëntgeloofsbrieven bestaan uit een cliënt herkenningsteken en cliënt geheim paar dat van het eindpunt van het Register van de Cliënt kan worden teruggewonnen.

De clientgegevens zijn geldig voor een onbeperkt tijdsbestek.

De cliënttoepassing moet de cliëntgeloofsbrieven voor onbepaalde tijd opslaan en hen gebruiken wanneer het moeten een toegangstoken terugwinnen.

Voor meer informatie, verwijs naar [ terugwinnen cliëntgeloofsbrieven ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) documentatie.

#### 7. Hoe te om cliëntgeloofsbrieven te beheren? {#registration-phase-faq7}

Wij adviseren de cliënttoepassing om een uniek paar cliëntgeloofsbrieven voor elke instantie van de gebruikerstoepassing in het geval van zowel cliënt-aan-server als server-aan-server integratie met de Authentificatie van Adobe Pass te beheren.

De cliënttoepassing moet de cliëntgeloofsbrieven voor onbepaalde tijd opslaan en hen gebruiken wanneer het moeten een toegangstoken terugwinnen.

#### 8. Wat gebeurt er als clientgegevens in de cache verloren gaan? {#registration-phase-faq8}

Wanneer de caching cliëntgeloofsbrieven worden verloren, zijn er drie belangrijke te overwegen gevolgen:

* De cliënttoepassing moet een nieuw paar cliëntgeloofsbrieven verkrijgen.
* De cliënttoepassing moet een nieuw toegangstoken verkrijgen gebruikend het nieuwe paar cliëntgeloofsbrieven.
* De clienttoepassing moet de gebruiker vragen opnieuw te verifiëren, omdat de clienttoepassing de toegang tot de eerder verkregen geverifieerde profielen verliest.

#### 9. Wat is een toegangstoken en hoe lang is het geldig? {#registration-phase-faq9}

Het toegangstoken is een termijn die in de [ verklarende woordenlijst ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token) documentatie wordt bepaald.

Het toegangstoken bestaat uit het teken van de a [ drager ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) dat van het Symbolische eindpunt van de Cliënt kan worden teruggewonnen.

Het toegangstoken is geldig voor een beperkt en kort tijdsbestek dat op het ogenblik van uitgifte wordt gespecificeerd.

De cliënttoepassing moet het toegangstoken opslaan en het gebruiken tot het verloopt wanneer het richten van REST API V2.

De cliënttoepassing moet een nieuw toegangstoken verkrijgen alvorens huidige verloopt om onbevoegde verzoeken te verhinderen.

Voor meer informatie, verwijs naar [ ontvang toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.

#### 10. Hoe kan de cliënttoepassing een toegangstoken verfrissen? {#registration-phase-faq10}

De cliënttoepassing moet een toegangstoken de zelfde manier verfrissen zoals het terugwinnen van een nieuw toegangstoken, maar het gebruiken van caching cliëntgeloofsbrieven.

De cliënttoepassing moet niet opnieuw registreren om een toegangstoken te verfrissen, in plaats daarvan moet het de opgeslagen cliëntgeloofsbrieven gebruiken, anders zouden de gebruikers moeten opnieuw voor authentiek verklaren.

Voor meer informatie, verwijs naar [ ontvang toegangstoken ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.
