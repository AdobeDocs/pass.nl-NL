---
title: Token voor korte media verkrijgen
description: short media token verkrijgen
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# (Verouderd) Token voor korte media verkrijgen {#obtain-short-media-token}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

>[!NOTE]
>
> De implementatie van REST API wordt begrensd door [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Productie - [`api.auth.adobe.com` ](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com` ](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [`api.auth.adobe.com` ](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com` ](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Hiermee wordt een kort mediatoken opgehaald.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken </br></br> of </br></br> &lt;SP_FQDN>/api/v1/tokens/media </br></br> bijvoorbeeld:</br></br> &lt;SP_FQDN>/api/v1/tokens/media | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. (Verplicht) aanvrager </br> .  deviceId (Verplicht) </br> .  middel (Verplicht) </br> 4.  device_info/x-apparaat-Info (Verplicht) </br> 5.  _deviceType_</br> 6.  _deviceUser_ (Afgekeurd) </br> 7.  _appId_ (Vervangen) | GET | XML of JSON met een Base64-gecodeerde media-token of foutdetails als dit niet lukt. | 200 - Succes </br> 403 - Geen succes |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Invoerparameter | Beschrijving |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| resource | Een tekenreeks die een resourceId (of MRSS-fragment) bevat, de inhoud identificeert die door een gebruiker is aangevraagd en die wordt herkend door de eindpunten van MVPD-autorisaties. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [ uitgesplitst per apparatentype ]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) wanneer het gebruiken van Clientless zijn, zodat de verschillende types van analyse voor kunnen worden uitgevoerd. Bijvoorbeeld Roku, AppleTV en Xbox.</br></br> zie [ Voordelen om clientless apparaat te gebruiken parameter ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat.</br></br>**Nota**: Als gebruikt, zou deviceUser de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. Indien gebruikt, `appId` zou de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

{style="table-layout:auto"}

### Samplereactie {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Compatibiliteit van de mediaverificatiebibliotheek

Het veld `serializedToken` van de aanroep &quot;Short Media Token verkrijgen&quot; is een token met Base64-codering dat kan worden geverifieerd met behulp van de Adobe Media Verification Library.
