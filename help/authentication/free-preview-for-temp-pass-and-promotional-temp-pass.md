---
title: Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties
description: Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!NOTE]
>
> REST API-implementatie is beperkt door [Draaimechanisme](/help/authentication/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

&lt;reggie_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Hiermee kunt u een verificatietoken maken voor Temperatuur-controle en Tijdelijke controle voor bevordering zonder dat u een tweede scherm nodig hebt.


| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | Streaming-app</br></br>of</br></br>Programmeringsservice | 1. aanvrager_id (verplicht)</br>    </br>2.  deviceId (verplicht)</br>    </br>3.  mso_id (verplicht)</br>    </br>4.  domain_name (verplicht)</br>    </br>5.  device_info/X-Device-Info (verplicht)</br>6.  deviceType</br>    </br>7.  deviceUser (Afgekeurd)</br>    </br>8.  appId (afgekeurd)</br>    </br>9.  generic_data (optioneel) | POST | De succesvolle reactie zal 204 Geen Inhoud zijn, erop wijzend dat het teken met succes werd gecreeerd en klaar voor gebruik voor de auteurstromen is. | 204 - Geen inhoud   </br>400 - Onjuist verzoek |

<div>


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager_id | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| mso_id | De MVPD-id waarvoor deze bewerking geldig is. |
| domain_name | De domeinnaam waarvoor een token wordt toegekend. Dit wordt vergeleken met de domeinen van de dienstverlener wanneer een toestemmingstoken wordt verleend. |
| device_info/</br></br>X-Apparaat-Info | Informatie over streaming apparaat.</br></br>**Opmerking**: This MAY BE passed device_info as a URL parameter, but due to the potential size of this parameter and constraints on the length of a GET URL, it should be passed as X-Device-Info in the http header. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br>Als deze parameter correct is ingesteld, biedt ESM metriek die [uitgesplitst per apparaattype](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bij gebruik van Clientless, zodat verschillende soorten analyses kunnen worden uitgevoerd voor bijvoorbeeld Roku, AppleTV, Xbox enz.</br></br>Zie [Voordelen van het gebruiken van clientless apparatentype parameters ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Opmerking**: device_info zal deze parameter vervangen. |
| _deviceUser_ | De gebruikers-id van het apparaat.</br></br>**Opmerking**: Indien gebruikt, moet deviceUser dezelfde waarden hebben als in het dialoogvenster [Registratiecode maken](/help/authentication/registration-code-request.md) verzoek. |
| _appId_ | De toepassings-id/-naam. </br></br>**Opmerking**: device_info vervangt deze parameter. Indien gebruikt, `appId` moeten dezelfde waarden hebben als in de [Registratiecode maken](/help/authentication/registration-code-request.md) verzoek. |
| generic_data | Gebruikt om het werkingsgebied van het teken voor de Pass van de Bevorderingstemperatuur te beperken. |


### [Terug naar REST API Reference](/help/authentication/rest-api-reference.md)
