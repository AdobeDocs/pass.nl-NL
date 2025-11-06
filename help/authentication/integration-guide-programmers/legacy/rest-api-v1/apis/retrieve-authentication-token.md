---
title: Verificatietoken ophalen
description: Verificatietoken ophalen
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# (Verouderd) Verificatietoken ophalen {#retrieve-authentication-token}

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

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Hiermee wordt het verificatietoken (AuthN) opgehaald.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn </br></br> Bijvoorbeeld:</br></br> &lt;SP_FQDN>/api/v1/tokens/authn | Streaming App </br></br> of </br></br> de Dienst van de Programmer | &#x200B;1. (Verplicht) aanvrager </br> .  deviceId (Verplicht) </br> .  device_info/x-apparaat-Info (Verplicht) </br> 4.  _deviceType_ (Afgekeurd) </br> 5.  _deviceUser_ (Afgekeurd) </br> 6.  _appId_ (Vervangen) | GET | XML of JSON met verificatiegegevens of foutdetails als dit mislukt. | 200 - Geslaagd.  </br> {404 - Symbolisch niet gevonden </br> 410 - Symbolisch verlopen |

{style="table-layout:auto"}


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br>**Nota**: device_info vervangt deze parameter. |
| _deviceUser_ | De gebruikers-id van het apparaat.</br></br>**Nota**: Als gebruikt, zou deviceUser de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. Indien gebruikt, `appId` zou de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

{style="table-layout:auto"}

</br>

### Samplereactie {#response}



#### Succes

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### Verificatietoken niet gevonden:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```


**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found"
    }
```
