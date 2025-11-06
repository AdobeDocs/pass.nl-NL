---
title: Lijst met vooraf geautoriseerde bronnen ophalen via tweede webtoepassing voor scherm
description: Lijst met vooraf geautoriseerde bronnen ophalen via tweede webtoepassing voor scherm
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# (Verouderd) Hiermee wordt de lijst met vooraf geautoriseerde bronnen opgehaald via de webtoepassing voor tweede scherm {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

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

Er zijn twee reeksen APIs: één reeks voor de Streaming App of de Dienst van Programmer en één reeks voor de Tweede SchermApp. Deze pagina beschrijft API voor de App AuthN.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{registration code} | Module AuthN | &#x200B;1. registratiecode </br>    (De component van de Weg) </br> 2.  (Verplicht) </br> .  bron (verplicht) | GET | XML of JSON met individuele aan de autorisatie voorafgaande beslissingen of foutdetails. Zie onderstaande voorbeelden. | 200 - Succes </br></br> 400 - het Onjuiste verzoek </br></br> 401 - ongeoorloofd </br></br> 405 - Methode niet toegestaan </br></br> 412 - Voorwaarde ontbrak </br></br> 500 - Interne Fout van de Server |



| Invoerparameter | Beschrijving |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| registratiecode | De waarde van de registratiecode die door de gebruiker aan het begin van de authentificatiestroom wordt verstrekt. |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| resource | Een tekenreeks die een door komma&#39;s gescheiden lijst met resourceIds bevat die de inhoud identificeert die toegankelijk kan zijn voor een gebruiker en die wordt herkend door MVPD-autorisatieeindpunten. |


### Samplereactie {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
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
    <resource>
</resources>
```

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
