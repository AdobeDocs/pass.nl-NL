---
title: Toegangstoken ophalen
description: Dynamische clientregistratie-API - Toegangstoken ophalen
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Toegangstoken ophalen {#retrieve-access-token}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De dynamische implementatie van de Registratie API van de Cliënt wordt begrensd door de [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

## Verzoek {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Bodyparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            De id-tekenreeks van de clienttoepassing.
            <br/><br/>
            Voor meer informatie over hoe te om het koord van het cliëntherkenningsteken te verkrijgen, verwijs naar <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> de 2&rbrace; API documentatie van de cliëntgeloofsbrieven &lbrace;terugwinnen.</a>
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_geheime</td>
      <td>
            De geheime tekenreeks van de clienttoepassing.
            <br/><br/>
            Voor meer informatie over hoe te om het cliënt geheime koord te verkrijgen, verwijs naar <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> terugwinnen cliëntgeloofsbrieven </a> API documentatie.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">subsidie_type</td>
      <td>
            De tekenreeks met het giftetype (bijvoorbeeld "client_credentials") die de clienttoepassing kan gebruiken voor het client-tokeneindpunt.
            <br/><br/>
            Voor meer informatie over hoe te om het subsidietype koord te verkrijgen, verwijs naar <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> de 2&rbrace; API documentatie van de cliëntgeloofsbrieven &lbrace;terugwinnen.</a>
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Apparaat-Info</td>
      <td>
         De generatie van de lading van de apparateninformatie wordt beschreven in <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md"> x-apparaat-Info </a> documentatie.
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

### Succes {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
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
         JSON-object met de volgende kenmerken:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">id</td>
               <td>De dekkende id die kan worden gebruikt voor het bijhouden van de gebruikersactiviteit.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>De waarde van het toegangstoken de cliënttoepassing voor de kopbal van de Vergunning moet gebruiken.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>De tijd waarop het toegangstoken werd uitgegeven.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">verloopt_in</td>
               <td>De tijd in seconden tot het toegangstoken verloopt.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Het tokentype (bijvoorbeeld "toonder").</td>
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
      <td>400</td>
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
      <td>
        De mogelijke waarden zijn:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Waarde</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    Het verzoek is om een van de volgende redenen ongeldig:
                    <ul>
                        <li>In het verzoek ontbreekt een vereiste parameter.</li>
                        <li>Het verzoek bevat een niet-ondersteunde parameterwaarde (anders dan een type subsidie).</li>
                        <li>De aanvraag herhaalt een parameter.</li>
                        <li>Het verzoek bevat meerdere referenties.</li>
                        <li>Het verzoek gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren.</li>
                        <li>Het verzoek is onjuist geformuleerd.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>De clientreferenties zijn ongeldig, de client moet nieuwe clientgegevens verkrijgen en het opnieuw proberen. Voor meer details verwijs naar <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> terugwinnen cliëntgeloofsbrieven </a> API documentatie.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">onbevoegd_client</td>
               <td>Het gebruikte subsidietype is ongeldig.</td>
            </tr>
         </table>
      </td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples}

### Toegangstoken ophalen {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB  Reactie - Succes ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1723227212,
  "expires_in": 86400,
  "token_type": "bearer"
}
```

>[!TAB  Reactie - Fout ]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
