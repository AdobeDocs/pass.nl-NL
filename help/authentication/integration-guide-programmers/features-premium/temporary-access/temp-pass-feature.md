---
title: Functie TempPass
description: Functie TempPass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# Functie TempPass {#temp-pass-feature}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

TempPass is een veelzijdige functie waarmee programmeurs tijdelijke toegang tot hun beveiligde inhoud kunnen bieden aan gebruikers zonder geldige MVPD-accountgegevens. Het dient als een effectief instrument om kijkers te betrekken, hetzij door basistoegangsscenario&#39;s of gerichte promotiecampagnes.

TempPass is een krachtige oplossing voor programmeurs om:

* **de Kijkers van de Mogelijkheid:** bied een smaak van premieinhoud aan om nieuwe abonnees aan te trekken.
* **Bevorderingen van de Aandrijving:** Looppas gerichte campagnes om inhoudsblootstelling te verhogen en merkloyaliteit te bouwen.
* **Behoud Controle:** beheert toegangsperioden, dwingt grenzen af, en herstelt toegang zoals nodig om met bedrijfsdoelstellingen te richten.

De TempPass-functie wordt geleverd door een pseudo-MVPD (verder genoemd &quot;Temp Pass&quot;) in de configuratie van de Adobe Pass-verificatieserver in te voeren als integratie met de deelnemende programmeur. De functie TempPass is beschikbaar in twee configuraties:

* [ Basis TempPass ](#basic-temp-pass) voor op tijd-gebaseerde toegang.
* [ Promotional TempPass ](#promotional-temp-pass) voor flexibele campagne-gedreven toegang.

>[!IMPORTANT]
>
> De TempPass-functie is een premiumfunctie en vereist een huidige licentie van Adobe.

In de volgende tabel vindt u een korte vergelijking tussen de functies Standaard en Promotional TempPass:

| **Eigenschap** | **Basis TempPass** | **Promotional TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Toegang tot Inhoud** | <ul><li>Op tijd gebaseerd</li></ul> | <ul><li>Op tijd gebaseerd</li><li>Beperkt tot een maximum aantal bronnen</li></ul> |
| **Veiligheid van de Toegang die op** wordt gebaseerd | <ul><li>Apparaat-id</li></ul> | <ul><li>Apparaat-id</li><li>Hash van de opgegeven gebruikers-id-informatie (bijvoorbeeld e-mail)</li></ul> |
| **Verbeterde Codes van de Fout** | Beschikbaar | Beschikbaar |
| **TempPass het Kwaliteit van het Terugstellen** | Beschikbaar | Beschikbaar |

>[!IMPORTANT]
> 
> De Authentificatie van Adobe Pass omvat geen ingebouwd mechanisme om de aan de gang zijnde stroom automatisch te stoppen zodra de toegewezen tijd (X minuten) is verstreken. Het is de verantwoordelijkheid van programmeurs om toegangsbeperkingen af te dwingen zodra de TempPass tijdens een aan de gang zijnde stroom verloopt.

Of u nu een blik werpt op uw inhoudsbibliotheek of een selectiekadergebeurtenis promoot, TempPass verschaft u de gereedschappen om uw publiek uit te breiden en de controle over de toegang te behouden.

## Standaardcontrole {#basic-temp-pass}

De basis eigenschap TempPass staat Programmeurs toe om tijd beperkte toegang tot inhoud te verlenen, catering aan diverse scenario&#39;s:

* **Korte Voorproeven:** de korte voorproeven van de aanbieding, zoals een 10 minieme dagelijkse toegangsperiode, om potentiële abonnees aan te trekken.
* **gebeurtenis-Gebaseerde Toegang:** laat langere toegang voor belangrijke gebeurtenissen, zoals een zitting van 4 uur toe.
* **Toegang van de Combinatie:** Mix en gelijke duur, zoals een aanvankelijke uitgebreide het bekijken periode die door kortere dagelijkse voorproeven over verscheidene dagen wordt gevolgd.

Voor bepaalde gebeurtenissen kan gefaseerde vrije toegang tot inhoud nodig zijn, zoals een eerste verlengde vrije toegangsperiode (bijvoorbeeld 4 uur), gevolgd door kortere dagelijkse vrije toegangsintervallen (bijvoorbeeld 10 minuten per dag). Om dit scenario uit te voeren, moeten de programmeurs met hun vertegenwoordiger van Adobe coördineren om twee MVPDs te vormen TempPass die aan hun behoeften wordt aangepast.

Als u bijvoorbeeld een eerste sessie van 4 uur gratis wilt houden, gevolgd door dagelijkse 10 minuten gratis sessies, kan Adobe configureren voor de programmeur:

* **TempPass1**: Gevormd met een tijd-aan-Levend (TTL) van 4 uren om de aanvankelijke vrije toegangsperiode te behandelen.
* **TempPass2**: Gevormd met een tijd-aan-Levend (TTL) van 10 minuten voor de verdere dagelijkse vrije toegangsintervallen.

Om juiste functionaliteit voor dagelijkse toegang te verzekeren, moet TempPass2 voor alle apparaten bij 00 :00 uren elke dag worden teruggesteld.

### Details onderdeel {#basic-temp-pass-feature-details}

**de parameters van de Configuratie:**

* **TTL (tijd-aan-Levend):** de programmeurs kunnen de duur van toegang specificeren. Deze op klok gebaseerde TTL verloopt ongeacht daadwerkelijke het bekijken tijd.

**identiteitskaart van de Gebruiker:**

De basisfunctie TempPass gebruikt de apparaat-id als een gebruikersidentificatieparameter.

In de volgende tabel ziet u hoe parameters voor gebruikersidentificatie van invloed zijn op de proefervaring van gebruikers:

| Apparaat-id | Resultaat |
|-------------------|----------------|
| Nieuw | Nieuwe proefversie |
| Bestaande | Bestaande proefversie |

**de tijdberekening van de Mening:**

De TTL vertegenwoordigt de duur van de aanvankelijke tijd van het vergunningsverzoek aan de vervaltijd, onafhankelijk van de daadwerkelijke tijd die het bekijken van inhoud besteedt. Elk toekomstig verzoek controleert de huidige servertijd tegen de opgeslagen vervaltijd om toegang te verlenen.

**Authentificatie:**

Verificatie is niet vereist voor Basic TempPass, zodat u rechtstreeks naar de autorisatiestap kunt gaan.

**Vergunning:**

Aangezien er geen interactie is met een werkelijke MVPD, zal de Basic- ‘Temp Pass’-MVPD alle bronnen autoriseren op basis van de TempPass die geldig is. Als de verificatie succesvol is, blijft de bibliotheek Media Token Verifier van toepassing voor het verifiëren van het media-token en het garanderen van de validatie van bronnen voordat het afspelen van inhoud wordt gestart.

Het vergunningsbesluit is gebaseerd op de gebruikersidentificatieparameters en de gevormde TTL. Om een vergunning voor een middel te verkrijgen, moet een geldig verzoek aan de volgende voorwaarden voldoen:

* **Onverbruikte duur:** de vervaltijd wordt gegevens verwerkt door de aanvankelijke tijd van het vergunningsverzoek (die in onze gegevensbestanden wordt bewaard) aan gevormde TTL toe te voegen. De huidige servertijd wordt vergeleken met deze vervaltijd om te bepalen of TempPass nog geldig is.

Als een gebruiker de geconfigureerde TTL overschrijdt, kan hij of zij de inhoud op hetzelfde apparaat niet meer bekijken, tenzij zijn of haar TempPass opnieuw wordt ingesteld.

**preAuthorisation:**

Wanneer er een aanvraag voor voorafgaande toestemming wordt ingediend voor een MVPD met eenvoudige &#39;Temp Pass&#39;, retourneert de reactie de volledige lijst met bronnen van de aanvraag als vooraf geautoriseerd. Dit gedrag weerspiegelt de logica van de vergunning, aangezien de vergunningsvoorwaarden op termijnen in plaats van specifieke middelen zijn gebaseerd. Zolang de tijdbeperking geldig is, zullen de gevraagde middelen worden toegelaten.

**Logout:**

Afmelden is niet vereist voor Basic TempPass, zodat u rechtstreeks naar de verificatiestap kunt gaan met een MVPD-gebruiker.

**het Volgen gegevens en analyses:**

Tijdens de standaard TempPass-flow wordt voor het bijhouden van gegevens een gehashte versie van de apparaat-id gebruikt, waarbij de MVPD-id is ingesteld op &#39;Temp Pass&#39;. Programmeurs moeten de waarden van TempPass van standaardMVPD metriek in hun analytische implementaties onderscheiden.

## Promotie TempPass {#promotional-temp-pass}

De promotionele TempPass-functie breidt de mogelijkheden van de basis-TempPass uit, die speciaal zijn ontworpen voor het uitvoeren van promotiecampagnes. Met deze functie kunnen programmeurs gebruikers inschakelen door toegang toe te staan tot een vooraf gedefinieerd aantal VOD-titels gedurende een opgegeven periode nadat ze een geldige gebruikersnaam, zoals een e-mailadres, hebben verzameld.

Promotionele TempPass omvat alle functionaliteit van de basisTempPass, met extra flexibiliteit voor:

* Het maximumaantal VOD-titels bepalen dat tijdens de promotieperiode toegankelijk is.
* Vaststelling van de periode waarin de promotionele toegang geldig is.

Als een gebruiker de vooraf gedefinieerde toegangslimieten (aantal VOD-titels of duur) overschrijdt, kan hij of zij de inhoud van hetzelfde apparaat of dezelfde gebruikersnaam niet meer bekijken, tenzij de TempPass opnieuw wordt ingesteld.

### Details onderdeel {#promotional-temp-pass-feature-details}

**de parameters van de Configuratie:**

* **Sleutel van de Informatie van de Gebruiker:** De sleutel die wordt gebruikt om het user-provided herkenningsteken, zoals een e-mailadres (d.w.z., is de sleutel e-mail) mee te delen.
* **Aantal Middelen:** bepaalt hoeveel titels van VOD een gebruiker kan toegang hebben.
* **TTL (tijd-aan-Levend):** de duur waarin de gebruiker de toegestane middelen kan verbruiken.

**identiteitskaart van de Gebruiker:**

De eigenschap Promotional TempPass gebruikt de knoeiboel van het user-provided herkenningsteken bovenop het apparatenherkenningsteken als parameters van de gebruikersidentificatie.

>[!IMPORTANT]
>
> De bevestiging en het hakken van user-provided herkenningsteken worden beheerd door Programmer, niet Adobe. Adobe slaat geen persoonlijk identificeerbare informatie (PII) op. Als zodanig is de programmeur verantwoordelijk voor het genereren en verzenden van een hash van de unieke door de gebruiker opgegeven id tijdens de interactie met de Adobe Pass-API&#39;s voor verificatie.

Adobe adviseert gebruikend de **SHA-2** familie of zijn specifieke **SHA-256**, **SHA-512** functies op gegevens alvorens het naar Adobe wordt verzonden. Bijvoorbeeld, **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a3 32b18b88d09069fb7&quot;**.

In de volgende tabel ziet u hoe parameters voor gebruikersidentificatie van invloed zijn op de proefervaring van gebruikers:

| Door gebruiker opgegeven id-hash | Apparaat-id | Resultaat |
|-------------------------------|-------------------|---------------------------------------------------------|
| Nieuw | Nieuw | Nieuwe proefversie |
| Bestaande | Nieuw | Bestaande proefversie (gebaseerd op door de gebruiker opgegeven id-hash) |
| Nieuw | Bestaande | Bestaande proefversie (gebaseerd op apparaat-id) |
| Bestaande | Bestaande | Bestaande proefversie |

**de tijdberekening van de Mening:**

De TTL vertegenwoordigt de duur van de aanvankelijke tijd van het vergunningsverzoek aan de vervaltijd, onafhankelijk van de daadwerkelijke tijd die het bekijken van inhoud besteedt. Elk toekomstig verzoek controleert de huidige servertijd tegen de opgeslagen vervaltijd om toegang te verlenen.

**Authentificatie:**

Verificatie is niet vereist voor Promotional TempPass, zodat u rechtstreeks naar de machtigingsstap kunt gaan.

Ter ondersteuning van de implementatie van de toepassing van de programmeur, stelt Promotional TempPass de volgende informatie van gebruikersmeta-gegevens beschikbaar, toegankelijk door overeenkomstige sleutels:

* **`remaining_resources`**: geeft het aantal bronnen aan dat de gebruiker nog mag gebruiken.
* **`used_assets`**: bevat een lijst met bronnen die de gebruiker al heeft verbruikt.
* **`expiration_date`**: geeft de vervaldatum weer van de tijdelijke controle voor speciale acties van de gebruiker.

**Vergunning:**

Aangezien er geen interactie met een daadwerkelijke MVPD is, zal de promotionele &#39;Temp Pass&#39;-MVPD alle bronnen toestaan, gezien de TempPass geldig is. Als de verificatie succesvol is, blijft de bibliotheek Media Token Verifier van toepassing voor het verifiëren van het media-token en het garanderen van de validatie van bronnen voordat het afspelen van inhoud wordt gestart.

Het vergunningsbesluit is gebaseerd op de parameters van gebruikersidentificatie, en het gevormde aantal middelen en TTL. Om een vergunning voor een middel te verkrijgen, moet een geldig verzoek aan de volgende voorwaarden voldoen:

* **Onverbruikte duur:** de vervaltijd wordt gegevens verwerkt door de aanvankelijke tijd van het vergunningsverzoek (die in onze gegevensbestanden wordt bewaard) aan gevormde TTL toe te voegen. De huidige servertijd wordt vergeleken met deze vervaltijd om te bepalen of TempPass nog geldig is.
* **Onverbruikte middelen:** het aantal verbruikte middelen wordt gevolgd (bewaard in onze gegevensbestanden). Het aantal verbruikte middelen wordt vergeleken tegen het gevormde aantal middelen om te bepalen als TempPass nog geldig is.

Als een gebruiker de geconfigureerde TTL of het aantal bronnen overschrijdt, kan hij of zij de inhoud op hetzelfde apparaat of met dezelfde door de gebruiker opgegeven id niet meer bekijken, tenzij zijn TempPass opnieuw wordt ingesteld.

**preAuthorisation:**

Wanneer er een aanvraag voor voorafgaande toestemming is ingediend voor een MVPD met speciale promotietemperatuur, retourneert de reactie de volledige lijst met bronnen van de aanvraag als vooraf geautoriseerd. Dit gedrag weerspiegelt de logica van de vergunning, aangezien de vergunningsvoorwaarden op termijnen en het totale aantal betreden middelen, eerder dan specifieke middelen gebaseerd zijn. Zolang de tijdbeperking geldig is en de bronnenlimiet niet is overschreden, worden de gevraagde bronnen geautoriseerd.

**Logout:**

Afmelden is niet vereist voor speciale TempPass-service, zodat u rechtstreeks naar de verificatiestap kunt gaan met een MVPD-gebruiker.

**het Volgen gegevens en analyses:**

Tijdens de promotionele TempPass-flow wordt voor het bijhouden van gegevens een gehashte versie van de apparaat-id gebruikt, waarbij de MVPD-id is ingesteld op &#39;Temp Pass&#39;. Programmeurs moeten de waarden van TempPass van standaardMVPD metriek in hun analytische implementaties onderscheiden.

## Toegang tot TempPass-API opnieuw instellen {#reset-tempass-api-access}

Voordat u toegang krijgt tot de API voor tempPass opnieuw instellen, moet u de vereiste stappen in het DCR-proces (Dynamic Client Registration) uitvoeren. Dit verplichte proces zorgt ervoor dat u over het benodigde toegangstoken beschikt voor interactie met de API voor het opnieuw instellen van TempPass.

Voor uitvoerige instructies, verwijs naar het [ Dynamische Overzicht van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.

## TempPass-API opnieuw instellen - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Om een specifieke TempPass voor een apparaat of alle apparaten opnieuw in te stellen, voorziet de Authentificatie van Adobe Pass Programmeurs van een API die voor zowel Basis als Promotional TempPass werkt.

### Verzoek {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Zoekparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">aanvrager_id</td>
      <td>De interne unieke id die tijdens het instapproces aan de Serviceleverancier is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>De interne unieke id die tijdens het instapproces aan de TempPass is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            De apparaat-id waarvoor deze herstelbewerking geldig is.
            <br/><br/>
            Als er geen waarde is opgegeven, wordt de herstelbewerking op alle apparaten toegepast.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van het tovenaarstoken nuttige lading wordt beschreven in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> ontvangt toegangstoken </a> documentatie.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

### Antwoord {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Geen inhoud</td>
      <td>
        Het opnieuw instellen is gelukt.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Verboden</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt nieuwe cliëntgeloofsbrieven en een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
</table>

### Voorbeelden {#reset-tempass-v3-reset-samples}

#### Temperatuurcontrole opnieuw instellen voor een specifiek apparaat {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Temperatuur-controle opnieuw instellen voor alle apparaten {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## TempPass-API opnieuw instellen - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Om een specifieke TempPass voor een generische sleutel (user-provided identifier hash) of alle sleutels terug te stellen, voorziet de Authentificatie van Adobe Pass Programmeurs van een API die voor Promotional TempPass werkt.

### Verzoek {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">pad</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">methode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Zoekparameters</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">aanvrager_id</td>
      <td>De interne unieke id die tijdens het instapproces aan de Serviceleverancier is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>De interne unieke id die tijdens het instapproces aan de TempPass is gekoppeld.</td>
      <td><i>vereist</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            De door de gebruiker opgegeven id-hash waarvoor deze herstelbewerking geldig is.
            <br/><br/>
            Als er geen waarde is opgegeven, is de herstelbewerking van toepassing op alle gebruikers.
      </td>
      <td>optioneel</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopteksten</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Toestemming</td>
      <td>De generatie van het tovenaarstoken nuttige lading wordt beschreven in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> ontvangt toegangstoken </a> documentatie.</td>
      <td><i>vereist</i></td>
   </tr>
</table>

### Antwoord {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Tekst</th>
      <th style="background-color: #EFF2F7;">Beschrijving</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Geen inhoud</td>
      <td>
        Het opnieuw instellen is gelukt.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ongeldig verzoek</td>
      <td>
        De aanvraag is ongeldig, de client moet de aanvraag corrigeren en het opnieuw proberen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Ongeautoriseerd</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Verboden</td>
      <td>
        Het toegangstoken is ongeldig, moet de cliënt nieuwe cliëntgeloofsbrieven en een nieuw toegangstoken verkrijgen en opnieuw proberen. Voor meer details verwijs naar de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Dynamische documentatie van de Registratie van de Cliënt van het Overzicht </a>.
      </td>
   </tr>
</table>

### Voorbeelden {#reset-tempass-v3-reset-generic-samples}

#### Temperatuur opnieuw instellen voor een specifieke toets {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Temperatuur opnieuw instellen voor alle toetsen {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

Leveraging de eigenschap TempPass vereist het uitvoeren van codeupdates om te wijzigen hoe uw toepassing van TV overal (TVE) met de Authentificatie van Adobe Pass [ REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in wisselwerking staat.

Voor een uitvoerige gids over deze updates en de bijbehorende werkschema&#39;s, verwijs naar de [ Tijdelijke documentatie van de Stromen van de Toegang ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
