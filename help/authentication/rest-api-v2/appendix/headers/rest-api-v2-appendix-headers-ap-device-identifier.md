---
title: Koptekst - AP-apparaat-id
description: REST API V2 - Koptekst - AP-apparaat-id
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# Koptekst - AP-apparaat-id {#header-ap-device-identifier}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AP-Apparaat-Herkenningsteken </b> verzoekkopbal bevat het stromen apparatenherkenningsteken aangezien het door de cliënttoepassing werd gecreeerd.

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AP-apparaat-Herkenningsteken </b>: &lt;type&gt; &lt;identifier&gt;</td>
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

<b>&lt;type> </b>

Het type apparaat-id.

Er zijn slechts één ondersteunde typen, zoals hieronder wordt weergegeven.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Type</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>vingerafdruk</td>
      <td>De apparaat-id bestaat uit een dekkende id en wordt gemaakt door de clienttoepassing.</td>
   </tr>
</table>


<b> &lt;identifier> </b>

De `Base64-encoded` -waarde van de apparaat-id.

## Voorbeeld {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
