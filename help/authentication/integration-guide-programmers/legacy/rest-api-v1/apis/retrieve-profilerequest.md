---
title: Platform SSO profiel-request ophalen
description: Platform SSO profiel-request ophalen
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# Platform SSO profiel-request ophalen {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

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

Deze bron produceert profielaanvragen voor een aanvraag-id en een MVPD-bestand.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-Requests/{mvpd} | Streaming App </br></br> of </br></br> de Dienst van de Programmer | 1. aanvrager (wegparam) </br> 2. mvpd (wegparam) </br> 3. deviceType (verplicht) | GET | Het antwoord Content-Type is application/octet-stream, omdat de werkelijke lading ondoorzichtig is voor de clienttoepassing.</br></br> de reactie zou door:sturen door de toepassing aan de motor van Platform </br></br> SSO voor het verkrijgen van een Profiel SSO. | 200 - Succes   </br> {400 - Onjuist verzoek |


| Invoerparameter | Beschrijving |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| mvpd | De MVPD-id waarvoor deze bewerking geldig is. |
| deviceType | Het Apple-platform waarvoor we een aanvraag voor een profiel proberen te verkrijgen.  Of **iOS** of **tvOS**. |
