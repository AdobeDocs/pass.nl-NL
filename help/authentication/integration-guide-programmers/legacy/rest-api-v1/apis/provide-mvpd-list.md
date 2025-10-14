---
title: MVPD-lijst opgeven
description: MVPD-lijst opgeven
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---

# (Verouderd) MVPD-lijst opgeven {#provide-mvpd-list}

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

Keert lijst van gevormde MVPDs voor de aanvrager terug.

| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId} </br></br> Bijvoorbeeld:</br></br> &lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass-verificatie | 1. Aanvrager </br>    (De component van de Weg) </br>_2.  deviceType (afgekeurd)_ | GET | XML of JSON met lijst van MVPD&#39;s. | 200 |

{style="table-layout:auto"}


| Invoerparameter | Beschrijving |
| --------------- | ------------------------------------------------------------- |
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| *deviceType* | Apparaattype. |

{style="table-layout:auto"}

### Samplereactie {#sample-response}

Hetzelfde als bestaande MVPD XML-respons op /config servlet

Opmerking: alle MVPD&#39;s die zijn geconfigureerd om gebruik te maken van Platform SSO, hebben de volgende extra eigenschappen binnen het corresponderende knooppunt (JSON/XML):

* **enablePlatformServices (boolean):** vlag erop wijst die of dit MVPD via Platform SSO wordt geïntegreerd
* **boardingStatus (koord):** vlag die erop wijst of MVPD volledig Platform SSO (STEUND) steunt of als MVPD slechts in de platformplukker (PICKER) verschijnt
* **displayInPlatformPicker (boolean):** moet dit MVPD in de platformkiezer verschijnen
* **platformMappingId (koord):** het herkenningsteken van dit MVPD zoals gekend door het platform
* **requiredMetadataFields (koordserie):** de gebieden van gebruikersmeta-gegevens die op succesvolle login worden verwacht beschikbaar te zijn
