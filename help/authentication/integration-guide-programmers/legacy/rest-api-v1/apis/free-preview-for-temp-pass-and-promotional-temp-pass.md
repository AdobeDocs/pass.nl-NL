---
title: Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties
description: Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Verouderd) Gratis voorvertoning voor een tijdelijke controle en een tijdelijke controle voor speciale acties {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Hiermee kunt u een verificatietoken maken voor Temperatuur-controle en Tijdelijke controle voor bevordering zonder dat u een tweede scherm nodig hebt.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Streaming App </br></br> of </br></br> de Dienst van de Programmer | &#x200B;1. request_or_id (Verplicht) </br>    </br> 2.  deviceId (Verplicht) </br>    </br> 3.  mso_id (Verplicht) </br>    </br> 4.  domain_name (Verplicht) </br>    </br> 5.  device_info/x-apparaat-Info (Verplicht) </br> 6.  deviceType</br>    </br> 7.  deviceUser (Afgekeurd) </br>    </br> 8.  appId (Afgekeurd) </br>    </br> 9.  generic_data (optioneel) | POST | De succesvolle reactie zal 204 Geen Inhoud zijn, erop wijzend dat het teken met succes werd gecreeerd en klaar voor gebruik voor de auteurstromen is. | 204 - Geen inhoud   </br> &lbrace;400 - Onjuist verzoek |

<div>


| Invoerparameter | Beschrijving |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aanvrager_id | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| mso_id | De MVPD-id waarvoor deze bewerking geldig is. |
| domain_name | De domeinnaam waarvoor een token wordt toegekend. Dit wordt vergeleken met de domeinen van de dienstverlener wanneer een toestemmingstoken wordt verleend. |
| device_info/</br></br> x-apparaat-Info | Informatie over streaming apparaat.</br></br>**Nota**: Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van GET URL, ZOU het als x-Apparaat-Info in de kopbal van http moeten worden overgegaan. </br></br> zie de volledige details in [&#x200B; het overgaan van Apparaat en de Informatie van de Verbinding &#x200B;](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [&#x200B; uitgesplitst per apparatentype &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) zijn wanneer het gebruiken van Clientless, zodat de verschillende soorten analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.</br></br> zie [&#x200B; Voordelen om clientless parameters van het apparatentype &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota** te gebruiken: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat.</br></br>**Nota**: Als gebruikt, zou deviceUser de zelfde waarden moeten hebben zoals in [&#x200B; creeer de verzoek van de Code van de Registratie &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | De toepassings-id/-naam. </br></br>**Nota**: device_info vervangt deze parameter. Indien gebruikt, `appId` zou de zelfde waarden moeten hebben zoals in [&#x200B; creeer de verzoek van de Code van de Registratie &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generic_data | Gebruikt om het werkingsgebied van het teken voor de Pass van de Bevorderingstemperatuur te beperken. |


### [&#x200B; Terug naar REST API Verwijzing &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
