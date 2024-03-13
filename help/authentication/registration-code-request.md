---
title: Registratiepagina
description: Registratiepagina
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Registratiepagina {#registration-page}

## REST API-eindpunten {#clientless-endpoints}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!NOTE]
>
> REST API-implementatie is beperkt door [Draaimechanisme](/help/authentication/throttling-mechanism.md)

&lt;reggie_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#create-reg-code-svc}

Retourneert willekeurig gegenereerde registratie- en aanmeldingspagina-URI.

| Endpoint | Geroepen  </br>Door | Invoer   </br>Parameter | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>Bijvoorbeeld:</br>REGGIE_FQDN/Reggie/v1/sampleRequestorId/regcode | Streaming-app</br>of</br>Programmeringsservice | 1. aanvrager  </br>    (component Path)</br>2.  deviceId (Hashed)   </br>    (Verplicht)</br>3.  device_info/X-Device-Info (verplicht)</br>4.  mvpd (optioneel)</br>5.  ttl (optioneel)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Verouderd)</br>8.  _appId_ (Verouderd) | POST | XML of JSON met een registratiecode en informatie of foutdetails als dit mislukt. Zie schema&#39;s en voorbeelden hieronder. | 201 |

{style="table-layout:auto"}

| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| device_info/</br>X-Apparaat-Info | Informatie over streaming apparaat.</br>**Opmerking**: This MAY BE passed device_info as a URL parameter, but due to the potential size of this parameter and constraints on the length of a GET URL, it should be passed as X-Device-Info in the http header. </br>Zie de volledige details in [Gegevens van apparaat en verbinding doorgeven](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | De MVPD-id waarvoor deze bewerking geldig is. |
| ttl | Hoe lang deze regcode in seconden zou moeten leven.</br>**Opmerking**: De maximaal toegestane waarde voor ttl is 36000 seconden (10 uur). Hogere waarden resulteren in een 400 HTTP-respons (onjuiste aanvraag). Indien `ttl` is leeg, stelt Adobe Pass-verificatie een standaardwaarde van 30 minuten in. |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br>Als deze parameter correct is ingesteld, biedt ESM metriek die [uitgesplitst per apparaattype](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) wanneer u Clientless gebruikt, zodat verschillende typen analyses kunnen worden uitgevoerd, bijvoorbeeld Roku, AppleTV en Xbox.</br>Zie, [Voordelen van het gebruiken van clientless apparatentype parameter in pasmetriek ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Opmerking**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. </br>**Opmerking**: device_info vervangt deze parameter. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**IP-adres van streaming apparaat**
></br>
>Voor client-aan-server implementaties, wordt het Streaming Apparaat IP Adres impliciet verzonden met deze vraag.  Voor server-aan-server implementaties, waar **herschrijven** De vraag wordt gemaakt is de Dienst van de Programmer en niet het Streaming Apparaat, wordt de volgende kopbal vereist om het Streaming Apparaat IP Adres over te gaan:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>waar `<streaming\_device\_ip>` is het openbare IP-adres van het streaming apparaat.
></br></br>
>Voorbeeld:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### XML-schema van reactie {#xml-schema}


#### Registratiecode XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| Elementnaam | Beschrijving |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID gegenereerd door de Registratiecode Service |
| code | Registratiecode gegenereerd door de registratiecodeservice |
| aanvrager | Id van aanvrager |
| mvpd | Mvpd-id |
| gegenereerd | Tijdstempel voor het maken van de registratiecode (in milliseconden sinds 1 januari 1970 GMT) |
| verloopt | Tijdstempel wanneer de registratiecode verloopt (in milliseconden sinds 1 januari 1970 GMT) |
| deviceId | Unieke apparaat-id (of XSTS-token) |
| deviceType | Apparaattype |
| deviceUser | Gebruiker heeft zich bij het apparaat aangemeld |
| appId | Toepassings-id |
| appVersion | Toepassingsversie |
| registrationURL | URL aan de Login App van het Web om aan het eind te tonen - gebruiker |

{style="table-layout:auto"}


</br>

### Foutbericht XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Samplereactie {#sample-response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
