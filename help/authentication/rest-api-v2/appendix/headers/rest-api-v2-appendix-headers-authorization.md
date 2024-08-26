---
title: Koptekst - Autorisatie
description: REST API V2 - Koptekst - Autorisatie
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---


# Koptekst - Autorisatie {#header-authorization}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De </b> verzoekkopbal van de Vergunning 0} {bevat het `Bearer` toegangstoken dat door de cliënttoepassing wordt vereist om tot Adobe Pass beschermde APIs toegang te hebben.<b>

Voor meer details over het mechanisme om tot Adobe Pass beschermde APIs toegang te hebben, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt 1} documentatie.](../../../dcr-api/dynamic-client-registration-overview.md)

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> Vergunning </b>: drager &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Type koptekst</td>
      <td>Koptekst aanvragen</td>
   </tr>
   <tr>
      <td>Standaard</td>
      <td>Ja</td>
   </tr>
</table>

## Richtlijnen {#directives}

<b> &lt;access_token></b>

De waarde van het toegangstoken is een ondoorzichtige waarde die een beperkte tijd-aan-levende (b.v., 24 uur) heeft die van Adobe Pass moet worden verkregen zoals die in [ wordt beschreven terugwinnen toegangstoken ](../../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.

## Voorbeelden {#examples}

```JSON
Authorization: bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
