---
title: MVPD-lijst opgeven
description: MVPD-lijst opgeven
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# MVPD-lijst opgeven {#provide-mvpd-list}

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

## Beschrijving {#description}

Keert lijst van gevormde MVPDs voor de aanvrager terug.

| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>Bijvoorbeeld:</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Adobe Pass-verificatie | 1. Aanvrager</br>    (component Path)</br>_2.  deviceType (afgekeurd)_ | GET | XML of JSON met lijst van MVPD&#39;s. | 200 |

{style="table-layout:auto"}


| Invoerparameter | Beschrijving |
| --------------- | ------------------------------------------------------------- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| *deviceType* | Apparaattype. |

{style="table-layout:auto"}

### Samplereactie {#sample-response}

Hetzelfde als de bestaande Reactie van XML MVPD op /config servlet

Opmerking: alle MVPD&#39;s die zijn geconfigureerd om gebruik te maken van Platform SSO, hebben de volgende extra eigenschappen binnen het corresponderende knooppunt (JSON/XML):

* **enablePlatformServices (boolean):** Markering die aangeeft of deze MVPD is geïntegreerd via Platform SSO
* **boardingStatus (tekenreeks):** markering die aangeeft of de MVPD Platform SSO volledig ondersteunt (ONDERSTEUND) of dat de MVPD alleen wordt weergegeven in de platformkiezer (PICKER)
* **displayInPlatformPicker (Boolean):** moet deze MVPD worden weergegeven in de platformkiezer
* **platformMappingId (tekenreeks):** de identificatiecode van deze MVPD zoals bekend door het platform
* **requiredMetadataFields (array string):** de velden met gebruikersmetagegevens die naar verwachting beschikbaar zijn bij een geslaagde aanmelding
