---
title: Registratierecord verwijderen
description: Registratieresord verwijderen
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Registratierecord verwijderen {#delete-registration-record}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## REST API-eindpunten {#clientless-endpoints}

&lt;reggie_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Beschrijving {#delete-record}

Verwijdert de reg code-record en geeft de reg-code vrij voor hergebruik.

| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Bijvoorbeeld:</br></br>&lt;reggie_fqdn>/reggie/v1/regcode/ER45RTY | Streaming-app</br></br>of</br></br>Programmeringsservice | 1. ID aanvrager  </br>    (component Path)</br>2.  Registratiecode  </br>    (component Path) | DELETE | Geen | 204 |

{style="table-layout:auto"}

</br>

| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| registratiecode | De waarde van de registratiecode die op het Streaming Apparaat (dat in de authentificatiestroom moet worden ingegaan) zou worden getoond. |

{style="table-layout:auto"}

</br>

### [Terug naar REST API Reference](/help/authentication/rest-api-reference.md)
