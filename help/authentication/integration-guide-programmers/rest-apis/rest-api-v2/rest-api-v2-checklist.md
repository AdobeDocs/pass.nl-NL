---
title: REST API V2 Checklist
description: REST API V2 Checklist
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2545'
ht-degree: 0%

---

# REST API V2 Checklist {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Dit document voegt op één plaats de verplichte vereisten en geadviseerde praktijken voor Programmers samen die cliënttoepassingen uitvoeren die de Authentificatie van Adobe Pass [ VERTONEN API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) verbruiken.

Het volgende document moet als onderdeel van uw acceptatiecriteria worden beschouwd bij de implementatie van REST API V2 en moet als controlelijst worden gebruikt om ervoor te zorgen dat alle noodzakelijke stappen zijn gezet om een succesvolle integratie te bereiken.

## Verplichte eisen {#mandatory-requirements}

### 1. Registratiefase {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Geregistreerde toepassingsbereikregels</i></td>
      <td>Gebruik een geregistreerde toepassing met REST API v2-bereik.</td>
      <td>Risico's die HTTP 401 "Onbevoegde"foutenreacties teweegbrengen, systeemmiddelen overbelasten en latentie verhogen.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Client Credentials Caching</i></td>
      <td>Sla de clientgegevens op in permanente opslag en hergebruik deze voor elk verzoek om een toegangstoken.</td>
      <td>Riseert authentificatieverlies wanneer de cliëntgeloofsbrieven opnieuw worden geproduceerd.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Tokens in cache plaatsen</i></td>
      <td>Sla de toegangstokens op in permanente opslag en gebruik ze opnieuw tot ze verlopen.<br/><br/> verzoek geen nieuw teken voor elke REST API v2 vraag, vernieuw de toegangstokens slechts wanneer zij verlopen.</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
</table>

### 2. Configuratiefase {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ophalen configuratie</i></td>
      <td>Haal de configuratiereactie alleen op wanneer u de gebruiker moet vragen zijn of haar MVPD (tv-provider) te selecteren voordat de verificatiefase plaatsvindt.<br/><br/> Er is geen behoefte om de configuratiereactie terug te winnen wanneer:<ul><li>De gebruiker is al geverifieerd.</li><li>De gebruiker krijgt tijdelijk toegang.</li><li>De gebruikersverificatie is verlopen, maar de gebruiker kan worden gevraagd te bevestigen dat hij of zij nog steeds een abonnee is van de eerder geselecteerde MVPD.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen en het vergroten van de latentie.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Selectie tv-provider in cache plaatsen</i></td>
      <td>Sla de selectie van de Pay TV provider (MVPD) van de gebruiker op in blijvende opslag om deze in alle volgende fasen te gebruiken:<ul><li>In het reactiebestand van de configuratie slaat de gebruiker de MVPD-id op.</li><li>Vanuit configuratierespons slaat de gebruiker MVPD "displayName" op.</li><li>Vanuit de configuratiereactie slaat u de gebruiker MVPD "logoUrl" op.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen en het vergroten van de latentie.</td>
   </tr>
</table>

### 3. Verificatiefase {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Opiniepeilingsmechanisme starten</i></td>
      <td>Begin het opiniepeilingsmechanisme onder de volgende voorwaarden:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> Authentificatie die binnen de primaire (scherm) toepassing </a></b> wordt uitgevoerd<ul><li>De primaire (het stromen) toepassing zou moeten beginnen opiniepeiling wanneer de gebruiker de definitieve bestemmingspagina bereikt, nadat de browser component URL laadt die voor de "redirectUrl"parameter in het het eindpuntverzoek van Sessies wordt gespecificeerd.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd </a></b><ul><li>De primaire (het stromen) toepassing zou moeten beginnen opiniepeiling zodra de gebruiker het authentificatieproces-recht na het ontvangen van de het eindpuntreactie van Sessies en het tonen van de authentificatiecode aan de gebruiker in werking stelt.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Opiniepeilingsmechanisme stoppen</i></td>
      <td>Stop het opiniepeilingsmechanisme onder de volgende voorwaarden:<br/><br/><b> Succesvolle authentificatie </b><ul><li>De het profielinformatie van de gebruiker wordt met succes teruggewonnen, bevestigend hun authentificatiestatus, daarom is de opiniepeiling niet meer nodig.</li></ul><br/><b> de zitting van de Authentificatie en coderepering </b><ul><li>De verificatiesessie en de code verlopen, de gebruiker moet het verificatieproces opnieuw starten en de opiniepeiling met de vorige verificatiecode moet onmiddellijk worden gestopt.</li></ul><br/><b> Nieuwe geproduceerde authentificatiecode </b><ul><li>Als de gebruiker om een nieuwe authentificatiecode verzoekt, dan wordt de bestaande zitting ongeldig gemaakt, en de opiniepeiling die de vorige authentificatiecode gebruikt zou onmiddellijk moeten worden tegengehouden.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Opiniepeilingsmechanisme configureren</i></td>
      <td>Vorm de frequentie van het opiniepeilingsmechanisme onder de volgende voorwaarden:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> Authentificatie die binnen de primaire (scherm) toepassing </a></b> wordt uitgevoerd<ul><li>De primaire (het stromen) toepassing zou om de 3-5 seconden of meer moeten opiniepeilen.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> Authentificatie die binnen een secundaire (scherm) toepassing wordt uitgevoerd </a></b><ul><li>De primaire (het stromen) toepassing zou om de 3-5 seconden moeten opiniepeilen.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>In cache plaatsen van profielen</i></td>
      <td>Sla delen van de profielgegevens van de gebruiker op in permanente opslag om de prestaties te verbeteren en onnodige REST API v2-aanroepen tot een minimum te beperken.<br/><br/> Caching zou op de volgende gebieden van de profielreactie moeten concentreren:<br/><br/><b> mvpd </b><ul><li>De clienttoepassing kan dit gebruiken om de geselecteerde tv-provider van de gebruiker bij te houden en deze verder te blijven gebruiken tijdens de fase voorafgaand aan autorisatie of autorisatie.</li><li>Wanneer het huidige gebruikersprofiel is verlopen, kan de clienttoepassing de onthouden MVPD-selectie gebruiken en de gebruiker vragen dit te bevestigen.</li></ul><br/><b> attributen </b><ul><li>Wordt gebruikt om de gebruikerservaring aan te passen op basis van verschillende metagegevens van de gebruiker (bijvoorbeeld zip, maxRating, enz.).</li><li>De meta-gegevens van de gebruiker worden beschikbaar nadat de authentificatiestroom voltooit, daarom te hoeven de cliënttoepassing niet om een afzonderlijk eindpunt te vragen om de informatie van gebruikersmeta-gegevens terug te winnen, aangezien het reeds inbegrepen in de profielinformatie is.</li><li>Bepaalde metagegevenskenmerken kunnen tijdens de machtigingsfase worden bijgewerkt, afhankelijk van de MVPD (bv. het Handvest) en het specifieke metagegevenskenmerk (bv. de huisnummer). Het gevolg is dat de clienttoepassing mogelijk opnieuw een query moet uitvoeren op de API's voor profielen nadat de laatste metagegevens van de gebruiker zijn opgehaald.</li></ul></td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
</table>

### 4. (Optioneel) Preautorisatiefase {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Besluiten tot voorafgaande toestemming Intrekking</i></td>
      <td>Gebruik beslissingen vóór toelating voor filteren van inhoud en nooit voor afspeelbeslissingen.</td>
      <td>Risico's die in strijd zijn met contractuele overeenkomsten tussen programmeur, MVPD en Adobe.<br/><br/> de risico's die onze controle en alarmerende systemen omzeilen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Voorkeursbeschikkingen Retrieval</i></td>
      <td>Behandel <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> verbeterde foutencodes </a> geschikt en gebruik het actieveld om de noodzakelijke saneringsstappen te bepalen.<br/><br/> slechts een beperkt aantal verbeterde foutencodes rechtvaardigen een herpoging, terwijl de meesten alternatieve resoluties zoals gespecificeerd in het actieveld vereisen.<br/><br/> zorg ervoor dat om het even welk die retry mechanisme voor het terugwinnen van pre-vergunningsbesluiten wordt uitgevoerd niet in een eindeloze lijn resulteert en dat het probeert tot een redelijk aantal beperkt (d.w.z., 2-3).</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Besluiten vóór toelating</i></td>
      <td>Plaats geslaagde vergunningsbeslissingen in het geheugen om de prestaties te verbeteren en onnodige REST API v2-aanroepen tot een minimum te beperken, aangezien abonnementsupdates tijdens het uitvoeren van de toepassing niet vaak worden uitgevoerd.</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
</table>

### 5. Vergunningsfase {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisatiebesluiten Ingetrokken</i></td>
      <td>Geef autorisatiebeslissingen op voordat u gaat afspelen, ongeacht of er een voorafgaande autorisatiebeslissing bestaat.<br/><br/> staat stromen toe om ononderbroken voort te zetten zelfs als het media teken tijdens playback verloopt en om een nieuw vergunningsbesluit verzoeken die (vers) media teken bevatten wanneer de gebruiker hun volgende playbackverzoek maakt, ongeacht het voor het zelfde of een verschillend middel is.<br/><br/> Levende stromen die voor langere periodes lopen kunnen verkiezen om een nieuw vergunningsbesluit na videoverrichtingen zoals het pauzeren van inhoud, het in werking stellen van commerciële onderbrekingen, of het wijzigen van activa-vlakke configuraties te verzoeken wanneer MRSS veranderingen ondergaat.</td>
      <td>Risico's die in strijd zijn met contractuele overeenkomsten tussen programmeur, MVPD en Adobe.<br/><br/> de risico's die onze controle en alarmerende systemen omzeilen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisatiebesluiten intrekken</i></td>
      <td>Behandel <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> verbeterde foutencodes </a> geschikt en gebruik het actieveld om de noodzakelijke saneringsstappen te bepalen.<br/><br/> slechts een beperkt aantal verbeterde foutencodes rechtvaardigen een herpoging, terwijl de meesten alternatieve resoluties zoals gespecificeerd in het actieveld vereisen.<br/><br/> zorg ervoor dat om het even welk die heruitzettingsmechanisme voor het terugwinnen van vergunningsbesluiten wordt uitgevoerd niet in een eindeloze lijn resulteert en dat het pogingen tot een redelijk aantal beperkt (d.w.z., 2-3).</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".</td>
   </tr>
</table>

### 6. Afmeldingsfase {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ondersteuning voor afmelden</i></td>
      <td>Implementeer de API voor afmelden zodat gebruikers zich handmatig kunnen afmelden, hun geverifieerde profiel kunnen beëindigen en de voor elk verwijderd profiel opgegeven naam voor de REST API v2-handeling kunnen volgen:<ul><li>Voor MVPDs die een logout eindpunt steunen vereist de cliënttoepassing om aan verstrekte "url"in een gebruiker-agent te navigeren.</li><li>Voor "appleSSO"typeprofielen, moet de cliënttoepassing de gebruiker begeleiden om zich ook uit het partnerniveau (het systeemmontages van Apple) te melden.</li></ul></td>
      <td>Risico dat de clienttoepassing niet goed functioneert als er ondersteuning aan de clienttoepassingszijde ontbreekt.</td>
   </tr>
</table>

### 7. Parameters en kopteksten {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisatieheader verzenden</i></td>
      <td>Verzend de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md"> kopbal van de Vergunning </a> voor elk REST API v2 verzoek.</td>
      <td>Risico's die HTTP 401 "Onbevoegde"foutenreacties teweegbrengen, systeemmiddelen overbelasten en latentie verhogen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>AP-apparaat-id-header verzenden</i></td>
      <td>Verzend <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md"> AP-apparaat-Herkenningsteken </a> kopbal voor elk REST API v2 verzoek.<br/><br/> zelfs wanneer het verzoek uit een server namens een apparaat voortkomt, moet de AP-apparaat-Herkenner kopbalwaarde op het daadwerkelijke het stromen apparatenherkenningsteken wijzen.</td>
      <td>Risico's die leiden tot HTTP 400 "Onjuiste Verzoek"foutenreacties, overladende systeemmiddelen en stijgende latentie.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>X-apparaat-Info-header verzenden</i></td>
      <td>Verzend <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md"> x-apparaat-Info </a> kopbal voor elk REST API v2 verzoek.<br/><br/> zelfs wanneer het verzoek uit een server namens een apparaat voortkomt, moet de x-Apparaat-Info kopbalwaarde op de daadwerkelijke het stromen apparateninformatie wijzen.</td>
      <td>Risico's die worden ingedeeld als afkomstig van een onbekend platform en als onveilig worden behandeld, worden onderworpen aan meer beperkende regels, zoals kortere authentificatie-TTL's.<br/><br/> bovendien, zijn sommige gebieden, zoals het stromen apparaat connectionIp en connectionPort, verplicht voor eigenschappen zoals de Authentificatie van de Basis van het Spectrum.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Id van stabiel apparaat</i></td>
      <td>Verwerk en sla een stabiel apparatenherkenningsteken op dat niet over updates of reboots voor <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md"> AP-Apparaat-Herkenningsteken </a> kopbal verandert.<br/><br/> voor platforms zonder een hardware herkenningsteken, produceer een uniek herkenningsteken van toepassingsattributen en handhaaf het.</td>
      <td>Riseert authentificatieverlies wanneer het apparatenherkenningsteken verandert.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Referenties van API's volgen</i></td>
      <td>Zorg ervoor dat u alleen parameters en kopteksten verzendt die door REST API v2 worden verwacht.</td>
      <td>Risico's die leiden tot HTTP 400 "Onjuiste Verzoek"foutenreacties, overladende systeemmiddelen en stijgende latentie.</td>
   </tr>
</table>

### 8. Foutafhandeling {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Verbeterde ondersteuning voor foutafhandeling</i></td>
      <td>Behandel <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> verbeterde foutencodes </a> geschikt en gebruik het actieveld om de noodzakelijke saneringsstappen te bepalen.<br/><br/> slechts een beperkt aantal verbeterde foutencodes rechtvaardigen een herpoging, terwijl de meesten alternatieve resoluties zoals gespecificeerd in het actieveld vereisen.<br/><br/> de meest verbeterde foutencodes die in <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2"> worden vermeld Verbeterde Codes van de Fout - REST API V2 </a> documentatie kan volledig worden verhinderd als correct behandeld tijdens ontwikkelingsfase alvorens de toepassing te lanceren.</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".<br/><br/> de mislukking van de cliënttoepassing van het risico toe te schrijven aan ontbrekende behandeling van de verbeterde foutencodes - veroorzakend onduidelijke foutenmeldingen, onjuiste gebruikershulp, of onjuist fallback gedrag.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ondersteuning voor HTTP-foutafhandeling</i></td>
      <td>Verschil tussen de afhandeling van HTTP-foutreacties (bijvoorbeeld 400, 401, 403, 404, 405, 500) en succesreacties (bijvoorbeeld 200, 201) die verbeterde foutcodes bevatten, zoals hierboven beschreven.<br/><br/> slechts een beperkt aantal de foutencodes van HTTP rechtvaardigen een herpoging, terwijl de meesten alternatieve resoluties vereisen.<br/><br/> De meeste de foutenreacties van HTTP kunnen volledig worden verhinderd als correct behandeld tijdens ontwikkelingsfase alvorens de toepassing te lanceren.</td>
      <td>Risico's bij het overladen van systeembronnen, het verhogen van de latentie en het mogelijk activeren van de foutreacties van HTTP 429 "Te veel verzoeken".<br/><br/> de mislukking van de cliënttoepassing van het risico toe te schrijven aan ontbrekende behandeling van de verbeterde foutencodes - veroorzakend onduidelijke foutenmeldingen, onjuiste gebruikershulp, of onjuist fallback gedrag.</td>
   </tr>
</table>

### 9. Testen {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Vereisten</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Levenscyclustests</i></td>
      <td>Ontwikkel en test de toepassing gebruikend de officiële milieu's van de Authentificatie van Adobe Pass niet-productie:<ul><li>Prequal-production</li><li>Geen staging</li></ul><br/>Voer grondige kwaliteitsborging (QA) in deze milieu's uit alvorens te lanceren aan productie.<br/><br/> de toepassingen van de Cliënt moeten niet aan versie-Productie zonder eerste volledige bevestiging van begin tot eind in niet productiemilieu's te werk gaan.</td>
      <td>Risico's die ontstaan met kritieke en grote defecten.<br/><br/> het Ontbreken van een kort en efficiënt het zuiveren weg kan de Steun en de Techniek van Adobe van het snel ingrijpen verhinderen.</td>
   </tr>
</table>

## Aanbevolen procedures {#recommended-practices}

### 1. Registratiefase {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validatie voor toegangstokens</i></td>
      <td>Controleer proactief de geldigheid van het toegangstoken om het te verfrissen wanneer verlopen.<br/><br/> zorg ervoor dat om het even welk hertry mechanisme voor de behandeling van HTTP 401 "Onbevoegde"fouten eerst het toegangstoken alvorens het originele verzoek opnieuw te proberen verfrist.</td>
      <td>Risico's die HTTP 401 "Onbevoegde"foutenreacties teweegbrengen, systeemmiddelen overbelasten en latentie verhogen.</td>
   </tr>
</table>

### 2. Configuratiefase {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuratie-caching</i></td>
      <td>Sla de configuratierespons op in geheugen of permanente opslag gedurende een korte periode (bijvoorbeeld 3-5 minuten) om de prestaties te verbeteren en onnodige REST API v2-aanroepen tot een minimum te beperken.</td>
      <td>Risico's bij het overladen van systeembronnen en het vergroten van de latentie.</td>
   </tr>
</table>

### 3. Verificatiefase {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validatie van verificatiecode (tweede schermverificatie)</i></td>
      <td>Bevestig authentificatiecode die door gebruikersinput op de secundaire (2de) toepassing (het scherm) wordt voorgelegd alvorens /api/v2/voor authentiek te verklaren API onder de volgende voorwaarden:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd"> Authentificatie die binnen secundaire (scherm) toepassing met vooraf geselecteerde mvpd </a></b> wordt uitgevoerd<ul><li>Maak gebruik van <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md"> hervat authentificatiesessie </a> - POST /api/v2/{serviceProvider}/zittingen/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd"> Authentificatie die binnen secundaire (scherm) toepassing zonder vooraf geselecteerde mvpd wordt uitgevoerd </a></b><ul><li>Maak gebruik van <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md"> wint authentificatiesessie </a> terug - GET /api/v2/{serviceProvider}/zittingen/{code}</li></ul><br/>De clienttoepassing ontvangt een fout als de opgegeven verificatiecode onjuist is getypt of als de verificatiesessie zou zijn verlopen.</td>
      <td>Er zijn verschillende reacties op fouten en problemen met de werkstroom tijdens verificatie.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ondersteuning voor meerdere profielen</i></td>
      <td>Zorg ervoor dat clienttoepassingen meerdere profielen kunnen verwerken door de gebruiker te vragen een profiel te selecteren of aangepaste logica toe te passen, zoals automatisch het profiel met de langste geldigheidsperiode selecteren.</td>
      <td>Risico dat de clienttoepassing niet goed functioneert als er ondersteuning aan de clienttoepassingszijde ontbreekt.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Optioneel) Ondersteuning voor niet-basisstromen</i></td>
      <td>Niet-basisstromen ondersteunen als de bedrijfsvoering van de clienttoepassing het volgende vereist:<ul><li>Verminderde toegangsstromen (premieeigenschap)</li><li>Tijdelijke toegangsstromen (premiumfunctie)</li><li>Single Sign-On toegangsstromen (standaardfunctie)</li></ul></td>
      <td>Risico's die tot een minder dan ideale gebruikerservaring leiden.</td>
   </tr>
</table>

### 4. (Optioneel) Preautorisatiefase {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Gebruikerservaring</i></td>
      <td>Toon duidelijke gebruiker terugkoppelt als een pre-vergunningsbesluit wordt ontkend, gebruikend overseinen die door MVPDs of Adobe via verbeterde foutencodes worden verstrekt.</td>
      <td>Risico's die tot een minder dan ideale gebruikerservaring leiden.</td>
   </tr>
</table>

### 5. Vergunningsfase {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validatie van mediatokens</i></td>
      <td>Valideer media tokens gebruikend de </a> bibliotheek van de Verificateur van het Symbolische van 0&rbrace; Media.<a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier"></td>
      <td>Risico's van frauduleuze regelingen zoals stroom ripping.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Gebruikerservaring</i></td>
      <td>Toon duidelijke gebruiker terugkoppelt als een vergunningsbesluit wordt ontkend, gebruikend overseinen die door MVPDs of Adobe via verbeterde foutencodes worden verstrekt.</td>
      <td>Risico's die tot een minder dan ideale gebruikerservaring leiden.</td>
   </tr>
</table>

### 6. Afmeldingsfase {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Gebruikerservaring</i></td>
      <td>Vermijd automatisch (programmatically) het roepen van logout API in scenario's zoals ontkende preauthentication of vergunning, aangezien logout API slechts in antwoord op een direct gebruikersverzoek zou moeten worden aangehaald.</td>
      <td>Risico's verwarrend de gebruiker dat de authentificatie ontbreekt.</td>
   </tr>
</table>

### 7. Parameters en kopteksten {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Code opnieuw gebruiken</i></td>
      <td>U kunt de code van REST API v1 opnieuw gebruiken voor het berekenen van de apparaat-id en apparaatinformatie met kleine aanpassingen, maar zorg ervoor dat u alleen voor REST API v2 verwachte parameters en headers verzendt.<br/><br/> hergebruik code van REST API v1 voor het roepen van DCR API om een toegangstoken terug te winnen.</td>
      <td>-</td>
   </tr>
</table>

### 8. Testen {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Handelingen</th>
      <th style="background-color: #EFF2F7;">Risico's</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Dekking testen</i></td>
      <td>Verzeker de volgende basisstromen over apparaten en platforms worden getest:<br/><br/><b> de stromen van de Authentificatie </b><ul><li>Verificatiescenario primaire toepassing (scherm)</li><li>Secundair verificatiescenario voor toepassing (scherm)</li></ul><br/><b> (Optioneel) Voorbereidende stromen </b><ul><li>Beslissingsscenario voor testvergunningen</li><li>Besluiten testen</li></ul><br/><b> de stromen van de Vergunning </b><ul><li>Beslissingsscenario voor testvergunningen</li><li>Besluiten testen</li></ul><br/><b> Logout stromen </b><br/><br/> extra, test andere toegangsstromen, indien van toepassing:<br/><br/><ul><li>Verminderde toegangsstromen (premieeigenschap)</li><li>Tijdelijke toegangsstromen (premiumfunctie)</li><li>Single Sign-On toegangsstromen (standaardfunctie)</li></ul><br/>Omvat de top-MVPD-integratie (die de meest gebruikte leveranciers omvat).</td>
      <td>Risico's van onvoorziene mislukkingen in productie, vooral over minder vaak geteste platforms of zeldzame stromen als negatieve scenario's.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Testgereedschappen</i></td>
      <td>Gebruik de <a href="https://developer.adobe.com/adobe-pass/"> Adobe Developer </a> website.</td>
      <td>-</td>
   </tr>
</table>

## Samenvatting {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Fase</th>
      <th style="background-color: #EFF2F7;">Verplicht</th>
      <th style="background-color: #EFF2F7;">(Sterk) Aanbevolen</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registratie</i></td>
      <td>De toegangstokens van de de cliëntgeloofsbrieven van het geheime voorgeheugen <br/><br/></td>
      <td>Toegangstokens valideren en vernieuwen</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuratie</i></td>
      <td>Ophalen van configuratiereacties minimaliseren</td>
      <td>Respons cacheconfiguratie</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Verificatie</i></td>
      <td>Het opiniepeilingsmechanisme bepaalt het stemmen <br/><br/> Geheime voorgeheugen delen van profielen</td>
      <td>De steun veelvoudige profielen &lbrace;de Eigenschap van de Verslechtering van de Steun (als bedrijfsvereiste) <br/><br/> de Eigenschap van de Steun TempPass (als bedrijfsvereiste) <br/><br/> Steun Enige Sign-On Eigenschap (als bedrijfsvereiste)<br/><br/></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Voorafgaande goedkeuring</i></td>
      <td>De vergunningsbesluiten van het geheime voorgeheugen &lbrace;<br/><br/> proberen mechanisme fijn het stemmen</td>
      <td>De gebruikerservaring verbeteren door foutcodes te gebruiken voor beslissingen waarbij voorafgaande toestemming is geweigerd</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Toestemming</i></td>
      <td>Wint vergunningsbesluit terug wanneer de gebruiker om playback <br/><br/> verzoekt wil mechanisme fijnstemmen</td>
      <td>Verbeter gebruikerservaring gebruikend foutencodes voor ontkende toestemmingsbesluiten <br/><br/> het symbolische bevestiging van Media</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Afmelden</i></td>
      <td>Implementeer de aanmeldings-API zodat gebruikers zich handmatig kunnen afmelden</td>
      <td>De aanmeldings-API niet automatisch aanroepen</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Verplicht</th>
      <th style="background-color: #EFF2F7;">(Sterk) Aanbevolen</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parameters en koppen</i></td>
      <td>Verplichte headerspecificaties volgen</td>
      <td>Code van REST API v1 opnieuw gebruiken</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Foutafhandeling</i></td>
      <td>Implementeer verbeterde fout behandeling <br/><br/> voert de fout van HTTP behandeling uit</td>
      <td>-</td>
   </tr>
</table>
