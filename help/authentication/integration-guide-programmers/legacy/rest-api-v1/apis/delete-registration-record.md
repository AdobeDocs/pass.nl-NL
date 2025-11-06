---
title: Registratierecord verwijderen
description: Registratieresord verwijderen
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# (Verouderd) Registratierecord verwijderen {#delete-registration-record}

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


## Beschrijving {#delete-record}

Verwijdert de reg code-record en geeft de reg-code vrij voor hergebruik.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br> bijvoorbeeld:</br></br> &lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Streaming App </br></br> of </br></br> de Dienst van de Programmer | &#x200B;1. ID aanvrager </br>    (De component van de Weg) </br> 2.  Registratiecode </br>    (component Path) | DELETE | Geen | 204 |

{style="table-layout:auto"}

</br>

| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| registratiecode | De waarde van de registratiecode die op het Streaming Apparaat (dat in de authentificatiestroom moet worden ingegaan) zou worden getoond. |

{style="table-layout:auto"}

</br>

### [&#x200B; Terug naar REST API Verwijzing &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
