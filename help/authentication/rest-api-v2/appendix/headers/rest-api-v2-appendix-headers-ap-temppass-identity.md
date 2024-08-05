---
title: Koptekst - AP-TempPass-Identity
description: REST API V2 - Koptekst - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Koptekst - AP-TempPass-Identity {#header-ap-temppass-identity}

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
