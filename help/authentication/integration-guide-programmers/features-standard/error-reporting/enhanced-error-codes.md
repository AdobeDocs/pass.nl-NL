---
title: Verbeterde foutcodes
description: Verbeterde foutcodes
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 7ac04991289c95ebb803d1fd804e9b497f821cda
workflow-type: tm+mt
source-wordcount: '2696'
ht-degree: 2%

---

# Verbeterde foutcodes {#enhanced-error-codes}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Enhanced Error Codes vertegenwoordigen een Adobe Pass-verificatiefunctie die aanvullende foutinformatie biedt voor clienttoepassingen die zijn geïntegreerd met:

* Adobe Pass-verificatie REST-API&#39;s:
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(Verouderd) REST API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass-verificatie-SDK&#39;s autoriseren-API:
   * [(Verouderd) JavaScript SDK (voorheen-API)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [(Verouderd) iOS/tvOS SDK (API voor voorafgaande autorisatie)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [(Verouderd) Android SDK (voorheen-API)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) Vooraf autoriseren API is de enige Adobe Pass Authentication SDK API die ondersteuning biedt voor Enhanced Error Codes._

>[!IMPORTANT]
>
> Toepassingen waarin Adobe Pass Authentication REST API v2 is geïntegreerd, profiteren standaard van Enhanced Error Codes zonder dat hiervoor extra configuratie nodig is.
>
> <br/>
>
> Toepassingen waarin de Adobe Pass Authentication REST API v1 of SDK&#39;s Preauthorize API zijn geïntegreerd, kunnen alleen profiteren van de uitgebreide foutcodes als de functie expliciet is ingeschakeld.
>
> <br/>
>
> Om deze eigenschap uitdrukkelijk toe te laten, creeer een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) voor hulp.

## Vertegenwoordiging {#enhanced-error-codes-representation}

Enhanced Error Codes kunnen worden weergegeven in de `JSON` - of `XML` -indeling, afhankelijk van de geïntegreerde Adobe Pass Authentication API en de gebruikte headerwaarde Accept (d.w.z. `application/json` of `application/xml`):

| Adobe Pass-verificAPI | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &amp;check; |         |
| REST API v1 | &amp;check; | &amp;check; |
| API voor voorafgaande autorisatie van SDK&#39;s | &amp;check; |         |

>[!IMPORTANT]
>
> Adobe Pass-verificatie kan uitgebreide foutcodes in twee formulieren doorgeven aan clienttoepassingen:
>
> <br/>
>
> * **Top-level fouteninformatie**: In dit geval, wordt het ***&quot;fout&quot;*** voorwerp gevestigd op het hoogste niveau, daarom kan het antwoordlichaam slechts het ***&quot;fout&quot;*** voorwerp bevatten.
> * **punt-vlakke fouteninformatie**: In dit geval, wordt het ***&quot;fout&quot;*** voorwerp gevestigd op het puntenniveau, daarom kan het antwoordlichaam een ***&quot;fout&quot;*** voorwerp voor alle punten bevatten die een fout terwijl het worden onderhouden ervaren.
>
> <br/>
>
> Controleer de openbare documentatie voor elke geïntegreerde Adobe Pass Authentication API om de representatiespecificaties van de Enhanced Error Codes te bepalen.

**REST API v2**

Raadpleeg de volgende HTTP-reacties met voorbeelden van Enhanced Error Codes, weergegeven als `JSON` van toepassing op REST API v2.

>[!BEGINTABS]

>[!TAB  REST API v2 - punt-vlakke fouteninformatie (JSON) ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB  REST API v2 - Top-level fouteninformatie (JSON) ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST API v1**

Raadpleeg de volgende HTTP-reacties met voorbeelden van Enhanced Error Codes, weergegeven als `JSON` of `XML` van toepassing op REST API v1.

>[!BEGINTABS]

>[!TAB  REST API v1 - punt-vlakke fouteninformatie (JSON) ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB  REST API v1 - top-level fouteninformatie (JSON) ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB  REST API v1 - Top-level fouteninformatie (XML) ]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### Structuur {#enhanced-error-codes-representation-structure}

Uitgebreide foutcodes bevatten de volgende `JSON` -velden of `XML` -kenmerken met voorbeelden:

| Naam | Type | Voorbeeld | Beperkt | Beschrijving |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *actie* | *koord* | *niets* | &amp;check; | De Adobe Pass-verificatie raadt u aan een actie uit te voeren om de situatie zoals die in dit document is gedefinieerd, te verhelpen. <br/><br/> voor meer details, verwijs naar de [ sectie van de Actie ](#enhanced-error-codes-action). |
| *status* | *geheel* | *403* | &amp;check; | De code van de de reactiestatus van HTTP zoals die in [ wordt bepaald RFC 7231 ](https://tools.ietf.org/html/rfc7231#section-6) document. <br/><br/> voor meer details, verwijs naar de [ 2} sectie van de Status {.](#enhanced-error-codes-status) |
| *code* | *koord* | *authentication_deny_by_mvpd* | &amp;check; | De unieke identificatiecode van de Adobe Pass-verificatie die is gekoppeld aan de fout zoals gedefinieerd in dit document. <br/><br/> voor meer details, verwijs naar de [ sectie van de Code ](#enhanced-error-codes-code). |
| *bericht* | *koord* | *MVPD is een &quot;Weigeren&quot;besluit teruggekeerd wanneer het verzoeken van vergunning voor het gespecificeerde middel* |            | Het leesbare bericht dat in sommige gevallen aan de eindgebruiker kan worden weergegeven. <br/><br/> voor meer details, verwijs naar de [ Behandeling van de Reactie ](#enhanced-error-codes-response-handling) sectie. |
| *details* | *koord* | *Uw abonnementspakket omvat niet het &quot;Levende&quot;kanaal* |            | Het gedetailleerde bericht dat in sommige gevallen door een dienstenpartner kon worden verstrekt, <br/><br/> Dit gebied zou niet aanwezig kunnen zijn voor het geval de de dienstenpartner geen douanebericht verstrekt. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | De openbare documentatie URL van de Authentificatie van Adobe Pass die met meer informatie over verbindt waarom deze fout en mogelijke oplossingen voorkwam. <br/><br/> Dit veld bevat een absolute URL en mag niet worden afgeleid van foutcode, afhankelijk van de foutcontext kan een andere URL worden opgegeven. |
| *spoor* | *koord* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | De unieke id voor de reactie die kan worden gebruikt wanneer contact wordt opgenomen met de ondersteuning van Adobe Pass-verificatie om specifieke problemen op te lossen. |

>[!IMPORTANT]
>
> De **Beperkte** kolom wijst erop als het respectieve gebied een waarde van een eindige reeks houdt, terwijl de onbeperkte gebieden om het even welke gegevens kunnen bevatten.
>
> <br/>
>
> In toekomstige updates van dit document kunnen waarden aan de eindige sets worden toegevoegd, maar worden bestaande sets niet verwijderd of gewijzigd.

### Handeling {#enhanced-error-codes-representation-action}

De uitgebreide foutcodes bevatten een veld &quot;Handeling&quot; met een aanbevolen handeling die de situatie zou kunnen verhelpen.

De mogelijke waarden voor het veld &quot;Handeling&quot; zijn:

| Handeling | Beschrijving | Categorie |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| none | Er is geen vooraf gedefinieerde actie om dit probleem op te lossen, maar in sommige gevallen kan dit wijzen op een onjuiste aanroep van de API. | Verbeter de verzoekcontext. |
| configuratie | Voor de clienttoepassing is een configuratiewijziging vereist. Deze wijziging vindt meestal plaats via het Adobe Pass TVE-dashboard. | Verbeter de context van de integratieconfiguratie. |
| registratie van aanvragen | De clienttoepassing moet zich opnieuw registreren. | Verbeter de context van de clienttoepassing. |
| verificatie | De clienttoepassing moet de gebruiker verifiëren of opnieuw verifiëren. | Verbeter de context van de clienttoepassing. |
| autorisatie | De cliënttoepassing vereist om vergunning voor het gespecificeerde middel te verkrijgen. | Verbeter de context van de clienttoepassing. |
| opnieuw proberen | De clienttoepassing moet de aanvraag opnieuw proberen. | Verbeter de verzoekcontext. |

_(*) Voor sommige fouten, zouden de veelvoudige acties mogelijke oplossingen kunnen zijn, maar het gebied van de &quot;actie&quot;wijst met de hoogste waarschijnlijkheid aan om de fout te bevestigen._

### Status {#enhanced-error-codes-representation-status}

Uitgebreide foutcodes bevatten een veld &quot;status&quot; dat de HTTP-statuscode aangeeft die aan de fout is gekoppeld.

De mogelijke waarden voor het veld status zijn:

| Code | Reden/woordgroep |
|------|-----------------------|
| 400 | Ongeldig verzoek |
| 401 | Ongeautoriseerd |
| 403 | Verboden |
| 404 | Niet gevonden |
| 405 | Methode niet toegestaan |
| 410 | Gone |
| 412 | Voorwaarde is mislukt |
| 500 | Interne serverfout |

Verbeterde foutcodes met een 4xx-&quot;status&quot; worden meestal weergegeven wanneer de fout door de client wordt gegenereerd en meestal impliceert dit dat de client extra werk nodig heeft om de fout te verhelpen.

Uitgebreide foutcodes met een &quot;status&quot; van 5 xx worden meestal weergegeven wanneer de fout door de server wordt gegenereerd en het grootste deel van de tijd dat dit impliceert dat de server extra werk nodig heeft om de fout te verhelpen.

>[!IMPORTANT]
>
> Er zijn gevallen waarin de statuscode van het HTTP-antwoord afwijkt van het veld &#39;status&#39; van de uitgebreide foutcode, met name bij interactie met een Adobe Pass-verificatie-API die uitgebreide foutcodes als foutinformatie op itemniveau communiceert.

### Code {#enhanced-error-codes-representation-code}

Uitgebreide foutcodes bevatten een veld &quot;code&quot; met een unieke id voor Adobe Pass-verificatie die aan de fout is gekoppeld.

De mogelijke waarden voor het &quot;code&quot;gebied worden samengevoegd [ hieronder ](#enhanced-error-codes-list) in twee lijsten die op de geïntegreerde Authentificatie API van Adobe Pass worden gebaseerd.

## Lijsten {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

In de onderstaande tabel worden mogelijke Enhanced Error Codes weergegeven die een clienttoepassing kan tegenkomen bij integratie met Adobe Pass Authentication REST API v2.

| Handeling | Code | Status | Bericht |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **niets** | *invalid_parameter_service_provider* | 400 | De parameterwaarde van het prepress-bureau ontbreekt of is ongeldig. |
|                              | *invalid_parameter_mvpd* | 400 | De parameterwaarde mvpd ontbreekt of is ongeldig. |
|                              | *invalid_parameter_code* | 400 | De waarde van de codeparameter ontbreekt of is ongeldig. |
|                              | *invalid_parameter_resources* | 400 | De parameterwaarde voor resources ontbreekt of is ongeldig. |
|                              | *invalid_parameter_redirect_url* | 400 | De waarde van de parameter redirect URL ontbreekt of is ongeldig. |
|                              | *invalid_parameter_partner* | 400 | De parameterwaarde van de partner ontbreekt of is ongeldig. |
|                              | *invalid_parameter_saml_response* | 400 | De parameterwaarde voor SAML-reactie ontbreekt of is ongeldig. |
|                              | *invalid_header_device_info* | 400 | De koptekstwaarde voor apparaatinformatie ontbreekt of is ongeldig. |
|                              | *invalid_header_device_identifier* | 400 | De koptekstwaarde van de apparaat-id ontbreekt of is ongeldig. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | De identiteit voor de koptekstwaarde voor tijdelijke toegang ontbreekt of is ongeldig. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | De statuswaarde van de toestemmingstoegang van de de statuskopbal van het partnerkader is niet aanwezig. |
|                              | *invalid_header_pfs_permission_access_not_determine* | 400 | De statuswaarde van de toestemmingstoegang van de de statuskopbal van het partnerkader is onbepaald. |
|                              | *invalid_header_pfs_permission_access_not_allowed* | 400 | De de statuswaarde van de toestemmingstoegang van de de statuskopbal van het partnerkader wordt niet verleend. |
|                              | *invalid_header_pfs_provider_id_not_determine* | 400 | De waarde van leverancier id van de de statuskopbal van het partnerkader wordt niet geassocieerd met bekende mvpd. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | De waarde van leverancier id van de de statuskopbal van het partnerkader past mvpd niet aan die als parameter wordt verzonden. |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | De leveranciersinformatie van de de statuskopbal van het partnerkader is verlopen. |
|                              | *invalid_integration* | 400 | De integratie tussen de opgegeven serviceprovider en mvpd bestaat niet of is uitgeschakeld. |
|                              | *invalid_authentication_session* | 400 | De verificatiesessie die aan dit verzoek is gekoppeld, ontbreekt of is ongeldig. |
|                              | *preauthentication_deny_by_mvpd* | 403 | De MVPD heeft een &quot;Weigeren&quot;-besluit geretourneerd wanneer zij een voorafgaande toestemming voor de opgegeven bron aanvraagt. |
|                              | *authentication_deny_by_mvpd* | 403 | De MVPD heeft een &quot;Weigeren&quot;-beslissing geretourneerd wanneer een aanvraag voor een vergunning voor de opgegeven bron wordt ingediend. |
|                              | *authentication_deny_by_parental_controls* | 403 | De MVPD heeft een &quot;Weigeren&quot;besluit wegens ouderlijke controlemontages voor de gespecificeerde middel teruggegeven. |
|                              | *authentication_deny_by_degradate_rule* | 403 | De integratie tussen de gespecificeerde dienstverlener en mvpd heeft een degradatieregel wordt toegepast die vergunning voor de gevraagde middelen ontkent. |
|                              | *internal_server_error* | 500 | De aanvraag is mislukt vanwege een interne serverfout. |
| **configuratie** | *too_many_resources* | 403 | De autorisatie- of voorafgaande autorisatieaanvraag is mislukt omdat er te veel bronnen zijn gevraagd. Neem contact op met het ondersteuningsteam om de beperkingen voor autorisatie en autorisatie correct te configureren. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | De configuratie van het gebruikerscertificaat voor metagegevens ontbreekt of is ongeldig. |
|                              | *invalid_configuration_temporary_access* | 500 | De tijdelijke toegangsconfiguratie is ongeldig. |
|                              | *invalid_configuration_platform* | 500 | De platformconfiguratie ontbreekt of is ongeldig voor integratie. |
|                              | *invalid_configuration_platform_id* | 500 | De configuratie van de platform-id ontbreekt of is ongeldig. |
|                              | *invalid_configuration_platform_trait* | 500 | De configuratie van de platformeigenschap ontbreekt of is ongeldig. |
|                              | *invalid_configuration_platform_category_trait* | 500 | De standaardconfiguratie van de platformcategorie ontbreekt of is ongeldig. |
|                              | *invalid_configuration_platform_services* | 500 | De configuratie van de platformservices ontbreekt of is ongeldig voor integratie. |
|                              | *invalid_configuration_mvpd_platform* | 500 | De configuratie van het mvpd-platform ontbreekt of is ongeldig voor mvpd en het platform. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | De statusconfiguratie voor instaptoegang via het mvpd-platform ontbreekt of is ongeldig voor mvpd en het platform. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | De configuratie voor het uitwisselen van profielen voor mvpd-platformen ontbreekt of is ongeldig voor mvpd en platform. |
| **toepassing-registratie** | *invalid_access_token_service_provider* | 401 | Het toegangstoken is ongeldig vanwege een ongeldige serviceprovider. |
|                              | *invalid_access_token_client_application* | 401 | Het toegangstoken is ongeldig vanwege een ongeldige clienttoepassing. |
| **authentificatie** | *authenticated_profile_missing* | 403 | Het geverifieerde profiel voor deze aanvraag ontbreekt. |
|                              | *authenticated_profile_expired* | 403 | Het geverifieerde profiel voor deze aanvraag is verlopen. |
|                              | *authenticated_profile_invalidate* | 403 | Het geverifieerde profiel dat aan dit verzoek is gekoppeld, is ongeldig. |
|                              | *temporary_access_duration_limit_over* | 403 | De tijdelijke toegangslimiet is overschreden. |
|                              | *temporary_access_resources_limit_over* | 403 | De limiet voor tijdelijke toegangsmiddelen is overschreden. |
|                              | *authentication_deny_by_hba_policies* | 403 | De MVPD heeft een &quot;Weigeren&quot;besluit wegens op huis-gebaseerd authentificatiebeleid teruggegeven. De huidige authentificatie werd verkregen door een op huis-gebaseerde authentificatiestroom en maar het apparaat is niet meer in-huis wanneer het verzoeken van om toestemming voor het gespecificeerde middel. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                              | *authentication_deny_by_session_invalidate* | 403 | De verificatiesessie is ongeldig gemaakt door de identiteitsprovider. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                              | *identity_not_recognized_by_mvpd* | 403 | Het verzoek om toestemming is mislukt omdat de identiteit van de gebruiker niet door de MVPD is erkend. |
| **opnieuw proberen** | *network_receive_error* | 403 | Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|                              | *network_connection_timeout* | 403 | Er was een verbindingsonderbreking met de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|                              | *maximum_executing_time_overtroffen* | 403 | De aanvraag is niet binnen de maximaal toegestane tijd voltooid. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |

### (Verouderd) REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

In de onderstaande tabel worden mogelijke Enhanced Error Codes weergegeven die een clienttoepassing kan tegenkomen bij integratie met Adobe Pass Authentication REST API v1.

| Handeling | Code | Status | Bericht |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **niets** | *invalid_requestor* | 400 | De parameter requestor ontbreekt of is ongeldig. |
|                    | *invalid_device_info* | 400 | De apparaatinformatie ontbreekt of is ongeldig. |
|                    | *invalid_device_id* | 400 | De apparaat-id ontbreekt of is ongeldig. |
|                    | *missing_resource* | 400 412 | De parameter resource ontbreekt. |
|                    | *misformed_authz_request* | 400 412 | Autorisatieaanvraag is null of ongeldig. |
|                    | *preauthentication_deny_by_mvpd* | 403 | De MVPD heeft een &quot;Weigeren&quot;-besluit geretourneerd wanneer zij een voorafgaande toestemming voor de opgegeven bron aanvraagt. |
|                    | *authentication_deny_by_mvpd* | 403 | De MVPD heeft een &quot;Weigeren&quot;-beslissing geretourneerd wanneer een aanvraag voor een vergunning voor de opgegeven bron wordt ingediend. |
|                    | *authentication_deny_by_parental_controls* | 403 | De MVPD heeft een &quot;Weigeren&quot;besluit wegens ouderlijke controlemontages voor de gespecificeerde middel teruggegeven. |
|                    | *internal_error* | 400 405 500 | De aanvraag is mislukt vanwege een interne serverfout. |
| **configuratie** | *unknown_integration* | 400 412 | De integratie tussen de opgegeven programmeur en identiteitsprovider bestaat niet. Gebruik het TVE-dashboard om de vereiste integratie te maken. |
|                    | *too_many_resources* | 403 | De autorisatie- of voorafgaande autorisatieaanvraag is mislukt omdat er te veel bronnen zijn gevraagd. Neem contact op met het ondersteuningsteam om de beperkingen voor autorisatie en autorisatie correct te configureren. |
| **authentificatie** | *authentication_session_publisher_mismatch* | 400 | De autorisatieaanvraag is mislukt omdat de aangegeven MVPD voor de autorisatiestroom anders is dan de die de autorisatiesessie heeft afgegeven. De gebruiker moet opnieuw verifiëren met de gewenste MVPD om verder te kunnen gaan. |
|                    | *authentication_deny_by_hba_policies* | 403 | De MVPD heeft een &quot;Weigeren&quot;besluit wegens op huis-gebaseerd authentificatiebeleid teruggegeven. De huidige authentificatie werd verkregen gebruikend een op huis-gebaseerde authentificatiestroom (HBA) maar het apparaat is niet meer thuis wanneer het verzoeken van om toestemming voor het gespecificeerde middel. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *authentication_deny_by_session_invalidate* | 403 | De verificatiesessie is ongeldig gemaakt door de identiteitsprovider. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *identity_not_recognized_by_mvpd* | 403 | Het verzoek om toestemming is mislukt omdat de identiteit van de gebruiker niet door de MVPD is erkend. |
|                    | *authentication_session_invalidate* | 403 | De verificatiesessie is ongeldig gemaakt door de identiteitsprovider. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *authentication_session_missing* | 403 412 | De verificatiesessie die aan deze aanvraag is gekoppeld, kan niet worden opgehaald. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *authentication_session_expired* | 403 412 | De huidige verificatiesessie is verlopen. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *preauthentication_authentication_session_missing* | 412 | De verificatiesessie die aan deze aanvraag is gekoppeld, kan niet worden opgehaald. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
|                    | *preauthentication_authentication_session_expired* | 412 | De huidige verificatiesessie is verlopen. De gebruiker moet opnieuw verifiëren met een ondersteunde MVPD om door te kunnen gaan. |
| **vergunning** | *authentication_not_found* | 403 404 | Er is geen autorisatie gevonden voor de opgegeven resource. De gebruiker moet een nieuwe vergunning verkrijgen om verder te gaan. |
|                    | *authentication_expired* | 410 | De vorige autorisatie voor de opgegeven resource is verlopen. De gebruiker moet een nieuwe vergunning verkrijgen om verder te gaan. |
| **opnieuw proberen** | *network_receive_error* | 403 | Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|                    | *network_connection_timeout* | 403 | Er was een verbindingsonderbreking met de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|                    | *maximum_executing_time_overtroffen* | 403 | De aanvraag is niet binnen de maximaal toegestane tijd voltooid. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |

### (Verouderd) SDK&#39;s autoriseren API vooraf {#enhanced-error-codes-lists-sdks-preauthorize-api}

Verwijs naar de vorige [ sectie ](#enhanced-error-codes-list-rest-api-v1) voor mogelijke Verbeterde Codes van de Fout een cliënttoepassing zou kunnen ontmoeten wanneer geïntegreerd met de Authentificatie SDKs van Adobe Pass preauthorize API.

## Reactieafhandeling {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Er zijn Verbeterde Codes van de Fout die automatisch in code van de cliënttoepassing kunnen worden behandeld, zoals het opnieuw proberen van een vergunningsverzoek in het geval van een netwerkonderbreking of vereisen de gebruiker om opnieuw voor authentiek te verklaren wanneer hun zitting is verlopen, maar andere types zouden configuratieveranderingen of de interactie van het team van de klantenzorg van de Authentificatie van Adobe Pass kunnen vereisen.
>
> <br/>
>
> Daarom is het belangrijk om volledige fouteninformatie te verzamelen en te verstrekken wanneer het creëren van een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com), om ervoor te zorgen dat de noodzakelijke veranderingen alvorens de nieuwe toepassing of de nieuwe eigenschap worden aangebracht.

Samenvattend, wanneer het behandelen van reacties die de Geavanceerde Codes van de Fout bevatten, zou u het volgende moeten overwegen:

1. **Agnostic aan API die de fout** terugkeren: Voer een gecentraliseerde fout-behandelende logica uit die de volledige catalogus van verbeterde foutencodes steunt, ongeacht welke API hen veroorzaakt. Verschillende uitgebreide foutcode worden gedeeld door de API&#39;s en moeten consistent worden afgehandeld.

1. **Agnostisch aan top-level versus punt-niveau fouteninformatie**: Behandel top-level en punt-vlakke fouteninformatie agnostisch aan de manier het wordt meegedeeld, zorg ervoor u beide vormen van het overbrengen van de Verbeterde Codes van de Fout kunt behandelen.

1. **Controle beide statuswaarden**: Controleer altijd zowel de code van de de reactiestatus van HTTP als het Verbeterde gebied van de Code van de Fout &quot;status&quot;. Ze kunnen verschillen en beide bieden waardevolle informatie.

1. **probeert logica** opnieuw: Voor fouten die een herpoging vereisen, zorg ervoor dat de pogingen (d.w.z. 2-3) beperkt zijn of met exponentiële steun worden gedaan om het overweldigen van de server te vermijden. In het geval van Adobe Pass Authentication API&#39;s die meerdere items tegelijk verwerken (bijvoorbeeld API vooraf autoriseren), moet u in de herhaalde aanvraag ook alleen die items opnemen die zijn gemarkeerd met &quot;retry&quot; en niet de volledige lijst.

1. **de veranderingen van de Configuratie**: Voor fouten die configuratieveranderingen vereisen, zorg ervoor dat de noodzakelijke veranderingen worden aangebracht alvorens de nieuwe toepassing of de nieuwe eigenschap te lanceren.

1. **Authentificatie en vergunning**: Voor fouten met betrekking tot authentificatie en vergunning, moet u de gebruiker ertoe aanzetten opnieuw voor authentiek te verklaren of nieuwe vergunning te verkrijgen zoals nodig.

1. **Gebruiker terugkoppelt**: Facultatief, gebruik het mens-leesbare &quot;bericht&quot;en (potentiële) &quot;details&quot;gebieden om de gebruiker over de kwestie te informeren. Het tekstbericht &quot;details&quot; kan worden doorgegeven vanuit de eindpunten van de MVPD-autorisatie of -autorisatie of vanuit de programmeur wanneer afbraakregels worden toegepast.
