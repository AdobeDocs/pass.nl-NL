---
title: Token voor autorisatie ophalen
description: Token voor autorisatie ophalen
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Token voor autorisatie ophalen {#retrieve-authorization-token}

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

Haalt autorisatietoken (AuthZ) op.


| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authz</br></br>Bijvoorbeeld:</br></br>&lt;sp_fqdn>/api/v1/tokens/authz | Streaming-app</br></br>of</br></br>Programmeringsservice | 1. Aanvrager (verplicht)</br>2.  deviceId (verplicht)</br>3.  bron (verplicht)</br>4.  device_info/X-Device-Info (verplicht)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Verouderd)</br>7.  _appId_ (Verouderd) | GET | 1. Succes</br>2.  Verificatietoken  </br>    niet gevonden of verlopen:   </br>    XML die reden verklaren  </br>    for authoring token not found</br>3.  Token voor autorisatie  </br>    niet gevonden:  </br>    XML-uitleg</br>4.  Token voor autorisatie  </br>    verlopen:  </br>    XML-uitleg | 200 - Succes  </br>412 - Geen AuthN</br></br>404 - Geen AuthZ</br></br>410 - AuthZ verlopen |

{style="table-layout:auto"}

</br>

| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| resource | Een koord dat een resourceId (of fragment MRSS) bevat, identificeert de inhoud die door een gebruiker wordt gevraagd en door MVPD vergunningseindpunten wordt erkend. |
| device_info/</br></br>X-Apparaat-Info | Informatie over streaming apparaat.</br></br>**Opmerking**: This MAY BE passed device_info as a URL parameter, but due to the potential size of this parameter and constraints on the length of a GET URL, it should be passed as X-Device-Info in the http header. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br>Als deze parameter correct is ingesteld, biedt ESM metriek die [uitgesplitst per apparaattype](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) wanneer u Clientless gebruikt, zodat verschillende typen analyses kunnen worden uitgevoerd, bijvoorbeeld Roku, AppleTV en Xbox.</br></br>Zie, [Voordelen van het gebruiken van clientless apparatentype parameter in pasmetriek ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Opmerking**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. </br></br>**Opmerking**: device_info vervangt deze parameter. |

{style="table-layout:auto"}


### Samplereactie {#response}



#### Succes

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Verificatietokken niet gevonden of verlopen:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Token voor autorisatie niet gevonden:

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
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Token voor autorisatie verlopen:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
