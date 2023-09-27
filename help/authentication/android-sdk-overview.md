---
title: Overzicht van Android SDK
description: Overzicht van Android SDK
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 0%

---

# Overzicht van Android SDK {#android-sdk-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

Android AccessEnabler is een Java Android-bibliotheek waarmee mobiele apps Adobe Pass-verificatie kunnen gebruiken voor de machtigingsservices van TV Anywhere. Een implementatie Android bestaat uit de interface AccessEnabler die de machtiging API bepaalt, en een protocol EntitlementDelegate dat callbacks beschrijft die de bibliotheek teweegbrengt. De interface samen met het protocol wordt bedoeld onder één gemeenschappelijke naam: de bibliotheek AccessEnabler Android.

## Android-vereisten {#reqs}

Voor de huidige technische vereisten met betrekking tot het Android-platform en de Adobe Pass-verificatie, raadpleegt u [Vereisten voor platform/apparaat/gereedschap](#android)of raadpleeg de releaseopmerkingen die bij het downloaden van de Android-SDK zijn inbegrepen.

## Native clientworkflows begrijpen {#native_client_workflows}

Native clientworkflows zijn doorgaans hetzelfde als of lijken sterk op die van op browsers gebaseerde Adobe Pass Authentication-clients. Er zijn echter enkele uitzonderingen, zoals hieronder beschreven.

- [Workflow na initialisatie](#post-init)
- [Generic Initial Authentication Workflow](#generic)
- [Logout-workflow](#logout)

### Workflow na initialisatie {#post-init}

Alle die machtigingswerkschema&#39;s door AccessEnabler worden gesteund veronderstellen dat u eerder hebt geroepen [`setRequestor()`](#setRequestor) om uw identiteit vast te stellen. U doet deze vraag om uw identiteitskaart van de Aanvrager slechts eenmaal te verstrekken, gewoonlijk tijdens de initialisatie/opstellingsfase van uw toepassing.


Met de native clients (bijvoorbeeld Android), na uw eerste oproep aan [`setRequestor()`](#setRequestor)hebt u een keuze voor de volgende stappen:

- U kunt onmiddellijk beginnen machtigingsvraag te maken en hen toe te laten om stil worden een rij gevormd, indien nodig.

- U kunt ook een bevestiging ontvangen van het succes/de mislukking van [`setRequestor()`](#setRequestor) door de callback setRequestorComplete() uit te voeren.

- Of doe beide.

Het is aan u om te wachten op de kennisgeving van het succes van [`setRequestor()`](#setRequestor) of om op het mechanisme van de vraagrij van AccessEnabler te vertrouwen. Aangezien alle volgende autorisatie- en verificatieverzoeken de aanvrager-id en de bijbehorende configuratiegegevens nodig hebben, [`setRequestor()`](#setRequestor) de methode blokkeert effectief alle authentificatie en vergunning API vraag tot de initialisering volledig is.

### Generic Initial Authentication Workflow {#generic}

Het doel van deze workflow is om een gebruiker aan te melden bij zijn MVPD.  Na een geslaagde aanmelding geeft de server de gebruiker een verificatietoken uit. Terwijl de authentificatie typisch als deel van het vergunningsproces wordt gedaan, beschrijft het volgende hoe de authentificatie afzonderlijk kan werken, en omvat geen toestemmingsstappen.

Hoewel de volgende native clientworkflow afwijkt van de typische browsergebaseerde verificatieworkflow, zijn de stappen 1-5 hetzelfde voor zowel native clients als op browsers gebaseerde clients:

1. Uw pagina of speler start de verificatieworkflow met een aanroep van [getAuthentication()](#getAuthN), die een geldig verificatietoken in de cache zoekt. Deze methode heeft een optionele `redirectURL` parameter; als u geen waarde opgeeft voor `redirectURL`, na een geslaagde verificatie wordt de gebruiker geretourneerd naar de URL waarvan de verificatie is geïnitialiseerd.
1. AccessEnabler bepaalt de huidige authentificatiestatus. Als de gebruiker momenteel voor authentiek wordt verklaard, roept AccessEnabler uw `setAuthenticationStatus()` callback functie, die een authentificatiestatus overgaat die op succes wijst (Stap 7 hieronder).
1. Als de gebruiker niet voor authentiek wordt verklaard, gaat AccessEnabler de authentificatiestroom door te bepalen of de laatste authentificatiepoging van de gebruiker met bepaalde MVPD succesvol was. Als een MVPD-id in de cache is geplaatst EN de `canAuthenticate` markering is waar OF er is een MVPD geselecteerd met [`setSelectedProvider()`](#setSelectedProvider)wordt de gebruiker niet gevraagd het dialoogvenster MVPD-selectie te openen. De authentificatiestroom gaat verder gebruikend de caching waarde van MVPD (namelijk zelfde MVPD die tijdens de laatste succesvolle authentificatie werd gebruikt). Een netwerkvraag wordt gemaakt aan de backendserver, en de gebruiker wordt opnieuw gericht aan de MVPD login pagina (Stap 6 hieronder).
1. Als er geen MVPD-id in de cache is opgeslagen en er geen MVPD is geselecteerd met [`setSelectedProvider()`](#setSelectedProvider) OF de `canAuthenticate` markering is ingesteld op false, de [`displayProviderDialog()`](#displayProviderDialog) callback wordt aangeroepen. Deze callback leidt uw pagina of speler om tot UI te leiden die de gebruiker met een lijst van MVPDs voorstelt om te kiezen van. Er wordt een array met MVPD-objecten geleverd, die de benodigde informatie bevat om de MVPD-kiezer te maken. Elk MVPD-object beschrijft een MVPD-entiteit en bevat informatie zoals de id van de MVPD (bijvoorbeeld XFINITY, AT\&amp;T, enz.) en de URL waar het MVPD-logo kan worden gevonden.
1. Zodra een bepaalde MVPD wordt geselecteerd, moet uw pagina of speler AccessEnabler op de hoogte brengen van de keus van de gebruiker. Voor niet-Flash cliënten, zodra de gebruiker gewenste MVPD selecteert, informeert u AccessEnabler van de gebruikersselectie via een vraag aan [`setSelectedProvider()`](#setSelectedProvider) methode. Flash clients verzenden in plaats daarvan een gedeelde `MVPDEvent` van het type &quot;`mvpdSelection`&quot;, geeft u de geselecteerde provider door.
1. Als com.android.chrome beschikbaar is voor Android-toepassingen, wordt de verificatie-URL geladen in Aangepaste Chrome-tabs.
1. Via Chrome Custom Tabs arriveert de gebruiker op de aanmeldingspagina van de MVPD en voert hij zijn gegevens in. Houd er rekening mee dat tijdens deze overdracht verschillende omleidingsbewerkingen plaatsvinden.
1. Wanneer de Aangepaste Tabs van Chrome ontdekken dat een url de regeling (adobepass://) en diepe verbinding van middel &quot;redirect\_uri&quot;(d.w.z., adobepass://com.adobepass) aanpast, wint AccessEnabler het daadwerkelijke authentificatietoken van de backendservers terug. De uiteindelijke omleidings-URL&#39;s zijn in feite ongeldig en zijn niet bedoeld voor aangepaste Chrome-tabs om deze daadwerkelijk te laden. Zij moeten slechts door SDK als signaal worden geïnterpreteerd dat de authentificatiestroom heeft voltooid.
1. AccessEnabler deelt uw toepassing mee dat de authentificatiestroom volledig is. AccessEnabler roept de [`setAuthenticationStatus()`](#setAuthNStatus) callback met statuscode 1, die op succes wijst. Als er een fout optreedt tijdens het uitvoeren van deze stappen, [`setAuthenticationStatus()`](#setAuthNStatus) callback wordt teweeggebracht met een statuscode van 0, samen met een overeenkomstige foutencode, die op authentificatiemislukking wijst.

### Logout-workflow {#logout}

Voor native clients worden aanmeldgegevens op dezelfde manier verwerkt als het hierboven beschreven verificatieproces. Na dit patroon, opent AccessEnabler de Aangepaste Tabs van het Chroom en laadt URL van het logout eindpunt op de backendserver.



**Opmerking:** Het programma openen van één zitting Programma/MVPD zal de onderliggende opslag voor dat specifieke MVPD met inbegrip van alle andere die authentificatietokens van de Programmer door SSO op dat apparaat worden verkregen ontruimen. Tokens die voor andere MVPD&#39;s of niet via SSO zijn verkregen, worden niet verwijderd.


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

- **Verificatietoken** - Het eindresultaat van het werkschema van de gebruikersauthentificatie zal een authentificatie GUID zijn die AccessEnabler kan gebruiken om vergunningsvragen namens de gebruiker te maken. Deze authentificatie GUID zal een bijbehorende tijd-aan-levende (TTL) waarde hebben die van de authentificatiesessie van de gebruiker zelf kan verschillend zijn. De Authentificatie van Adobe Pass produceert een authentificatietoken door authentificatie GUID aan het apparaat te binden die de authentificatieverzoeken in werking stelt.
- **Token voor autorisatie** - verleent toegang tot een specifieke beschermde bron die door een uniek wordt geïdentificeerd `resourceID`. Het bestaat uit een door de vergunningverlenende partij afgegeven vergunning, samen met het origineel `resourceID`. Deze informatie is gebonden aan het apparaat dat het verzoek initieert.
- **Kortstondig media-token** - AccessEnabler verleent toegang tot de het ontvangen toepassing voor een bepaalde middel door een kort-levende Token van Media terug te keren. Dit token wordt gegenereerd op basis van het machtigingstoken dat eerder werd verworven voor die specifieke resource. Bovendien is dit token niet gebonden aan het apparaat en is de bijbehorende levensduur aanzienlijk korter (standaard: 5 minuten).

Bij geslaagde verificatie en autorisatie geeft Adobe Pass Authentication, Authentificatie en kortstondige mediatokens uit. Deze tokens zouden op het apparaat van de gebruiker moeten worden in het voorgeheugen ondergebracht en voor de duur van hun bijbehorende leven-spanwijdten worden gebruikt.



### Richtlijnen voor caching {#caching}

- Verificatietoken
- Token voor autorisatie
- Mediumtoken


#### Verificatietoken

- **AccessEnabler 1.6 en ouder** - De manier waarop verificatietokens in het cachegeheugen worden geplaatst, is afhankelijk van de instelling &quot;**Verificatie per aanvrager&quot;** vlag verbonden aan de huidige MVPD:


1. Als de functie Verificatie per aanvrager is ingesteld op *uitgeschakeld*, dan zal één enkel authentificatietoken plaatselijk in het globale plakbord worden opgeslagen. Dit teken zal tussen alle toepassingen worden gedeeld die met huidige MVPD geïntegreerd zijn.
1. Als de functie Verificatie per aanvrager is ingesteld op *enabled*, dan zal een teken uitdrukkelijk met de Programmer worden geassocieerd die de authentificatiestroom uitvoerde (het teken zal niet in het globale plakbord worden opgeslagen, maar in een privé dossier dat slechts aan de toepassing van die Programmer zichtbaar is). Specifieker, Single Sign-On (SSO) tussen verschillende toepassingen zal worden onbruikbaar gemaakt; de gebruiker zal de authentificatiestroom uitdrukkelijk moeten uitvoeren wanneer het schakelen naar een nieuwe app (op voorwaarde dat Programmer van tweede app met huidige MVPD wordt geïntegreerd en dat geen authentificatietoken voor die Programmer in het lokale geheime voorgeheugen bestaat).

   **Opmerking:** AE 1.6 Google GSON Tech Note: [Hoe te om de gebiedsdelen van Gson op te lossen](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Deze SDK introduceert een nieuwe methode van tokenopslag, toelatend veelvoudige programmer-MVPD emmers, en daarom, veelvoudige authentificatietokens. Vanaf AE 1.7 wordt dezelfde opslaglay-out gebruikt voor zowel het scenario &#39;Verificatie per aanvrager&#39; als voor de normale verificatiestroom. Het enige verschil tussen twee is in de manier de authentificatie wordt uitgevoerd: &quot;Authentificatie per Aanvrager&quot;bevat een nieuwe verbetering (passieve authentificatie) die het voor AccessEnabler mogelijk maakt om backchannel authentificatie uit te voeren, die op het bestaan van een authentificatietoken in de opslag (voor een verschillende Programmer) wordt gebaseerd. De gebruiker hoeft slechts eenmaal te autoriseren en deze sessie wordt gebruikt om verificatietokens te verkrijgen in volgende apps. Deze backkanaalstroom vindt plaats tijdens [`setRequestor()`](#setRequestor) en is voor het grootste deel transparant voor de programmeur. Er is echter één belangrijke eis: de programmeur moet een oproep doen [`setRequestor()`](#setRequestor) van de belangrijkste draad UI en van binnen een Activiteit.


#### Token voor autorisatie

Op om het even welk bepaald ogenblik wordt slechts ÉÉN toestemmingstoken per middel caching door AccessEnabler. Er kunnen veelvoudige toestemmingstokens in het voorgeheugen ondergebracht zijn, maar zij worden geassocieerd met verschillende middelen. Wanneer een nieuw toestemmingstoken wordt uitgegeven en oud reeds voor het zelfde middel bestaat, beschrijft het nieuwe teken de bestaande caching waarde.



#### Mediumtoken

De media-token voor korte tijd mag NIET in de cache worden opgeslagen. Het mediatoken moet van de server worden opgehaald telkens wanneer een autorisatie-API wordt aangeroepen, omdat dit beperkt is tot eenmalig gebruik.



### Persistentie {#persistence}

Tokens moeten blijvend zijn in opeenvolgende uitvoeringen van dezelfde toepassing. Dit betekent dat zodra de verificatie- en autorisatietokens zijn verkregen en de gebruiker de toepassing sluit, dezelfde tokens beschikbaar zijn voor de toepassing wanneer de gebruiker de toepassing opnieuw opent. Bovendien is het wenselijk dat deze tokens voor meerdere toepassingen blijvend zijn. Met andere woorden, nadat een gebruiker een toepassing gebruikt om zich aan te melden bij een specifieke identiteitsprovider (met succes authentificatie en toestemmingstkens), kunnen de zelfde tokens door een verschillende toepassing worden gebruikt, en de gebruiker wordt niet meer veroorzaakt voor geloofsbrieven wanneer het registreren via de zelfde identiteitsleverancier.



Dit type naadloze verificatie-/autorisatieworkflow maakt van de Adobe Pass-verificatieoplossing een echte implementatie op tv en overal. Vanuit zuiver technisch oogpunt werkt de Android AccessEnabler-bibliotheek rond de problemen van het delen van toepassingsgegevens door de tokengegevens op te slaan in een databasebestand dat zich op externe opslag bevindt. Deze op systeemniveau gedeelde middelen verstrekken de belangrijkste ingrediënten die de implementatie van de gewenste blijvende tokens gebruiks-geval toelaten:

- Ondersteuning voor gestructureerde opslag - De opslag van het Adobe Pass-verificatietoken is niet alleen een eenvoudige lineaire geheugenstructuur die lijkt op een buffer. Het verstrekt een woordenboekachtig opslagmechanisme dat voor gegevens het indexeren op user-specified zeer belangrijke waarden toestaat.
- Ondersteuning voor gegevenspersistentie met behulp van het onderliggende bestandssysteem - De inhoud van het databasebestand blijft standaard behouden en de gegevens worden opgeslagen in het externe geheugen van het apparaat.



Zodra een bepaald teken in het symbolische geheime voorgeheugen wordt geplaatst, zal zijn geldigheid op verschillende tijden door de bibliotheek AccessEnabler worden gecontroleerd.  Een geldig token wordt gedefinieerd als:

- TTL van het token is niet verlopen
- De uitgever van de token is opgenomen in de lijst van toegestane identiteitsproviders



Beginnend met AccessEnabler 1.7, kan de symbolische opslag veelvoudige combinaties programmmer-MVPD steunen, die zich op een op meerdere niveaus genestelde kaartstructuur baseren die veelvoudige authentificatietokens kan houden. Deze nieuwe opslag heeft op geen enkele wijze invloed op de openbare API van AccessEnabler en vereist geen wijzigingen aan de kant van de programmeur. Hier volgt een voorbeeld van deze nieuwere functionaliteit:

1. Open App1 (ontwikkeld door Programmer1).
1. Verifieer met MVPD1 (die met Programmer1) geïntegreerd is.
1. Onderbreek of sluit de huidige toepassing en open App2 (ontwikkeld door Programmer2).
1. Laten wij veronderstellen dat Programmer2 niet met MVPD2 geïntegreerd is; daarom zal de gebruiker NIET in App2 voor authentiek worden verklaard.
1. Verifieer met MVPD2 (die met Programmer2) in App2 geïntegreerd is.
1. De schakelaar terug naar App1; de gebruiker zal nog met Programmer1 voor authentiek worden verklaard.

In oudere versies van AccessEnabler, zou Stap 6 de gebruiker als niet-voor authentiek verklaard teruggeven, omdat de symbolische opslag vroeger slechts één authentificatietoken steunde.


**OPMERKING:** Als u zich afmeldt bij een programmeur-/MVPD-sessie, wordt de onderliggende opslag gewist, inclusief alle andere programmamarkerverificatietokens op het apparaat met SSO. Tokens die voor andere MVPD&#39;s of niet via SSO zijn verkregen, worden niet verwijderd. De verificatiestroom annuleren (aanroepen) [`setSelectedProvider(null)`](#setSelectedProvider)) zal NIET de onderliggende opslag ontruimen, maar zal slechts de huidige de authentificatiepoging van Programmer/MVPD beïnvloeden (door MVPD voor de huidige Programmer te wissen).


Een andere opslag-verwante eigenschap die in AccessEnabler 1.7 inbegrepen is maakt het mogelijk om authentificatietokens van oudere opslaggebieden in te voeren. Deze &quot;Symbolische Importer&quot;helpt om verenigbaarheid tussen opeenvolgende versies te bereiken AccessEnabler, die de SSO staat handhaven zelfs wanneer de opslagversie wordt bevorderd.

De importeur wordt uitgevoerd tijdens de [`setRequestor()`](#setRequestor) stroom, en looppas in de volgende twee scenario&#39;s (veronderstellend dat geen geldig authentificatietoken voor huidige Programmer in de huidige opslag aanwezig is):

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

Let in de bovenstaande XML-items op de tag met de titel `simpleTokenFingerprint`. Het doel van deze tag is om de individualisatiegegevens van de native apparaat-id op te slaan. De bibliotheek AccessEnabler kan dergelijke individualisatieinformatie verkrijgen en het ter beschikking stellen van de diensten van de Authentificatie van Adobe Pass tijdens de machtigingsvraag. De dienst zal deze informatie gebruiken en zal het in de daadwerkelijke tokens inbedden, zo effectief binden de tokens aan een specifiek apparaat. Het uiteindelijke doel hiervan is om de tokens niet-overdraagbaar te maken op verschillende apparaten.



Let in de bovenstaande XML-lijsten op de tag simpleTokenFingerprint. Het doel van deze tag is om de individualisatiegegevens van de native apparaat-id op te slaan. De bibliotheek AccessEnabler kan dergelijke individualisatieinformatie verkrijgen en het ter beschikking stellen van de diensten van de Authentificatie van Adobe Pass tijdens de machtigingsvraag. De dienst zal deze informatie gebruiken en zal het in de daadwerkelijke tokens inbedden, zo effectief binden de tokens aan een specifiek apparaat. Het uiteindelijke doel hiervan is om de tokens niet-overdraagbaar te maken op verschillende apparaten.



Aangezien dit duidelijk een veiligheids verwante eigenschap is, is deze informatie inherent &quot;gevoelig&quot;uit veiligheidsoogpunt. Daarom moet deze informatie worden beschermd tegen knoeien en afluisteren. Het afluisterprobleem wordt opgelost door de verificatie-/autorisatieverzoeken via het HTTPS-protocol te verzenden. De manipulatiebeveiliging wordt afgehandeld door de apparaatidentificatiegegevens digitaal te ondertekenen. De AccessEnabler-bibliotheek berekent een apparaat-id aan de hand van de informatie die door het apparaat wordt verstrekt en verzendt de apparaat-id vervolgens &quot;in the clear&quot; naar de Adobe Pass-verificatieservers als een aanvraagparameter.  De servers van de Authentificatie van Adobe Pass ondertekenen digitaal apparatenidentiteitskaart met de privé sleutel van de Adobe en voegen het aan het authentificatietoken toe dat aan AccessEnabler is teruggekeerd. Aldus wordt apparaat-id gebonden aan het verificatietoken.  Tijdens de vergunningsstroom, verzendt AccessEnabler opnieuw apparatenidentiteitskaart in duidelijk, samen met het authentificatietoken.  Als het validatieproces mislukt, mislukt de verificatie-/autorisatieworkflows automatisch.  De Adobe Pass-verificatieservers passen de persoonlijke sleutel toe op de apparaat-id en vergelijken deze met de waarde in het verificatietoken.  Als ze niet overeenkomen, mislukt die machtigingsstroom.


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
