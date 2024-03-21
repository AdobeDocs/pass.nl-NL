---
title: CMU API-toegang
description: CMU API-toegang
source-git-commit: 30631ac006b7944cb2eb8996c2c165343b6be5fe
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# Toegang tot API voor gelijktijdige bewaking {#cmu-api-usage-access}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan. Neem contact op met uw Adobe voor vragen over beschikbaarheid.

## Overzicht van de toegangsprocedure {#api-access-procedure-overview}

Wij hebben CMU- rapporten toegang bijgewerkt om met het Dynamische Protocol van de Registratie van de Cliënt van OAuth 2.0 compatibel te zijn.
Een douane OAuth 2.0 vergunningsserver wordt opgesteld om de behoeften van de toepassing van de Controle van de Valuta te richten. \
Opdat de toepassingen van de Cliënt de vergunning OAuth 2.0 gebruiken, moet de server dynamisch registreren om specifieke informatie (cliëntgeloofsbrieven) te verkrijgen om met het te kunnen communiceren. Als deel van het registratieproces, moet de cliënt een reeks ingebouwde meta-gegevens aan het eindpunt van de cliëntregistratie voorleggen.
Deze metagegevens worden doorgegeven als een softwareinstructie, die een &quot;software_id&quot; bevat waarmee onze verificatieserver verschillende exemplaren van een toepassing kan correleren met dezelfde softwareinstructie.
Een softwareverklaring is een Token van het Web JSON (JWT) die meta-gegevenswaarden over de cliëntsoftware als bundel bevestigt. Wanneer voorgesteld aan de vergunningsserver als deel van een verzoek van de cliëntregistratie, moet de softwareverklaring digitaal worden ondertekend of MACed gebruikend de Handtekening van het Web JSON (JWS). \
In de officiële documentatie vindt u een gedetailleerdere uitleg van de software-instructies en de manier waarop deze werken  <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Voer de stappen in de onderstaande secties uit om toegang te krijgen.

## Toegangsprocedures {#access-procedure-steps}

1. Een geregistreerde toepassing hebben op de Adobe Pass DCR-server. Neem voor deze stap contact op met onze [Ondersteuningsteam](mailto:tve-support@adobe.com).
2. De software-instructie ophalen
   1. Ga naar TVE-dashboard <a href="https://console-preprod.auth.adobe.com/#!/" target="_blank"> Pre-Prod </a> of <a href="https://console.auth.adobe.com/" target="_blank">PROD</a>
   2. Programmeur selecteren
   3. Ga naar tabblad Toepassingen
   4. Toepassing selecteren
   5. Klik op Software-instructie DownLoad om een bestand weer te geven dat vergelijkbaar is met onderste opname
      <figure>
          <img src="assets/software_statement_1_download.png"
               alt="Softwareinstructie downloaden">
       </figure>
      <figure>
          <img src="assets/software_statement_2.png"
               alt="Voorbeeld van softwareinstructie">
       </figure>

3. Toegangstoken verkrijgen
   1. Krijg cliëntgeloofsbrieven door de hierboven verkregen softwareverklaring te gebruiken en de hieronder vraag uit te voeren. Op deze manier zal een client_id - client_geheime paar worden verkregen, die kan worden gebruikt om het toegangstoken te krijgen.
      *Deze stap mag niet elke keer worden uitgevoerd. Het zou opnieuw moeten worden gedaan slechts wanneer de geloofsbrieven verlopen.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Clientgegevens ophalen">
       </figure>

   2. Krijg toegangstoken door de lagere vraag te gebruiken. Gebruik dit toegangstoken om een CMU API aan te roepen tot het teken zal verlopen.
      *Deze stap zou slechts moeten worden uitgevoerd als het laatste geproduceerde teken verliep.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Toegangstoken ophalen">
       </figure>

4. Vraag CMU API - zie verwante informatie hieronder.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="CMU-API aanroepen">
       </figure>

## Gerelateerde informatie {#related-information}

* [CMU-overzicht](/help/concurrency-monitoring/cm-usage-reports.md)
* [CMU-API](/help/concurrency-monitoring/cmu-api.md)
