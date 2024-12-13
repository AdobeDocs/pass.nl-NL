---
title: Verificatietoken controleren
description: Verificatietoken controleren
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# (Verouderd) Verificatietoken controleren {#check-authentication-token}

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

Geeft aan of het apparaat een niet-verlopen verificatietoken heeft.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthoring | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. (Verplicht) aanvrager </br> .  deviceId (Verplicht) </br> .  device_info/x-apparaat-Info (Verplicht) </br> 4.  _deviceType_ </br> 5.  _deviceUser_ (Afgekeurd) </br> 6.  _appId_ (Vervangen) | GET | XML of JSON met foutdetails als dit mislukt. | 200 - Succes   </br> {403 - Geen succes |

{style="table-layout:auto"}


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http MOETEN worden overgegaan. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)--> . |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [ uitgesplitst per apparatentype ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) zijn wanneer het gebruiken van Clientless, zodat de verschillende soorten analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.</br></br> voor details, zie [ Voordelen om de parameter Clientless deviceType in de metriek van de Authentificatie van Adobe Pass te gebruiken ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam.</br>**Nota**: device_info vervangt deze parameter. |

{style="table-layout:auto"}


## Reactie (indien mislukt) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [ Terug naar REST API Verwijzing ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
