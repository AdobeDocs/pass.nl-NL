---
title: Afmelden starten
description: Afmelden starten
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Afmelden starten {#initiate-logout}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!NOTE]
>
> De implementatie van REST API wordt begrensd door [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

AuthN en AuthZ tekenen uit opslag verwijderen.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. aanvrager </br> 2.  deviceId (Verplicht) </br> .  device_info/x-apparaat-Info (Verplicht) </br> 4.  _deviceType_</br> 5.  _deviceUser_ (Afgekeurd) </br> 6.  _appId_ (Vervangen) | DELETE | Geen | 204 |


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [ uitgesplitst per apparatentype ](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) zijn wanneer het gebruiken van Clientless, zodat de verschillende soorten analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.</br></br> Zie [ Voordelen om de clientless parameter van het apparatentype in pas metriek ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota** te gebruiken: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat.</br></br>**Nota**: Als gebruikt, zou deviceUser de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/registration-code-request.md). |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. Indien gebruikt, `appId` zou de zelfde waarden moeten hebben zoals in [ creeer de verzoek van de Code van de Registratie ](/help/authentication/registration-code-request.md). |

>[!IMPORTANT]
> 
>De logout vraag heeft momenteel de volgende beperking: het ontruimt de tokens AuthN en AuthZ van opslag (d.w.z., op de kant van de Authentificatie van Programmer/Adobe Pass), maar **roept niet** het MVPD logout eindpunt.
