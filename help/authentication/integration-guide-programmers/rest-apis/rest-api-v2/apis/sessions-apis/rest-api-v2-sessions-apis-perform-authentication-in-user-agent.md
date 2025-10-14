---
title: Verificatie uitvoeren in gebruikersagent
description: REST API V2 - Verificatie uitvoeren in gebruikersagent
exl-id: d615dde0-71a8-4b6c-a12e-1e3b5e20728c
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 1%

---

# Verificatie uitvoeren in gebruikersagent {#perform-authentication-in-user-agent}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

>[!MORELIKETHIS]
>
> Zorg ervoor om [&#x200B; REST API V2 FAQs &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) ook te bezoeken.

## Verzoek {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/v2/authenticate/{serviceProvider}/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>De interne unieke id die tijdens het instapproces aan de Serviceleverancier is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">code</td>
      <td>De verificatiecode die wordt verkregen na het maken van de verificatiesessie op het streamingapparaat.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Gebruikersagent</td>
      <td>De gebruikersagent van de clienttoepassing.</td>
      <td>optioneel</td>
   </tr>
</table>

## Antwoord {#response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>302</td>
      <td>Gevonden</td>
      <td>
        De hoofdtekst van de reactie bevat een locatie die wordt omgeleid om door te gaan met de stroom tot de MVPD-aanmeldingspagina is bereikt
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode niet toegestaan</td>
      <td>
        De HTTP-methode is ongeldig, de client moet een HTTP-methode gebruiken die is toegestaan voor de aangevraagde resource en het opnieuw proberen. Voor meer details verwijs naar de <a href="#request"> 1&rbrace; sectie van het Verzoek &lbrace;.</a>
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server.
      </td>
   </tr>
</table>

### Succes {#success}

De succesvolle reactie is een reeks van één of veelvoudige omleidingen tot het bereiken van de MVPD login pagina.

### Fout {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400 405 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>text/html</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">-</td>
      <td>-</td>
      <td>-</td>
   </tr>
</table>

## Voorbeelden {#samples}

### 1. Verificatie uitvoeren in gebruikersagent

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
GET /api/v2/authenticate/REF30/8KHP9RW HTTP/1.1

    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 302 Found

Location :  https://sp.auth.adobe.com/adobe-services/authenticate/saml?noflash=true&mso_id=Cablevision&requestor_id=REF30&no_iframe=false&domain_name=adobe.com&redirect_url=http%3A%2F%2Fadobe.com%2Fapitest%2Flive.html
```

>[!ENDTABS]
