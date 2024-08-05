---
title: Koptekst - AP-Partner-Framework-Status
description: REST API V2 - Koptekst - AP-Partner-Kader-Status
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# Koptekst - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De <b> AP-partner-kader-status </b> verzoekkopbal bevat statusinformatie die van een partnerkader wordt verkregen om enig teken-op (SSO) te bereiken.

## Syntaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b> AP-Partner-Kader-Status </b>: &lt;partner_framework_status_information&gt;</td>
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

<b> &lt;partner_framework_status_information> </b>

De `Base64-encoded` -waarde van het JSON-element dat de volgende kenmerken bevat:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Dit is een verplicht kenmerk.
         <br/><br/>
         De de statusinformatie van gebruikerstoestemmingen die door het partnerkader is teruggekeerd en door de toepassing wordt verwerkt.
         <br/><br/>
         Dit is een JSON-element met de volgende kenmerken:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Dit is een verplicht kenmerk.
                  <br/><br/>
                  Dit is een opsomming met de volgende mogelijke waarden:
                  <br/>
                  <ul>
                     <li>toegekend - De gebruiker heeft de toepassing toegang verleend tot abonnementsgegevens.</li>
                     <li>afgewezen - De gebruiker heeft de toepassing de toegang tot abonnementsgegevens geweigerd.</li>
                     <li>in behandeling - De gebruiker heeft er nog niet voor gekozen de toepassing toegang te verlenen tot abonnementsgegevens.</li>
                     <li>notDetermined - De toepassing heeft geen toegang tot abonnementsgegevens.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>fout</td>
               <td>
                  Dit is een optioneel kenmerk.
                  <br/><br/>
                  Dit kan worden gebruikt om de fout van het partnerkader over te gaan voor het geval dat men terwijl het vragen voor de statusinformatie van gebruikerstoestemmingen wordt teweeggebracht.
                  <br/><br/>
                  Dit is een JSON-element met de volgende kenmerken:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Een tekenreeks die de fout uniek identificeert zoals gedefinieerd door het partnerframework.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Een tekenreeks die de beschrijving van de fout bevat zoals gedefinieerd door het partnerframework.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Dit is een verplicht kenmerk.
         <br/><br/>
         De de statusinformatie van de leverancierslogin die door het partnerkader is teruggekeerd en door de toepassing wordt verwerkt.
         <br/><br/>
         Dit is een JSON-element met de volgende kenmerken:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Dit is een verplicht kenmerk.
                  <br/><br/>
                  Dit is mappingId die MVPD identificeert die tijdens de authentificatiestroom op het niveau van het partnerkader wordt gebruikt.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Dit is een verplicht kenmerk.
                  <br/><br/>
                  Dit is de vervaldatum van het voor authentiek verklaarde gebruikersprofiel, voor het geval de gebruiker met succes het programma heeft geopend gebruikend gesteunde MVPD op het niveau van het partnerkader.
               </td>
            </tr>
            <tr>
               <td>fout</td>
               <td>
                  Dit is een optioneel kenmerk.
                  <br/><br/>
                  Dit kan worden gebruikt om de fout van het partnerkader over te gaan voor het geval dat men terwijl het vragen naar de informatie van de leverancierslogin wordt teweeggebracht.
                  <br/><br/>
                  Dit is een JSON-element met de volgende kenmerken:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Kenmerk</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Een tekenreeks die de fout uniek identificeert zoals gedefinieerd door het partnerframework.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Een tekenreeks die de beschrijving van de fout bevat zoals gedefinieerd door het partnerframework.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Voorbeelden {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
