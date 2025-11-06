---
title: Koptekst - AP-bezoeker-id
description: REST API V2 - Koptekst - AP-bezoeker-id
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# Koptekst - AP-bezoeker-id {#header-ap-visitor-identifier}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AP-Bezoeker-Herkenningsteken </b> verzoekkopbal bevat `ECID` die door de cliënttoepassing wordt vereist om een bezoeker over de oplossingen van Adobe Experience Cloud uniek te identificeren.

Voor meer details over het gebruik van ECID in de Authentificatie van Adobe Pass, verwijs naar [ Gebruikend identiteitskaart van Experience Cloud in de Authentificatie van Adobe Pass ](../../../../features-premium/analytics/exp-cloud-id-authn.md) documentatie.

## Syntaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AP-Bezoeker-Herkenningsteken </b>: &lt;bezoekor_identifier&gt;</td>
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

## Voorbeelden {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
