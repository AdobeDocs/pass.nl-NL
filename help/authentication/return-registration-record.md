---
title: Registratierecord retourneren
description: Registratierecord retourneren
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Registratierecord retourneren {#return-registration-record}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## REST API-eindpunten {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## Beschrijving {#description}

Retourneert de registratie-code die de registratiecode UUID, de registratiecode en de hashed-apparaat-id bevat.






| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>Bijvoorbeeld:<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJCFK?format=xml | Streaming-app</br></br>of</br></br>Programmeringsservice | 1. aanvrager  </br>    (component Path)</br>2.  registratiecode  </br>    (component Path) | GET | XML of JSON met een registratiecode en informatie. Zie schema en voorbeeld hieronder. | 200 |

{style="table-layout:auto"}




| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| registratiecode | De waarde van de registratiecode die op het Streaming Apparaat (dat in de authentificatiestroom moet worden ingegaan) zou worden getoond. |




## XML-schema van reactie {#response-xml-schema}

### Registratiecode XSD

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

| Elementnaam | Beschrijving |
| --- | --- |
| id | UUID gegenereerd door de Registratiecode Service |
| code | Registratiecode gegenereerd door de registratiecodeservice |
| aanvrager | Id van aanvrager |
| mvpd | MVPD-id |
| gegenereerd | Tijdstempel voor het maken van de registratiecode (in milliseconden sinds 1 januari 1970 GMT) |
| verloopt | Tijdstempel wanneer de registratiecode verloopt (in milliseconden sinds 1 januari 1970 GMT) |
| deviceId | Unieke apparaat-id (of XSTS-token) |
| deviceType | Apparaattype |
| deviceUser | Gebruiker heeft zich bij het apparaat aangemeld |
| appId | Toepassings-id |
| appVersion | Toepassingsversie |
| registrationURL | URL aan de Login App van het Web om aan het eind te tonen - gebruiker |

{style="table-layout:auto"}

### Samplereactie {#sample-response}

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
