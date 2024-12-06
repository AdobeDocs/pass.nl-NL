---
title: Overzicht voor programmeurs
description: Overzicht voor programmeurs
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# Overzicht voor programmeurs {#programmers-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#introduction}

Dit overzicht is bedoeld voor de inhoudprovider (Programmer) die van plan is Adobe® Pass in hun website of toepassing te integreren. Zie Verwante informatie hieronder voor aanvullende documentatie, waaronder handleidingen voor Kickstart en integratie.

Vandaag kunnen uw kijkers op Internet op om het even welk ogenblik krijgen, overal, en om toegang tot beschermde inhoud direct van u, de Programmer verzoeken. Misschien willen ze een eenmalig evenement bekijken, of ze willen misschien kijkrechten voor een hele serie televisieprogramma&#39;s die je in beeld brengt.

Maar voordat u toegang tot uw beveiligde inhoud toestaat, moet u bepalen of een klant het recht heeft die inhoud te bekijken. Hebben zij een abonnement met een Multichannel Video Programming Distributor (MVPD)? Zo ja, omvat dat abonnement uw programmering?

Het bepalen van de machtiging van een kijker is niet altijd eenvoudig voor een programmeur. De MVPD&#39;s hebben de identificatiegegevens en toegangsrechten voor hun klanten.  Voeg het feit toe dat viewers die toegang proberen te krijgen tot uw beveiligde inhoud, zich abonneren op verschillende MVPD&#39;s, die allemaal verschillende systemen hebben. Het is dan ook eenvoudig te zien dat het vaststellen van het recht van de viewer op beveiligde inhoud snel ingewikkeld en technisch lastig kan worden:

![](../assets/user-ent-by-progr.png)

*Cijfer: De machtiging van de gebruiker die direct door programmeur* wordt bepaald

Adobe Pass Authentication for TV Anywhere bemiddelt deze machtigingstransacties tussen Programmers en MVPD&#39;s veilig. Met Adobe Pass Authentication kunnen programmeurs eenvoudig, snel en veilig beveiligde inhoud leveren aan geldige klanten:

![](../assets/user-ent-mediatedby-authn.png)

*Cijfer: De Entitlement van de gebruiker door de Authentificatie van Adobe Pass wordt gemedieerd*

Adobe Pass Authentication fungeert als proxy in uitwisselingen met deelnemende MVPD&#39;s, zodat u uw viewers een consistente interface voor meerdere sites kunt bieden. Met Adobe Pass Authentication kunt u uw viewers ook een Single Sign-On (SSO)-verificatie en -verificatie bieden. De authentificatie en de vergunning worden gevolgd voor alle deelnemende diensten, zodat een abonnee niet aan login opnieuw na hun eerste authentificatie op hun eigen systeem hoeft te hoeven.

* **Authentificatie** - het proces om met MVPD te bevestigen dat een bepaalde gebruiker een bekende klant is.
* **Vergunning** - het proces om met MVPD te bevestigen dat een voor authentiek verklaarde gebruiker een geldig abonnement op een gespecificeerd middel heeft.

### Werken met Adobe Pass-verificatie {#HowItWorks}

De inhoudsweergavetoepassing van de programmeur communiceert met Adobe Pass-verificatie via de client component Access Enabler of de RESTful-webservices van de client-API (voor apparaten die niet geschikt zijn voor het web, zoals intelligente tv&#39;s, spelconsoles, set-top boxes, enz.). De functie Toegangsbeheer wordt uitgevoerd op het systeem van de gebruiker, waar alle workflows met bevoegdheden worden vereenvoudigd.  De component Access Enabler wordt gedownload van de hostsite op de Adobe wanneer klanten toegang krijgen tot uw site en beveiligde inhoud aanvragen.  Adobe Pass Authentication-servers hosten de RESTful-webservices die worden gebruikt in de oplossing zonder client.

Adobe Pass Authentication verwerkt de werkelijke workflows voor machtigingen en biedt primitieven die u gebruikt:

* Stel uw identiteit in. (De programmeur is de &quot;aanvrager&quot; in de machtigingsstroom voor Adobe Pass-verificatie.)
* Verifieer een gebruiker met bepaalde MVPD.  (MVPD is de &quot;identiteitsprovider&quot;of &quot;IdP&quot;in de de machtigingsstroom van de Authentificatie van Adobe Pass.)
* Autoriseer een gebruiker met MVPD voor een bepaalde middel.
* Meld u af bij de gebruiker.

De programmeur is verantwoordelijk voor hun webpagina of spelertoepassing op het hoogste niveau die het volgende doet:

* Implementeert de gebruikersinterface
* Werkt met de API-webservices zonder toegang of zonder client

Het doel van de Authentificatie van Adobe Pass is een eenvoudige, modulaire manier voor zowel Programmeurs als MVPDs tot stand te brengen om machtigingscontrole te behandelen.

## Tokens begrijpen {#understanding-tokens}

De Adobe Pass Authentication-machtigingsoplossing richt zich op het genereren van specifieke gegevens die worden gemaakt wanneer de workflows voor verificatie en autorisatie met succes zijn voltooid. Deze gegevens worden tokens genoemd. Tokens hebben een beperkte levensduur; wanneer ze verlopen, moeten tokens opnieuw worden uitgegeven door het opnieuw opstarten van de verificatie- en autorisatieworkflows.

Raadpleeg de volgende secties voor meer informatie over tokens:

* [Typen tokens](#token-types)
* [Tokenopslag](#token-storage)
* [Tokenbeveiliging](#token-security)
* [Token delen](#token-sharing)

### Typen tokens {#token-types}

Tijdens de workflows voor verificatie en autorisatie worden drie typen tokens uitgegeven. De AuthN- en AuthZ-tokens zijn &#39;langlevend&#39; en zorgen voor continuïteit in de kijkervaring van de gebruiker. De token Media Token is een token van korte duur dat ondersteuning biedt voor de beste praktijken van de branche om fraude te voorkomen door het rippen van streams. Programmeurs geven de time-to-live (TTL) waarden op voor elk type token op basis van overeenkomsten met MVPD&#39;s. De programmeurs beslissen over een waarde van TTL die het best uw zaken en uw klanten dient.

* **Symbolisch AuthN** (&quot;Lange-levende&quot;): Op succesvolle authentificatie, leidt de Authentificatie van Adobe Pass tot een teken AuthN verbonden aan zowel het verzoekende apparaat als globaal uniek herkenningsteken (GUID).
   * Adobe Pass Authentication verzendt het token AuthN naar Access Enabler, die het veilig in de cache plaatst op het systeem van de client.  Het AuthN-token is er en is nog niet verlopen, maar is beschikbaar voor alle toepassingen die Adobe Pass-verificatie gebruiken. De Toegangsmanager gebruikt het teken AuthN voor de vergunningsstroom.
   * Op een gegeven moment wordt slechts één AuthN-token in de cache geplaatst. Wanneer een nieuw token AuthN wordt uitgegeven en een oud token al bestaat, overschrijft Adobe Pass Authentication het token in de cache.
* **Token AuthZ** (&quot;Lang-levend&quot;): Op succesvolle vergunning, leidt de Authentificatie van Adobe Pass tot een teken AuthZ verbonden aan het verzoekende apparaat en een specifiek beschermd middel.  De beschermde bron wordt geïdentificeerd door een unieke bron-id.
   * De Authentificatie van Adobe Pass verzendt het teken AuthZ naar Toegangsmanager, die het veilig op het lokale systeem in het voorgeheugen onderbrengt. De Toegangsmanager gebruikt dan het teken AuthZ om het symbolische teken van Media tot stand te brengen van korte duur dat voor daadwerkelijke het bekijken toegang wordt gebruikt.
   * Op elk moment wordt slechts één AuthZ-token per resource in de cache opgeslagen. Adobe Pass-verificatie kan meerdere AuthZ-tokens in cache plaatsen, mits deze aan verschillende bronnen zijn gekoppeld. Wanneer een nieuw token AuthZ wordt uitgegeven en er al een oud token bestaat voor dezelfde resource, overschrijft Adobe Pass Authentication het token in de cache.
* **Symbolisch van Media** (&quot;Kort-Levend&quot;): De Toegangsmanager gebruikt het teken AuthZ om een kort-levend (gebrek: 7 minuten) teken van Media te produceren. Dit is het punt waarop een succesvol spelverzoek wordt geacht te hebben plaatsgevonden.
   * Voordat toegang tot de beveiligde bron wordt verleend, moet uw mediaserver een Adobe Pass-verificatiecomponent, de Media Token Verifier, gebruiken om het Media-token te valideren.
   * Omdat het token Media niet aan het apparaat is gebonden, is de levensduur aanzienlijk korter (standaard: 7 minuten) dan die van de langlevende AuthN- en AuthZ-tokens.
   * Het media-token met een korte levensduur is beperkt tot eenmalig gebruik en wordt nooit in de cache opgeslagen. Deze wordt opgehaald van de Adobe Pass-verificatieserver telkens wanneer een autorisatie-API wordt aangeroepen.

### Tokenopslag {#token-storage}

Toegangsbeheer slaat langlevende tokens (AuthN en AuthZ) in plaatsen specifiek voor zijn milieu op:

* **Flash 10.1** (of hoger): De langlevende tekenen worden opgeslagen als Lokale Gedeelde Voorwerpen.
* **HTML5**: De langlevende tekenen worden veilig gehouden in de lokale opslag van browser van HTML5.
* **iOS**: De langlevende tekenen worden opgeslagen op een blijvend plakbord, waar zij door andere de cliënttoepassingen van de Authentificatie van Adobe Pass kunnen worden betreden.
* **Android**: De langlevende tekenen worden opgeslagen in een gedeeld gegevensbestanddossier, waar zij door andere de cliënttoepassingen van de Authentificatie van Adobe Pass kunnen worden betreden.
* **Clientless API apparaten**: De tokens worden opgeslagen op de servers van de Authentificatie van Adobe Pass.

### Tokenbeveiliging {#token-security}

De Adobe Pass-verificatieserver ondertekent digitaal alle langlevende tokens met behulp van de apparaat-id (die is afgeleid van de hardwarekenmerken van het apparaat). De digitale handtekening verschilt in de manier waarop deze wordt gegenereerd, beveiligd en gevalideerd, afhankelijk van de omgeving:

* **Flash 10.1** (of hoger) - Apparaatidentiteitskaart baseert zich op de apparatenreferentie, een uniek certificaat dat van de server van de Individualisatie van de Adobe wordt uitgegeven. Deze beveiliging is gelijk aan FAXS DRM-technologie. Deze server-zijbevestiging vergelijkt unieke apparatenidentiteitskaart in het teken met de apparatenreferentie (veilig die van Flash Player aan de Authentificatie van Adobe Pass wordt meegedeeld). De apparaatreferentie identificeert ook de versie van de FAXS-client en de versie van de Flash Player (of AIR) waaraan deze is uitgegeven. De apparaatbinding is sterker dan bij HTML5, zodat de time-to-live (TTL) voor tokens doorgaans langer is bij Flash.
* **HTML5** - het apparaat wordt geïndividualiseerd op de cliëntkant. Het gebruikt kenmerken beschikbaar door JavaScript om pseudo-apparaat identiteitskaart te produceren die browser en OS versies, een IP adres, en een browser koekje GUID (globally uniek herkenningsteken) omvat. Deze token-apparaat-id wordt vergeleken met de huidige pseudo-apparaat-id voor het apparaat. Omdat het IP adres tijdens normaal gebruik, zelfs in de zelfde zitting kan veranderen, slaat de Authentificatie van Adobe Pass HTML5 tokens in twee plaatsen op: localStorage en sessionStorage. Als IP verandert en het sessionStorage teken anders nog geldig is, wordt de zitting gehandhaafd. Met HTML5, is de apparatenband niet zo sterk, zodat is TTL voor tekenen typisch korter dan voor Flash.
* **Eigen Clients** (iOS en Android) - de langlevende tokens houden inheemse info van de apparatenindividualisatie van identiteitskaart en zijn daarom gebonden aan het het vragen apparaat. De verificatie- en autorisatieverzoeken worden verzonden via HTTPS en de informatie over de apparaat-id wordt digitaal ondertekend door de bibliotheek Access Enabler voordat deze naar de back-endservers wordt verzonden. Aan de serverzijde worden de gegevens van de apparaat-id gevalideerd tegen de bijbehorende digitale handtekening.
* **Clientless API Clients** - De Clientless API oplossing heeft zijn reeks veiligheidsprotocollen die digitaal het ondertekenen van alle API vraag impliceren. Tokens die tijdens de machtigingsstromen worden gegenereerd, worden veilig opgeslagen op de Adobe Pass-verificatieservers.

Adobe Pass Authentication valideert elk token van lange duur om te controleren of het apparaat dat toegang heeft tot de inhoud hetzelfde is als het apparaat dat het token heeft uitgegeven. Voor alle tokens, zorgt een cliënt-zijbevestiging ervoor dat de digitale handtekening intact is, en dat de integriteit van het teken wordt bewaard. Wanneer de validatie van de apparaat-id mislukt, wordt de verificatiesessie ongeldig en wordt de gebruiker gevraagd zich opnieuw aan te melden, waardoor de tokens opnieuw worden ingesteld.

### Token delen {#token-sharing}

Toepassingen op verschillende platforms delen geen tokens. Hiervoor zijn een aantal redenen, waaronder de volgende:

* Zoals die in [ Symbolische Opslag ](#token-storage) wordt beschreven, varieert de methode om tokens op te slaan tussen platforms (bijvoorbeeld, Lokale Gedeelde Voorwerpen voor Flash, WebStorage voor JavaScript).
* De mate van tokenbeveiliging verschilt per platform. Bijvoorbeeld, zijn de tokens van de Flash sterk gebonden aan een apparaat gebruikend FAXS. Tokens in een pure JavaScript-omgeving hebben niet hetzelfde niveau van DRM-ondersteuning als in Flash.  Door JS-tokens te delen met Flash-toepassingen, wordt de kans op minder veilige tokens die gebruikmaken van een veiliger omgeving, groter.

## Levenscyclus van programmeerintegratie {#prog-integ-lifecycle}

![](../assets/progr-flow-int-lifecycle.png)

*Figuur: Het integreren Authentificatie met de website en de toepassing van de programmeur*


## Logische stromen {#logical-flows}

### Entitlement FlowChart {#chart}

In het volgende stroomdiagram wordt het algemene proces beschreven voor het bevestigen van machtigingen (met behulp van de Adobe Pass Authentication Access-clientcomponent):

![](../assets/ent-flowchart.png)

*Cijfer: Proces om betiteling* te bevestigen

### Verificatiestappen {#authn-steps}

In de volgende stappen wordt een voorbeeld gegeven van de verificatiestroom van Adobe Pass.  Dit is het deel van het machtigingsproces waarin een Programmer bepaalt als de gebruiker een geldige klant van MVPD is.  In dit scenario, is de gebruiker een geldige abonnee aan MVPD.  De gebruiker probeert beveiligde inhoud weer te geven met behulp van de toepassing Flash van de programmeur:

1. De gebruiker bladert naar de Web-pagina van de Programmer, die de toepassing van de Flash van de Programmer en de componenten van de Toegang van de Authentificatie van Adobe Pass op de machine van de gebruiker laadt. De toepassing van de Flash gebruikt Toegang Enabler om het herkennen van de programmeur met de Authentificatie van Adobe Pass te plaatsen, en de Authentificatie van Adobe Pass stelt de Toegangsmanager met configuratie en staatsgegevens voor die programmeur (de &quot;aanvrager&quot;) voor. De Toegangsfunctie moet deze gegevens van de server ontvangen voordat andere API-aanroepen kunnen worden uitgevoerd. Technische nota: De programmeur plaatst zijn identiteit met de methode van Enabler van de Toegang `setRequestor()`; voor details, zie de [ Gids van de Integratie van de Programmer ](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).
1. Wanneer de gebruiker probeert om de beschermde inhoud van de Programmer te bekijken, stelt de toepassing van de Programmer de gebruiker met een lijst van MVPDs voor, waarvan de gebruiker een leverancier selecteert.
1. De gebruiker wordt opnieuw gericht aan een server van de Authentificatie van Adobe Pass, waar een gecodeerd [ SAML ](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) verzoek voor user-selected MVPD wordt gecreeerd. Dit verzoek wordt verzonden als authentificatieverzoek namens de programmeur aan MVPD. Afhankelijk van het systeem van MVPD, wordt browser van de gebruiker dan of opnieuw gericht aan de plaats van MVPD aan login, of een login iFrame wordt gecreeerd in app van de Programmer.
1. In beide gevallen (omleiding of iFrame) accepteert de MVPD de aanvraag en geeft de aanmeldingspagina weer.
1. De gebruiker login met MVPD, MVPD bevestigt de status van de gebruiker als betalende klant, en dan leidt MVPD zijn eigen zitting van HTTP.
1. Wanneer de gebruiker wordt bevestigd, leidt MVPD tot een reactie (SAML &amp; gecodeerd), die MVPD terug naar de Authentificatie van Adobe Pass verzendt.
1. De Authentificatie van Adobe Pass ontvangt de reactie MVPD, ziet dat er een open zitting van HTTP van de Authentificatie van Adobe Pass is, bevestigt de reactie van SAML van MVPD, en richt terug naar de plaats van de Programmer.
1. De site van de programmeur wordt opnieuw geladen, de Access Enabler wordt opnieuw geladen en de programmeur roept setRequestor() opnieuw aan.  De tweede vraag aan setRequestor () is noodzakelijk omdat de huidige configuratie is veranderd-er nu een vlag aanwezig is die toelaat van de Toegang meedeelt dat een teken AuthN op de server wacht te worden geproduceerd.
1. Toegangsbeheer ziet dat er een verificatie in behandeling is en vraagt het token aan bij de Adobe Pass-verificatieserver. Het token wordt opgehaald van de server door de DRM-mogelijkheden van de Flash Player aan te roepen.
1. Het teken AuthN wordt opgeslagen in het geheime voorgeheugen van de Flash Player LSO van de Programmer; de authentificatie is nu volledig, en de zitting wordt vernietigd op de server van de Authentificatie van Adobe Pass.

### Autorisatiestappen {#authz-steps}

De volgende stappen blijven op van de [ Stappen van de Authentificatie ](#authn-steps):

1. Wanneer de gebruiker probeert om tot de beschermde inhoud van de Programmer toegang te hebben, controleert de toepassing van de Programmer eerst op een teken AuthN op de lokale machine of het apparaat van de gebruiker.  Als dat teken daar niet is, dan worden de [ Stappen van de Authentificatie ](#authn-steps) hierboven gevolgd.  Als het token AuthN aanwezig is, gaat de machtigingsstroom verder met de toepassing van de programmeur die een aanroep van Access Enabler initieert met een verzoek om de weergaverechten van de gebruiker voor een specifiek item van beveiligde inhoud op te halen.
1. Het specifieke item van de beveiligde inhoud wordt vertegenwoordigd door een &quot;resource identifier&quot;.  Dit kan een eenvoudige tekenreeks of een complexere structuur zijn, maar in elk geval wordt de aard van de resource identifier vooraf overeengekomen tussen de programmeur en de MVPD.  De toepassing van de Programmer gaat het middelherkenningsteken tot Toegangstoegelaten over.  De Toegangsfunctie controleert op een AuthZ-token op de lokale computer of het lokale apparaat van de gebruiker.  Als het token AuthZ niet aanwezig is, geeft de Access Enabler het verzoek door aan de backend Adobe Pass Authentication-server.
1. De server van de Authentificatie van Adobe Pass communiceert met het MVPDs vergunningseindpunt gebruikend gestandaardiseerde protocollen.  Als de reactie van MVPD erop wijst dat de gebruiker de beschermde inhoud mag bekijken, leidt de server van de Authentificatie van Adobe Pass tot een teken AuthZ en geeft het terug naar Enabler van de Toegang, die het teken AuthZ op de machine van de gebruiker opslaat.
1. Met een token AuthZ dat op de computer of het apparaat van de gebruiker is opgeslagen, roept de toepassing van de programmeur de Access Enabler op om een token van de Adobe Pass-verificatieserver te verkrijgen en verstrekt deze token aan de toepassing van de programmeur.
1. Tot slot gebruikt de toepassing van de Programmeur de component van de Verificator van het Boek van Media om te bevestigen dat de juiste gebruiker de juiste inhoud bekijkt, en met het Symbolische teken op zijn plaats, wordt de gebruiker toegestaan om de beschermde inhoud te bekijken.


## Registratie en initialisatie {#reg-and-init}

De eerste stap bij het werken met de Authentificatie van Adobe Pass is bij Adobe, of met een de Authentificatie-Erkende partner van Adobe Pass te registreren.

Wanneer u zich registreert, geeft u een lijst op met de domeinen vanwaar u met Adobe Pass-verificatie gaat communiceren. De Turner Broadcasting System-domeinen bevatten bijvoorbeeld tbs.com en tnt.tv als met Adobe Pass Authentication-geregistreerde domeinen. Elk van deze inhoudsplaatsen wordt toegang tot de Authentificatie van Adobe Pass verleend, en zijn eigen identiteitskaart van de Aanvrager toegewezen (bijvoorbeeld, &quot;TBS&quot;, en &quot;TNT&quot;). Als u besluit om extra plaatsen toe te voegen, moet u Adobe van de extra domeinnamen op de hoogte brengen om hen te krijgen op de lijst van toegestane domeinen worden geplaatst en extra Vraag IDs worden gegeven.

>[!WARNING]
>
>Terwijl u uw integratie test, zorg ervoor dat uw testserver op een geregistreerd domein is dat u in productie van plan bent te gebruiken. Verzoeken, zelfs testverzoeken, die afkomstig zijn uit niet-gewhitelisteerde domeinen worden genegeerd. Als u bijvoorbeeld hebt gevraagd domain.com te gebruiken in productie, zorgt u ervoor dat u de testintegratie implementeert onder domain.com, test.domain.com en staging.domain.com.
>
>Aanvragen die een gebruikersnaam en/of wachtwoord in de URL bevatten, worden genegeerd, zelfs als domeinen worden gewhitelist. Voorbeeld: `//username@registered-domain/`

De aanvrager-id identificeert de client van de programmeur op unieke wijze in alle communicatie met de Adobe Pass Authentication&#39;s Access Enabler-clientcomponent. Alle statische gegevens voor Adobe Pass-verificatie die aan de programmeur zijn gekoppeld, worden aan deze id vastgezet.

>[!TIP]
>
>Naast uw aanvrager-id ontvangt u bij het registreren ook functionele URL&#39;s voor de client component Access Enabler en de token-verificateur van Media.

## Integratiestappen {#integration-steps}

>[!TIP]
>
>Als u het Open Source Media Framework van de Adobe (&quot;OSMF&quot;) voor uw media spelerontwikkeling gebruikt, is de snelste manier om de Authentificatie van Adobe Pass te gebruiken de Insteekmodule OSMF *(Vervangen)* in de code van uw speler te integreren.
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [Instellen van aanvrager](#requestor)
1. [Verificatie en autorisatie afhandelen](#authn-authz)
1. [Ondersteuning voor Single Logout](#ssl)

### 1. Instellen van aanvrager {#requestor}

#### 1 bis. Registreren bij Adobe

In de eerste stap moet u zich registreren bij Adobe of bij een geautoriseerde partner voor Adobe Pass-verificatie.  Wanneer u registreert wordt u uitgegeven één of meerdere globaal unieke herkenningstekens (GUIDs). Elke GUID u wordt uitgegeven wordt geassocieerd met een domein waarvan de toegang aan de Authentificatie van Adobe Pass wordt toegestaan. U gaat een GUID (genoemd verzoekeridentiteitskaart) voor het het vragen domein over om uw identiteit voor elke zitting te registreren waarin u met Toegankelijkheid in wisselwerking staat. Voor details, zie [ Registratie en Initialisatie ](#reg-and-init) in de Gids van de Integratie van de Programmer.

#### 1 ter. De aanvankelijke Integratie van Toegelaten van de Toegang

De volgende stap bestaat uit het integreren van Access Enabler in uw bestaande mediaspeler-app of -webpagina:

* U kunt de versie van de Flash, `AccessEnabler.swf` , insluiten in een op Flash gebaseerde videospeler, of u kunt de versie direct insluiten in de HTML van uw webpagina. U kunt met de SWF van Toegangsbeheer in of ActionScript of JavaScript communiceren. De basis-API is ActionScript, maar als u liever met JavaScript werkt, is een volledige omvattende bibliotheek beschikbaar voor insluiting op uw pagina&#39;s.
* Voor omgevingen die geen Flash zijn, kunt u het volgende doen:
   * Gebruik de HTML5/JavaScript-versie, AccessEnabler.js, en communiceer ermee via de JavaScript API
   * Gebruik de AIR Native Extension for Adobe Pass Authentication om native code te combineren met ingebouwde ActionScript-klassen
   * Gebruik een van de native clientversies van de Access Enabler-bibliotheek (iOS of Android)

### 2. Verwerking van verificatie en autorisatie {#authn-authz}

#### 2 bis. Communiceren met de Access Enabler

De communicatie tussen de Access Enabler en uw webpagina of speler-app is asynchroon. Uw toepassing roept de methodes van Enabler van de Toegang, en Toegangsactivering antwoordt via callbacks die u met de bibliotheek van Inschakelen van de Toegang registreert.

* Wanneer uw toepassing een vergunningsverzoek indient, stelt de Toegangsmanager automatisch een authentificatieverzoek in werking, als een geldig teken AuthN niet reeds op het lokale systeem is. Wanneer de authentificatie slaagt, wordt het token van AuthN van de gebruiker lokaal opgeslagen, zodat zij zich niet opnieuw hoeven aan te melden. Als zij met succes door de Authentificatie van Adobe Pass in een andere context (bijvoorbeeld, door de MVPD website of met een verschillende programmeur) voor authentiek zijn verklaard, heeft Toegangsbeheer toegang tot het lokale teken AuthN en een extra authentificatie is niet nodig.
* Wanneer een gebruiker toegang probeert te krijgen tot uw beveiligde inhoud, stuurt u een verzoek om toestemming naar de Access Enabler. Nadat de verificatie is geverifieerd (of is gestart), neemt de Access Enabler contact op met de MVPD (via de Adobe Pass-verificatieserver) om te bepalen of de klant het recht heeft de beveiligde inhoud te bekijken. Uw toepassing hoeft het verzoek alleen naar Access Enabler te verzenden en vervolgens de reactie (geslaagd van de autorisatie of mislukt) af te handelen. Als de vergunning slaagt, wordt een teken AuthZ opgeslagen op het cliëntsysteem.  Tot slot ontvangt uw toepassing een token voor kortstondige media voor gebruik in uw eigen autorisatieprocedure.

>[!NOTE]
>
>* De authentificatie komt als uitwisseling SAML voor, tussen de Authentificatie van Adobe Pass als Dienstverlener (SP) en MVPD als Identiteitsprovider (IdP).
>
>* De vergunning gebruikt een achter-kanaal (server-aan-server) Web-dienst uitwisseling tussen de Authentificatie van Adobe Pass (SP) en MVPD (IdP).


#### 2 ter. Een machtigingsgebruikersinterface bieden {#entitlement-ui}

U verstrekt uw eigen UI voor gebruikerstoegang tot uw inhoud. Sommige elementen, zoals het daadwerkelijke login proces, worden verstrekt door MVPD, en sommige elementen zijn naar keuze beschikbaar als deel van de Authentificatie van Adobe Pass. Voer minimaal de volgende handelingen uit:

* **voert een MVPD selectieinterface uit die een nieuwe gebruiker toestaat om hun MVPD en login voor het eerst te identificeren**. Voor ontwikkeling, verstrekt Toegangs Enabler een basisgebruikersinterface die de klant een keus van MVPDs geeft en het login proces in werking stelt. Voor productie, moet u uw eigen Dialoog van de Selecteur uitvoeren MVPD. Sommige MVPD&#39;s leiden om naar hun eigen site voor aanmelding en andere vereisen dat hun aanmeldingspagina&#39;s binnen een iFrame worden weergegeven. U moet een callback uitvoeren die tot dit iFrame leidt om de gevallen te behandelen waar MVPD van de gebruiker hun login pagina in een iFrame toont.
* **identificeer beschermde inhoud**. Voor beveiligde inhoud is toegangsmachtiging vereist. Uw interface moet aangeven welke inhoud is beveiligd en welke inhoud is geautoriseerd.  De status van de autorisatie wordt vaak aangegeven met &quot;niet-vergrendelde&quot; en &quot;vergrendelde&quot; pictogrammen.
* **toon dat een gebruiker** voor authentiek verklaard is. U moet de verificatiestatus van een gebruiker opgeven als onderdeel van een methode waarmee u beveiligde inhoud kunt identificeren. U kunt de Toegangsmanager vragen om te bepalen of de klant reeds voor authentiek is verklaard.

#### 2 quater. De token-verificator voor media integreren {#int-media-token-ver}

U moet de component Adobe Pass Authentication Media Token Verifier integreren in uw mediaserver.  Dit is zodat uw bestaande tokenverificateur de kortstondige tokens van Media kan erkennen die van de Authentificatie van Adobe Pass met een succesvolle vergunning worden verstrekt. De token-verificateur van Media valideert de mediatoetens als de laatste beveiligingsstap voordat u de gebruiker toegang geeft tot beveiligde inhoud. U ontvangt de locatie vanwaar u de token-verificateur voor media wilt downloaden wanneer u zich bij Adobe registreert.

### 3. Ondersteuning voor Single Logout {#ssl}

In de meeste gevallen is uw mediaspeler verantwoordelijk voor het afhandelen van gebruikersaanmeldingen via een eenvoudige API voor toegangsbeheer. Wanneer u logout() aanroept, doet de Access Enabler het volgende:

* Hiermee wordt de huidige gebruiker afgemeld
* Wist alle authentificatie en vergunningsinformatie voor de geregistreerde gebruiker
* Verwijdert alle AuthN en AuthZ tokens van het lokale systeem van de gebruiker

Als de gebruiker zijn computer lang genoeg inactief laat voordat de tokens verlopen, kan de gebruiker nog steeds terugkeren naar de sessie en de aanmelding starten. De Authentificatie van Adobe Pass zorgt ervoor dat alle tokens worden geschrapt, en brengt het MVPD op de hoogte om hun zitting eveneens te schrappen.

Wanneer logout van een plaats in werking wordt gesteld die niet met de Authentificatie van Adobe Pass geïntegreerd is, kan MVPD de dienst van de Enige Logout van de Authentificatie van Adobe Pass door browser redirect aanhalen.

## Gebruikersnaam {#user-ids}

Elke gebruiker die een machtigingsstroom start, is gekoppeld aan één unieke gebruikers-id.  Tijdens een machtigingsstroom kan één gebruiker-id echter op verschillende manieren worden weergegeven, afhankelijk van de API waarvan u de id ontvangt.

sessionGUID in het Korte Token van Media is de veilige vorm van UserID, die via sendTrackingData () vraag beschikbaar is.   In alle huidige integratie, is dit een blijvende GUID voor de gebruiker over tijd en apparaten, maar de bron van GUID begint met UserID in de reactie van SAML van MVPD.   Nochtans, konden sommige MVPDs hun mening in de toekomst veranderen, en beginnen een voorbijgaande GUID te verzenden.  Als een programmeur ervoor wil zorgen dat MVPD bronGebruikersidentiteitskaart in de reactie AuthN blijvend is, zouden zij in hun overeenkomsten met MVPDs moeten ervoor zorgen.

Hier volgen de verschillende manieren waarop de gebruikersnaam wordt weergegeven in de API&#39;s voor Adobe Pass-verificatie:

* `sendTrackingData()` eigenschap GUID - Dit is de gehashte versie van de Adobe van de MVPD-gebruikersnaam.  Deze gebruikersnaam kan dus niet worden bijgehouden naar de bron van de MVPD.   Deze id is uniek en doorgaans blijvend, maar kan niet met de MVPD worden gedeeld om specifiek gebruiksgedrag te vergelijken met wat MVPD&#39;s aan hun kant hebben.   Het is niet digitaal ondertekend, dus niet voor fraudebestrijding ondoenbaar, maar het is goed genoeg voor analyses.  Deze vorm van de Gebruiker - identiteitskaart wordt verstrekt cliënt-kant op alle gebeurtenissen die de Authentificatie van Adobe Pass in de stroom AuthN/AuthZ produceert.
* De eigenschap `sessionGUID` van Short Media Token - Dit is hetzelfde als de gebruikersnaam via `sendTrackingData()` . Deze wordt echter digitaal ondertekend om de integriteit ervan te beschermen.  Hierdoor is deze waarde voldoende voor het bijhouden van fraude bij gelijktijdig gebruik. Het is bedoeld om op de server te worden verwerkt nadat u onze validatiebibliotheek hebt gebruikt en kan worden geanalyseerd op fraudepatronen voordat de videostream aan de client wordt vrijgegeven.  Het uitvoeren van om het even welk van deze taken is aan de Programmer.
* `getMetadata() userID ` bezit - Dit bezit zal Adobe toestaan om daadwerkelijke bronMVPD UserID aan Programmer bloot te stellen. Het zal met de openbare sleutel van het certificaat worden gecodeerd wij van de Programmer hebben, zodat het niet in duidelijk aan de cliënt wordt blootgesteld. Dit geeft de programmeur de daadwerkelijke Gebruiker ID van MVPD, zodat is het iets dat voor rekening aaneenschakeling of fraudeonderzoek direct met MVPD kan worden gebruikt.

**In conclusie**

* De MVPD Gebruiker - identiteitskaart is over het algemeen, hoewel niet gewaarborgd, blijvende unieke identiteitskaart die **van MVPDs wordt geproduceerd en tot Adobe op succesvolle authentificatie** overgegaan. Het is over het algemeen verenigbaar over alle netwerken met sommige uitzonderingen.
* De MVPD-gebruikersnaam bevat geen PII en is GEEN rekeningnummer. Het hoeft niet in gecodeerde vorm te worden aangeboden, aangezien we met alle MVPD&#39;s hebben gevalideerd die geen PII worden verzonden.


Hoe u de gebruikersnaam gebruikt, hangt af van het gebruik:

* Als u het voor tracking/analytics nodig hebt, kunt u het beste deze ophalen uit `sendTrackingData()` .
* Als u het op de server-kant voor stroomversie, fraude, of operationele gegevens nodig hebt, kunt u het van de Symbolische Validator krijgen van Media Token.
* Als u het voor rekening het verbinden en diepere fraude nodig hebt, controleer met uw contact van de Adobe voor beschikbaarheid.

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
