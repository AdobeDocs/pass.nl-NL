---
title: Draaimechanisme
description: Ken het Throttling mechanisme dat in de Authentificatie van Adobe Pass wordt gebruikt. Bekijk een overzicht van dit mechanisme op deze pagina.
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---


# Draaimechanisme {#throttling-mechanism}

Alle klanten van de Authentificatie van de Voldoende moeten tot de Authentificatie API van de Voldoende voor elk van hun gebruikers, volgens instructies en hun bedrijfscase kunnen toegang hebben.

De Authentificatie van de pas introduceert een Throttling mechanisme om billijke verdeling van middelen onder de gebruikers van onze klanten te verzekeren.

Dit mechanisme is om een paar redenen belangrijk:

- Één van de gouden regels van multi-huurdersarchitectuur is dat het gedrag van één gebruiker niet iemand anders zou moeten beïnvloeden.
- Snelheidsbeperking is belangrijk voor API&#39;s omdat het eenvoudig is fouten te maken bij de integratie met een API. Omdat APIs programmatically zijn, is het vrij gemakkelijk om per ongeluk meer verzoeken te verzenden dan bedoeld.

## Overzicht van het mechanisme {#mechanism-overview}

### Clientimplementaties

De Authentificatie van de pas verstrekt richtlijnen en SDK voor het in wisselwerking staan met API maar controleert niet hoe de klant het gebruikt. Sommige implementaties kunnen een rudimentaire implementatie hebben en kunnen de service overspoelen met onnodige API-verzoeken, of dit nu per ongeluk gebeurt of niet, kan ertoe leiden dat andere gebruikers trager worden of problemen met de capaciteit ondervinden.

De dienst zelf moet elke redelijke capaciteit kunnen afhandelen. Maar hoe schaalbaar of uitvoerbaar een service ook is, er zijn altijd grenzen. Als dusdanig, moet de dienst grenzen hebben die voor het aantal toegelaten vraag binnen een specifiek tijdinterval worden gevormd.

### Bezig met introduceren van vertragen

De Authentificatie van de pas is gebaseerd op gebruikersidentificatie en een symbolische algoritme van de het tariefbeperking van de emmer met vooraf bepaalde waarden om de apparatentoegang van elke gebruiker tot onze API te controleren.

### Apparaatidentificatiemechanisme

Het voorgestelde vertragingsmechanisme gebruikt de geïdentificeerde apparaten individueel, met behulp van &quot;x-Door:sturen-voor&quot;kopbal. Limieten worden op dezelfde manier toegepast voor elk apparaat.

### Vereiste updates

Server-aan-server implementaties moeten de IP van hun cliënt adressen door:sturen gebruikend het &quot;X-Door:sturen-voor&quot;kopbalmechanisme.

U kunt meer details vinden over hoe te om x-Door:sturen-voor kopbal over te gaan [hier](rest-api-cookbook-servertoserver.md).

### Werkelijke grenswaarden en eindpunten

Momenteel, staat de standaardgrens een maximum van 1 verzoek per seconde toe., met een aanvankelijke uitbarsting van 3 verzoeken (eenmalig toelage op de eerste interactie van de geïdentificeerde cliënt, die initialisering zou moeten toestaan om met succes te beëindigen). Dit zou geen regelmatige bedrijfsgeval over al onze klanten moeten beïnvloeden.

Het vertragingsmechanisme zal op de volgende eindpunten worden toegelaten:

- /o/client/register
- /o/client/token
- /o/client/scope
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authoring
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthing
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preAuthze
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-Requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK-implementatie

Aangezien de cliënten die de Authentificatie gebruiken van Adobe Pass verstrekte SDKs niet uitdrukkelijk met om het even welke eindpunten in wisselwerking staan, zal deze sectie de bekende functies presenteren, hoe zij zich wanneer het ontmoeten van een vertragingsreactie, en de acties gedragen die zouden moeten worden genomen.

#### setRequestor

Bij het bereiken van de snelheidslimiet met `setRequestor` vanuit de SDK retourneert de SDK via `errorHandler` callback.

#### getAuthorization

Bij het bereiken van de snelheidslimiet met `getAuthorization` vanuit de SDK retourneert de SDK een Z100-foutcode via `errorHandler` callback.

#### checkPreauthorisedResources

Bij het bereiken van de snelheidslimiet met `checkPreauthorizedResources` functie van de SDK, retourneert de SDK via `errorHandler` callback.

#### getMetadata

Bij het bereiken van de snelheidslimiet met `getMetadata` functie van de SDK, retourneert de SDK een lege reactie via `setMetadataStatus` callback.

Raadpleeg de specifieke SDK-documentatie voor elke specifieke implementatiedetails.

- [JavaScript SDK API-naslag](javascript-sdk-api-reference.md)
- [Referentie voor API van Android](android-sdk-api-reference.md)
- [iOS/tvOS API-naslaggids](iostvos-sdk-api-reference.md)

### Wijzigingen in API-reactie en reactie

Wanneer wij identificeren dat de grens wordt geschonden, zullen wij dit verzoek met een specifieke reactiestatus (HTTP 429 Te Vele Verzoeken) merken, die dat u alle tokens hebt verbruikt die aan het gebruikersapparaat (IP adres) voor het tijdinterval worden toegewezen.

De vertraging verloopt na één seconde van de eerste 429 reacties. Elke toepassing die een reactie van 429 ontvangt, moet minstens 1 seconde wachten alvorens een nieuw verzoek te produceren.

Alle klantentoepassingen zouden de &quot;429 Te Vele antwoorden van Verzoeken&quot;geschikt moeten behandelen.

Hier volgt een voorbeeld van een 429-antwoordbericht:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Effect en vereiste wijzigingen

### X-Forwarded-For-header doorgeven

Klanten die een aangepaste implementatie (inclusief server-naar-server) gebruiken om te communiceren met de API voor controle van de controle van de controle moeten ervoor zorgen dat zij hun IP-adres van de gebruiker kunnen vastleggen en het correct kunnen doorsturen, met behulp van de X-Forwarded-For-header die verder gaat naar de API voor verificatie van de controle van de controle van de controle.

Zie [hier](rest-api-cookbook-servertoserver.md) voor meer informatie .

### Reageren op nieuwe antwoordcode

De klanten die een douaneimplementatie (met inbegrip van server-aan-server degenen) gebruiken om met de Authentificatie API van de Voldoende te communiceren zouden ervoor moeten zorgen dat om het even welke verdere vraag die na het ontvangen van 429 Te veel Verzoeken wordt gemaakt een minimum 1 tweede wachtperiode omvat. Deze wachttijd biedt de mogelijkheid om dit mechanisme te wijzigen en een geldige zakelijke reactie te verkrijgen.

## Scenario-voorbeeld voor vertragen

| Tijd sinds eerste aanvraag | Antwoord ontvangen | Toelichting |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Seconde 0 | De vraag ontvangt de code van de successtatus | 1 vraag die van de grens wordt verbruikt |
| Seconde 0,3 | De vraag ontvangt de code van de successtatus | 1 vraag die van de grens wordt verbruikt en 1 vraag duidelijk als uitbarsting |
| Seconde 0,6 | De vraag ontvangt de code van de successtatus | 1 vraag die van de grens wordt verbruikt en 2 vraag duidelijk als uitbarsting |
| Seconde 0,9 | De vraag ontvangt de code van de successtatus | 1 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting |
| Tweede punt 1.2 | De vraag ontvangt de code van de successtatus | 2 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting |
| Tweede punt 1.4 | De vraag ontvangt 429 statuscode | 2 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting en 1 vraag ontvangt &quot;429 Te veel verzoeken&quot; |
| Tweede punt 1.6 | De vraag ontvangt 429 statuscode | 2 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting en 2 vraag ontvangt &quot;429 Te veel verzoeken&quot; |
| Tweede punt 1.8 | De vraag ontvangt 429 statuscode | 2 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting en 3 vraag ontvangt &quot;429 Te veel verzoeken&quot; |
| Tweede punt 2.1 | De vraag ontvangt de code van de successtatus | 3 vraag die van de grens wordt verbruikt en 3 vraag duidelijk als uitbarsting en 3 vraag ontvangt &quot;429 Te veel verzoeken&quot; |
