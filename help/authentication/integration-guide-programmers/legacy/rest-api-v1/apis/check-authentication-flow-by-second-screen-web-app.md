---
title: Verificatiestroom controleren op tweede scherm van webtoepassing
description: Verificatiestroom controleren op tweede scherm van webtoepassing
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# (Verouderd) Verificatiestroom controleren op tweede scherm van webtoepassing {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

>[!NOTE]
>
> De implementatie van REST API wordt begrensd door [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Productie - [&#x200B; api.auth.adobe.com &#x200B;](http://api.auth.adobe.com/)
* Het opvoeren - [&#x200B; api.auth-staging.adobe.com &#x200B;](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [&#x200B; api.auth.adobe.com &#x200B;](http://api.auth.adobe.com/)
* Het opvoeren - [&#x200B; api.auth-staging.adobe.com &#x200B;](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Deze API moet worden gebruikt door de tweede schermaanmelding via de webapp om te bevestigen dat Adobe Pass Authentication de geslaagde aanmelding door MVPD heeft bevestigd. Wij adviseren roepend deze API alvorens een succesbericht aan het eind te tonen - gebruiker die hem/haar instrueert om aan de apparatenconsole te werk te gaan om met de werkschema&#39;s verder te gaan.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauther/{registration code} | Aanmeldingswebtoepassing | 1. registratiecode </br>    (De component van de Weg) </br> 2.  aanvrager </br>    (Verplicht) | GET | XML of JSON met foutdetails als dit mislukt. | 200 - Succes   </br> &lbrace;403 - Verboden |

</br>

| Invoerparameter | Beschrijving |
| ----------------- | --------------------------------------------------------------------------------------------- |
| registratiecode | De waarde van de registratiecode die door de gebruiker aan het begin van de authentificatiestroom wordt verstrekt. |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |


### Samplereactie (in geval van een fout) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [&#x200B; Terug naar REST API Verwijzing &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
