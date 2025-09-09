---
title: Win configuratie voor specifieke dienstverlener terug
description: REST API V2 - Configuratie ophalen voor specifieke serviceprovider
exl-id: ad7e4c6d-ed96-4ae7-82a9-3c24e5fc9302
source-git-commit: 8952283ccce38f70f4f504f07714c583fcb7e272
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---

# Win configuratie voor specifieke dienstverlener terug {#retrieve-configuration-for-specific-service-provider}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

>[!MORELIKETHIS]
>
> Zorg ervoor om [ REST API V2 FAQs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general) ook te bezoeken.

## Verzoek {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/v2/{serviceProvider}/configuration</td>
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
      <th style="background-color: #EFF2F7;">Zoekparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profiel</td>
      <td>-</td>
      <td>optioneel</td>
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
      <td>optioneel</td>
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
      <td>optioneel</td>
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
        De responsinstantie bevat een lijst met MVPD's die een actieve integratie met de 'serviceProvider' hebben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
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
        Er is een probleem opgetreden op de server. Het reactielichaam kan fouteninformatie bevatten die aan de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON bevat een lijst met elementen, elk element met de volgende kenmerken:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">apparaat</td>
                <td>Apparaattype</td>
                <td><i>vereist</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">clientType</td>
                <td>Type client</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">errorReporting</td>
                <td>Object</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">aanvrager</td>
                <td>
                    JSON-object met de volgende kenmerken:
                    <ul>
                        <li><b> identiteitskaart </b><br/> het interne unieke herkenningsteken verbonden aan de Dienstverlener tijdens onboarding proces.</li>
                        <li><b> naam </b><br/> de commerciële (merk) naam verbonden aan de Dienstverlener tijdens aan boord gaan proces.</li>
                        <li><b> domeinen </b><br/> de lijst van domeinnamen die aan de Authentificatie van Adobe Pass worden vermeld om de Dienstverlener te vertegenwoordigen.</li>
                    </ul>
                </td>
                <td><i>vereist</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpds</td>
                <td>
                    JSON-object met de volgende kenmerken:
                    <ul>
                        <li><b> identiteitskaart </b><br/> het interne unieke herkenningsteken verbonden aan de Leverancier van de Identiteit tijdens onboarding proces.</li>
                        <li><b> displayName </b><br/> de commerciële (merk) naam verbonden aan de Leverancier van de Identiteit tijdens het instapproces.</li>
                        <li><b> logoUrl </b><br> URL van waar te om het embleem te downloaden verbonden aan de Leverancier van de Identiteit.</li>
                        <li><b> isTempPass </b><br/> De vlag die specificeert als MVPD wordt ontworpen om <a href="../../../../features-premium/temporary-access/temp-pass-feature.md"> TempPass </a> eigenschapfunctionaliteit te verstrekken.</li>
                        <li><b> isProxy </b><br/> De vlag die specificeert als MVPD een proxied MVPD is.</li>
                        <li><b> boardingStatus </b><br/> de status die specificeert als de Leverancier van de Identiteit door het het stromen apparatenplatform voor enige sign-on stromen wordt bezet.</li>
                        <li><b> platformMappingId </b><br/> het interne unieke herkenningsteken verbonden aan de Leverancier van de Identiteit door het het stromen apparatenplatform voor enige sign-on stromen.</li>
                        <li><b> enablePlatformServices </b><br/> De vlag die specificeert als de configuratie van de Leverancier van de Identiteit voor het stromen apparatenplatform voor enige sign-on stromen wordt toegelaten.</li>
                        <li><b> displayInPlatformPicker </b><br/> De vlag die specificeert als de Leverancier van de Identiteit in de het stromen apparatenplatformkiezer voor enige sign-on stromen kan worden getoond.</li>
                        <li><b> enforplatformPermissions </b><br/> De vlag die specificeert als het het stromen apparaat de gebruikerstoestemmingen moet afdwingen die door het platform voor enige sign-on stromen worden verstrekt.</li>
                    </ul>
                </td>
                <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">tijd</td>
               <td></td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>Het reactielichaam kan extra fouteninformatie verstrekken die aan de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples}

### &#x200B;1. Win configuratie voor specifieke dienstverlener terug

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
GET /api/v2/REF30/configuration/ HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
 
{
    "device": "unknown",
    "clientType": "html5",
    "os": "Unknown",
    "requestor": {
        "id": "REF30",
        "name": "Reference site only in 30",
        "domains": [
            {
                "name": "adobe.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobe.io",
                "mvpdInitiated": false
            },
            {
                "name": "adobepass.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobeptime.com",
                "mvpdInitiated": false
            },
            {
                "name": "anvilcreative.com",
                "mvpdInitiated": false
            },
            {
                "name": "testadobe.com",
                "mvpdInitiated": false
            }
        ],
        "mvpds": [
            {
                "id": "AdobePass_SMI",
                "displayName": "Adobe Pass SMI",
                "logoUrl": "https://blogs.adobe.com/conversations/files/2010/08/adobe-logo.jpg",
                "authPerAggregator": false
            },
            {
                "id": "TempPass_TEST40",
                "displayName": "Adobe Temp Pass Test 3 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "TempPass_TEST44",
                "displayName": "Adobe Temp Pass Test 30 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "AdobeShibboleth",
                "displayName": "AdobeShibboleth",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/adobe.png"
            },
            {
                "id": "ATTOTT",
                "displayName": "DIRECTV STREAM",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/directvstream.jpg"
            },
            {
                "id": "ElasticSSO",
                "displayName": "ElasticSSO",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": false
            },
            {
                "id": "TempPass",
                "displayName": "Temp-Pass",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "passiveAuthnEnabled": false,
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "Comcast_SSO_Perf",
                "displayName": "Xfinity Perf",
                "logoUrl": "https://login.comcast.net/static/images/ci/tve/mvpd_comcast_logo112x33.gif",
                "authPerAggregator": true,
                "authPerBrowserSession": true
            }
        ]
    }
}  
```

>[!ENDTABS]
