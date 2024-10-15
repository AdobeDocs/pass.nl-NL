---
title: Koptekst - AP-TempPass-Identity
description: REST API V2 - Koptekst - AP-TempPass-Identity
exl-id: a6238a58-a3f1-495d-a9d1-82475f5ffc60
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
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

X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
