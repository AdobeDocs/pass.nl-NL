---
title: MVPD-integratiefuncties
description: MVPD-integratiefuncties
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 2%

---

# MVPD-integratiefuncties

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#mvpd-int-features-overview}

Adobe Pass Authentication biedt ondersteuning voor de nieuwe standaarden voor tv-overal. Het is volgzaam met de **(Online Toegang van de Inhoud) specificatie CableLabs OLCA**, die technische vereisten en architectuur voor de levering van video aan een klant van de Tv van de Betaal van online bronnen verstrekt. Adobe nam in juni 2011 deel aan het gezamenlijke CableLabs-interoptestproject en stemde in met het testproces voor een Service Provider-implementatie.  De Adobe is ook een actief lid van **OATC (het Open Technische Consortium van de Authentificatie)** en neemt aan verscheidene van de specificatie-opstellende projecten van subcomités als deel van dat lichaam deel.

Nadat u de Adobe Pass Authentication OLCA-compatibiliteit hebt beschreven, en de deelname van de Adobe aan OATC, is het ook belangrijk om te weten dat Adobe Pass Authentication eigenlijk &#39;protocol agnostic&#39; is.  Maar in deze fase van het TVE-tijdperk is Adobe Pass Authentication zeker gericht op de OLCA-standaarden.  De normen bepalen momenteel overeengekomen manieren voor de verschillende spelers TVE (Programmers, MVPDs, en Dienstverleners) om de eigenschappen van TVE uit te voeren. Veel van deze functies worden weergegeven in de onderstaande tabellen, met koppelingen naar verwante pagina&#39;s die details en voorbeelden geven van hoe u de functies kunt implementeren.

De informatie in de tabellen is bedoeld om het Adobe Pass Authentication-integratieproces te sturen naar consistente functionaliteit in alle MVPD-integraties. De prioritaire kolom rangschikt de eigenschappen in A, B, en C:

* A - Dit zijn &quot;must&quot;-kenmerken voor de eerste uitrol van een integratie.
* B - Dit zijn belangrijke verbeteringen aan de eerste integratie, die na de eerste uitrol moeten worden toegevoegd.
* C - Dit zijn verdere verbeteringen van de integratie die kunnen worden geïmplementeerd na de B-vereisten.


## 1. Functionele basisfuncties {#core-func-features}


| Nee. | Functie | Beschrijving | Prioriteit | Notities |
|------|---------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [ Programmer-in werking gestelde Login ](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md) | De viewer selecteert de MVPD en start de verificatiestroom (AuthN) vanaf de website of toepassing van het merk Programmer. | A+ |                                                                                                                                                          |
| 1,2 | [ op kanaal-gebaseerde Vergunning ](/help/authentication/integration-guide-mvpds/authz-usecase.md) | Zodra voor authentiek verklaard, kan de vergunning (AuthZ) op de achtergrond voorkomen, met het overgaan van slechts een netwerkkanaalherkenningsteken, en een gebruikersidentificatie, aan MVPD. | A+ |                                                                                                                                                          |
| 1,3 | Persistentie van gebruikersnaam | De MVPD biedt een onduidelijke, permanente gebruikersnaam. | A |                                                                                                                                                          |
| 1,4 | [ Enige Sign-On Steun ](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-support.md) | De viewer meldt zich aan bij de MVPD op de site van één merk en kan vervolgens naar een ander merk gaan en niet worden gevraagd zich opnieuw aan te melden. De sessie AuthN wordt gedeeld tussen de merken. SSO-ondersteuning is zowel voor websites als voor mobiele toepassingen/apparaattoepassingen.  Voor MVPDs, vereist het het terugkeren van of een Gebruiker - identiteitskaart of één of ander ander ander gebruikerstoken dat voor AuthZ over merken kan worden gebruikt. | A |                                                                                                                                                          |
| 1,5 | Apparaatgeoptimaliseerde gebruikerservaring voor aanmelding (UX) | De MVPD biedt ondersteuning voor het wijzigen van de grootte van het aanmeldingsscherm, zodat dit past bij de afmetingen van het apparaat dat de weergave gebruikt. | A |                                                                                                                                                          |
| 1,6 | Standaard MVPD Picker-logo | De MVPD biedt een URL naar een standaardlogo met de juiste afmetingen (112 x 33 pixels). | A | Het logo moet worden gehost door de MVPD en in de cache worden opgeslagen door de CDN. |
| 1,7 | [ Scoping Service Provider ](/help/authentication/integration-guide-mvpds/serv-provider-scoping.md) | MVPD ondersteunt het doorgeven van de merknaam (waarde van de aanvrager) in de AuthN-aanvraag. | A- | Dit laat een dienstverlener-specifieke login ervaring toe. |
| 1,8 | [ Veelvoudige Toestemming van het Kanaal ](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight-multich-authz) | De MVPD ondersteunt een programmeur die meerdere kanalen verzendt in één vergunningsaanvraag. | B+ |                                                                                                                                                          |
| 1,9 | Verificatie op basis van iFrame of &#39;JS Pop-up&#39; | De programmeur kan de login stroom in een iFrame of pop-up ervaring in plaats van HTTP redirect integreren. | B |                                                                                                                                                          |
| 1,10 | [ Programmer-in werking gestelde Logout ](/help/authentication/integration-guide-mvpds/usecase-mvpd-logout.md) | De viewer heeft een geverifieerde sessie zowel op de site of de app van de programmeur als met de MVPD. De gebruiker kan een gefederaliseerde logout van de plaats van de Programmer in werking stellen, en logout ontruimt ook de zitting op het portaal van MVPD. | B | Zorgt ervoor dat gedeelde computers beter worden beschermd tegen misbruik vanuit het perspectief van de programmeur. |
| 1,11 | [ het Overseinen van de Fout van de Toestemming van de Douane ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) | De MVPD geeft een eigen aangepaste fouttekenreeks door die geschikt is voor de gefedereerde site of de app van de programmeur om aan de gebruiker weer te geven. | B | Laat omhoog-verkoopscenario&#39;s toe |
| 1,12 | **Metagegevens van de Gebruiker op de Reactie van de Authentificatie** | De MVPD AuthN-reactie kan gebruikersmetagegevens bevatten die fungeren als een tip voor het aanpassen van de gebruikerservaring tijdens de machtigingsstroom. Deze eis maakt ouderlijke controlehints mogelijk van de MVPD naar de programmeur. | B- |                                                                                                                                                          |
| 1,13 | MVPD-verificatie | De viewer voltooit een geslaagde AuthN-sessie op het MVPD-portaal en navigeert vervolgens naar de TVE-site van de programmeur. De gebruiker wordt niet gevraagd naar de MVPD-kiezer en wordt automatisch geverifieerd. | B- |                                                                                                                                                          |
| 1,14 | Gebruikersnamen | De MVPD-gebruikersnaam moet in twee vormen worden gebruikt: een voor programmeurs en een voor de hele Adobe voor fraudedoeleinden.  Hierdoor kan de Adobe de MVPD User-id delen die onder het bereik van de programmeur valt, zonder verdere codering/verduistering. | C |                                                                                                                                                          |
| 1,15 | Autorisatie op basis van bedrijfsmiddelen | Zodra AuthN wordt voltooid, kan AuthZ op de achtergrond gebeuren, door gestructureerde gegevens over te gaan die netwerk, tonen, activa, ouderlijke controle classificatie, en meer kunnen omvatten zoals nodig. Dit laat ouderlijke controles op elke vraag AuthZ van de Programmer aan MVPD toe. | C |                                                                                                                                                          |
| 1,16 | Afmelden bij MVPD gestart | De viewer heeft een geverifieerde sessie op de site of in de app van de programmeur en met de MVPD. De kijker kan een gefederaliseerde logout van de plaats van de MVPD in werking stellen die ook de zitting op alle gefederaliseerde plaatsen van de Programmer ontruimt. | C |                                                                                                                                                          |
| 1,17 | Vergunningsverplichtingen | MVPD verstrekt extra voorwaarden in de reactie AuthZ, zoals registreren, of een verfrist maximum ouderlijke controle classificatie (OLCA). | C |                                                                                                                                                          |
| 1,18 | Context IP-adres | MVPD vereist uitdrukkelijk het IP adres overgaan. Voor AuthZ, waar de vraag server-kant is, geeft dit de fraude het volgen informatie van MVPD over waar de gebruiker van op de vraag AuthZ komt. | C |                                                                                                                                                          |
| 1,19 | Token Persistence Time-to-live (TTL) Waarde dynamisch gedefinieerd | MVPD kan TTL van het teken van de Authentificatie van Adobe Pass dynamisch door eigenschappen in de reactie plaatsen, zodat de Adobe uit de lijn voor de veranderingen van TTL is. | C- |                                                                                                                                                          |
| 1,20 | Apparaattype | MVPD steunt het overgaan van het apparatentype in het verzoek AuthN of AuthZ. Deze eigenschap informeert de MVPD over de aard van het apparaat waar de inhoud wordt verbruikt, zodat ze de TTL van de token AuthN of AuthZ kunnen aanpassen om hun eigen beveiligingsoverwegingen voor het apparaat in acht te nemen. | C- | Dit is nuttig voor het clientless platform, waar het moeilijk kan zijn om elk apparatentype als config plaatsen op de kant van de Authentificatie van Adobe Pass bloot te stellen. |



## 2. Operationele kenmerken {#operational-features}

| Nee | Functie | Beschrijving | Prioriteit | Notities |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | 24/7 Up-time | Een overeenkomst van MVPD om 24 uur per dag voor 7 uur te ijveren en tegen die tijd in te grijpen. | A |       |
| 2,2 | Referenties testen voor elke programmeur | De MVPD biedt voor elke programmeur een aparte testaccount. | A |       |
| 2,3 | Certificaatvervaldatum en verlengingsschema | Een MVPD-gedocumenteerd plan met een programma om de certificaten van SAML te verzekeren is bijgewerkt. | A- |       |
| 2,4 | Gemiddelde latentie onder 250 ms/respons | Een overeenkomst van MVPD om te streven naar een lage latentie en om dat in de loop der tijd te meten. | A- |       |
| 2,5 | Eigen MVPD Picker-logo hosten | De MVPD moet hun eigen logo hosten en het moet worden beschermd door CDN-caching. | B |       |
| 2,6 | Ondersteuningsschaalingsplan | Een MVPD-gedocumenteerd plan voor escalatie dat wordt bijgehouden en minstens elk kwartaal herzien. | A |       |
| 2,7 | Uitvalbeveiligingsplan | Een gezamenlijk plan van MVPD met Adobe over afbraakmaatregelen dat zo nodig algemeen kan worden gebruikt. | B |       |
| 2,8 | Afzonderlijke eindpunten voor staging en productie behouden | Een MVPD-integratietest waarvoor geen spoofing van het hostbestand is vereist om de integratie met testfasen te testen. | B |       |
| 2,9 | Aanvullend QA-eindpunt | MVPD handhaaft een extra integratie QA IdP voor gezamenlijke nieuwe eigenschapontwikkeling.  Als dit wordt ondersteund, is het minder waarschijnlijk dat we speciale UAT-aanvragen voor cert-tests voor App Store nodig hebben. | C |       |





## 3. Verificatieervaringsfuncties {#authn-exp-features}


| Nee | Functie | Beschrijving | Prioriteit | Notities |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Verificatieomzetting over minimumverwachting | De MVPD zorgt voor een minimale conversiegraad die functioneel (5%) en redelijk (30%) is. | A |                                                                                                                                                           |
| 3,2 | Inline wachtwoordherstel | MVPD verstrekt een middel om wachtwoorden aan de gefedereerde stroom terug te krijgen AuthN. | A |                                                                                                                                                           |
| 3,3 | Inline-accountregistratie | De MVPD biedt een manier om een nieuwe account te maken inline van een gefedereerde AuthN-flow. | A |                                                                                                                                                           |
| 3,4 | Inline Help/ondersteuning | MVPD verstrekt een middel om hulp tijdens de federated stroom te verlenen AuthN. | A |                                                                                                                                                           |
| 3,5 | Op modem gebaseerde interne verificatie | MVPD verifieert automatisch een apparaat wanneer het op het lokale netwerk van een geregistreerd model (ISP MVPD slechts) is. | B | Dit is lagere prioriteit omdat het een optimalisering is die velen nog niet kunnen steunen en sommige uitdagingen voor fraude matiging en ouderlijke controles introduceert |

U kunt de code van de Lijst van de Prijsverlaging nu direct invoeren gebruikend Dossier/de gegevens van de Lijst van de Plak... dialoog.


## 4. Analytische kenmerken {#analytics-features}


| Nee | Functie | Beschrijving | Prioriteit |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Uitlijning van configuratiefunctie voor conversie | MVPD heeft metrisch voor verzoeken AuthN die met het verzoek van SAML AuthN begint - om met de de authentificatie van Adobe Pass en de verzoekmetriek van AuthN van de Programmer te richten. | A |
| 4,2 | Unieke gebruikers | Gebruikers die met succes zijn geverifieerd en waarvan het duplicaat maandelijks tijdens het bijwerken over dagen is verwijderd. | A |
| 4,3 | Tijdzoneboekhouding | Rapporten bevatten tijdzone voor wanneer de dag overgaat. | A |





## 5. Fraudebestrijdingskenmerken {#fraud-mitgn-features}


| Nee | Functie | Beschrijving | Prioriteit | Notities |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validatie videoabonnee bij verificatie | MVPD zorgt ervoor dat de gebruiker een geldige videoabonnee tijdens de stroom AuthN is. | A |                                                                                                |
| 5,2 | Snelheidlimiet voor verificatie/autorisatie | De MVPD biedt ondersteuning voor het wijzigen van de snelheid van hun webservices om het gebruik van een specifieke gebruikersaccount te beperken, indien van toepassing. | B |                                                                                                |
| 5,3 | Blijvende gebruikersnaam met algemeen bereik naar Adobe | De MVPD zorgt ervoor dat de Adobe een UserID heeft die door Programmeurs voor fraude kan worden gevolgd. Dit hoeft niet rechtstreeks aan de klant te worden geleverd. | B | Online Multimedia Authorization Protocol (OMAP) Specification and Real User Monitoring (RUM). |
| 5,4 | Validatie van gelijktijdig gebruik | De MVPD beschikt over een manier om het gelijktijdige gebruik van de abonneeaccount boven een bedrijfsdrempel te volgen en te beperken. | B |                                                                                                |

## P1. Proxy-specifieke functies {#proxy-sp-func-features}

| Nee | Beschrijving | Prioriteit | Notities |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy verantwoordelijk voor het bijhouden van de nauwkeurigheid van de bijgewerkte lijst van subVPD&#39;s | A |                                                   |
| P 1.2 | De volmacht levert aangewezen gerangschikte embleem voor elk subMVPD | A | Sommige live subMVPD&#39;s hebben niet de juiste grootte voor het logo |
| P 1.3 | De volmacht levert geschikt branded login pagina voor elke subMVPD | A |                                                   |
| P 1.4 | Proxy verantwoordelijk voor het richten van specifieke dienstverlener merken met subMVPD lijst | B |                                                   |
| P 1.5 | Proxy geeft alle subMVPD-eigenschappen correct op (iFrame-grootte, TTL, logo, enz.) | A |                                                   |
| P 1.6 | Proxy geeft een afzonderlijke entiteit-id op | B |                                                   |

## P2. Operationele proxyfuncties {#proxy-op-features}

| Nee | Beschrijving | Prioriteit |
|-------|----------------------------------------------------------------------------------------|----------|
| P. 2.1 | API-sleutel is beveiligd | A |
| P. 2.2 | De geloofsbrieven van de test voor eerste subMVPD die in levende integratie voor elke nieuwe aanvrager wordt gebruikt | A |
| P. 2.3 | SubVPD-lijsten zijn nauwkeurig en volledig per aanvrager | A |
