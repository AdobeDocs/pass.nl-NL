---
title: Profiel ophalen voor specifieke mvpd
description: REST API V2 - Profiel ophalen voor specifieke mvpd
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 0%

---


# Profiel ophalen voor specifieke mvpd {#retrieve-profile-for-specific-mvpd}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

## Verzoek {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/api/v2/{serviceProvider}/profiles/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Padparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van de toonder symbolische nuttige lading wordt beschreven in de <a href="../../../dynamic-client-registration-api.md"> Dynamische documentatie van de Registratie van de Cliënt </a>.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-apparaat-id</td>
      <td>De generatie van de apparatenherkenningstekenlading wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md"> AP-apparaat-Herkenningsteken </a> documentatie.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Apparaat-Info</td>
      <td>
         De generatie van de lading van de apparateninformatie wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md"> x-apparaat-Info </a> documentatie.
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
        De generatie van enige sign-on lading voor de methode van de Identiteit van het Platform wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md"> Adobe-Onderwerp-Symbolische </a> documentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen die een platformidentiteit gebruiken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> Enige sign-on gebruikend de stromen van de platformidentiteit </a> documentatie.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        De generatie van enige sign-on lading voor de Symbolische methode van de Dienst wordt beschreven in de <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md"> AD-dienst-Symbolische </a> documentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen gebruikend een de dienstteken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> Enige sign-on gebruikend de stromen van het de dienstteken </a> documentatie.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Kader-Status</td>
      <td>
        De generatie van enige sign-on lading voor de methode van de Partner wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md"> AP-partner-kader-Status </a> documentatie.
        <br/><br/>
        Voor meer details over enige sign-on toegelaten stromen die een partner gebruiken, verwijs naar <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> Enige sign-on gebruikend de documentatie van de partnerstromen </a>.</td>
      <td>optioneel</td>
    </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>De generatie van de gebruiker unieke herkenningstekenlading wordt beschreven in <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md"> AP-TempPass-Identity </a> documentatie.</td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Code</th>
      <th style="background-color: #EFF2F7; width: 20%;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        De hoofdtekst van de reactie bevat een kaart met geldige profielen, die leeg kan zijn.
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopteksten</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Lichaam</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profielen</td>
      <td>
        JSON met een overzicht van sleutel-, waardeparen.
        <br/><br/>
        Het hoofdelement wordt gedefinieerd door de volgende waarde:
        <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Waarde</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>De interne unieke id die tijdens het instapproces aan de identiteitsprovider is gekoppeld.</td>
               <td><i>vereist</i></td>
            </tr>
         </table>
         Het element value wordt gedefinieerd door de volgende kenmerken:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Kenmerk</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>De tijdstempel waarvóór het profiel niet geldig is.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>De tijdstempel waarna het profiel niet geldig is.</td>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">uitgever</td>
               <td>
                  De entiteit die eigenaar is van het profiel.
                  <br/><br/>
                  Mogelijke waarden zijn:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Waarde</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd <br/><br/> b.v., Spectrum, Cablevision, enz.</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Basisverificatie</li>
                                <li>Single Sign-On met platformidentiteit</li>
                                <li>Single Sign-On met servicetoken</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Adobe</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Verminderde toegang</li>
                                <li>Tijdelijke toegang</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Apple</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Single Sign-On met partner Apple</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Het type profiel.
                  <br/><br/>
                  Mogelijke waarden zijn:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Waarde</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">regelmatig</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Basisverificatie</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">aangetast</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Verminderde toegang</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">tijdelijk</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Tijdelijke toegang</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">appleSSO</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Single Sign-On met partner Apple</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">platformSSO</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Single Sign-On met platformidentiteit</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">serviceTokenSSO</td>
                        <td>
                            Het profiel is gemaakt als resultaat van:
                            <ul>
                                <li>Single Sign-On met servicetoken</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>vereist</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">attributes</td>
               <td>
                    De lijst met metagegevenskenmerken van de gebruiker.
                    <br/><br/>
                    Deze kenmerken kunnen zijn:
                    <ul>
                        <li>Verplicht, zoals 'userId'</li>
                        <li>Niet-verplicht, zoals 'zip', 'familyId', 'maxRating', enz.</li>
                    </ul>
                    De waarden voor de kenmerken kunnen zijn:
                    <ul>
                        <li>eenvoudig</li>
                        <li>list</li>
                        <li>map</li>
                    </ul>
               </td>
               <td><i>vereist</i></td>
            </tr>
         </table>
      </td>
      <td><i>vereist</i></td>
</table>

### Fout {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Lichaam</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">fout</td>
      <td>De fout verstrekt extra informatie die aan de <a href="../../../enhanced-error-codes.md"> Verbeterde documentatie van de Codes van de Fout </a> volgt.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

## Voorbeelden {#samples}

### 1. Hiermee worden alle bestaande en geldige geverifieerde profielen opgehaald die zijn verkregen via basisverificatie voor specifieke mvpd

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/Spectrum  

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Spectrum" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Spectrum",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Haal alle bestaande en geldige geverifieerde profielen op, inclusief de profielen die zijn verkregen via Single Sign-On-verificatie met de methode Servicetokken voor specifieke mvpd

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/AdobeShibboleth  

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AD-Service-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Hiermee worden alle bestaande en geldige geverifieerde profielen opgehaald, inclusief profielen die zijn verkregen via Single Sign-On-verificatie met de methode Platform Identity voor specifieke mvpd

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/AdobePass_SMI  
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Adobe-Subject-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8    
 
{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 4. Profielgegevens ophalen voor tijdelijke doorgifte

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/TempPass_TEST40
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie - Beschikbaar ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697718650206,
            "notAfter": 1697718710206,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697718710206,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB  Reactie - Begonnen ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697719584085,
            "notAfter": 1697719704085,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697719704085,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB  Reactie - Verlopen ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8    
 
{
    "status": 200,
    "code": "temppass_expired",
    "message": "TempPass has expired.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB  Reactie - Ongeldige Configuratie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 5. Profielgegevens ophalen voor tijdelijke promotiedoorgifte

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/flexibleTempPass
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie - Beschikbaar ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 5,
                    "state": "plain"
                },
                "used_assets": {
                    "value": 0,
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697719102666,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB  Reactie - Begonnen ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697720528524,
            "notAfter": 1697720588524,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 1,
                    "state": "plain"
                },
                "used_assets": {
                    "value": [
                        "res04",
                        "res02",
                        "res03",
                        "res01"
                    ],
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697720528524,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB  Reactie - Verlopen ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "status": 200,
    "code": "temppass_expired",
    "message": "TempPass has expired.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB  Reactie - Verbruikte ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_max_resources_exceeded",
                "message": "Flexible TempPass maximum resources exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB  Reactie - Ongeldige Configuratie ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB  Reactie - Ongeldige Identiteit ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "status": 400,
    "code": "temppass_invalid_identity",
    "message": "TempPass is not available for the specified identity.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 6. Profielgegevens ophalen voor gedegradeerde mvpd

>[!BEGINTABS]

>[!TAB  Verzoek ]

```JSON
GET /api/v2/REF30/profiles/degradedMvpd
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  Reactie - Verslechtering AuthNAll ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles": {
        "degradedMvpd": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes":
                "userID": {
                    "value": "95cf93bcd183214a0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

**Nota:** 95cf93bcd183214a is een degradatie specifiek voorvoegsel.

>[!ENDTABS]
