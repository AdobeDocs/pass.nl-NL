---
title: Home-Based Authentication (HBA)
description: Home-Based Authentication (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Home-Based Authentication (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Home-Based Authentication (HBA) is een overal-functie voor tv-abonnees die online toegang tot tv-inhoud kunnen krijgen zonder MVPD-referenties in te voeren wanneer ze verbinding maken met hun thuisnetwerk. Hierdoor wordt de verificatie aanzienlijk verbeterd.

Volgens het OATC:

> &quot;Automatische interne verificatie is het proces waarbij een MVPD/OVD gebruikmaakt van kenmerken van het thuisnetwerk (of van herkenningstekens die automatisch toegankelijk zijn tussen apparaten in het thuisnetwerk) om te verifiëren welke abonneeaccount aan dat thuisnetwerk is gekoppeld, zodat gebruikers niet handmatig aanmeldingsgegevens hoeven in te voeren wanneer zij een TVE-sessie starten voor toegang tot beveiligde inhoud.&quot;

Raadpleeg de volgende bronnen voor meer informatie over Home-Based Authentication (HBA) en de relevante industriestandaarden:

>[!MORELIKETHIS]
>
> * [&#x200B; Onmiddellijke Toegang (HBA) door CTAM &#x200B;](http://www.ctamtve.com/instantaccess)
> * [&#x200B; huis-Gebaseerde Gevallen en Vereisten van het Gebruik van de Authentificatie door OATC &#x200B;](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [&#x200B; Op huis-Gebaseerde Authentificatie Infographic door Adobe &#x200B;](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [&#x200B; Authentificatie met OAuth2 toegelaten MVPDs &#x200B;](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [&#x200B; Authentificatie met SAML toegelaten MVPDs &#x200B;](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [&#x200B; Metagegevens van de Gebruiker &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## HBA-voordelen {#hba-benefits}

Home-Based Authentificatie (HBA) is een zeer belangrijke eigenschap die de login barrière voor kijkers thuis met een actief kabelabonnement verwijdert. Deze hindernis is een belangrijke uitdaging voor tv-diensten overal, waarbij bijna de helft van alle aanmeldingspogingen tot mislukken leidt.

HBA kan de betrokkenheid van de viewer aanzienlijk verbeteren, zodat gebruikers overal toegang krijgen tot tv-inhoud.

## HBA-ondersteuning {#hba-support}

>[!IMPORTANT]
>
> Als u geïnteresseerd bent in het gebruik van HBA-functionaliteit, kunt u contact opnemen met uw Adobe Pass Authentication-vertegenwoordiger voor meer informatie, aangezien bepaalde HBA-stromen in het Premium Workflow-pakket zijn opgenomen.

HBA wordt gesteund door een aantal MVPDs die met de Authentificatie van Adobe Pass worden geïntegreerd, maar om van HBA te profiteren, zou u sommige extra stappen kunnen moeten nemen.

**SAML MVPDs**

Voor SAML MVPDs, wordt HBA geactiveerd slechts op de kant van MVPD.

**OAuth2 MVPDs**

Voor OAuth2 MVPDs, kan HBA via het [&#x200B; Adobe Pass TVE Dashboard van de Integratie van het Dashboard van TVE &#x200B;](https://experience.adobe.com/#/pass/authentication) worden in- en uitgeschakeld door de stappen van de [&#x200B; Gids van de Gebruiker van de Integratie van het Dashboard van TVE te volgen &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPD&#39;s {#hba-support-mvpds}

**SAML MVPDs**

De volgende lijst verstrekt een overzicht van SAML toegelaten MVPDs die HBA steunen:

| MVPD | Basisfunctionaliteit beschikbaar? | Verzendt vlag op authentificatiereactie? |
|--------------|--------------------------------|----------------------------------------|
| DirecTV | Ja | Nee |
| Dish | Ja | Nee |
| Spectrum | Ja | Ja |
| Cox | Ja | Nee |
| AT&amp;T | Ja | Nee |
| Verizon | Ja | Ja |
| Cablevision | Ja | Nee |
| Mediacom | Ja | Nee |
| Midcontinent | Ja | Nee |
| Massilon | Ja | Nee |
| AlticeOne | Ja | Ja |

**OAuth2 MVPDs**

De volgende lijst verstrekt een overzicht van OAuth2 toegelaten MVPDs die HBA steunen:

| MVPD | Basisfunctionaliteit beschikbaar? | Verzendt vlag op authentificatiereactie? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Ja | Ja |

### Adobe Pass-verificatie {#hba-support-adobe-pass-authentication}

In deze sectie wordt de door HBA ingeschakelde ervaring beschreven en wordt de ondersteuning beschreven die wordt geboden door Adobe Pass Authentication, waarbij belangrijke functies worden benadrukt, zoals:

* **Identificatie HBA:** Mogelijkheid om aan Programmeurs te wijzen of de authentificatie HBA of niet-HBA (vereist de steun van MVPD) was.
* **Configureerbare Authentificatie TTLs:** Mogelijkheid om verschillende authentificatie te plaatsen tijd-aan-Levende (TTL) waarden voor authentificatie HBA tegenover niet-HBA (vereist de steun van MVPD).

De volgende tabel biedt een overzicht op hoog niveau van de gebruikerservaring in een HBA en een regelmatige (niet-HBA) verificatiestroom:

| Type verificatie | Gebruikerservaring |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Gebruikers selecteren hun MVPD en worden automatisch geverifieerd bij verbinding met hun thuisnetwerk. Zodra de authentificatie verloopt, moeten de gebruikers hun MVPD opnieuw selecteren, waarna zij automatisch opnieuw voor authentiek zullen worden verklaard. |
| Standaard (niet-HBA) | Gebruikers selecteren hun MVPD en worden gevraagd hun gegevens in te voeren, ongeacht de verbinding met een thuisnetwerk. Zodra de authentificatie verloopt, moeten de gebruikers hun MVPD opnieuw selecteren en hun geloofsbrieven opnieuw ingaan. |

**SAML MVPDs**

De volgende lijst verstrekt een overzicht van de implementatie HBA in het geval van SAML toegelaten MVPDs:

| Handelingen van gebruikers | Systeemhandelingen |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| De gebruiker probeert een video af te spelen.<br/><br/> de plukker van MVPD wordt getoond.<br/><br/> de gebruiker selecteert hun MVPD en blijft login. | Er wordt een achtergrondcontrole uitgevoerd wanneer de MVPD de regels voor gebruikersidentificatie toepast. Dit kan bijvoorbeeld inhouden dat het IP-adres van de gebruiker wordt toegewezen aan het MAC-adres van modules die voorzien zijn van een distributeur of set-top boxes met een breedbandverbinding. |
| Er wordt een scherm weergegeven dat ongeveer 3 seconden aanhoudt.<br/><br/> een interstitiële pagina kan de gebruiker meedelen dat zij automatisch binnen gebruikend hun rekening van MVPD worden ondertekend. | De toepassing opent de authentificatie URL in een gebruikersagent, die een HTTP- verzoek aan het eindpunt van de Authentificatie van Adobe Pass in werking stelt.<br/><br/> het eindpunt van de Authentificatie van Adobe Pass door:sturen het verzoek aan het de authentificatieeindpunt van MVPD via een gebruikersagent redirect.<br/><br/> MVPD wordt verwacht om een authentificatiebesluit in de vorm van een reactie te verzenden SAML die de vlag HBA (`hba_status`) met een waarde van of &quot;waar&quot;of &quot;vals&quot;omvat.<br/><br/> de achtergrond van de Authentificatie van Adobe Pass doet een verzoek aan het eindpunt van het gebruikersprofiel van MVPD om de `hba_status` vlag als deel van [&#x200B; gebruikersmeta-gegevens &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) bloot te stellen. |
| De gebruiker is geverifieerd en kan nu bladeren naar tv-inhoud overal. | De toepassing haalt het gebruikersprofiel op en kan doorgaan met de besluitvorming om de inhoud af te spelen. |

**OAuth2 MVPDs**

De volgende tabel biedt een overzicht van de HBA-implementatie in het geval van MVPD&#39;s die geschikt zijn voor OAuth2:

| Handelingen van gebruikers | Systeemhandelingen |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| De gebruiker probeert een video af te spelen.<br/><br/> de plukker van MVPD wordt getoond.<br/><br/> de gebruiker selecteert hun MVPD en blijft login. | Er wordt een achtergrondcontrole uitgevoerd wanneer de MVPD de regels voor gebruikersidentificatie toepast. Dit kan bijvoorbeeld inhouden dat het IP-adres van de gebruiker wordt toegewezen aan het MAC-adres van modules die voorzien zijn van een distributeur of set-top boxes met een breedbandverbinding. |
| Er wordt een scherm weergegeven dat ongeveer 3 seconden aanhoudt.<br/><br/> een interstitiële pagina kan de gebruiker meedelen dat zij automatisch binnen gebruikend hun rekening van MVPD worden ondertekend. | De toepassing opent de authentificatie URL in een gebruikersagent, die een HTTP- verzoek aan het eindpunt van de Authentificatie van Adobe Pass in werking stelt.<br/><br/> het eindpunt van de Authentificatie van Adobe Pass door:sturen het verzoek aan het de authentificatieeindpunt van MVPD via een gebruikersagent redirect.<br/><br/> het de authentificatieeindpunt van MVPD verzendt een vergunningscode naar het eindpunt van de Authentificatie van Adobe Pass.<br/><br/> de Authentificatie van Adobe Pass gebruikt de vergunningscode om te verzoeken verfrist token en een toegangstoken van het het symbolische eindpunt van MVPD.<br/><br/> MVPD wordt verwacht om een authentificatiebesluit te verzenden dat de vlag HBA (`hba_status`) met een waarde van of &quot;waar&quot;of &quot;vals&quot;als deel van `id_token` omvat.<br/><br/> de achtergrond van de Authentificatie van Adobe Pass doet een verzoek aan het eindpunt van het gebruikersprofiel van MVPD om de `hba_status` vlag als deel van [&#x200B; gebruikersmeta-gegevens &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) bloot te stellen.<br/><br/> MVPD plaatst vernieuwt tekenTTL aan een MVPD-Toegestane waarde en de Adobe plaatst authentificatieTTL aan een waarde minder of gelijk aan de waarde van verfrist teken. |
| De gebruiker is geverifieerd en kan nu bladeren naar tv-inhoud overal. | De toepassing haalt het gebruikersprofiel op en kan doorgaan met de besluitvorming om de inhoud af te spelen. |

>[!IMPORTANT]
>
> In de stroom HBA voor MVPDs die het OAuth 2.0 authentificatieprotocol gebruikt, geeft MVPD een vernieuwingstoken met TTL uit die door zijn bedrijfsvereisten wordt bepaald, terwijl de Adobe een de authentificatietoken van HBA met TTL uitgeeft die niet de vernieuwingstoken TTL moet overschrijden.

## Veelgestelde vragen {#faqs}

1. Waarom is er een onderscheid tussen implementatie HBA voor SAML en protocollen OAuth2?

   De scheiding tussen op huis-Gebaseerde Authentificatie (HBA) voor SAML en protocollen OAuth2 bestaat omdat deze protocollen verschillend in termen van authentificatiemechanismen, configuratie, en implementatieflexibiliteit werken. Voor SAML MVPDs, wordt geen actie vereist van programmeur om HBA toe te laten, terwijl voor OAuth2 MVPDs, HBA via het [&#x200B; Adobe Pass TVE Dashboard &#x200B;](https://experience.adobe.com/#/pass/authentication) kan worden van een knevel voorzien.

1. Wanneer HBA wordt toegelaten, moeten de gebruikers nog hun gebruikersbenaming en wachtwoord tijdens hun aanvankelijke authentificatie ingaan?

   Nee, gebruikersnaam en wachtwoord zijn niet vereist.

1. Hoe kunt u ouderlijke controles afdwingen?

   Adobe Pass Authentication kan HBA uitschakelen voor integratie met kanalen die goedkeuring van de ouderlijke controle nodig hebben. We werken ook samen met OATC aan een UX-document waarin wordt aanbevolen hoe de HBA-ervaring met ouderlijke controle kan worden ingesteld.

1. Gebruiken providers die HBA ondersteunen kortere TTL-vensters voor HBA in vergelijking met regelmatige verificatie?

   De TTL-instelling is configureerbaar. We raden u aan een kortere TTL in te stellen voor MVPD-integratie met HBA om foutafhandeling te voorkomen.
