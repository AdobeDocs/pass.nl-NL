---
title: Gebruik geen '&'reg_code in /authenticate Request
description: Gebruik geen '&'reg_code in /authenticate Request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# (Verouderd) Gebruik geen &#39;&amp;&#39;reg_code in /authenticate Request&#39; {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>



## Probleem

De browser IE 9 interpreteert &#39;\&amp;reg&#39; als een speciale opdracht en zet deze om in ®.

## Toelichting

Als de aanvraag `/authenticate` als volgt is samengesteld...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...het wordt hieronder door de browser van het IE geïnterpreteerd en wordt naar de Adobe verzonden in de volgende notatie:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


De aanvrager\_id wordt geïnterpreteerd als univision®\_code=EKAFMFI, aangezien er geen &#39;&amp;&#39; is, en Adobe zal geen `regCode` param vinden om het token met te associëren.  Er is een kans dat het token AuthN helemaal niet wordt gemaakt. In dat geval zullen aanroepen van `/checkauthn` geen tokens vinden.



## Oplossing

U kunt dit probleem op een van de volgende manieren oplossen:

1. Vermijd het gebruik van de parameter `&reg_code` tussen de andere parameters van de queryreeks.  Verplaats het in plaats daarvan naar de eerste query-string parameter in de request-URL, waarbij de request-URL als volgt wordt uitgevoerd:


       &lt;FQDN>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Op deze manier wordt de parameter `&reg` niet onjuist geïnterpreteerd.

1. Normaliseer `&reg_code` als u `&amp;reg_code` gebruikt.

1. Adobe kon een nieuwe eigenschap introduceren om een foutencode terug naar het tweede scherm in antwoord op een authentificatievraag te verzenden, als de symbolische verwezenlijking AuthN ontbrak.
