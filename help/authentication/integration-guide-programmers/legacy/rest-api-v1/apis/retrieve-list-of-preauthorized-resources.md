---
title: Lijst met vooraf gemachtigde bronnen ophalen
description: Lijst met vooraf gemachtigde bronnen ophalen
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# (Verouderd) Lijst met vooraf gemachtigde bronnen ophalen {#retrieve-list-of-preauthorized-resources}

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

Een verzoek aan de Authentificatie van Adobe Pass om de lijst van vooraf geautoriseerde middelen te verkrijgen.

Er zijn twee reeksen APIs: één reeks voor de Streaming App of de Dienst van Programmer en één reeks voor de Tweede SchermApp. Op deze pagina wordt de API voor de Streaming App of Programmer Service beschreven.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. (Verplicht) aanvrager </br> .  deviceId (Verplicht) </br> .  middellijst (Verplicht) </br> 4.  device_info/x-apparaat-Info (Verplicht) </br> 5.  _deviceType_</br> 6.  _deviceUser_ (Afgekeurd) </br> 7.  _appId_ (Vervangen) | GET | XML of JSON met individuele aan de autorisatie voorafgaande beslissingen of foutdetails. Zie onderstaande voorbeelden. | 200 - Succes </br></br> 400 - het Onjuiste verzoek </br></br> 401 - ongeoorloofd </br></br> 405 - Methode niet toegestaan </br></br> 412 - Voorwaarde ontbrak </br></br> 500 - Interne Fout van de Server |


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| resourcerelijst | Een tekenreeks die een door komma&#39;s gescheiden lijst met resourceIds bevat die de inhoud identificeert die toegankelijk kan zijn voor een gebruiker en die wordt herkend door MVPD-autorisatieeindpunten. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [ uitgesplitst per apparatentype ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) zijn wanneer het gebruiken van Clientless, zodat de verschillende types van analyse kunnen worden uitgevoerd bijvoorbeeld, Roku, AppleTV, en Xbox.</br></br> zie, [ voordelen om clientless apparatentype parameter in pas te gebruiken metriek ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: `device_info` zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. |



### Samplereactie {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
