---
title: Client-referenties ophalen
description: Dynamische clientregistratie-API - clientgegevens ophalen
exl-id: 0b39768b-25b8-47b9-8080-59c56fb829fb
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Client-referenties ophalen {#retrieve-client-credentials}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De dynamische implementatie van de Registratie API van de Cliënt wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

## Verzoek {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/o/client/register</td>
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
      <td style="background-color: #DEEBFF;">software_statement</td>
      <td>
            De softwareverklaring verbonden aan de geregistreerde toepassing die van <a href="https://experience.adobe.com/#/pass/authentication"> wordt gecreeerd en van Dashboard van Adobe Pass TVE </a> wordt gedownload.
            <br/><br/>
            Het beheer van geregistreerde toepassingen wordt beschreven in de <a href="../dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirect_uri</td>
      <td>De omleiding URI verbonden aan de plaats waaraan de gebruikersagent navigeert wanneer de authentificatiestroom wordt voltooid.</td>
      <td>optioneel</td>
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
         Het moet application/json zijn.
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
               <td style="background-color: #DEEBFF;">client_id</td>
               <td>De id-tekenreeks van de clienttoepassing.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_geheime</td>
               <td>De geheime tekenreeks van de clienttoepassing.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id_issued_at</td>
               <td>Het tijdstip waarop de id van de clienttoepassing is uitgegeven.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">redirect_uris</td>
               <td>De array van URI-tekenreeksen voor omleiding die de clienttoepassing kan gebruiken in op omleiding gebaseerde stromen.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Grant_types</td>
               <td>Het subsidietype koorden die de cliënttoepassing voor het cliënt symbolische eindpunt kan gebruiken.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">bereik</td>
               <td>De bereiktekenreeksen die de Adobe Pass-verificatie-API's definiëren die de clienttoepassing kan gebruiken.</td>
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
                        <li>De aanvraag bevat een niet-ondersteunde parameterwaarde.</li>
                        <li>De aanvraag herhaalt een parameter.</li>
                        <li>Het verzoek is onjuist geformuleerd.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_redirect_uri</td>
               <td>De aanvraag bevat een ongeldige waarde voor de omleiding-URI.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_software_statement</td>
               <td>De aanvraag bevat een ongeldige waarde voor de softwareinstructie.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsigned_software_statement</td>
               <td>De aanvraag bevat een waarde voor de softwareinstructie die niet is goedgekeurd voor gebruik door de Adobe Pass Authentication-server.</td>
            </tr>
         </table>
      </td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples}

### Client-referenties ophalen {#samples-retrieve-client-credentials}

>[!BEGINTABS]

>[!TAB  Verzoek ]

```HTTPS
POST /o/client/register HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/json
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

{
    "software_statement": "eyJhbGciOiJSUzI1NiJ9.
        eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
        bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
        aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
        GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
        zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
        5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
        fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
        U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
        IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
    "redirect_uri": "adobepass://com.programmer"  
 }
```

>[!TAB  Reactie - Succes ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "client_id": "s6BhdRkqt3",
    "client_secret": "t7AkePiru4",
    "redirect_uris": [
        "app://com.programmer.adobe#sdasdsadas"
    ],
    "grant_types": [
        "client_credentials"
    ],
    "scopes": [
        "api:client:v2"
    ],
    "client_id_issued_at": 1723227212
}
```

>[!TAB  Reactie - Fout ]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
