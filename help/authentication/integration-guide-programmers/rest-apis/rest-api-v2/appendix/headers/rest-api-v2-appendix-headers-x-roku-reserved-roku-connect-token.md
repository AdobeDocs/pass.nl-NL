---
title: Koptekst - X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 - Koptekst - X-Roku-Reserved-Roku-Connect-Token
exl-id: 21016d5b-4d10-4018-a82c-f2797b2d9fb9
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Koptekst - X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> x-Roku-Gereserveerde-roku-Connect-Symbolische </b> verzoekkopbal bevat het unieke platformherkenningsteken zoals `JWS` of `JWE` die van een identiteitsdienst of een bibliotheek buiten de systemen van de Authentificatie van Adobe Pass wordt verkregen.

Deze kopbal wordt ontworpen voor gebruik in enig sign-on (SSO) toegelaten stromen die de methode van de Identiteit van het Platform gebruiken.

Voor meer details over enige sign-on (SSO) toegelaten stromen leveraging de methode van de Identiteit van het Platform, verwijs naar [&#x200B; Enige sign-on gebruikend de stromen van de platformidentiteit &#x200B;](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

## Syntaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> x-Roku-Gereserveerd-roku-verbinden-Token </b>: &lt;unique_platform_identifier&gt;</td>
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

* [Roku SSO Cookbook (REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
