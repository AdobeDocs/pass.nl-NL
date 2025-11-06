---
title: Header - Adobe-Subject-token
description: REST API V2 - Koptekst - Adobe-Onderwerptoken
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Header - Adobe-Subject-token {#header-adobe-subject-token}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> Adobe-Onderwerp-Symbolische </b> verzoekkopbal bevat het unieke platformherkenningsteken zoals `JWS` of `JWE` die van een identiteitsdienst of een bibliotheek wordt verkregen die buiten de systemen van de Authentificatie van Adobe Pass loopt.

Deze kopbal wordt ontworpen voor gebruik in enig sign-on (SSO) toegelaten stromen die de methode van de Identiteit van het Platform gebruiken.

Voor meer details over enige sign-on (SSO) toegelaten stromen leveraging de methode van de Identiteit van het Platform, verwijs naar [&#x200B; Enige sign-on gebruikend de stromen van de platformidentiteit &#x200B;](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

## Syntaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> Adobe-Subject-Token </b>: &lt;unique_platform_identifier&gt;</td>
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

<b> unique_platform_identifier </b>

De handtekening van JSON-web (`JWS`) of JSON-webcodering (`JWE` ) die een ondertekend of gecodeerd JSON-webtoken (`JWT` ) is dat unieke informatie over de platform-id bevat.

Dit is beschikbaar voor de volgende platforms:

* [Amazon SSO Cookbook (REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Voorbeelden {#examples}

Zie de voorbeelden die worden beschreven voor de volgende platformen:

* [Amazon SSO Cookbook (REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
