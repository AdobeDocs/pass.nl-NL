---
title: Veelgestelde vragen over dynamische clientregistratie
description: Veelgestelde vragen over dynamische clientregistratie
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# Veelgestelde vragen over dynamische clientregistratie {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document biedt een uitgebreid overzicht van veelgestelde vragen over de toepassing van Adobe Pass Authentication Dynamic Client Registration (DCR).

Voor meer informatie over de Dynamische Registratie van de Cliënt (DCR) algemeen, zie het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

## Algemene veelgestelde vragen {#general-faqs}

Begin met deze sectie als u aan een toepassing werkt die de Dynamische Registratie van de Cliënt (DCR) moet integreren, of het een nieuwe of bestaande toepassing is die van één van de vorige mechanismen migreert.

>[!MORELIKETHIS]
>
> * [&#x200B; REST API v2 FAQs &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### Veelgestelde vragen over REST API V2 Access {#rest-api-v2-access-faqs}

+++Veelgestelde vragen over REST API V2 Access

#### &#x200B;1. Wat is het doel van de registratiefase? {#rest-api-v2-access-faq1}

Het doel van de Fase van de Registratie is de cliënttoepassing tegen de Authentificatie van Adobe Pass door het [&#x200B; Dynamische Registratie van de Cliënt (DCR) te registreren &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) proces.

Het dynamische proces van de Registratie van de Cliënt (DCR) vereist de cliënttoepassing om een paar cliëntgeloofsbrieven te verkrijgen en een toegangstoken als einddoel van de Fase van de Registratie terug te winnen.

Voor meer informatie, verwijs naar het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

#### &#x200B;2. Is de registratiefase verplicht? {#rest-api-v2-access-faq2}

De fase van de Registratie is verplicht, maar de cliënttoepassing kan deze fase overslaan als het een caching paar cliëntgeloofsbrieven en een toegangstoken heeft die nog geldig zijn.

#### &#x200B;3. Wat is een softwarestatement en hoe lang is het geldig? {#rest-api-v2-access-faq3}

De softwareverklaring is een termijn die in de [&#x200B; verklarende woordenlijst &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement) documentatie wordt bepaald.

De softwareverklaring bestaat uit een Token van het Web JSON (JWT) die van het Dashboard van Adobe Pass [&#x200B; TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass kan worden geproduceerd en worden gedownload handelend namens u.

De software-instructie is geldig voor een onbeperkt tijdsbestek, maar u kunt desgewenst een Adobe Pass-verificatievertegenwoordiger vragen deze te allen tijde in te trekken.

De clienttoepassing moet de softwareinstructie opslaan en gebruiken wanneer deze clientreferenties moet ophalen.

Voor meer details, verwijs naar het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

#### &#x200B;4. Hoe een softwareinstructie te genereren en te downloaden? {#rest-api-v2-access-faq4}

Deze verrichting kan door het Dashboard van Adobe Pass [&#x200B; worden voltooid TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van de Kanalen van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) of [&#x200B; de Gids van de Gebruiker van de Programmering van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) documentatie.

#### &#x200B;5. Wat gebeurt er als een softwareinstructie wordt ingetrokken? {#rest-api-v2-access-faq5}

Wanneer de softwareverklaring wordt ingetrokken, is er één belangrijk gevolg om te overwegen:

* De cliënttoepassingen die de ingetrokken softwareverklaring gebruiken zullen niet meer door de [&#x200B; betitelingsstromen &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) kunnen gaan, betekenend dat de gebruikers zullen worden geblokkeerd van het spelen van inhoud.

#### &#x200B;6. Wat zijn clientreferenties en hoe lang zijn deze geldig? {#rest-api-v2-access-faq6}

De cliëntgeloofsbrieven zijn een termijn die in de [&#x200B; verklarende woordenlijst &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials) documentatie wordt bepaald.

De cliëntgeloofsbrieven bestaan uit een cliënt herkenningsteken en cliënt geheim paar dat van het eindpunt van het Register van de Cliënt kan worden teruggewonnen.

De clientgegevens zijn geldig voor een onbeperkt tijdsbestek.

De cliënttoepassing moet de cliëntgeloofsbrieven opslaan en hen voor onbepaalde tijd gebruiken wanneer het moeten een toegangstoken terugwinnen.

Voor meer informatie, verwijs naar [&#x200B; terugwinnen cliëntgeloofsbrieven &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) documentatie.

#### &#x200B;7. Hoe te om cliëntgeloofsbrieven te beheren? {#rest-api-v2-access-faq7}

Wij adviseren de cliënttoepassing om een uniek paar cliëntgeloofsbrieven voor elke instantie van de gebruikerstoepassing in het geval van zowel cliënt-aan-server als server-aan-server integratie met de Authentificatie van Adobe Pass te beheren.

#### &#x200B;8. Moet de clienttoepassing de clientgegevens in een permanente opslag in cache plaatsen? {#rest-api-v2-access-faq8}

De cliënttoepassing moet de cliëntgeloofsbrieven opslaan en hen voor onbepaalde tijd gebruiken wanneer het moeten een toegangstoken terugwinnen.

#### &#x200B;9. Wat gebeurt er als de clientgegevens in de cache verloren gaan? {#rest-api-v2-access-faq9}

Wanneer de caching cliëntgeloofsbrieven worden verloren, zijn er drie belangrijke te overwegen gevolgen:

* De cliënttoepassing moet een nieuw paar cliëntgeloofsbrieven verkrijgen.
* De cliënttoepassing moet een nieuw toegangstoken verkrijgen gebruikend het nieuwe paar cliëntgeloofsbrieven.
* De clienttoepassing moet de gebruiker vragen opnieuw te verifiëren, omdat de clienttoepassing de toegang tot de eerder verkregen geverifieerde profielen verliest.

#### &#x200B;10. Wat is een toegangstoken en hoe lang is het geldig? {#rest-api-v2-access-faq10}

Het toegangstoken is een termijn die in de [&#x200B; verklarende woordenlijst &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token) documentatie wordt bepaald.

Het toegangstoken bestaat uit het teken van de a [&#x200B; drager &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) dat van het Symbolische eindpunt van de Cliënt kan worden teruggewonnen.

Het toegangstoken is geldig voor een beperkt en kort tijdsbestek dat op het ogenblik van uitgifte wordt gespecificeerd.

De cliënttoepassing moet het toegangstoken opslaan en het gebruiken tot het verloopt wanneer het richten van REST API V2.

De cliënttoepassing moet een nieuw toegangstoken verkrijgen alvorens huidige verloopt om onbevoegde verzoeken te verhinderen.

Voor meer informatie, verwijs naar [&#x200B; ontvang toegangstoken &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.

#### &#x200B;11. Moet het toegangstoken door de clienttoepassing in cache worden opgeslagen in een permanente opslagruimte? {#rest-api-v2-access-faq11}

De cliënttoepassing moet het toegangstoken opslaan en gebruiken tot het verloopt, dan het verwerpen en nieuwe verkrijgen.

#### &#x200B;12. Hoe kan de cliënttoepassing een toegangstoken verfrissen? {#rest-api-v2-access-faq12}

De cliënttoepassing moet een toegangstoken de zelfde manier verfrissen zoals het terugwinnen van een nieuw toegangstoken, maar het gebruiken van caching cliëntgeloofsbrieven.

De cliënttoepassing moet niet opnieuw registreren om een toegangstoken te verfrissen, in plaats daarvan moet het de opgeslagen cliëntgeloofsbrieven gebruiken, anders zouden de gebruikers moeten opnieuw voor authentiek verklaren.

Voor meer informatie, verwijs naar [&#x200B; ontvang toegangstoken &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) documentatie.

+++

## Veelgestelde vragen over migratie {#migration-faqs}

Ga verder met deze sectie als u aan een toepassing werkt die een bestaande toepassing moet migreren om Dynamische Registratie van de Cliënt (DCR) te gebruiken.

>[!MORELIKETHIS]
>
> * [&#x200B; REST API v2 FAQs &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### Veelgestelde vragen over REST API V2-migratie {#rest-api-v2-migration-faqs}

+++Veelgestelde vragen over REST API V2-migratie

#### &#x200B;1. Kan de clienttoepassing de bestaande geregistreerde toepassingen (softwareinstructies) opnieuw gebruiken? {#rest-api-v2-migration-faq1}

De clienttoepassing kan de bestaande geregistreerde toepassingen (softwareinstructies) niet opnieuw gebruiken. Daarom moet de toepassing een nieuwe geregistreerde toepassing (softwareinstructies) genereren en downloaden die speciaal is bedoeld voor het gebruik van REST API V2.

Deze verrichting kan door het Dashboard van Adobe Pass [&#x200B; worden voltooid TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van de Kanalen van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) of [&#x200B; de Gids van de Gebruiker van de Programmering van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) documentatie.

Voor het ogenblik zult u een vertegenwoordiger van de Authentificatie van Adobe Pass moeten vragen om het gebruik van REST API V2 voor uw nieuwe geregistreerde toepassingen (softwareverklaringen) toe te laten, tot het Dashboard van Adobe Pass [&#x200B; TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) zal worden bijgewerkt om het zelf-beheer van deze verrichting toe te staan.

Als u onderscheid wilt maken tussen de geregistreerde toepassingen (softwareinstructies) die worden gebruikt in clienttoepassingen die REST API V2 gebruiken, moet u een specifiek achtervoegsel toevoegen aan de geregistreerde toepassingsnaam, zoals &quot;RESTV2&quot;.

#### &#x200B;2. Kan de clienttoepassing de bestaande aangepaste schema&#39;s opnieuw gebruiken? {#rest-api-v2-migration-faq2}

De cliënttoepassing kan de bestaande douaneregelingen opnieuw gebruiken die door het Dashboard van Adobe Pass [&#x200B; worden geproduceerd TVE &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard).

Voor meer details, verwijs naar de [&#x200B; Gids van de Gebruiker van de Kanalen van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) of [&#x200B; de Gids van de Gebruiker van de Programmering van het Dashboard van TVE &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) documentatie.

+++
