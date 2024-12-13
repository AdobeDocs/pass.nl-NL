---
title: Verificatie starten
description: Verificatie starten
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# (Verouderd) Verificatie starten {#initiate-authentication}

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

Hiermee wordt het verificatieproces gestart door een MVPD-selectiegebeurtenis op de hoogte te stellen. Hiermee maakt u een record in de Adobe Pass Authentication-database, die wordt afgestemd wanneer de MVPD een succesvol antwoord heeft ontvangen.



| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | Module AuthN | 1. request_id (Verplicht) </br> .  mso_id (Verplicht) </br> .  reg_code (Verplicht) </br> 4.  domain_name (Verplicht) </br> 5.  noflash=true - </br>    (Verplicht, Resterende parameter) </br> 6.  no_iframe=true (Verplicht, Resterende parameter) </br> 7.  extra (Facultatieve) parameters </br> 8.  redirect_url (verplicht) | GET | De Web-app voor aanmelding wordt omgeleid naar de aanmeldingspagina van MVPD. | 302 voor volledige omleiding |

{style="table-layout:auto"}


| Invoerparameter | Beschrijving |
| --- | --- |
| aanvrager_id | De programmeeraanvrager waarvoor deze bewerking geldig is. |
| mso_id | De MVPD-id waarvoor deze bewerking geldig is. |
| reg_code | De registratiecode die door de Reggie-dienst wordt gegenereerd. |
| domain_name | Het oorspronkelijke domein. |
| redirect_url | De URL voor omleiding van de webapp voor aanmelding nadat de verificatie is voltooid. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Belangrijk: Verplichte parameters -** Ongeacht de cliënt-zijimplementatie, zijn alle hierboven vermelde parameters verplicht.
>
>
>Voorbeeld:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Belangrijk: Facultatieve parameters**
>
>De oproep kan ook optionele parameters bevatten die andere functies mogelijk maken, zoals:
>
> * algemeen \_data - laat het gebruik van [ Promotional TempPass ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md) toe
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Nota&#39;s** {#notes}

* De waarde van de parameter `domain_name` moet worden ingesteld op een van de domeinnamen die zijn geregistreerd bij Adobe Pass-verificatie. Voor meer details, verwijs naar [ Registratie en Initialisatie ](/help/authentication/kickstart/programmer-overview.md).

* [Gebruik geen &#39;&amp;&#39;reg\_code in /authenticate request (Tech Note)](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* De parameter `redirect_url` moet de laatste zijn.

* De waarde van de parameter `redirect_url` moet URL-gecodeerd zijn
