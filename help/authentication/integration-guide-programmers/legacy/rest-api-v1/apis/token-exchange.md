---
title: Exchange a Platform SSO token for an Adobe token
description: Exchange a Platform SSO token for an Adobe token
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# (Verouderd) Exchange a Platform SSO token for an Adobe token {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Hiermee wordt een Platform SSO-profiel &quot;uitgewisseld&quot; voor een Adobe-token.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Streaming App </br></br> of </br></br> de Dienst van de Programmer | &#x200B;1. aanvrager (Verplicht) </br>    </br> 2.  deviceId (Verplicht) </br>    </br> 3.  mvpd (Verplicht) </br>    </br> 4.  deviceType (Verplicht) </br>    </br> 5.  SAMLResponse (Verplicht) </br>    </br> 6.  deviceUser (Afgekeurd) </br>    </br> 7.  appId (afgekeurd) | POST | De succesvolle reactie zal 204 Geen Inhoud zijn, erop wijzend dat het teken met succes werd gecreeerd en klaar voor gebruik voor de auteurstromen is. | 204 - Geen inhoud   </br> {400 - Onjuist verzoek |


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| mvpd | De MVPD-id waarvoor deze bewerking geldig is. |
| deviceType | Het Apple-platform waarvoor we een aanvraag voor een profiel proberen te verkrijgen.  Of **iOS** of **tvOS**. |
| SAMLResponse | Het werkelijke profiel dat wordt geretourneerd door Platform SSO. |
| _deviceUser_ | De gebruikers-id van het apparaat. |
| _appId_ | De toepassings-id/-naam. |
