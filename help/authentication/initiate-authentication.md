---
title: Verificatie starten
description: Verificatie starten
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Verificatie starten {#initiate-authentication}

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

Hiermee wordt het verificatieproces gestart door een MVPD-selectiegebeurtenis op de hoogte te stellen. Maakt een record in de Adobe Pass Authentication-database, die wordt afgestemd wanneer een geslaagde reactie van de MVPD wordt ontvangen.



| Endpoint | Geroepen  </br>Door | Invoer   </br>Params | HTTP  </br>Methode | Antwoord | HTTP  </br>Antwoord |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | Module AuthN | 1. aanvrager_id (verplicht)</br>2.  mso_id (verplicht)</br>3.  reg_code (verplicht)</br>4.  domain_name (verplicht)</br>5.  noflash=true -  </br>    (Verplicht, residuele parameter)</br>6.  no_iframe=true (verplicht, residuparameter)</br>7.  extra parameters (optioneel)</br>8.  redirect_url (verplicht) | GET | De Login App van het Web wordt opnieuw gericht aan de MVPD login pagina. | 302 voor volledige omleiding |

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
>**Belangrijk: verplichte parameters -** Ongeacht de implementatie op de client zijn alle bovenstaande parameters verplicht.
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
>**Belangrijk: optionele parameters**
>
>De oproep kan ook optionele parameters bevatten die andere functies mogelijk maken, zoals:
>
> * generic\_data - maakt het gebruik van [Promotie TempPass](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notities** {#notes}

* De waarde van `domain_name` parameter moet worden ingesteld op een van de domeinnamen die zijn geregistreerd bij Adobe Pass-verificatie. Raadpleeg voor meer informatie [Registratie en initialisatie](/help/authentication/programmer-overview.md).

* [Gebruik geen &#39;&amp;&#39;reg\_code in /authenticate request (Tech Note)](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* De `redirect_url` parameter moet de laatste zijn in volgorde

* De waarde van `redirect_url` parameter moet URL-gecodeerd zijn
