---
title: Koptekst - Onderwerptoken voor Adobe
description: REST API V2 - Koptekst - Onderwerptoken voor Adobe
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Koptekst - Onderwerptoken voor Adobe {#header-adobe-subject-token}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> Adobe-Onderwerp-Symbolische </b> verzoekkopbal bevat het unieke platformherkenningsteken zoals `JWS` of `JWE` die van een identiteitsdienst of een bibliotheek wordt verkregen die buiten de systemen van de Authentificatie van Adobe Pass loopt.

Deze kopbal wordt ontworpen voor gebruik in enig sign-on (SSO) toegelaten stromen die de methode van de Identiteit van het Platform gebruiken.

Voor meer details over enige sign-on (SSO) toegelaten stromen leveraging de methode van de Identiteit van het Platform, verwijs naar [ Enige sign-on gebruikend de stromen van de platformidentiteit ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) documentatie.

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> Adobe-Onderwerp-Token </b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon SSO Cookbook (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Voorbeelden {#examples}

Zie de voorbeelden die worden beschreven voor de volgende platformen:

* [Amazon SSO Cookbook (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
