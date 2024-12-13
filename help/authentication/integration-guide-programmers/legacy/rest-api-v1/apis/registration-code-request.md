---
title: Registratiepagina
description: Registratiepagina
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# (Verouderd) Registratiepagina {#registration-page}

## REST API-eindpunten {#clientless-endpoints}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!NOTE]
>
> De implementatie van REST API wordt begrensd door [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

<br>

## Beschrijving {#create-reg-code-svc}

Retourneert willekeurig gegenereerde registratie- en aanmeldingspagina-URI.

| Endpoint | Geroepen <br> door | Invoer   <br> Parameter | HTTP <br> Methode | Antwoord | HTTP-respons <br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode <br> bijvoorbeeld:<br> REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Streaming App <br> of <br> de Dienst van de Programmer | 1. aanvrager <br>    (De component van de Weg) <br> 2.  deviceId (Hashed)   <br>    (Verplicht) <br> 3.  device_info/x-apparaat-Info (Verplicht) <br> 4.  mvpd (Facultatief) <br> 5.  ttl (Facultatief) <br> | POST | XML of JSON met een registratiecode en informatie of foutdetails als dit mislukt. Zie onderstaande voorbeelden. | 201 |

{style="table-layout:auto"}

| Invoerparameter | Type | Beschrijving |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Toestemming | Koptekst <br> Waarde: drager &lt;access_token> | DCR-toegangstoken |
| Accepteren | Koptekst <br> Waarde: toepassing/json | aangeven welk inhoudstype de client moet kunnen begrijpen |
| aanvrager | Query-parameter | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Query-parameter | Het apparaat-id bytes. |
| device_info/<br> x-apparaat-Info | device_info: Body <br> X-Device-Info: Koptekst | Informatie over streaming apparaat.<br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van een GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. <br> zie de volledige details in [ het overgaan van Apparaat en de Informatie van de Verbinding ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| mvpd | Query-parameter | De MVPD-id waarvoor deze bewerking geldig is. |
| ttl | Query-parameter | Hoe lang deze regcode in seconden zou moeten leven.<br>**Nota**: De maximumwaarde die voor ttl wordt toegestaan is 36000 seconden (10 uren). Hogere waarden resulteren in een 400 HTTP-respons (onjuiste aanvraag). Als `ttl` leeg blijft, stelt Adobe Pass Authentication een standaardwaarde van 30 minuten in. |
| _deviceType_ | Query-parameter | Vervangen, niet gebruiken. |
| _deviceUser_ | Query-parameter | Vervangen, niet gebruiken. |
| _appId_ | Query-parameter | Vervangen, niet gebruiken. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**het Streamen IP van het Apparaat Adres**
><br>
>Voor client-aan-server implementaties, wordt het Streaming Apparaat IP Adres impliciet verzonden met deze vraag.  Voor Server-aan-Server implementaties, waar de **regcode** vraag wordt gemaakt is de Dienst van de Programmer en niet het Streaming Apparaat, wordt de volgende kopbal vereist om het Streaming IP van het Apparaat Adres over te gaan:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>waarbij `<streaming\_device\_ip>` het openbare IP-adres van het streaming apparaat is.
><br><br>
>Voorbeeld: <br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### Response JSON


#### Registratiecode JSON-MONSTERS

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Elementnaam | Beschrijving |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID gegenereerd door de Registratiecode Service |
| code | Registratiecode gegenereerd door de registratiecodeservice |
| aanvrager | Id van aanvrager |
| mvpd | Mvpd-id |
| gegenereerd | Tijdstempel voor het maken van de registratiecode (in milliseconden sinds 1 januari 1970 GMT) |
| verloopt | Tijdstempel wanneer de registratiecode verloopt (in milliseconden sinds 1 januari 1970 GMT) |
| deviceId | Base64 Unique Device ID |
| info:deviceId | Base64-apparaattype |
| info:deviceInfo | Base64 Normalized de Informatie van het Apparaat bouwt op informatie die van gebruiker-Agent, x-Apparaat-Info of device_info wordt ontvangen |
| info:userAgent | Gebruikersagent verzonden door de toepassing |
| info:originalUserAgent | Gebruikersagent verzonden door de toepassing |
| info:authenticationType | OAUTH2 voor vraag die DCR gebruiken |
| info:sourceApplicationInformation | Application Info zoals geconfigureerd in DCR |

{style="table-layout:auto"}


<br>

### Foutbericht JSON-responsvoorbeeld (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

