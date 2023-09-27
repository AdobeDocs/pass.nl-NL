---
title: Gebruik geen '&'reg_code in /authenticate Request
description: Gebruik geen '&'reg_code in /authenticate Request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Gebruik geen &#39;&amp;&#39;reg_code in /authenticate Request {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>



## Probleem

De browser IE 9 interpreteert &#39;\®&#39; als een speciale opdracht en zet deze om in ®.

## Toelichting

Als de `/authenticate` De aanvraag is als volgt samengesteld...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...het wordt hieronder door de browser van het IE geïnterpreteerd en wordt naar de Adobe verzonden in de volgende notatie:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


De aanvrager\_id wordt geïnterpreteerd als univision®\_code=EKAFMFI, aangezien er geen &#39;&amp;&#39; is en de Adobe geen `regCode` param om het token aan te koppelen.  Er is een kans dat het token AuthN helemaal niet wordt gemaakt, in welk geval `/checkauthn` de vraag zal om het even welke tokens niet vinden.



## Oplossing

U kunt dit probleem op een van de volgende manieren oplossen:

1. Vermijd het gebruik van de `&reg_code` param tussen de andere parameters van het vraagkoord.  Verplaats het in plaats daarvan naar de eerste query-string parameter in de request-URL, waarbij de request-URL als volgt wordt uitgevoerd:


       &lt;fqdn>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Op die manier `&reg` param wordt niet onjuist geïnterpreteerd.

1. Normaliseren `&reg_code` zoals gebruiken `&amp;reg_code`.

1. Adobe kon een nieuwe eigenschap introduceren om een foutencode terug naar het tweede scherm in antwoord op een authentificatievraag te verzenden, als de symbolische verwezenlijking AuthN ontbrak.
