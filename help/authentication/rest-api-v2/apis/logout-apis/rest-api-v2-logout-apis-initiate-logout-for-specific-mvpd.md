---
title: Afmelden starten voor specifieke mvpd
description: REST API V2 - Afmelden starten voor specifieke mvpd
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 0%

---


# Afmelden starten voor specifieke mvpd {#initiate-logout-for-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Zoekparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        De definitieve omleiding URL waaraan de gebruikersagent navigeert wanneer de logout stroom voor MVPD wordt voltooid.
        <br/><br/>
        De waarde moet URL-gecodeerd zijn.
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
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md"> 1} kopbaldocumentatie van de Toestemming {.</a></td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md"> AP-apparaat-Identifier </a> kopbaldocumentatie.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Apparaat-Info</td>
      <td>
         De generatie van de lading van de apparateninformatie wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md"> x-apparaat-Info </a> kopbaldocumentatie.
         <br/><br/>
         Het wordt ten zeerste aanbevolen deze altijd te gebruiken wanneer het apparaatplatform van de toepassing expliciet geldige waarden biedt.
         <br/><br/>
         Als deze optie is opgegeven, worden waarden met geëxtraheerde waarden expliciet samengevoegd met de Adobe Pass-verificatieachterkant (standaard).
         <br/><br/>
         Als deze optie niet is opgegeven, gebruikt de Adobe Pass Authentication-backend impliciet geëxtraheerde waarden (standaard).
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
      <td style="background-color: #DEEBFF;">Adobe-Onderwerptoken</td>
      <td>
        De generatie van enige sign-on lading voor de methode van de Identiteit van het Platform wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md"> Adobe-Onderwerp-Symbolische </a> kopbaldocumentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen die een platformidentiteit gebruiken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> Enige sign-on gebruikend de stromen van de platformidentiteit </a> documentatie.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        De generatie van enige sign-on lading voor de Symbolische methode van de Dienst wordt beschreven in de <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md"> AD-dienst-Symbolische </a> kopbaldocumentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen gebruikend een de dienstteken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> Enige sign-on gebruikend de stromen van het de dienstteken </a> documentatie.
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
        De hoofdtekst van de reactie bevat een lijst met acties die de client moet uitvoeren om de afmeldingsstroom voor elk verwijderd profiel te voltooien.
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
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="../../../dcr-api/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
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
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan <a href="../../../enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
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
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>application/json</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">logouts</td>
      <td>
         JSON met een overzicht van sleutel-, waardeparen.
         <br/><br/>
         Het hoofdelement wordt gedefinieerd door de volgende waarde:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Waarde</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
               <td><i>vereist</i></td>
         </table>
         Het element value wordt gedefinieerd door de volgende kenmerken:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  De handeling die het streamingapparaat moet uitvoeren om de uitlogingsstroom te voltooien.
                  <br/><br/>
                  De mogelijke waarden zijn:
                  <ul>
                    <li><b> logout </b><br/> het stromen apparaat moet verstrekte URL in een gebruikersagent openen.<br/> Deze actie is voor de volgende scenario's van toepassing: logout van MVPD met een logout eindpunt.</li>
                    <li><b> volledig </b><br/> het stromen apparaat te hoeven niet om het even welke verdere acties uit te voeren.<br/> Deze actie is voor de volgende scenario's van toepassing: logout van MVPD zonder een logout eindpunt (dummy logout eigenschap), logout tijdens degraded toegang, logout tijdens tijdelijke toegang.</li>
                    <li><b> ongeldig </b><br/> het stromen apparaat te hoeven niet om het even welke verdere acties uit te voeren.<br/> Deze actie is voor de volgende scenario's van toepassing: logout van MVPD wanneer geen geldig profiel wordt gevonden.</li>
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
                    <li><b> interactief </b><br/> Dit type is op de volgende waarden van het ` actionName attribuut van toepassing: <b> logout </b>.</li>
                    <li><b> niets </b><br/> Dit type is op de volgende waarden van het ` actionName attribuut van toepassing: <b> volledig </b>, <b> ongeldig </b>.</li>
                  </ul>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  URL die wordt gebruikt om de logout stroom met het MVPD eindpunt uit te voeren.
                  <br/><br/>
                  Dit is niet aanwezig voor de volgende waarden van het attribuut "actionName":
                  <ul>
                    <li><b>complete</b></li>
                    <li><b>ongeldig</b></li>
                  </ul>
               </td>
               <td>optioneel</td>
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

### 1. Eenvoudige afmelding voor mvpd met ondersteuning voor afmelden

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)  
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Cablevision" : {
            "actionName" : "logout",
            "actionType" : "interactive",
            "mvpd" : "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### 2. Eenvoudige afmelding starten voor mvpd die geen ondersteuning biedt voor afmeldingsstroom

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/Dish?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Dish" : {
            "actionName" : "complete",
            "actionType" : "none",
            "mvpd" : "Dish"
       }
    }
}
```

>[!ENDTABS]

### 3. Afmelden starten, inclusief geverifieerde profielen die zijn verkregen via een eenmalige aanmelding met de methode Platform Identity

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 4. Afmelden starten, inclusief geverifieerde profielen die zijn verkregen via Single Sign-On met de methode Servicetokken

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 5. Afmelden starten voor (promotionele) tijdelijke doorgifte

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "TempPass_5mins" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]

### 6. Afmelden starten voor gedegradeerde mvpd

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/logout/REF30/ATTOTT?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "ATTOTT" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "ATTOTT",
        }
    }
}
```

>[!ENDTABS]
