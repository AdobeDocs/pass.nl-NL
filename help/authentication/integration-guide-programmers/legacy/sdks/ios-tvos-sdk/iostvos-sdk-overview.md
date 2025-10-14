---
title: iOS/tvOS SDK - Overzicht
description: iOS/tvOS SDK - Overzicht
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# (Verouderd) iOS/tvOS SDK - Overzicht {#iostvos-sdk-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>


## Inleiding {#intro}

iOS AccessEnabler is een Objectieve C iOS/tvOS-bibliotheek waarmee mobiele apps Adobe Pass Authentication for TV Anywhere&#39;s machtigingsservices kunnen gebruiken. De implementatie bestaat uit de *AccessEnabler* interface die de betitelings API, en *EntitlementDelegate* en *[EntitlementStatus](#ios%20entitlement%20status)* protocollen bepaalt die callbacks beschrijven die de bibliotheek trekkers. De interface samen met het protocol wordt bedoeld onder één gemeenschappelijke naam: de bibliotheek AccessEnabler.

## iOS- en tvOS-vereisten {#reqs}

Voor huidige technische vereisten met betrekking tot het iOS en tvOS platform en de Authentificatie van Adobe Pass, zie [&#x200B; de Vereisten van het Platform / van het Apparaat / van het Hulpmiddel &#x200B;](#ios), en raadpleeg de versienota&#39;s inbegrepen met de download van SDK. In de rest van deze pagina ziet u secties die wijzigingen bevatten die van toepassing zijn op bepaalde SDK-versies en hoger. Hier volgt bijvoorbeeld een legitieme opmerking met betrekking tot de 1.7.5-SDK:

## Native clientworkflows begrijpen {#flows}

Native clientworkflows zijn doorgaans gelijk aan of lijken sterk op die van Adobe Pass-verificatieclients op browserbasis. Er zijn echter enkele uitzonderingen, zoals hieronder beschreven.

- [Workflow na initialisatie](#post-init)
- [Generic Initial Authentication Workflow](#generic)
- [Logout-workflow](#logout)


### Workflow na initialisatie {#post-init}

Alle machtigingswerkstromen die door AccessEnabler worden gesteund veronderstellen dat u eerder [`setRequestor()`](#setReq) hebt geroepen om uw identiteit te vestigen. U doet deze vraag om uw identiteitskaart van de Aanvrager slechts eenmaal te verstrekken, gewoonlijk tijdens de initialisatie/opstellingsfase van uw toepassing.


Na uw eerste aanroep van [`setRequestor()`](#setReq) naar een native iOS-client kunt u kiezen hoe u wilt doorgaan:

- U kunt onmiddellijk beginnen machtigingsvraag te maken en hen toe te laten om stil worden een rij gevormd, indien nodig.

- U kunt een bevestiging van het succes/de mislukking van [`setRequestor()`](#setReq) ontvangen door [`setRequestorComplete()`](#setReqComplete) callback uit te voeren.

- U kunt beide bovenstaande handelingen uitvoeren.

Het is uw keuze om uw app te laten wachten op een melding van het succes van [`setRequestor()`](#setReq) of ervoor te zorgen dat deze afhankelijk is van het mechanisme voor de wachtrij van AccessEnabler. Aangezien alle volgende autorisatie- en verificatieaanvragen de aanvrager-id en de bijbehorende configuratiegegevens nodig hebben, blokkeert de methode [`setRequestor()`](#setReq) in feite alle API-aanroepen voor verificatie en autorisatie totdat de initialisatie is voltooid.



### Generic Initial Authentication Workflow {#generic}

Het doel van deze workflow is om een gebruiker aan te melden bij zijn MVPD. Na een geslaagde aanmelding geeft de server de gebruiker een verificatietoken uit. Terwijl de authentificatie typisch als deel van het vergunningsproces wordt gedaan, beschrijft het volgende hoe de authentificatie afzonderlijk kan werken, en omvat geen toestemmingsstappen.

Hoewel deze workflow voor native clients verschilt van de typische browsergebaseerde verificatieworkflow, zijn de stappen 1-5 hetzelfde voor zowel native clients als op browsers gebaseerde clients.

1. Uw toepassing stelt het authentificatiewerkschema met een vraag in werking aan API van AccessEnabler `getAuthentication() ` methode, die op een geldig in de cache opgenomen authentificatietoken controleert.
1. Als de gebruiker momenteel voor authentiek wordt verklaard, roept AccessEnabler uw [`setAuthenticationStatus()`](#setAuthNStatus) callback functie, die een authentificatiestatus overgaat die op succes wijst, en die de stroom beëindigt.
1. Als de gebruiker momenteel niet voor authentiek wordt verklaard, gaat AccessEnabler de authentificatiestroom door te bepalen of de laatste authentificatiepoging van de gebruiker met bepaalde MVPD succesvol was. Als een MVPD-id in de cache wordt opgeslagen EN de markering `canAuthenticate` true is OF als een MVPD is geselecteerd met [`setSelectedProvider()`](#setSelProv) , wordt de gebruiker niet gevraagd het dialoogvenster voor MVPD-selectie te openen. De verificatiestroom gaat verder met gebruik van de cachewaarde van de MVPD (hetzelfde MVPD dat tijdens de laatste geslaagde verificatie is gebruikt). Er wordt een netwerkaanroep naar de back-endserver gemaakt en de gebruiker wordt omgeleid naar de MVPD-aanmeldingspagina (stap 6 hieronder).
1. Als er geen MVPD-id in de cache is opgeslagen EN er geen MVPD is geselecteerd met [`setSelectedProvider()`](#setSelProv) OF als de markering `canAuthenticate` is ingesteld op false, wordt de callback [`displayProviderDialog()`](#dispProvDialog) aangeroepen. Deze callback draagt uw toepassing op om tot UI te leiden die de gebruiker een lijst van MVPDs voorstelt om te kiezen van. Er is een array met MVPD-objecten beschikbaar, die de benodigde informatie bevat voor het maken van de MVPD-kiezer. Elk MVPD-object beschrijft een MVPD-entiteit en bevat informatie zoals de id van de MVPD (bijvoorbeeld XFINITY, AT\&amp;T, enzovoort) en de URL waar het MVPD-logo kan worden gevonden.
1. Zodra een bepaalde MVPD wordt geselecteerd, moet uw toepassing AccessEnabler van de keus van de gebruiker op de hoogte brengen. Zodra de gebruiker de gewenste MVPD selecteert, informeert u AccessEnabler over de gebruikersselectie via een aanroep van de methode [`setSelectedProvider()`](#setSelProv) .
1. De iOS AccessEnabler roept de `navigateToUrl:` callback of `navigateToUrl:useSVC:` callback aan om de gebruiker om te leiden naar de MVPD-aanmeldingspagina. Door één van beide teweeg te brengen, vraagt AccessEnabler aan uw toepassing om tot een `UIWebView/WKWebView or SFSafariViewController` controlemechanisme te leiden en URL te laden die in de parameter `url` van callback wordt verstrekt. Dit is URL van het authentificatieeindpunt op de backendserver. Voor tvOS AccessEnabler, wordt [&#x200B; status () &#x200B;](#status_callback_implementation) callback geroepen met een `statusDictionary` parameter en de opiniepeiling voor de tweede het schermauthentificatie is onmiddellijk begonnen. `statusDictionary` bevat `registration code` die moet worden gebruikt voor de tweede schermverificatie.
1. In het geval van iOS AccessEnabler, landt de gebruiker op de MVPD login pagina om zijn geloofsbrieven door het middel van uw toepassing `UIWebView/WKWebView or SFSafariViewController ` controlemechanisme in te voeren. Houd er rekening mee dat tijdens deze overdracht verschillende omleidingsbewerkingen plaatsvinden en dat uw toepassing de URL&#39;s moet controleren die door de controller worden geladen tijdens de meervoudige omleidingsbewerkingen.
1. In het geval van iOS AccessEnabler, wanneer het `UIWebView/WKWebView or SFSafariViewController` controlemechanisme een specifieke douane URL laadt moet uw toepassing het controlemechanisme sluiten en de 1&rbrace; API methode roepen van AccessEnabler &lbrace;. `handleExternalURL:url ` Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet alleen door uw toepassing worden geïnterpreteerd als een signaal dat de verificatiestroom is voltooid en dat het veilig is om de `UIWebView/WKWebView or SFSafariViewController` -controller te sluiten. Als uw toepassing a `SFSafariViewController ` controlemechanisme moet gebruiken wordt de specifieke douane URL bepaald door `application's custom scheme` (b.v.: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke douane URL bepaald door de `ADOBEPASS_REDIRECT_URL` constante (d.w.z. `adobepass://ios.app`).
1. Zodra uw toepassing het `UIWebView/WKWebView or SFSafariViewController` controlemechanisme sluit en de 1&rbrace; API methode van AccessEnabler &lbrace;roept, wint AccessEnabler het authentificatietoken van de backendserver terug en informeert uw toepassing dat de authentificatiestroom volledig is. `handleExternalURL:url ` AccessEnabler roept [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 1 aan, die op succes wijst. Als er tijdens de uitvoering van deze stappen een fout optreedt, wordt de callback van [`setAuthenticationStatus()`](#setAuthNStatus) geactiveerd met een statuscode 0 die een verificatiefout en een bijbehorende foutcode aangeeft.


>[!WARNING]
>
> Tijdens de stappen waar AccessEnabler controle aan uw app opgeeft (d.w.z., wanneer het de dialoogvakje van de leveranciersselectie wordt getoond, of wanneer UIWebView/WKWebView of SFSafariViewController wordt getoond) heeft de gebruiker de kans om de authentificatiestroom te annuleren. In deze situaties is uw toepassing verantwoordelijk voor het informeren van de AccessEnabler over deze gebeurtenis en het aanroepen van de API-methode [`setSelectedProvider()`](#setSelProv) , waarbij null wordt doorgegeven als een parameter. Dit geeft AccessEnabler een kans om zijn interne staat op te schonen en de authentificatiestroom terug te stellen. Op tvOS kunt u dezelfde methode gebruiken om de opiniepeiling te annuleren.


### Logout-workflow {#logout}

Voor native clients wordt afmelding op dezelfde manier afgehandeld als het hierboven beschreven verificatieproces.

1. Uw toepassing stelt het logout werkschema met een vraag in werking aan API van AccessEnabler `logout() ` methode. De logout is het resultaat van een reeks HTTP-omleidingsbewerkingen omdat de gebruiker moet worden afgemeld bij zowel de Adobe Pass Authentication-servers als bij de MVPD-servers. Omdat deze stroom niet kan worden voltooid met een eenvoudige HTTP-aanvraag die is uitgegeven door de AccessEnabler-bibliotheek, moet een `UIWebView/WKWebView or SFSafariViewController` -controller worden geïnstantieerd om de HTTP-omleidingsbewerkingen te kunnen volgen.

1. Een patroon gelijkend op de authentificatiestroom wordt gebruikt. De iOS AccessEnabler activeert de callback `navigateToUrl:` of de `navigateToUrl:useSVC:` om een controller `UIWebView/WKWebView or SFSafariViewController` te maken en de URL te laden die in de parameter `url` van de callback wordt opgegeven. Dit is URL van het logout eindpunt op de achtergrondserver. Voor tvOS AccessEnabler wordt noch de callback `navigateToUrl:` , noch de callback `navigateToUrl:useSVC:` aangeroepen.

1. Aangezien het door verscheidene omleidingen gaat, moet uw toepassing de activiteit van het `UIWebView/WKWebView or SFSafariViewController ` controlemechanisme controleren en het moment ontdekken wanneer het een specifieke douane URL laadt. Deze specifieke aangepaste URL is in feite ongeldig en is niet bestemd voor de controller om deze daadwerkelijk te laden. Het moet door uw toepassing slechts als signaal worden geïnterpreteerd dat de logout stroom heeft voltooid en dat het veilig is om het controlemechanisme te sluiten. Wanneer het controlemechanisme deze specifieke douane URL laadt moet uw toepassing het controlemechanisme sluiten en de API methode van AccessEnabler `handleExternalURL:url ` roepen. In het geval dat uw toepassing a `SFSafariViewController ` controlemechanisme moet gebruiken wordt de specifieke douane URL bepaald door `application's custom scheme` (b.v. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), anders wordt deze specifieke douane URL bepaald door de ` ADOBEPASS_REDIRECT_URL  ` constante (d.w.z. `adobepass://ios.app`).

1. Uiteindelijk zal AccessEnabler [`setAuthenticationStatus()`](#setAuthNStatus) callback met een statuscode van 0 roepen, die op succes van de logout stroom wijst.

De logout-flow verschilt van de verificatiestroom omdat de gebruiker op geen enkele wijze met de `UIWebView/WKWebView or SFSafariViewController` -controller hoeft te communiceren. Daarom adviseert de Adobe dat u de controle onzichtbaar (d.w.z. verborgen) tijdens het logout proces maakt.

## Tokens {#tokens}

- [Definities en gebruik](#definitions)
- [Richtlijnen voor caching](#caching)
- [Persistentie](#persistence)
- [Indeling](#format)
- [Apparaatbinding](#device_binding)


### Definities en gebruik {#definitions}

De Adobe Pass Authentication-machtigingsoplossing draait om het genereren van specifieke gegevens (tokens) die door Adobe Pass Authentication worden gegenereerd wanneer de workflows voor verificatie en autorisatie met succes zijn voltooid. Deze tokens worden lokaal opgeslagen op het iOS-apparaat van de client.



Tokens hebben een beperkte levensduur; bij het verlopen moeten tokens opnieuw worden uitgegeven door het opnieuw opstarten van de workflows voor verificatie en/of autorisatie.



Er zijn drie typen tokens die worden uitgegeven tijdens de workflows voor machtigingen:

- **het teken van de Authentificatie:** het eindresultaat van het werkschema van de gebruikersauthentificatie zal een authentificatie GUID zijn die AccessEnabler kan gebruiken om vergunningsvragen op naam van de gebruiker te maken. Deze authentificatie GUID zal een bijbehorende tijd-aan-levende (TTL) waarde hebben die van de authentificatiesessie van de gebruiker zelf kan verschillend zijn. Een authentificatietoken zal worden geproduceerd door authentificatie GUID aan het apparaat te binden dat de authentificatieverzoeken in werking stelt.
- **teken van de Toestemming:** verleent toegang tot een specifiek beschermd middel dat door een unieke resourceID wordt geïdentificeerd. Het bestaat uit een door de vergunningverlenende partij afgegeven autorisatiesubsidie samen met de oorspronkelijke resourceID. Deze informatie is gebonden aan het apparaat dat het verzoek initieert.
- **Kortstondig media teken:** AccessEnabler verleent toegang tot de ontvangende toepassing voor een bepaald middel door een kort-levend media teken terug te keren. Dit token wordt gegenereerd op basis van het machtigingstoken dat eerder werd verworven voor die specifieke resource. Dit token is niet gebonden aan het apparaat en de bijbehorende levensduur is aanzienlijk korter (standaard: 5 minuten).

Bij geslaagde verificatie en autorisatie geeft Adobe Pass Authentication, Authentificatie en kortstondige mediatokens uit. Deze tokens zouden op het apparaat van de gebruiker moeten worden in het voorgeheugen ondergebracht en voor de duur van hun bijbehorende leven-spanwijdten worden gebruikt.



### Richtlijnen voor caching {#caching}

- Verificatietoken
- Token voor autorisatie
- Token voor kortstondige media


#### Verificatietoken

- **AccessEnabler 1.7:** Deze SDK introduceert een nieuwe methode van symbolische opslag, toelatend veelvoudige emmers programmer-MVPD, en daarom, veelvoudige authentificatietokens. Nu, wordt de zelfde opslaglay-out gebruikt voor zowel het &quot;Authentificatie per Vraag&quot;scenario als voor de normale authentificatiestroom. Het enige verschil tussen beide is in de manier de authentificatie wordt uitgevoerd: &quot;Authentificatie per Aanvrager&quot;bevat een nieuwe verbetering (Passieve Authentificatie) die het voor AccessEnabler mogelijk maakt om backchannel authentificatie uit te voeren, die op het bestaan van een authentificatietoken in de opslag (voor een verschillende Programmer) wordt gebaseerd. De gebruiker hoeft slechts eenmaal te verifiëren en deze sessie wordt gebruikt om verificatietokens te verkrijgen in aanvullende apps. Deze backchannel flow vindt plaats tijdens de [`setRequestor()`](#setReq) oproep en is meestal transparant voor de programmeur. **is er, echter, één belangrijk vereiste hier: De programmeur MOET setRequestor () van de belangrijkste draad UI roepen.**
- **AccessEnabler 1.6 en ouder:** De manier waarin de authentificatietokens in het voorgeheugen ondergebracht op het apparaat van de &quot;**Authentificatie per Vraagster&quot;** vlag verbonden aan huidige MVPD afhangt:

<!-- end list -->

1. Als de functie &#39;Verificatie per aanvrager&#39; is uitgeschakeld, wordt één verificatietoken lokaal opgeslagen in het algemene plakbord. Dit token wordt gedeeld tussen alle toepassingen die zijn geïntegreerd met de huidige MVPD.
1. Als de eigenschap &quot;Authentificatie per Aanvrager&quot;wordt toegelaten, dan zal een teken uitdrukkelijk met de Programmer worden geassocieerd die de authentificatiestroom uitvoerde (het teken zal niet in het globale plakbord, maar in een privé dossier worden opgeslagen dat slechts aan de toepassing van die Programmer zichtbaar is). Specifieker, Single Sign-On (SSO) tussen verschillende toepassingen zal worden onbruikbaar gemaakt; de gebruiker zal de authentificatiestroom uitdrukkelijk moeten uitvoeren wanneer het schakelen naar een nieuwe app (op voorwaarde dat Programmer van tweede app met huidige MVPD wordt geïntegreerd en dat geen authentificatietoken voor die Programmer in het lokale geheime voorgeheugen bestaat).



#### Token voor autorisatie

Op om het even welk bepaald ogenblik wordt slechts ÉÉN toestemmingstoken PER BRON caching door AccessEnabler. Er kunnen meerdere machtigingstokens in de cache zijn opgeslagen, maar deze zijn gekoppeld aan verschillende bronnen. Wanneer een nieuw toestemmingstoken wordt uitgegeven en oude reeds voor *het zelfde middel* bestaat, overschrijft het nieuwe teken de bestaande caching waarde.



#### Mediumtoken

De media-token voor korte tijd mag NIET in de cache worden opgeslagen. Het mediatoken moet van de server worden opgehaald telkens wanneer een autorisatie-API wordt aangeroepen, omdat dit beperkt is tot eenmalig gebruik.



### Persistentie {#persistence}

Tokens moeten blijvend zijn in opeenvolgende uitvoeringen van dezelfde toepassing. Dit betekent dat zodra de verificatie- en autorisatietokens zijn verkregen en de gebruiker de toepassing sluit, dezelfde tokens beschikbaar zijn voor de toepassing wanneer de gebruiker de toepassing opnieuw opent. Bovendien is het wenselijk dat deze tokens voor meerdere toepassingen blijvend zijn. Met andere woorden, nadat een gebruiker een toepassing gebruikt om zich aan te melden bij een specifieke identiteitsprovider (met succes authentificatie en toestemmingstkens), kunnen de zelfde tokens door een verschillende toepassing worden gebruikt, en de gebruiker wordt niet meer veroorzaakt voor geloofsbrieven wanneer het registreren via de zelfde identiteitsleverancier. Dit type naadloze verificatie-/autorisatieworkflow maakt van de Adobe Pass Authentication-oplossing een echte tv-omgeving
uitvoering.



## iOS

De iOS AccessEnabler bibliotheek werkt rond de kwesties van dwars-toepassingsgegevens die door de symbolische gegevens in een &quot;klembord-als&quot;gegevensstructuur op te slaan genoemd het *deegraad*. Dit systeem-vlakke gedeelde middel verstrekt de belangrijkste ingrediënten die de implementatie van de gewenste blijvende tokens gebruiksgeval toelaten:

- **Steun voor gestructureerde opslag** - de deegraad is niet enkel een eenvoudige, lineaire buffer-als geheugenstructuur. Het verstrekt een woordenboekachtig opslagmechanisme dat voor gegevens het indexeren op user-specified zeer belangrijke waarden toestaat.
- **Steun voor gegevenspersistentie gebruikend het onderliggende dossiersysteem** - de inhoud van de structuur van het deegbord kan als blijvend worden gemerkt, waarin het gegeven op het interne geheugen van het apparaat wordt bewaard.



Zodra een bepaald teken in het symbolische geheime voorgeheugen wordt geplaatst, zal zijn geldigheid op verschillende tijden door de bibliotheek AccessEnabler worden gecontroleerd.  Een geldig token wordt gedefinieerd als:

- De TTL van het token is niet verlopen.
- De uitgever van het token is opgenomen in de lijst van toegestane identiteitsproviders.



## tvOS

Aangezien het plakbord op tvOS niet beschikbaar is, gebruikt de bibliotheek tvOS AccessEnabler de NsUserDefaults als opslagoptie. Dit lost het probleem van het voortbestaan authentificatie en toestemmingstkens op maar de opgeslagen informatie kan niet onder verschillende toepassingen worden gedeeld.



**iOS 10 Veranderingen van het Plakbord -** wanneer het bevorderen van een vorige versie van iOS, zal het plakbord worden gewist. Dit betekent dat alle toepassingen opnieuw moeten worden geverifieerd.



**iOS 7 de Veranderingen van het Plakbord -** wegens veranderingen in hoe de plakborden op iOS 7 functioneren, zal er beperkte SSO tussen toepassingen zijn die op iOS 7 lopen. Toepassingen met dezelfde `<Bundle Seed ID>` (ook wel `<Team ID>` genoemd) delen tokens, wat betekent dat apps A1 en A2 van dezelfde programmeur X tokens delen, terwijl app A1 (programmeur X) en app A3 (programmeur Y) geen tokens delen.

- Bundlebron-id/team-id is hetzelfde tussen twee apps als deze worden gegenereerd door hetzelfde inrichtingsprofiel. Meer informatie vindt u op de volgende link:
  [&#x200B; http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Deze &quot;Cross SSO&quot;-beperking komt voor in iOS 7, ongeacht de gebruikte Adobe Pass Authentication SDK.

Lees deze technische notitie voor meer informatie over het configureren van SSO in iOS 7 en hoger (de technische notitie is van toepassing op Access Enabler v1.8 en hoger): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Token Storage (AccessEnabler 1.7)

Beginnend met AccessEnabler 1.7, kan de symbolische opslag veelvoudige combinaties programmmer-MVPD steunen, die op een op meerdere niveaus genestelde kaartstructuur vertrouwen die veelvoudige authentificatietokens kan houden. Deze nieuwe opslag heeft op geen enkele wijze invloed op de openbare API van AccessEnabler en vereist geen wijzigingen aan de kant van de programmeur. Hier is een voorbeeld dat
illustreert deze nieuwe functionaliteit:

1. Open App1 (ontwikkeld door Programmer1).
1. Verifieer met MVPD1 (die met Programmer1) wordt geïntegreerd.
1. Onderbreek of sluit de huidige toepassing en open App2 (ontwikkeld door Programmer2).
1. Laten we aannemen dat Programmer2 niet is geïntegreerd met MVPD2. Daarom wordt de gebruiker NIET geverifieerd in App2.
1. Verifieer met MVPD2 (die met Programmer2) in App2 wordt geïntegreerd.
1. De schakelaar terug naar App1; de gebruiker zal nog met Programmer1 voor authentiek worden verklaard.

In oudere versies van AccessEnabler, zou Stap 6 de gebruiker als niet-voor authentiek verklaard teruggeven, omdat de symbolische opslag vroeger slechts één authentificatietoken steunde.



Als u zich afmeldt bij een programmeur-/MVPD-sessie, wordt de volledige onderliggende opslag gewist, inclusief alle andere verificatie-tokens van de programmeur/MVPD op het apparaat. Anderzijds zal het annuleren van de authentificatiestroom (het aanhalen van [`setSelectedProvider(null)`](#setSelProv)) NIET de onderliggende opslag ontruimen, maar het zal slechts de huidige de authentificatiepoging van Programmer/MVPD beïnvloeden (door MVPD voor de huidige Programmer te wissen).



### Token Importer (AccessEnabler 1.7)

Een andere opslag-verwante eigenschap die in AccessEnabler 1.7 inbegrepen is maakt het mogelijk om authentificatietokens van oudere opslaggebieden in te voeren. Deze &quot;Symbolische Importer&quot;helpt om verenigbaarheid tussen opeenvolgende versies te bereiken AccessEnabler, die de SSO staat handhaven zelfs wanneer de opslagversie wordt bevorderd. De importer wordt uitgevoerd tijdens de [`setRequestor()`](#setReq) flow en wordt uitgevoerd in de volgende twee scenario&#39;s (ervan uitgaande dat er geen geldig verificatietoken voor de huidige programmeur aanwezig is in de huidige opslag):

- De eerste installatie van een 1.7-app die is ontwikkeld door een specifieke programmeur
- Upgradeweg aan een toekomstige AccessEnabler die een nieuwe opslag gebruikt

De importbewerking is transparant voor de programmeur en vereist geen codewijziging in de clienttoepassing.



### Token Sanitizer (AccessEnabler 1.7.5)

Van AccessEnabler 1.7.5 door:sturen, loopt deze dienst op [`setRequestor()`](#setReq)`. ` het werd ontwikkeld als resultaat van iOS 7 schakelaar van het adres van WiFi MAC aan IDFA voor het volgen. De sanitizer zorgt ervoor dat de huidige opslag alleen geldige verificatietokens bevat (geldig in termen van de apparaat-id, eerder berekend met het MAC-adres, vóór iOS7). Met de Token Sanitizer worden alle ongeldige tokens verwijderd.



De Symbolische dienst Sanitizer is het nuttigst wanneer een toepassing AccessEnabler 1.7.5 op iOS 6 wordt gebruikt, en dan de gebruiker werkt aan iOS 7 bij. Als dit gebeurt, zijn alle verificatietokens die zijn verkregen op iOS 6 nu ongeldig (omdat het algoritme voor de apparaat-id is gewijzigd van het gebruik van het MAC-adres in de IDFA). De sanitizer zal alle ongeldige tokens ontruimen, en de gebruiker zal dan unauthenticated zijn.



Zonder de Symbolische Sanitizer die ongeldige tokens verwijdert, zou AccessEnabler geen vergunningen wegens de aanwezigheid van ongeldige authentificatietokens verkrijgen. Dit zou voor de eindgebruiker lastig blijken om te zuiveren, aangezien de berichten van de vergunningsfout cryptisch en niet overdreven nuttig kunnen zijn in het bepalen van wat het probleem veroorzaakte.



### Indeling {#format}

- [AuthN Token](#authn_token)
- [AuthZ Token](#authz_token)
- [Token voor korte media](#short_token)
- [Apparaatbinding](#device_binding)


Merk op dat het formaat van de tokens AuthN en AuthZ hier voor achtergrondinformatie slechts inbegrepen zijn. De structuur van deze tokens kan op elk gewenst moment worden gewijzigd door Adobe Pass-verificatie. Programmeurs hoeven niet de nauwkeurige structuur van de tokens AuthN en AuthZ te kennen om hun apps uit te voeren, aangezien de tokens AuthN en AuthZ niet op het lokale apparaat worden blootgesteld. Het Korte teken van Media *wordt* blootgesteld aan de toepassing van de Programmer.



#### Verificatietoken {#authn_token}

Hieronder ziet u de indeling van het verificatietoken:

```
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

```
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


#### Token voor korte media {#short_token}

Hieronder ziet u de indeling van de token voor korte media. Dit teken wordt blootgesteld aan de toepassing van de Programmer. Het wordt doorgegeven aan de toepassing van de programmeur na een succesvol machtigingsproces:

```
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


### Apparaatbinding {#device_binding}

In de bovenstaande XML-lijsten noteert u het label `simpleTokenFingerprint` . Het doel van deze tag is om de individualisatiegegevens van de native apparaat-id op te slaan. De bibliotheek AccessEnabler kan dergelijke individualisatieinformatie verkrijgen en het ter beschikking stellen van de diensten van de Authentificatie van Adobe Pass tijdens de machtigingsvraag. De dienst zal deze informatie gebruiken en zal het in de daadwerkelijke tokens inbedden, zo effectief binden de tokens aan een specifiek apparaat. Het uiteindelijke doel hiervan is om de tokens niet-overdraagbaar te maken op verschillende apparaten.



Aangezien dit duidelijk een veiligheids verwante eigenschap is, is deze informatie inherent &quot;gevoelig&quot;uit veiligheidsoogpunt. Daarom moet deze informatie worden beschermd tegen knoeien en afluisteren. Het afluisterprobleem wordt opgelost door de verificatie-/autorisatieverzoeken via het HTTPS-protocol te verzenden. De manipulatiebeveiliging wordt afgehandeld door de apparaatidentificatiegegevens digitaal te ondertekenen. De AccessEnabler-bibliotheek berekent een apparaat-id aan de hand van de informatie die door het apparaat wordt verstrekt en verzendt de apparaat-id vervolgens &quot;in the clear&quot; naar de Adobe Pass-verificatieservers als een aanvraagparameter. De servers van de Authentificatie van Adobe Pass ondertekenen digitaal apparatenidentiteitskaart met de privé sleutel van de Adobe en voegen het aan het authentificatietoken toe dat aan AccessEnabler is teruggekeerd. Aldus wordt apparaat-id gebonden aan het verificatietoken. Tijdens de vergunningsstroom, verzendt AccessEnabler opnieuw apparatenidentiteitskaart in duidelijk, samen met het authentificatietoken. Als het validatieproces mislukt, zullen de workflows voor verificatie en autorisatie automatisch mislukken. De Adobe Pass-verificatieservers passen de persoonlijke sleutel toe op de apparaat-id en vergelijken deze met de waarde in het verificatietoken. Als ze niet overeenkomen, mislukt die machtigingsstroom.



**Nota op ApparaatBinding in AccessEnabler 1.7.5:** Beginnend met AccessEnabler 1.7.5, is er een verandering in hoe apparatenidentiteitskaart wordt berekend om individualiseringsinformatie voor een apparaat van iOS te verstrekken. Deze wijziging is het gevolg van een wijziging in iOS 7: vanaf iOS 7 biedt Apple niet langer het WiFi MAC-adres als traceringsoptie, ten gunste van de ID (Identifier for Advertisers) ervan. Aangezien de informatie over individualisatie voor een toepassing die op iOS 7 wordt uitgevoerd op IDFA is gebaseerd, en die informatie in de machtigingsstroomtokens wordt ingebed, betekent dit dat er een aantal verschillende mogelijke effecten op gebruikerservaring zijn die uit deze wijziging voortvloeien. De verschillende mogelijke effecten zijn gebaseerd op welke versie van iOS de gebruiker bijwerkt in combinatie met welke versie van AccessEnabler de programmeur bijwerkt. Voor details over deze verandering, zie de nota&#39;s van de Versie inbegrepen met AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
