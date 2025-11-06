---
title: Android SDK - Overzicht
description: Android SDK - Overzicht
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2754'
ht-degree: 0%

---

# (Verouderd) Android SDK - Overzicht {#android-sdk-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#intro}

Android AccessEnabler is een Java Android-bibliotheek waarmee mobiele apps Adobe Pass-verificatie kunnen gebruiken voor de machtigingsservices van TV Anywhere. Een implementatie van Android bestaat uit de interface AccessEnabler die de machtiging API bepaalt, en een protocol EntitlementDelegate dat callbacks beschrijft die de bibliotheek teweegbrengt. De interface samen met het protocol wordt bedoeld onder één gemeenschappelijke naam: de bibliotheek van Android AccessEnabler.

## Android-vereisten {#reqs}

Voor huidige technische vereisten met betrekking tot het platform van Android en de Authentificatie van Adobe Pass, zie [&#x200B; de Vereisten van het Platform / van het Apparaat / van het Hulpmiddel &#x200B;](#android), of raadpleeg de versienota&#39;s inbegrepen met de download van Android SDK.

## Native clientworkflows begrijpen {#native_client_workflows}

Native clientworkflows zijn doorgaans hetzelfde als of lijken sterk op die van op browsers gebaseerde Adobe Pass Authentication-clients. Er zijn echter enkele uitzonderingen, zoals hieronder beschreven.

- [Workflow na initialisatie](#post-init)
- [Generic Initial Authentication Workflow](#generic)
- [Logout-workflow](#logout)

### Workflow na initialisatie {#post-init}

Alle machtigingswerkstromen die door AccessEnabler worden gesteund veronderstellen dat u eerder [`setRequestor()`](#setRequestor) hebt geroepen om uw identiteit te vestigen. U doet deze vraag om uw identiteitskaart van de Aanvrager slechts eenmaal te verstrekken, gewoonlijk tijdens de initialisatie/opstellingsfase van uw toepassing.


Met de native clients (bijvoorbeeld Android) hebt u na uw eerste aanroep naar [`setRequestor()`](#setRequestor) de keuze hoe u wilt doorgaan:

- U kunt onmiddellijk beginnen machtigingsvraag te maken en hen toe te laten om stil worden een rij gevormd, indien nodig.

- U kunt ook een bevestiging ontvangen van het succes/de mislukking van [`setRequestor()`](#setRequestor) door de callback setRequestorComplete() uit te voeren.

- Of doe beide.

Het is aan u om op bericht van het succes van [`setRequestor()`](#setRequestor) te wachten of op het mechanisme van de vraagrij van AccessEnabler te vertrouwen. Aangezien alle volgende autorisatie- en verificatieaanvragen de aanvrager-id en de bijbehorende configuratiegegevens nodig hebben, blokkeert de methode [`setRequestor()`](#setRequestor) in feite alle API-aanroepen voor verificatie en autorisatie totdat de initialisatie is voltooid.

### Generic Initial Authentication Workflow {#generic}

Het doel van deze workflow is om een gebruiker aan te melden bij zijn MVPD.  Na een geslaagde aanmelding geeft de server de gebruiker een verificatietoken uit. Terwijl de authentificatie typisch als deel van het vergunningsproces wordt gedaan, beschrijft het volgende hoe de authentificatie afzonderlijk kan werken, en omvat geen toestemmingsstappen.

Hoewel de volgende native clientworkflow afwijkt van de typische browsergebaseerde verificatieworkflow, zijn de stappen 1-5 hetzelfde voor zowel native clients als op browsers gebaseerde clients:

1. Uw pagina of speler stelt het authentificatiewerkschema met een vraag [&#x200B; in werking getAuthentication () &#x200B;](#getAuthN), die voor een geldig in de cache opgenomen authentificatietoken controleert. Deze methode heeft een optionele parameter `redirectURL` ; als u geen waarde opgeeft voor `redirectURL` , wordt de gebruiker na een geslaagde verificatie geretourneerd naar de URL vanwaar de verificatie is geïnitialiseerd.
1. AccessEnabler bepaalt de huidige authentificatiestatus. Als de gebruiker momenteel voor authentiek wordt verklaard, roept AccessEnabler uw `setAuthenticationStatus()` callback functie, die een authentificatiestatus overgaat die op succes wijst (Stap 7 hieronder).
1. Als de gebruiker niet voor authentiek wordt verklaard, gaat AccessEnabler de authentificatiestroom door te bepalen of de laatste authentificatiepoging van de gebruiker met bepaalde MVPD succesvol was. Als een MVPD-id in de cache wordt opgeslagen EN de markering `canAuthenticate` true is OF als een MVPD is geselecteerd met [`setSelectedProvider()`](#setSelectedProvider) , wordt de gebruiker niet gevraagd het dialoogvenster voor MVPD-selectie te openen. De verificatiestroom gaat verder met gebruik van de cachewaarde van de MVPD (hetzelfde MVPD dat tijdens de laatste geslaagde verificatie is gebruikt). Er wordt een netwerkaanroep naar de back-endserver gemaakt en de gebruiker wordt omgeleid naar de MVPD-aanmeldingspagina (stap 6 hieronder).
1. Als er geen MVPD-id in de cache is opgeslagen EN er geen MVPD is geselecteerd met [`setSelectedProvider()`](#setSelectedProvider) OF als de markering `canAuthenticate` is ingesteld op false, wordt de callback [`displayProviderDialog()`](#displayProviderDialog) aangeroepen. Deze callback leidt uw pagina of speler om tot UI te leiden die de gebruiker met een lijst van MVPDs voorstelt om te kiezen van. Er is een array met MVPD-objecten beschikbaar, die de benodigde informatie bevat voor het maken van de MVPD-kiezer. Elk MVPD-object beschrijft een MVPD-entiteit en bevat informatie zoals de id van de MVPD (bijvoorbeeld XFINITY, AT\&amp;T, enzovoort) en de URL waar het MVPD-logo kan worden gevonden.
1. Wanneer een bepaalde MVPD is geselecteerd, moet uw pagina of speler de AccessEnabler op de hoogte stellen van de keuze van de gebruiker. Voor niet-Flash clients informeert u de AccessEnabler via een aanroep van de methode [`setSelectedProvider()`](#setSelectedProvider) wanneer de gebruiker de gewenste MVPD heeft geselecteerd. Flash-clients verzenden in plaats daarvan een gedeelde `MVPDEvent` van het type &quot;`mvpdSelection`&quot;, waarbij de geselecteerde provider wordt doorgegeven.
1. Als com.android.chrome beschikbaar is voor Android-toepassingen, wordt de verificatie-URL geladen in Chrome Custom Tabs.
1. Via Chrome Custom Tabs arriveert de gebruiker op de aanmeldingspagina van MVPD en voert hij zijn gegevens in. Houd er rekening mee dat tijdens deze overdracht verschillende omleidingsbewerkingen plaatsvinden.
1. Wanneer Chrome Custom Tabs detecteert dat een URL overeenkomt met het schema (adobepass://) en de diepe koppeling van de resource &quot;redirect\_uri&quot; (i.e. adobepass://com.adobepass), haalt AccessEnabler het daadwerkelijke verificatietoken op van de back-endservers. De uiteindelijke omleidings-URL&#39;s zijn in feite ongeldig en zijn niet bedoeld voor Chrome Custom Tabs om deze daadwerkelijk te laden. Zij moeten slechts door SDK als signaal worden geïnterpreteerd dat de authentificatiestroom heeft voltooid.
1. AccessEnabler deelt uw toepassing mee dat de authentificatiestroom volledig is. AccessEnabler roept [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 1 aan, die op succes wijst. Als er tijdens de uitvoering van deze stappen een fout optreedt, wordt de callback van [`setAuthenticationStatus()`](#setAuthNStatus) geactiveerd met een statuscode 0, samen met een bijbehorende foutcode die verificatiefout aangeeft.

### Logout-workflow {#logout}

Voor native clients worden aanmeldgegevens op dezelfde manier verwerkt als het hierboven beschreven verificatieproces. Na dit patroon, opent AccessEnabler Chrome Custom Tabs en laadt URL van het logout eindpunt op de backendserver.



**Nota:** het programma openen uit één zitting van de Programmer/van MVPD zal ontruimen
de onderliggende opslag voor die specifieke MVPD, met inbegrip van alle
andere door SSO op verkregen programmamachtigingstokens
dat apparaat. Tokens die voor andere MVPD&#39;s of niet door SSO worden verkregen zullen niet
worden geschrapt.


## Tokens {#tokens}

- [Definities en gebruik](#definitions)
- [Richtlijnen voor caching](#caching)
- [Persistentie](#persistence)
- [Indeling](#format)
- [Apparaatbinding](#device_binding)



### Definities en gebruik {#definitions}

De Adobe Pass Authentication-machtigingsoplossing draait om het genereren van specifieke gegevens (tokens) die door Adobe Pass Authentication worden gegenereerd wanneer de workflows voor verificatie en autorisatie met succes zijn voltooid. Deze tokens worden lokaal opgeslagen op het Android-apparaat van de client.

Tokens hebben een beperkte levensduur; bij het verlopen moeten tokens opnieuw worden uitgegeven door het opnieuw opstarten van de workflows voor verificatie en/of autorisatie.

Er zijn drie typen tokens die worden uitgegeven tijdens de workflows voor machtigingen:

- **het teken van de Authentificatie** - het eindresultaat van het werkschema van de gebruikersauthentificatie zal een authentificatie GUID zijn die AccessEnabler kan gebruiken om vergunningsvragen op naam van de gebruiker te maken. Deze authentificatie GUID zal een bijbehorende tijd-aan-levende (TTL) waarde hebben die van de authentificatiesessie van de gebruiker zelf kan verschillend zijn. De Authentificatie van Adobe Pass produceert een authentificatietoken door authentificatie GUID aan het apparaat te binden die de authentificatieverzoeken in werking stelt.
- **teken van de Toestemming** - verleent toegang tot een specifiek beschermd middel dat door een uniek `resourceID` wordt geïdentificeerd. Het bestaat uit een door de vergunningverlenende partij samen met het origineel verleende autorisatiesubsidie `resourceID` . Deze informatie is gebonden aan het apparaat dat het verzoek initieert.
- **Kortstondig media teken** - AccessEnabler verleent toegang tot de ontvangende toepassing voor een bepaald middel door een kortstondig Symbolisch van Media terug te keren. Dit token wordt gegenereerd op basis van het machtigingstoken dat eerder werd verworven voor die specifieke resource. Bovendien is dit token niet gebonden aan het apparaat en is de bijbehorende levensduur aanzienlijk korter (standaard: 5 minuten).

Bij geslaagde verificatie en autorisatie geeft Adobe Pass Authentication, Authentificatie en kortstondige mediatokens uit. Deze tokens zouden op het apparaat van de gebruiker moeten worden in het voorgeheugen ondergebracht en voor de duur van hun bijbehorende leven-spanwijdten worden gebruikt.



### Richtlijnen voor caching {#caching}

- Verificatietoken
- Token voor autorisatie
- Mediumtoken


#### Verificatietoken

- **AccessEnabler 1.6 en ouder** - de manier waarin de authentificatietokens in het voorgeheugen ondergebracht op het apparaat afhangt van &quot;**Authentificatie per Vraagster&quot;** vlag verbonden aan huidige MVPD:


1. Als de &quot;Authentificatie per de eigenschap&quot;*gehandicapt* is, dan zal één enkel authentificatietoken plaatselijk in het globale plakbord worden opgeslagen. Dit token wordt gedeeld tussen alle toepassingen die zijn geïntegreerd met de huidige MVPD.
1. Als de &quot;Authentificatie per de eigenschap van de Aanvrager&quot;** wordt toegelaten, dan zal een teken uitdrukkelijk met de Programmer worden geassocieerd die de authentificatiestroom (het teken zal niet in het globale plakbord worden opgeslagen, maar in een privé dossier dat slechts aan de toepassing van die Programmer zichtbaar is). Specifieker, Single Sign-On (SSO) tussen verschillende toepassingen zal worden onbruikbaar gemaakt; de gebruiker zal de authentificatiestroom uitdrukkelijk moeten uitvoeren wanneer het schakelen naar een nieuwe app (op voorwaarde dat Programmer van tweede app met huidige MVPD wordt geïntegreerd en dat geen authentificatietoken voor die Programmer in het lokale geheime voorgeheugen bestaat).

   **Nota:** AE 1.6 Google GSON Tech Nota: [&#x200B; hoe te om de gebiedsdelen van Gson op te lossen &#x200B;](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Deze SDK introduceert een nieuwe methode van symbolische opslag, toelatend veelvoudige emmers programmer-MVPD, en daarom, veelvoudige authentificatietokens. Vanaf AE 1.7 wordt dezelfde opslaglay-out gebruikt voor zowel het scenario &#39;Verificatie per aanvrager&#39; als voor de normale verificatiestroom. Het enige verschil tussen twee is in de manier de authentificatie wordt uitgevoerd: &quot;Authentificatie per Aanvrager&quot;bevat een nieuwe verbetering (passieve authentificatie) die het voor AccessEnabler mogelijk maakt om backchannel authentificatie uit te voeren, die op het bestaan van een authentificatietoken in de opslag (voor een verschillende Programmer) wordt gebaseerd. De gebruiker hoeft slechts eenmaal te autoriseren en deze sessie wordt gebruikt om verificatietokens te verkrijgen in volgende apps. Deze backchannel flow vindt plaats tijdens de [`setRequestor()`](#setRequestor) oproep en is meestal transparant voor de programmeur. Er is hier echter één belangrijke vereiste: de programmeur moet [`setRequestor()`](#setRequestor) vanuit de hoofdthread van de gebruikersinterface en vanuit een activiteit aanroepen.


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



Beginnend met AccessEnabler 1.7, kan de symbolische opslag veelvoudige combinaties programmmer-MVPD steunen, die op een op meerdere niveaus genestelde kaartstructuur vertrouwen die veelvoudige authentificatietokens kan houden. Deze nieuwe opslag heeft op geen enkele wijze invloed op de openbare API van AccessEnabler en vereist geen wijzigingen aan de kant van de programmeur. Hier volgt een voorbeeld van deze nieuwere functionaliteit:

1. Open App1 (ontwikkeld door Programmer1).
1. Verifieer met MVPD1 (die met Programmer1) wordt geïntegreerd.
1. Onderbreek of sluit de huidige toepassing en open App2 (ontwikkeld door Programmer2).
1. Laten we aannemen dat Programmer2 niet is geïntegreerd met MVPD2. Daarom wordt de gebruiker NIET geverifieerd in App2.
1. Verifieer met MVPD2 (die met Programmer2) in App2 wordt geïntegreerd.
1. De schakelaar terug naar App1; de gebruiker zal nog met Programmer1 voor authentiek worden verklaard.

In oudere versies van AccessEnabler, zou Stap 6 de gebruiker als niet-voor authentiek verklaard teruggeven, omdat de symbolische opslag vroeger slechts één authentificatietoken steunde.


**NOTA:** Logging uit van één zitting van de Programmer/van MVPD zal de onderliggende opslag, met inbegrip van alle andere de authentificatietokens van de Programmer op het apparaat met SSO ontruimen. Tokens die voor andere MVPD&#39;s of niet via SSO zijn verkregen, worden niet verwijderd. Als de verificatiestroom wordt geannuleerd (door [`setSelectedProvider(null)`](#setSelectedProvider) aan te roepen), wordt de onderliggende opslag NIET gewist, maar wordt alleen de huidige verificatiepoging van programmeur/MVPD beïnvloed (door de MVPD voor de huidige programmeur te wissen).


Een andere opslag-verwante eigenschap die in AccessEnabler 1.7 inbegrepen is maakt het mogelijk om authentificatietokens van oudere opslaggebieden in te voeren. Deze &quot;Symbolische Importer&quot;helpt om verenigbaarheid tussen opeenvolgende versies te bereiken AccessEnabler, die de SSO staat handhaven zelfs wanneer de opslagversie wordt bevorderd.

De importer wordt uitgevoerd tijdens de [`setRequestor()`](#setRequestor) -flow en wordt uitgevoerd in de volgende twee scenario&#39;s (ervan uitgaande dat er geen geldig verificatietoken voor de huidige programmeur aanwezig is in de huidige opslag):

- De eerste installatie van een 1.7-app die is ontwikkeld door een specifieke programmeur
- Upgradeweg aan een toekomstige AccessEnabler die een nieuwe opslag gebruikt

De importbewerking is transparant voor de programmeur en vereist geen codewijziging in de clienttoepassing.



### Indeling {#format}

- [Verificatietoken](#authn_token)
- [Token voor autorisatie](#authz_token)
- [Token voor korte media](#short_media_token)
- [Apparaatbinding](#device_binding)

#### Verificatietoken {#authn_token}

Hieronder ziet u de indeling van het verificatietoken:

```XML
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

```XML
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

```XML
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



Aangezien dit duidelijk een veiligheids verwante eigenschap is, is deze informatie inherent &quot;gevoelig&quot;uit veiligheidsoogpunt. Daarom moet deze informatie worden beschermd tegen knoeien en afluisteren. Het afluisterprobleem wordt opgelost door de verificatie-/autorisatieverzoeken via het HTTPS-protocol te verzenden. De manipulatiebeveiliging wordt afgehandeld door de apparaatidentificatiegegevens digitaal te ondertekenen. De AccessEnabler-bibliotheek berekent een apparaat-id aan de hand van de informatie die door het apparaat wordt verstrekt en verzendt de apparaat-id vervolgens &quot;in the clear&quot; naar de Adobe Pass-verificatieservers als een aanvraagparameter.  De servers van de Authentificatie van Adobe Pass ondertekenen digitaal apparatenidentiteitskaart met Adobe privé sleutel en voegen het aan het authentificatietoken toe dat aan AccessEnabler is teruggekeerd. Aldus wordt apparaat-id gebonden aan het verificatietoken.  Tijdens de vergunningsstroom, verzendt AccessEnabler opnieuw apparatenidentiteitskaart in duidelijk, samen met het authentificatietoken.  Als het validatieproces mislukt, mislukt de verificatie-/autorisatieworkflows automatisch.  De Adobe Pass-verificatieservers passen de persoonlijke sleutel toe op de apparaat-id en vergelijken deze met de waarde in het verificatietoken.  Als ze niet overeenkomen, mislukt die machtigingsstroom.


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
