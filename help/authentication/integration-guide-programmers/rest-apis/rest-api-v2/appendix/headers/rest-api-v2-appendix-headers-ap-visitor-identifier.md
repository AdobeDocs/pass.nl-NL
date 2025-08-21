---
title: Koptekst - AP-bezoeker-id
description: REST API V2 - Koptekst - AP-bezoeker-id
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
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
