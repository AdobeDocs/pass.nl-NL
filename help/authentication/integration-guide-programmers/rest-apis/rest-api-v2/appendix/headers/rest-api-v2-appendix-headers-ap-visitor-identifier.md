---
title: Koptekst - AP-bezoeker-id
description: REST API V2 - Koptekst - AP-bezoeker-id
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 7d19319d83af0122624ab3b80b1f31fc0cbbf182
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

Voor meer details over het gebruik van ECID in de Authentificatie van Adobe Pass, verwijs naar [&#x200B; Gebruikend identiteitskaart van Experience Cloud in de Authentificatie van Adobe Pass &#x200B;](../../../../features-premium/analytics/exp-cloud-id-authn.md) documentatie.

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
