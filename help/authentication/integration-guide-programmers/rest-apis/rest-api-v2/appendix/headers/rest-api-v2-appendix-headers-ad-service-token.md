---
title: Koptekst - AD-Service-token
description: REST API V2 - Koptekst - AD-service-token
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---

# Koptekst - AD-Service-token {#header-ad-service-token}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AD-dienst-Symbolische </b> verzoekkopbal bevat het unieke gebruikersherkenningsteken zoals `JWS` die van een identiteitsdienst wordt verkregen die buiten de systemen van de Authentificatie van Adobe Pass loopt.

Deze kopbal wordt ontworpen voor gebruik in enig sign-on (SSO) toegelaten stromen leveraging de Symbolische methode van de Dienst.

Voor meer details over enige sign-on (SSO) toegelaten stromen leveraging de Symbolische methode van de Dienst, verwijs naar [ Enige sign-on gebruikend de stromen van het de dienstteken ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) documentatie.

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AD-dienst-Symbolisch </b>: &lt;unique_user_identifier&gt;</td>
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

<b> unique_user_identifier </b>

De handtekening van JSON-web (`JWS`) die een ondertekend JSON-webtoken (`JWT` ) is dat unieke informatie over de gebruikersnaam bevat.

De `JWT` heeft de volgende kenmerken:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>is</td>
      <td>De unieke id die is gekoppeld aan de entiteit die de toepassing een externe identiteitsservice biedt voor het bereiken van Single Sign-On (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>De unieke id voor de gebruiker, zoals geretourneerd door de externe identiteitsservice.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>Het publiek, dat "Adobe" zou moeten zijn.</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>De publicatie op tijdstempel voor de huidige JWT.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>De vervaltijdstempel voor de huidige JWT.</td>
   </tr>
</table>

De `JWT` moet met het algoritme `SHA256withRSA` worden ondertekend.

`JWT` moet met een privé sleutel, deel van een paar privé sleutel van RSA - openbare sleutel worden ondertekend die door de externe identiteitsdienst wordt beheerd.

De openbare sleutel van dat paar moet aan de Authentificatie van Adobe Pass worden overhandigd om `JWT` tekenen te kunnen erkennen die met de bovengenoemde privé sleutel worden ondertekend.

## Voorbeelden {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
