---
title: Autorisatie starten
description: Autorisatie starten
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# (Verouderd) Autorisatie starten {#initiate-authorization}

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

Verkrijgt de reactie van de vergunning.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. (Verplicht) aanvrager </br> .  deviceId (Verplicht) </br> .  middel (Verplicht) </br> 4.  device_info/x-apparaat-Info (Verplicht) </br> 5.  _deviceType_</br> 6.  _deviceUser_ (Afgekeurd) </br> 7.  _appId_ (Vervangen) </br> 8.  extra parameters (optioneel) | GET | XML of JSON met machtigingsdetails of foutdetails als dit mislukt. Zie onderstaande voorbeelden. | 200 - Succes </br> 403 - Geen succes |

{style="table-layout:auto"}

</br>


| Invoerparameter | Beschrijving |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| resource | Een tekenreeks die een resourceId (of MRSS-fragment) bevat, de inhoud identificeert die door een gebruiker is aangevraagd en die wordt herkend door de eindpunten van MVPD-autorisaties. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [ uitgesplitst per apparatentype ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) zijn wanneer het gebruiken van Clientless, zodat de verschillende soorten analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.</br></br> zie [ Voordelen van clientless apparatentype parameter in pas metriek ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. |
| extra parameters | De vraag kan facultatieve parameters ook bevatten die andere functionaliteiten zoals toelaten:</br></br>* generic_data - laat het gebruik van [ Promotional TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br> Voorbeeld toe: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**het Streamen IP van het Apparaat Adres**</br>
>Voor client-aan-server implementaties, wordt het Streaming Apparaat IP Adres impliciet verzonden met deze vraag.  Voor Server-aan-Server implementaties, waar de **regcode** vraag door de Dienst van de Programmer en niet het Streaming Apparaat wordt gemaakt, wordt de volgende kopbal vereist om het Streaming IP van het Apparaat Adres over te gaan:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>waarbij `<streaming\_device\_ip>` het openbare IP-adres van het streaming apparaat is.</br></br>
>Voorbeeld: </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Samplereactie {#sample-response}

* **Geval 1: Succes**
</br>
  * **XML:**

  </br>

     &quot;XML 
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone= &quot;yes&quot;?>
     &lt;authentication>
    &lt;expired>1348148289000&lt;/expired>
    &lt;mvpd>sampleMvpdId&lt;/mvpd> 
     &lt;requestor>sampleRequestorId&lt;/requestor> 
     &lt;resource>sampleResourceId&lt;/resource> 
    &lt;/authentication> 
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Wanneer de reactie afkomstig is van een Proxy MVPD, kan deze een extra element met de naam `proxyMvpd` bevatten.



* **Geval 2: Ontkende Vergunning**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
