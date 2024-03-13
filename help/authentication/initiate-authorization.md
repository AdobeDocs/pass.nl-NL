---
title: Autorisatie starten
description: Autorisatie starten
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Autorisatie starten {#initiate-authorization}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!NOTE]
>
> REST API-implementatie is beperkt door [Draaimechanisme](/help/authentication/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

&lt;reggie_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Verkrijgt de reactie van de vergunning.

| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | Streaming-app</br></br>of</br></br>Programmeringsservice | 1. Aanvrager (verplicht)</br>2.  deviceId (verplicht)</br>3.  bron (verplicht)</br>4.  device_info/X-Device-Info (verplicht)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Verouderd)</br>7.  _appId_ (Verouderd)</br>8.  extra parameters (optioneel) | GET | XML of JSON met machtigingsdetails of foutdetails als dit mislukt. Zie onderstaande voorbeelden. | 200 - Succes  </br>403 - Geen succes |

{style="table-layout:auto"}

</br>


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| resource | Een koord dat een resourceId (of fragment MRSS) bevat, identificeert de inhoud die door een gebruiker wordt gevraagd en door MVPD vergunningseindpunten wordt erkend. |
| device_info/</br></br>X-Apparaat-Info | Informatie over streaming apparaat.</br></br>**Opmerking**: This MAY BE passed device_info as a URL parameter, but due to the potential size of this parameter and constraints on the length of a GET URL, it should be passed as X-Device-Info in the http header. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br>Als deze parameter correct is ingesteld, biedt ESM metriek die [uitgesplitst per apparaattype](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bij gebruik van Clientless, zodat verschillende soorten analyses kunnen worden uitgevoerd voor bijvoorbeeld Roku, AppleTV, Xbox enz.</br></br>Zie [Voordelen van clientless apparatentype parameter in pasmetriek ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Opmerking**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. </br></br>**Opmerking**: device_info vervangt deze parameter. |
| extra parameters | De oproep kan ook optionele parameters bevatten die andere functies mogelijk maken, zoals:</br></br>* generic_data - maakt het gebruik van [Promotie TempPass](/help/authentication/promotional-temp-pass.md)</br></br>Voorbeeld: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-adres van streaming apparaat**</br>
>Voor client-aan-server implementaties, wordt het Streaming Apparaat IP Adres impliciet verzonden met deze vraag.  Voor server-aan-server implementaties, waar **herschrijven** De vraag wordt gemaakt door de Dienst van de Programmer en niet het Streaming Apparaat, wordt de volgende kopbal vereist om het Streaming Apparaat IP Adres over te gaan:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>waar `<streaming\_device\_ip>` is het openbare IP-adres van het streaming apparaat.</br></br>
>Voorbeeld:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Samplereactie {#sample-response}

* **Zaak 1: Succes**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
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
>Wanneer de reactie uit een Volmacht MVPD komt, kan het een extra genoemd element omvatten `proxyMvpd`.



* **Zaak 2: Vergunning geweigerd**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
