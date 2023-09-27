---
title: Dynamic Client-registratie-API
description: Dynamic Client-registratie-API
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Dynamic Client-registratie-API {#dynamic-client-registration-api}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

Op dit moment zijn er twee manieren waarop Adobe Pass-verificatie toepassingen identificeert en registreert:

* browsergebaseerde clients worden geregistreerd via toegestane [domeinlijst](/help/authentication/programmer-overview.md)
* native toepassingsclients, zoals iOS- en Android-toepassingen, worden geregistreerd via het mechanisme voor ondertekende aanvragers.

Adobe Pass Authentication stelt een nieuw mechanisme voor voor de registratie van toepassingen. Dit mechanisme wordt in de volgende alinea&#39;s beschreven.

## Het aanvraagregistratiemechanisme {#appRegistrationMechanism}

### Technische redenen {#reasons}

Het verificatiemechanisme bij Adobe Pass-verificatie was gebaseerd op sessiecookies, maar vanwege [Aangepaste tabbladen van Android-chroom](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}Dit doel kan echter niet meer worden bereikt.

Gezien deze beperkingen introduceert de Adobe een nieuw registratiemechanisme voor al haar cliënten. Het is gebaseerd op OAuth 2.0 RFC en bestaat uit de volgende stappen:

1. De softwareinstructie ophalen van het TVE-dashboard
1. Klantenreferenties ophalen
1. Toegangstoken verkrijgen

### Softwareinstructie ophalen van TVE-dashboard {#softwareStatement}

Voor elke toepassing die u vrijgeeft, moet u een softwareverklaring verkrijgen. Alle softwareinstructies worden geleverd via het TVE-dashboard, zodra toepassingen zijn gemaakt. De softwareverklaring zou samen met de toepassing op het apparaat van de gebruiker moeten worden opgesteld.

>[!IMPORTANT]
>
>Wanneer u een softwareinstructie gebruikt, is het id-mechanisme van de ondertekende aanvrager niet meer nodig.

Ga voor meer informatie over het maken van softwareinstructies naar [Clientregistratie op het TVE-dashboard](/help/authentication/dynamic-client-registration.md).

### Klantenreferenties verkrijgen {#clientCredentials}

Nadat u een softwareinstructie hebt opgehaald van het TVE-dashboard, moet u uw toepassing registreren bij de Adobe Pass-verificatieserver. Doe dit door een /register vraag uit te voeren en uw unieke cliënt herkenningsteken terug te winnen.

**Verzoek**

| HTTP-aanroep |                    |
|-----------|--------------------|
| pad | /o/client/register |
| methode | POST |

| velden |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | De softwareinstructie die is gemaakt in het TVE-dashboard. | verplicht |
| redirect_uri | De URI die de toepassing gebruikt om de verificatiestroom te voltooien. | optioneel |

| aanvraagheaders |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Inhoudstype | application/json | verplicht |
| X-Apparaat-Info | De apparaatgegevens zoals gedefinieerd in het Doorgeven van apparaat- en verbindingsgegevens | verplicht |
| Gebruikersagent | De gebruikersagent | verplicht |

**Antwoord**

| antwoordheaders |                  |           |
|------------------|------------------|-----------|
| Inhoudstype | application/json | verplicht |

| responsvelden |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | String | verplicht |
| client_geheime | String | verplicht |
| client_id_issued_at | lang | verplicht |
| redirect_uris | lijst met tekenreeksen | verplicht |
| Grant_types | lijst met tekenreeksen<br/> **geaccepteerde waarde**<br/> `client_credentials`: Wordt gebruikt door onveilige clients, zoals de Android SDK. | verplicht |
| fout | **geaccepteerde waarden**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unsigned_software_statement</li></ul> | verplicht in een foutstroom |


#### Foutreactie {#error-response}

In het geval van een fout reageert de registratieserver met de HTTP 400-statuscode (Bad Request) en bevat deze de volgende parameters in de reactie:

| statuscode | responsorgaan | beschrijving |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | In de aanvraag ontbreekt een vereiste parameter, wordt een niet-ondersteunde parameterwaarde opgenomen, wordt een parameter herhaald of wordt de parameter op een andere manier onjuist geformuleerd. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Redirect_uri is niet toegestaan voor deze cliënt die op zijn geregistreerde toepassing wordt gebaseerd. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | De software-instructie is niet geldig. |
| HTTP 400 | {&quot;error&quot;: &quot;unsigned_software_statement&quot;} | Software_id wordt niet gevonden in de configuratie. |

#### Voorbeeld van clientreferenties {#client-credentials-example}

**Verzoek:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
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
  "redirect_uri": "adobepass://com.programmer"  }
```

**Reactie:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Foutreactie:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Toegangstoken verkrijgen {#accessToken}

Nadat u de unieke client-id (client-id en clientgeheim) voor uw toepassing hebt opgehaald, moet u een toegangstoken verkrijgen. Het toegangstoken is een verplicht token OAuth 2.0 dat wordt gebruikt om de API&#39;s voor Adobe Pass-verificatie aan te roepen.

>[!NOTE]
>
>Toegang tot tokens hebben momenteel 24 uur de tijd om te leven.

**Verzoek**


| **HTTP-aanroep** | |
| --- | --- |
| pad | `/o/client/token` |
| methode | POST |

| **aanvraagparameters** | |
| --- | --- |
| `grant_type` | Ontvangen in het registratieproces voor de client.<br/> **Geaccepteerde waarde**<br/>`client_credentials`: Wordt gebruikt voor onveilige clients, zoals de Android-SDK. |
| `client_id` | Client-id verkregen in het clientregistratieproces. |
| `client_secret` | Client-id verkregen in het clientregistratieproces. |

**Antwoord**

| responsvelden | | |
| --- | --- | --- |
| `access_token` | De toegangstoken-waarde die u moet gebruiken om de Adobe Pass API&#39;s aan te roepen | verplicht |
| `expires_in` | De tijd in seconden tot access_token verloopt | verplicht |
| `token_type` | Het type token **drager** | verplicht |
| `created_at` | De uitgiftetijd van de token | verplicht |
| **antwoordheaders** | | |
| `Content-Type` | application/json | verplicht |

**Foutreactie**

In het geval van een fout reageert de verificatieserver met de HTTP 400-statuscode (Bad Request) en bevat deze de volgende parameters in de reactie:

| statuscode | responsorgaan | beschrijving |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Het verzoek mist een vereiste parameter, omvat een niet gestaafde parameterwaarde (buiten giftetype), herhaalt een parameter, omvat veelvoudige geloofsbrieven, gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren, of anders misvormd is. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Clientverificatie is mislukt omdat de client onbekend was. De SDK MOET zich opnieuw bij de machtigingsserver registreren. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorised_client&quot;} | De geverifieerde client is niet gemachtigd om dit type autorisatieverlening te gebruiken. |

#### Voorbeeld van toegangstoken verkrijgen: {#obt-access-token}

**Verzoek:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Reactie:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Foutreactie:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Verificatieverzoeken uitvoeren {#autheticationRequests}

Het toegangstoken gebruiken om Adobe Pass uit te voeren [API-aanroepen voor verificatie](/help/authentication/initiate-authentication.md). Hiervoor moet het toegangstoken op een van de volgende manieren aan de API-aanvraag worden toegevoegd:

* door een nieuwe vraagparameter aan het verzoek toe te voegen. Deze nieuwe parameter wordt aangeroepen **access_token**.

* door een nieuwe HTTP-header toe te voegen aan de aanvraag: Autorisatie: Drager. Wij adviseren u om de kopbal van HTTP te gebruiken, aangezien de vraagkoorden in serverlogboeken doorgaans zichtbaar zijn.

In het geval van een fout kunnen de volgende foutreacties worden geretourneerd:

| Foutreacties |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | Het verzoek is onjuist geformuleerd. |
| invalid_client | 403 | De client-id mag geen aanvragen meer uitvoeren. Er moeten nieuwe clientreferenties worden gegenereerd. |
| access_deny | 401 | De access_token is niet geldig (verlopen of ongeldig). De cliënt MOET om een nieuw access_token verzoeken. |

### Voorbeelden van verificatieverzoeken worden uitgevoerd:

**Toegangstoken verzenden als aanvraagparameter:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Toegangstoken verzenden als HTTP-header:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Foutreacties als antwoordinstantie:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Foutreacties als URL-parameters:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
