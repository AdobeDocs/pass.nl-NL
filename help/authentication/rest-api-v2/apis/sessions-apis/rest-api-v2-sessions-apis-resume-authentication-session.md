---
title: Verificatiesessie hervatten
description: REST API V2 - Verificatiesessie hervatten
source-git-commit: d59afc0384a1c3617143efcef4ab5fb1a323e511
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 0%

---


# Verificatiesessie hervatten {#resume-authentication-session}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

## Verzoek {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/v2/{serviceProvider}/sessies/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>POST</td>
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
      <th style="background-color: #EFF2F7;">Bodyparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.
        <br/><br/>
        Als het streamingapparaatplatform beperkingen heeft op het opgeven van een waarde, moet een toepassing de verificatiesessie hervatten en een geldige waarde opgeven.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Het oorspronkelijke domein van de toepassing die MVPD-aanmelding uitvoert.
        <br/><br/>
        Als het streamingapparaatplatform beperkingen heeft op het opgeven van een waarde, moet een toepassing de verificatiesessie hervatten en een geldige waarde opgeven.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        De laatste omleiding URL waaraan de gebruikersagent navigeert wanneer de authentificatiestroom voor MVPD wordt voltooid.
        <br/><br/>
        De waarde moet URL-gecodeerd zijn.
        <br/><br/>
        Als het streamingapparaatplatform beperkingen heeft op het opgeven van een waarde, moet een toepassing de verificatiesessie hervatten en een geldige waarde opgeven.
        </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="../../../dynamic-client-registration-api.md"> Dynamische documentatie van de Registratie van de Cliënt </a>.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>
         Het geaccepteerde mediatype voor de bronnen die worden verzonden.
         <br/><br/>
         Het moet application/x-www-form-urlencoded zijn.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Het IP-adres van het streamingapparaat.
         <br/><br/>
         Het wordt sterk geadviseerd om het voor server aan serverimplementaties altijd te gebruiken, vooral wanneer de vraag door de programmeerdienst eerder dan het het stromen apparaat wordt gemaakt.
         <br/><br/>
         Voor client-naar-server-implementaties wordt het IP-adres van het streamingapparaat impliciet verzonden.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepteren</td>
      <td>
         Het mediatype dat door de clienttoepassing wordt geaccepteerd.
         <br/><br/>
         Indien gespecificeerd, moet het application/json zijn.
      </td>
      <td>optioneel</td>
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
      <td>200</td>
      <td>OK</td>
      <td>
        De reactieinstantie bevat informatie over de volgende acties nodig om authentificatie uit te voeren.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="../../../enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="../../../dynamic-client-registration-api.md"> Dynamische documentatie van de Registratie van de Cliënt </a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode niet toegestaan</td>
      <td>
        De HTTP-methode is ongeldig, de client moet een HTTP-methode gebruiken die is toegestaan voor de aangevraagde resource en het opnieuw proberen. Voor meer details verwijs naar de <a href="#request"> 1} sectie van het Verzoek {.</a>
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interne serverfout</td>
      <td>
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="../../../enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
</table>

### Succes {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-object met de volgende kenmerken:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  De handeling die het streamingapparaat moet uitvoeren om de verificatiestroom te voltooien.
                  <br/><br/>
                  De mogelijke waarden zijn:
                  <ul>
                    <li><b> verklaart </b><br/> voor authentiek het stromen apparaat of een ander apparaat moet verstrekte URL in een gebruikersagent openen.</li>
                    <li><b> herprobeer </b><br/> het stromen apparaat of een ander apparaat moet de ontbrekende parameters verstrekken en opnieuw proberen herstellend de authentificatiesessie gebruikend de code.</li>
                    <li><b> machtigt </b><br/> het stromen apparaat kan met besluitvormingsstromen direct te werk gaan.</li>
                  </ul> 
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Het type interactie dat het streamingapparaat moet uitvoeren om door te gaan met de handeling die is opgegeven door het kenmerk 'actionName'.
                  <br/><br/>
                  De mogelijke waarden zijn:
                  <ul>
                    <li><b> interactief </b><br/> De stroom gaat met een navigatie aan verstrekte URL voort gebruikend een gebruikersagent.</li>
                    <li><b> direct </b><br/> de stroom gaat met een directe vraag aan verstrekte URL voort gebruikend een cliënt van HTTP beschikbaar voor de cliëntimplementatie.</li>
                  </ul>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>De ontbrekende parameters die moeten worden verstrekt om de basisauthentificatiestroom te voltooien.</td>
               <td>optioneel</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>De URL waar de clienttoepassing moet navigeren.</td>
               <td>optioneel</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">code</td>
               <td>De authentificatiecode die op een secundaire toepassing kan worden gebruikt om de authentificatiesessie te hervatten.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>De dekkende id die kan worden gebruikt voor het bijhouden van de gebruikersactiviteit.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
               <td>optioneel</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>De interne unieke id die tijdens het instapproces aan de Serviceleverancier is gekoppeld.</td>
               <td><i>vereist</i></td>
            </tr>
         </table>
      </td>
      <td><i>vereist</i></td>
</table>

### Fout {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">fout</td>
      <td>De fout verstrekt extra informatie die aan de <a href="../../../enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples}

### 1. Hervat de authentificatiesessie zonder waarden voor alle vereiste parameters te verstrekken

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
