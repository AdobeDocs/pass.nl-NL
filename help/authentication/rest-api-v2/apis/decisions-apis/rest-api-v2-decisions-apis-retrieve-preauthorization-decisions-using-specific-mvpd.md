---
title: Besluiten vóór toelating met specifieke mvpd ophalen
description: REST API V2 - beslissingen vóór toelating ophalen met specifieke mvpd
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---


# Besluiten vóór toelating met specifieke mvpd ophalen {#retrieve-preauthorization-decisions-using-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/decisions/preAuthze/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Bodyparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bronnen</td>
      <td>De lijst van middelen die een besluit MVPD vereisen alvorens zij konden worden getoond.</td>
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
      <td style="background-color: #DEEBFF;">Inhoudstype</td>
      <td>
         Het geaccepteerde mediatype voor de bronnen die worden verzonden.
         <br/><br/>
         Het moet application/json zijn.
      </td>
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
      <td style="background-color: #DEEBFF;">AP-Partner-Kader-Status</td>
      <td>
        De generatie van enige sign-on lading voor de methode van de Partner wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md"> AP-partner-kader-status </a> kopbaldocumentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen die een partner gebruiken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> Enige sign-on gebruikend de documentatie van de partnerstromen </a>.</td>
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
        De responsinstantie bevat een lijst van besluiten met aanvullende informatie.
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
      <td style="background-color: #DEEBFF;">besluiten</td>
      <td>
         JSON bevat een lijst met elementen, elk element met de volgende kenmerken:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>De resource-id waarvoor het voorafgaande besluit is geretourneerd.</td>
                <td><i>vereist</i></td>
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
                <td style="background-color: #DEEBFF;">geautoriseerd</td>
                <td>De beslissingsstatus voor de bron, die ofwel "waar" of "onwaar" kan zijn.</td>
                <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">bron</td>
               <td>
                  Informatie over de beslissingsbron:
                  <br/><br/>
                  De mogelijke waarden zijn:
                  <ul>
                    <li><b> mvpd </b><br/> wordt het Besluit uitgegeven door het MVPD voortoelatingseindpunt.</li>
                    <li><b> degradatie </b><br/> Besluit wordt uitgegeven als resultaat van gedegradeerde toegang.</li>
                    <li><b> temppass </b><br/> Besluit wordt uitgegeven als resultaat van tijdelijke toegang.</li>
                    <li><b> dummy </b><br/> Besluit wordt uitgegeven als resultaat van dummy preAuthorisation eigenschap.</li>
                  </ul>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">fout</td>
               <td>De fout verstrekt extra informatie over "ontken"besluit dat aan de <a href="../../../enhanced-error-codes.md"> Verbeterde Codes van de Fout </a> documentatie volgt.</td>
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

### 1. Besluiten tot voorafgaande goedkeuring met behulp van de gewone mvpd intrekken

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
POST /api/v2/REF30/decisions/preauthorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:       

{
    "resources": ["resource1", "resource2", "resource3"]
}
```

>[!TAB  Reactie - toestemmings en ontken besluiten ]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json  

{
   "decisions": [
      {
         "resource": "resource1 ",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource2",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource3",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": false,
         "error": {
            "status": 202,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"
         }
      }
   ]
}
```

>[!ENDTABS]

### 2. Besluiten tot voorafgaande toestemming met een gedegradeerde mvpd ophalen

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
POST /api/v2/REF30/decisions/preauthorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB  Reactie - Verslechtering AuthNAll ]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**Nota:** In dit geval, heeft de regel AuthZAll &quot;kanaal&quot;: &quot;apasST1&quot;

>[!TAB  Reactie - Verslechtering AuthZAll ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**Nota:** In dit geval, heeft de regel AuthZAll &quot;kanaal&quot;: &quot;apasST1&quot;

>[!TAB  Reactie - Afbraak AuthZNone ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**Nota:** In dit geval, heeft de regel AuthZNone &quot;kanaal&quot;: &quot;apasST1&quot;

>[!TAB  Reactie - Verlopen Verslechteringsregel ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
