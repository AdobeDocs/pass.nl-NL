---
title: Technisch overzicht van Amazon FireOS
description: Technisch overzicht van Amazon FireOS
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2166'
ht-degree: 0%

---

# Technisch overzicht van Amazon FireOS {#amazon-fireos-technical-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Inleiding {#intro}

Amazon FireOS AccessEnabler wordt vertegenwoordigd door twee componenten: een AccessEnabler-stub-bibliotheek die door de toepassing wordt gebruikt en een Java Android-bibliotheek op systeemniveau waarmee mobiele apps Adobe Pass-verificatie kunnen gebruiken voor tv-machtigingsservices van overal. Een Android-implementatie voor Amazon FireOS bestaat uit de AccessEnabler-interface die de machtiging-API definieert, en een EntitlementDelegate-protocol dat de callbacks beschrijft die de bibliotheek activeert. De Android-bibliotheek op systeemniveau AccessEnabler maakt toegang tot Amazon-services mogelijk om Single Sing On op platformniveau in te schakelen.

## Native clientworkflows begrijpen {#native_client_workflows}

Native clientworkflows zijn doorgaans hetzelfde als of lijken sterk op die van op browsers gebaseerde Adobe Pass Authentication-clients. Er zijn echter enkele uitzonderingen, zoals hieronder beschreven.


### Workflow na initialisatie {#post-init}

Alle machtigingswerkstromen die door AccessEnabler worden gesteund veronderstellen dat u eerder [`setRequestor()`](#setRequestor) hebt geroepen om uw identiteit te vestigen. U doet deze vraag om uw identiteitskaart van de Aanvrager slechts eenmaal te verstrekken, gewoonlijk tijdens de initialisatie/opstellingsfase van uw toepassing.

Met de native clients (bijvoorbeeld AmazonFireOS) hebt u na uw eerste aanroep naar [`setRequestor()`](#setRequestor) de keuze hoe u wilt doorgaan:

- U kunt onmiddellijk beginnen machtigingsvraag te maken en hen toe te laten om stil worden een rij gevormd, indien nodig.
- U kunt ook een bevestiging ontvangen van het succes/de mislukking van [`setRequestor()`](#setRequestor) door de callback setRequestorComplete() uit te voeren.
- Of doe beide.

Het is aan u om op bericht van het succes van [`setRequestor()`](#setRequestor) te wachten of op het mechanisme van de vraagrij van AccessEnabler te vertrouwen. Aangezien alle volgende autorisatie- en verificatieaanvragen de aanvrager-id en de bijbehorende configuratiegegevens nodig hebben, blokkeert de methode [`setRequestor()`](#setRequestor) in feite alle API-aanroepen voor verificatie en autorisatie totdat de initialisatie is voltooid.

### Generic Initial Authentication Workflow {#generic}

Het doel van deze workflow is om een gebruiker aan te melden bij zijn MVPD.  Na een geslaagde aanmelding geeft de server de gebruiker een verificatietoken uit. Terwijl de authentificatie typisch als deel van het vergunningsproces wordt gedaan, beschrijft het volgende hoe de authentificatie afzonderlijk kan werken, en omvat geen toestemmingsstappen.

Hoewel de volgende native clientworkflow afwijkt van de typische browsergebaseerde verificatieworkflow, zijn de stappen 1-5 hetzelfde voor zowel native clients als op browsers gebaseerde clients:

1. Uw pagina of speler stelt het authentificatiewerkschema met een vraag [ in werking getAuthentication () ](#getAuthN), die voor een geldig in de cache opgenomen authentificatietoken controleert. Deze methode heeft een optionele parameter `redirectURL` ; als u geen waarde opgeeft voor `redirectURL` , wordt de gebruiker na een geslaagde verificatie geretourneerd naar de URL vanwaar de verificatie is geïnitialiseerd.
1. AccessEnabler bepaalt de huidige authentificatiestatus. Als de gebruiker momenteel voor authentiek wordt verklaard, roept AccessEnabler uw `setAuthenticationStatus()` callback functie, die een authentificatiestatus overgaat die op succes wijst (Stap 7 hieronder).
1. Als de gebruiker niet voor authentiek wordt verklaard, gaat AccessEnabler de authentificatiestroom door te bepalen of de laatste authentificatiepoging van de gebruiker met bepaalde MVPD succesvol was. Als een MVPD-id in de cache is opgeslagen EN de markering `canAuthenticate` true is OF als een MVPD is geselecteerd met [`setSelectedProvider()`](#setSelectedProvider) , wordt de gebruiker niet gevraagd het dialoogvenster MVPD-selectie te openen. De authentificatiestroom gaat verder gebruikend de caching waarde van MVPD (namelijk zelfde MVPD die tijdens de laatste succesvolle authentificatie werd gebruikt). Een netwerkvraag wordt gemaakt aan de backendserver, en de gebruiker wordt opnieuw gericht aan de MVPD login pagina (Stap 6 hieronder).
1. Als er geen MVPD-id in de cache is opgeslagen EN er geen MVPD is geselecteerd met [`setSelectedProvider()`](#setSelectedProvider) OF als de markering `canAuthenticate` is ingesteld op false, wordt de callback [`displayProviderDialog()`](#displayProviderDialog) aangeroepen. Deze callback leidt uw pagina of speler om tot UI te leiden die de gebruiker met een lijst van MVPDs voorstelt om te kiezen van. Er wordt een array met MVPD-objecten geleverd, die de benodigde informatie bevat om de MVPD-kiezer te maken. Elk MVPD-object beschrijft een MVPD-entiteit en bevat informatie zoals de id van de MVPD (bijvoorbeeld XFINITY, AT\&amp;T, enz.) en de URL waar het MVPD-logo kan worden gevonden.
1. Zodra een bepaalde MVPD wordt geselecteerd, moet uw pagina of speler AccessEnabler op de hoogte brengen van de keus van de gebruiker. Voor niet-Flash cliënten, zodra de gebruiker gewenste MVPD selecteert, informeert u AccessEnabler van de gebruikersselectie via een vraag aan de [`setSelectedProvider()`](#setSelectedProvider) methode. Clients van Flash verzenden in plaats daarvan een gedeelde `MVPDEvent` van het type &quot;`mvpdSelection`&quot;, waarbij de geselecteerde provider wordt doorgegeven.
1. Voor Amazon-toepassingen wordt de callback van [`navigateToUrl()`](#navigagteToUrl) genegeerd. De bibliotheek van Inschakelen van de Toegang vergemakkelijkt toegang tot een gemeenschappelijke controle WebView om gebruikers voor authentiek te verklaren.
1. Via `WebView` arriveert de gebruiker op de aanmeldingspagina van de MVPD en voert hij zijn gegevens in. Houd er rekening mee dat tijdens deze overdracht verschillende omleidingsbewerkingen plaatsvinden.
1. Zodra WebView de authentificatie zal voltooien, zal het sluiten en AccessEnabler informeren dat de gebruiker met succes heeft het programma geopend, AccessEnabler wint het daadwerkelijke authentificatietoken van de backendserver terug. AccessEnabler roept [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 1 aan, die op succes wijst. Als er een fout optreedt tijdens het uitvoeren van deze stappen, wordt de callback van [`setAuthenticationStatus()`](#setAuthNStatus) geactiveerd met een statuscode 0, samen met een bijbehorende foutcode, die aangeeft dat de gebruiker niet is geverifieerd.

### Logout-workflow {#logout}

Voor native clients worden aanmeldgegevens op dezelfde manier verwerkt als het hierboven beschreven verificatieproces. Na dit patroon, zal AccessEnabler een `WebView` controle tot stand brengen en zal de controle met URL van het logout eindpunt op de backendserver laden. Zodra het logout-proces is voltooid, worden de tokens gewist.

Merk op dat de logout stroom van de authentificatiestroom verschilt in zoverre dat de gebruiker niet wordt vereist om met `WebView` op enige manier in wisselwerking te staan. Zodra logout volledig is, roept AccessEnabler `setAuthenticationStatus()` callback met een statuscode van 0 erop wijst, die dat de gebruiker niet voor authentiek wordt verklaard.

## Tokens {#tokens}

### Definities en gebruik {#definitions}

De Adobe Pass Authentication-machtigingsoplossing draait om het genereren van specifieke gegevens (tokens) die door Adobe Pass Authentication worden gegenereerd wanneer de workflows voor verificatie en autorisatie met succes zijn voltooid. Deze tokens worden lokaal opgeslagen op het Amazon FireOS-apparaat van de client.

Tokens hebben een beperkte levensduur; bij het verlopen moeten tokens opnieuw worden uitgegeven door het opnieuw opstarten van de workflows voor verificatie en/of autorisatie.

Er zijn drie typen tokens die worden uitgegeven tijdens de workflows voor machtigingen:

- **het teken van de Authentificatie** - het eindresultaat van het werkschema van de gebruikersauthentificatie zal een authentificatie GUID zijn die AccessEnabler kan gebruiken om vergunningsvragen op naam van de gebruiker te maken. Deze authentificatie GUID zal een bijbehorende tijd-aan-levende (TTL) waarde hebben die van de authentificatiesessie van de gebruiker zelf kan verschillend zijn. De Authentificatie van Adobe Pass produceert een authentificatietoken door authentificatie GUID aan het apparaat te binden die de authentificatieverzoeken in werking stelt.
- **teken van de Toestemming** - verleent toegang tot een specifiek beschermd middel dat door een uniek `resourceID` wordt geïdentificeerd. Het bestaat uit een door de vergunningverlenende partij samen met het origineel verleende autorisatiesubsidie `resourceID` . Deze informatie is gebonden aan het apparaat dat het verzoek initieert.
- **Kortstondig media teken** - AccessEnabler verleent toegang tot de ontvangende toepassing voor een bepaald middel door een kortstondig Symbolisch van Media terug te keren. Dit token wordt gegenereerd op basis van het machtigingstoken dat eerder werd verworven voor die specifieke resource. Bovendien is dit token niet gebonden aan het apparaat en is de bijbehorende levensduur aanzienlijk korter (standaard: 5 minuten).

Bij geslaagde verificatie en autorisatie geeft Adobe Pass Authentication, Authentificatie en kortstondige mediatokens uit. Deze tokens zouden op het apparaat van de gebruiker moeten worden in het voorgeheugen ondergebracht en voor de duur van hun bijbehorende leven-spanwijdten worden gebruikt.

### Richtlijnen voor caching {#caching}


#### Verificatietoken

- **AccessEnabler 1.10.1 voor FireOS** is gebaseerd op AccessEnabler voor Android 1.9.1 - Deze SDK introduceert een nieuwe methode van symbolische opslag, toelatend veelvoudige Programmer-MVPD emmers, en daarom, veelvoudige authentificatietokens.

#### Token voor autorisatie

Op om het even welk bepaald ogenblik wordt slechts ÉÉN toestemmingstoken per middel caching door AccessEnabler. Er kunnen veelvoudige toestemmingstokens in het voorgeheugen ondergebracht zijn, maar zij worden geassocieerd met verschillende middelen. Wanneer een nieuw toestemmingstoken wordt uitgegeven en oud reeds voor het zelfde middel bestaat, beschrijft het nieuwe teken de bestaande caching waarde.

#### Mediumtoken

De media-token voor korte tijd mag NIET in de cache worden opgeslagen. Het mediatoken moet van de server worden opgehaald telkens wanneer een autorisatie-API wordt aangeroepen, omdat dit beperkt is tot eenmalig gebruik.

### Persistentie {#persistence}

Tokens moeten blijvend zijn in opeenvolgende uitvoeringen van dezelfde toepassing. Dit betekent dat zodra de verificatie- en autorisatietokens zijn verkregen en de gebruiker de toepassing sluit, dezelfde tokens beschikbaar zijn voor de toepassing wanneer de gebruiker de toepassing opnieuw opent. Bovendien is het wenselijk dat deze tokens voor meerdere toepassingen blijvend zijn. Met andere woorden, nadat een gebruiker een toepassing gebruikt om zich aan te melden bij een specifieke identiteitsprovider (met succes authentificatie en toestemmingstkens), kunnen de zelfde tokens door een verschillende toepassing worden gebruikt, en de gebruiker wordt niet meer veroorzaakt voor geloofsbrieven wanneer het registreren via de zelfde identiteitsleverancier.

Dit type naadloze verificatie-/autorisatieworkflow maakt van de Adobe Pass-verificatieoplossing een echte implementatie op tv en overal. Vanuit zuiver technisch oogpunt werkt de Android AccessEnabler-bibliotheek rond de problemen van het delen van gegevens tussen toepassingen door de tokengegevens op te slaan in een databasebestand dat zich op externe opslag bevindt. Deze op systeemniveau gedeelde middelen verstrekken de belangrijkste ingrediënten die de implementatie van de gewenste blijvende tokens gebruiks-geval toelaten:

- Ondersteuning voor gestructureerde opslag - De opslag van het Adobe Pass-verificatietoken is niet alleen een eenvoudige lineaire geheugenstructuur die lijkt op een buffer. Het verstrekt een woordenboekachtig opslagmechanisme dat voor gegevens het indexeren op user-specified zeer belangrijke waarden toestaat.
- Ondersteuning voor gegevenspersistentie met behulp van het onderliggende bestandssysteem - De inhoud van het databasebestand blijft standaard behouden en de gegevens worden opgeslagen in het externe geheugen van het apparaat.

Zodra een bepaald teken in het symbolische geheime voorgeheugen wordt geplaatst, zal zijn geldigheid op verschillende tijden door de bibliotheek AccessEnabler worden gecontroleerd.  Een geldig token wordt gedefinieerd als:

- TTL van het token is niet verlopen
- De uitgever van de token is opgenomen in de lijst van toegestane identiteitsproviders

De symbolische opslag kan veelvoudige combinaties programmmer-MVPD steunen, die op een op meerdere niveaus genestelde kaartstructuur vertrouwen die veelvoudige authentificatietokens kan houden. Deze nieuwe opslag heeft op geen enkele wijze invloed op de openbare API van AccessEnabler en vereist geen wijzigingen aan de kant van de programmeur. Hier volgt een voorbeeld van deze nieuwere functionaliteit:

1. Open App1 (ontwikkeld door Programmer1).
1. Verifieer met MVPD1 (die met Programmer1) geïntegreerd is.
1. Onderbreek of sluit de huidige toepassing en open App2 (ontwikkeld door Programmer2).
1. Laten wij veronderstellen dat Programmer2 niet met MVPD2 geïntegreerd is; daarom zal de gebruiker NIET in App2 voor authentiek worden verklaard.
1. Verifieer met MVPD2 (die met Programmer2) in App2 geïntegreerd is.
1. De schakelaar terug naar App1; de gebruiker zal nog met Programmer1 voor authentiek worden verklaard.

### Indeling {#format}

#### Verificatietoken {#authn_token}

Hieronder ziet u de indeling van het verificatietoken:

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Token voor autorisatie {#authz_token}

De onderstaande lijst geeft de indeling van de machtigingstoken weer:

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Token voor korte media {#short_media_token}

Hieronder ziet u de indeling van de token voor korte media.  Dit teken wordt blootgesteld aan de toepassing van de Programmer.  Het wordt doorgegeven aan de toepassing van de programmeur na een succesvol machtigingsproces:

```JSON
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Apparaatbinding {#device_binding}

In de bovenstaande XML-lijsten noteert u het label `simpleTokenFingerprint` . Het doel van deze tag is om de individualisatiegegevens van de native apparaat-id op te slaan. De bibliotheek AccessEnabler kan dergelijke individualisatieinformatie verkrijgen en het ter beschikking stellen van de diensten van de Authentificatie van Adobe Pass tijdens de machtigingsvraag. De dienst zal deze informatie gebruiken en zal het in de daadwerkelijke tokens inbedden, zo effectief binden de tokens aan een specifiek apparaat. Het uiteindelijke doel hiervan is om de tokens niet-overdraagbaar te maken op verschillende apparaten.

Let in de bovenstaande XML-lijsten op de tag simpleTokenFingerprint. Het doel van deze tag is om de individualisatiegegevens van de native apparaat-id op te slaan. De bibliotheek AccessEnabler kan dergelijke individualisatieinformatie verkrijgen en het ter beschikking stellen van de diensten van de Authentificatie van Adobe Pass tijdens de machtigingsvraag. De dienst zal deze informatie gebruiken en zal het in de daadwerkelijke tokens inbedden, zo effectief binden de tokens aan een specifiek apparaat. Het uiteindelijke doel hiervan is om de tokens niet-overdraagbaar te maken op verschillende apparaten.

Aangezien dit duidelijk een veiligheids verwante eigenschap is, is deze informatie inherent &quot;gevoelig&quot;uit veiligheidsoogpunt. Daarom moet deze informatie worden beschermd tegen knoeien en afluisteren. Het afluisterprobleem wordt opgelost door de verificatie-/autorisatieverzoeken via het HTTPS-protocol te verzenden. De manipulatiebeveiliging wordt afgehandeld door de apparaatidentificatiegegevens digitaal te ondertekenen. De AccessEnabler-bibliotheek berekent een apparaat-id aan de hand van de informatie die door het apparaat wordt verstrekt en verzendt de apparaat-id vervolgens &quot;in the clear&quot; naar de Adobe Pass-verificatieservers als een aanvraagparameter.  De servers van de Authentificatie van Adobe Pass ondertekenen digitaal apparatenidentiteitskaart met de privé sleutel van de Adobe en voegen het aan het authentificatietoken toe dat aan AccessEnabler is teruggekeerd. Aldus wordt apparaat-id gebonden aan het verificatietoken.  Tijdens de vergunningsstroom, verzendt AccessEnabler opnieuw apparatenidentiteitskaart in duidelijk, samen met het authentificatietoken.  Als het validatieproces mislukt, mislukt de verificatie-/autorisatieworkflows automatisch.  De Adobe Pass-verificatieservers passen de persoonlijke sleutel toe op de apparaat-id en vergelijken deze met de waarde in het verificatietoken.  Als ze niet overeenkomen, mislukt die machtigingsstroom.
