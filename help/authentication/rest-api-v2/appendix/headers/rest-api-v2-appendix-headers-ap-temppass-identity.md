---
title: Koptekst - AP-TempPass-Identity
description: REST API V2 - Koptekst - AP-TempPass-Identity
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 0%

---


# Koptekst - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AP-TempPass-Identity </b> verzoekkopbal bevat de informatie van de gebruikersidentiteit die wordt gebruikt om promotionele TempPass te bereiken.

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AP-TempPass-Identity </b>: &lt;user_identity_information&gt;</td>
   </tr>
   <tr>
      <td>Type koptekst</td>
      <td>Koptekst aanvragen</td>
   </tr>
   <tr>
      <td>Standaard</td>
      <td>Nee</td>
   </tr>
</table>

## Richtlijnen {#directives}

<b> &lt;user_identity_information></b>

De `Base64-encoded` waarde over de informatie van de gebruikersidentiteit verbonden aan de eindgebruiker waaraan een promotionele tijdelijke toegang moet worden verleend.

## Voorbeelden {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
