---
title: Overzicht van Entitlement Service-controle
description: Overzicht van Entitlement Service-controle
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Overzicht van Entitlement Service-controle {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#introduction}

TVE-sites en -toepassingen moeten 24/7 beschikbaar zijn, dus klanten hebben realtime inzicht nodig in machtigingsgebeurtenissen om problemen zo snel mogelijk te kunnen detecteren en corrigeren. Zij moeten ook maandgegevens analyseren om te bepalen welke platforms het grootste deel van het verkeer leveren, en welke platforms een slechte implementatie en slechte omrekeningskoersen zouden kunnen hebben.

De Controle van de Dienst van het recht (ESM) verstrekt Programmers en MVPDs van een gegevensvoer die zicht in real time in hun Authentificatie en de gebeurtenissen van de Vergunning aanbiedt. De gegevens worden verzameld bij de Adobe Pass-verificatiesystemen en via een RESTful-API verstrekt.  Klanten kunnen de gegevens direct of vanuit hun eigen aangepaste, operationele dashboards gebruiken.

De kernelementen van het ESM-systeem zijn de meetwaarden en dimensies ervan. ESM produceert rapporten die samengevoegde metriek volgens de afmetingsselectie bevatten. Aangezien de gebeurtenissen van Adobe Pass PST timezone het programma worden geopend, zijn de rapporten ESM ook beschikbaar in timezone PST.

De ESM API is niet algemeen beschikbaar.  Neem contact op met uw Adobe voor vragen over beschikbaarheid.

## ESM voor programmeurs {#esm-for-programmers}

### Programmeurs kunnen de volgende meetgegevens controleren: {#programmers-monitor-metrics}


| *Naam van Metriek* | *Beschrijving* |
|-------------------------|--------------------------|
| authnPogingen | Aantal geïnitieerde verificatiestromen |
| gelukt | Aantal verificatietokens dat is verkregen door clients |
| in afwachting van goedkeuring | Aantal met succes geproduceerde authentificatietokens (het negeren van al dan niet de cliënt werkelijk het verwierf) |
| authn-failed | Aantal mislukte verificatie uitgevoerd via een extern systeem. |
| clientless-tokens | Aantal Clientless tokens dat is uitgegeven |
| clientless-failure | Aantal mislukte pogingen om tokens van de Clientless API te ontvangen |
| autorisatiepogingen | Aantal pogingen tot het verlenen van vergunningen |
| authz succesvol | Aantal geslaagde vergunningen |
| authz-failed | Aantal door de MVPD’s geweigerde vergunningen op toepassingsniveau |
| afgekeurd | Aantal pogingen van vergunningen die als kwaadwillig door de Leverancier van de Dienst van Adobe worden beschouwd en als resultaat van een de aanvalspreventie van Dos worden verworpen |
| authz-latentie | Totaal aantal milliseconden besteed aan het eindpunt van MVPD |
| media-tokens | Aantal gegenereerde korte mediatokens (die overeenkomen met het aantal afspeelverzoeken) |
| unieke accounts | Aantal unieke gebruikers die machtigingsacties (AuthN / AuthZ) in het geselecteerde tijdinterval hebben uitgevoerd. (Deze metrische waarde geeft alleen aan of er dagelijkse waarden worden aangevraagd.) </br> Deze waarde wordt berekend voor elk afzonderlijk datacenter. Wanneer de afmeting &quot;dc&quot;niet wordt gevraagd, zal metrisch niet worden getoond. |
| unieke sessies | Aantal unieke sessies dat aanroepen van de verificatiestroom naar de Adobe Pass-verificatieservice binnen het geselecteerde tijdsinterval heeft uitgevoerd. (Deze metrische waarde geeft alleen aan of er dagelijkse waarden worden aangevraagd.) </br> Deze waarde wordt berekend voor elk afzonderlijk datacenter. Wanneer de afmeting &quot;dc&quot;niet wordt gevraagd, zal metrisch niet worden getoond. |
| aantal | Een eenvoudige teller die in de gebeurtenis-georiënteerde rapporten wordt gebruikt |

</br>

### Programmeurs kunnen de hierboven vermelde metriek door de volgende afmetingen filtreren: {#progr-filter-metrics}


| *Naam van het Dimension* | *Beschrijving* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jaar | Jaar met 4 cijfers |
| maand | De maand van het jaar (1-12) |
| dag | Dag van de maand (1-31) |
| uur | Het uur van de dag |
| minuut | De minuut van het uur |
| mediabedrijf | Het mediabedrijf dat eigenaar is van de website die het machtigingsproces voor de gebruiker heeft gestart |
| dc | (Gegevenscentrum) Het thuisgebied waar het verzoek is ontvangen. |
| proxy | De proxy-MVPD (die &quot;Direct&quot; zal zijn voor directe integratie) |
| mvpd | MVPD verantwoordelijk voor het toekennen van rechten aan de gebruiker |
| aanvrager-id | De aanvrager-id die wordt gebruikt voor het uitvoeren van de machtigingsaanvraag |
| kanaal | De kanaalwebsite, die uit het middelgebied wordt gehaald (die uit de nuttige lading MRSS als kanaal/titel indien verstrekt wordt gehaald, of aan de middelwaarde in kaart gebracht als het niet in formaat RSS is). |
| resource-id | De feitelijke titel van de bron die bij het vergunningsverzoek is betrokken (geëxtraheerd uit de MRSS-lading als item/titel indien verstrekt) |
| apparaat | Het apparaatplatform (pc, mobiel, console, enz.) |
| afhaken | De externe verificatieprovider wanneer de verificatiestroom wordt uitgevoerd via een extern systeem. </br> De waarden kunnen zijn: </br> - N.v.t. - de verificatie is geleverd door Adobe Pass Authentication </br> - Apple - het externe systeem dat de verificatie heeft geleverd, is Apple |
| os-family | Besturingssysteem dat op het apparaat wordt uitgevoerd |
| browser-familie | Gebruikersagent voor toegang tot Adobe Pass-verificatie |
| cdt | Het apparaatplatform (alternatief), dat momenteel wordt gebruikt voor Clientless. </br> De waarden kunnen zijn: </br> - N.v.t. - de gebeurtenis is niet afkomstig van een client-less SDK </br> - Onbekend - Omdat de deviceType-parameter van een client-less API optioneel is, zijn er aanroepen die geen waarde bevatten. </br> - elke andere waarde die via de client-API is verzonden, bijvoorbeeld xbox, appletv, roku enzovoort. </br> |
| platformversie | De versie van de Clientless SDK |
| van het type os | Besturingssysteem dat op het apparaat wordt uitgevoerd, alternatief (momenteel niet gebruikt) |
| browserversie | Versie van gebruikersagent |
| nsdk | De client-SDK wordt gebruikt (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-versie | De versie van de Adobe Pass Authentication-client SDK |
| event | De naam van de Adobe Pass-verificatiegebeurtenis |
| reden | De reden voor fouten, zoals gemeld door Adobe Pass-verificatie |
| van het type | Het onderliggende SSO-mechanisme: platform/passief/adobe. Geeft aan dat het verificatietoken is uitgegeven door AuthN opnieuw te gebruiken in een andere toepassing |
| platform | Het apparaat identificeerde platform. Mogelijke waarden: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - enzovoort |
| application-name | De toepassingsnaam die in het TVE-dashboard is geconfigureerd voor de DCR-geregistreerde toepassing die is geconfigureerd om te worden gebruikt. |
| toepassingsversie | De toepassingsversie die in het TVE-dashboard is geconfigureerd voor de DCR-geregistreerde toepassing die is geconfigureerd voor gebruik. |
| klant-app | De identiteitskaart van de douanetoepassing ging via [ Informatie van het Apparaat ](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md) over. |
| inhoudscategorie | De categorie van de inhoud die door uw toepassing wordt aangevraagd. |

## ESM voor MVPD&#39;s {#esm-for-mvpds}

### MVPDs kan de volgende metriek controleren:

| *Metrische Naam* | *Beschrijving* |
|---|---|
| authnPogingen | Aantal geïnitieerde verificatiestromen |
| gelukt | Aantal verificatietokens dat is verkregen door clients |
| in afwachting van goedkeuring | Aantal met succes geproduceerde authentificatietokens (het negeren van al dan niet de cliënt werkelijk het verwierf) |
| authn-failed | Aantal mislukte verificatie uitgevoerd via een extern systeem. |
| autorisatiepogingen | Aantal pogingen tot het verlenen van vergunningen |
| authz succesvol | Aantal geslaagde vergunningen |
| authz-failed | Aantal door de MVPD’s geweigerde vergunningen op toepassingsniveau |
| afgekeurd | Aantal pogingen van vergunningen die als kwaadwillig door de Leverancier van de Dienst van Adobe worden beschouwd en als resultaat van een de aanvalspreventie van Dos worden verworpen |
| authz-latentie | Totaal aantal milliseconden besteed aan het eindpunt van MVPD |

### MVPD&#39;s kunnen de hierboven vermelde metingen filteren op de volgende afmetingen:

| *Naam van het Dimension* | *Beschrijving* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jaar | Jaar met 4 cijfers |
| maand | De maand van het jaar (1-12) |
| dag | Dag van de maand (1-31) |
| uur | Het uur van de dag |
| minuut | De minuut van het uur |
| mvpd | De mvpd-id die wordt gebruikt voor het uitvoeren van de machtigingsaanvraag |
| aanvrager-id | De aanvrager-id die wordt gebruikt voor het uitvoeren van de machtigingsaanvraag |
| afhaken | De externe verificatieprovider wanneer de verificatiestroom wordt uitgevoerd via een extern systeem. </br> De waarden kunnen zijn: </br> - N.v.t. - de verificatie is geleverd door Adobe Pass Authentication </br> - Apple - het externe systeem dat de verificatie heeft geleverd, is Apple |
| cdt | Het apparaatplatform (alternatief), dat momenteel wordt gebruikt voor Clientless. </br> De waarden kunnen zijn: </br> - N.v.t. - de gebeurtenis is niet afkomstig van een client-less SDK </br> - Onbekend - Omdat de deviceType-parameter van een client-less API optioneel is, zijn er aanroepen die geen waarde bevatten. </br> - elke andere waarde die via de client-API is verzonden, bijvoorbeeld xbox, appletv, roku enzovoort. </br> |
| sdk-type | De SDK van de client die wordt gebruikt (Flash, HTML5, Android native, iOS, Clientless enz.) |
| platform | Het apparaat identificeerde platform. Mogelijke waarden: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - enzovoort |
| nsdk | De client-SDK wordt gebruikt (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-versie | De versie van de Adobe Pass Authentication-client SDK |

## Gevallen gebruiken {#use-cases}

U kunt de ESM-gegevens gebruiken voor de volgende gebruiksgevallen:

- **Controle** - Ops of controleteams kunnen een dashboard of grafiek tot stand brengen die API elke minuut roept. Aan de hand van de weergegeven informatie kunnen ze een probleem detecteren (met Adobe Pass-verificatie of met een MVPD) zodra het wordt weergegeven.

- **het Zuiveren/het Testen van de Kwaliteit** - omdat het gegeven ook door platform, apparaat, browser, en OS wordt gebroken, kan het analyseren van gebruikspatronen problemen op specifieke combinaties (b.v., Safari op OSX) identificeren.

- **Analytics** - de verstrekte gegevens kunnen worden gebruikt om de gegevens van de cliëntkant aan te vullen/te controleren die door Adobe Analytics of een ander analysehulpmiddel worden verzameld.

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
