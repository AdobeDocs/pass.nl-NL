---
title: Profielen
description: Profielen
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Profielen {#profiles}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De profielen worden gecreeerd door de Authentificatie van Adobe Pass [ REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) wanneer een gebruiker met succes met hun leverancier van de Tv van de Betaal (MVPD) verklaart.

Het type profiel varieert op basis van de gebruikte verificatiemethode:

* **Regelmatig**

  Gemaakt via basisverificatie.

* **Apple SSO**

  Gemaakt via Single Sign-On (SSO) met behulp van Apple Video Subscriber Account Framework.

* **Platform SSO**

  Gemaakt via Single Sign-On (SSO) met behulp van een platform-id.

* **Symbolische SSO van de Dienst**

  Gemaakt via Single Sign-On (SSO) met een servicetoken.

Profielen slaan sleutelgegevens op die cliënttoepassingen toelaten om:

* Bepaal de verificatiestatus van de gebruiker.
* Identificeer de gebruikte authentificatiemethode.
* Identificeer de identiteitsprovider.
* Toegang [ gebruikersmeta-gegevens ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Profielen worden veilig opgeslagen op de achtergrond van de Adobe Pass-verificatie en zijn gekoppeld aan de toepassing, het apparaat en de id van de serviceprovider die ze aanvragen. Zij blijven geldig voor een beperkte tijd, zoals die door de [ Tijd-aan-Levende Authentificatie (TTL) ](#authentication-ttl-management) wordt bepaald.

## Verificatietijd-aan-Levende (TTL) Beheer {#authentication-ttl-management}

De Tijd-aan-Levende van de authentificatie (TTL) bepaalt hoe lang een gebruiker voor het moeten opnieuw voor authentiek verklaren voor authentiek blijft. Deze termijn is beperkt en moet worden overeengekomen met vertegenwoordigers van MVPD. De waarden van TTL kunnen variëren gebaseerd op:

* Platformcategorie (bijvoorbeeld desktop, mobiel, met tv verbonden apparaten)
* Specifiek platform (bijvoorbeeld iOS, Android, tvOS, Roku, FireTV)

De authentificatie (authN) TTL kan door het Dashboard van Adobe Pass [ worden bekeken en worden veranderd TVE ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) door één van uw organisatiebeheerders of door een vertegenwoordiger van de Authentificatie van Adobe Pass handelend namens u.

Voor meer details, verwijs naar de ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) documentatie van de Gebruiker van de Gids van de Integratie van het Dashboard van 0} TVE.[

## REST API V2 {#rest-api-v2}

De profielen kunnen worden opgehaald met de volgende API&#39;s:

* [Profielen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profiel ophalen voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profiel ophalen voor specifieke code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Verwijs naar de **secties van de Reactie** en **Steekproeven** van bovengenoemde APIs om de structuur van de profielen te begrijpen.

Raadpleeg de volgende documenten voor meer informatie over hoe en wanneer u de bovenstaande API&#39;s wilt integreren:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [ FAQs van de Fase van de Authentificatie ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
