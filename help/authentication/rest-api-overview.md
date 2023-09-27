---
title: REST API-overzicht
description: Overzicht voor rest-API's
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1599'
ht-degree: 0%

---

# REST API-overzicht {#rest-api-overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Overzicht {#over}

De Adobe Pass Authentication REST API biedt directe toegang tot de TVE-services (TVE) voor verificatie en autorisatie. Deze API ondersteunt twee primaire architecturen: server-naar-server of aangesloten apparaten (bijvoorbeeld spelconsoles, slimme tv&#39;s, set-top boxes, enz.) toepassingen die geen mogelijkheden voor webbrowsing hebben.



### Server-naar-server

Server-aan-server oplossingen impliceren de cliënttoepassingen van de Programmer die met de diensten van de Programmer integreren die met de diensten van de Authentificatie van Adobe Pass voor stromen TVE verbinden. Deze benadering verplaatst het grootste deel van implementatie van TVE van de cliënt naar de server waar één enkele, verenigde vergunningsmodule kan worden gebouwd en worden gehandhaafd. De primaire resterende verantwoordelijkheid van de clienttoepassingen is het beheer van een webweergave voor gebruikersverificatie.



### Verbonden apparaten

Connected Devices-apps communiceren rechtstreeks met Adobe Pass Authentication via de REST API&#39;s om configuratie-, registratie-, verificatiestatus- en verificatiestromen uit te voeren, terwijl een tweede scherm (browser)-app vereist is voor de verificatiestroom. Native SDK&#39;s worden daarom niet gebruikt.



### Andere architecturen

Naast de twee primaire REST API-architecturen, server-naar-server- en Direct-clientoplossingen voor slimme apparaten, zijn er andere architecturen.  Primair onder hen is de architectuur van SDK, die een cliëntcomponent genoemd Toegangsbeheer gebruikt die de Authentificatie van Adobe Pass aan Programmeurs verstrekt.  De app gebruikt API&#39;s van Access Enabler voor het starten, verifiëren, autoriseren en afmelden van bestanden.  Alle communicatie tussen de toepassing van de Programmer en de servers van de Authentificatie van Adobe Pass komt door toe Enabler van de Toegang.  Er is een ander kenmerk van Access Enabler beschikbaar voor de volgende platforms: JavaScript, iOS, tvOS, Android en FireTV.

Hoewel het mogelijk is om REST API op cliëntplatforms direct te gebruiken die inheemse SDKs buiten een server-aan-server oplossing steunen, wordt deze benadering niet geadviseerd.



## API Pros en Cons REST {#ProsAndCons}

De Adobe Pass Authentication REST API is gemaakt om een TVE-oplossing (TV Anywhere) te bieden voor apparaten die geen mogelijkheden voor surfen op het web of permanente opslag hebben. De REST API biedt ondersteuning voor alle verificatie- en autorisatiestromen, maar omdat er geen native SDK-component in aanwezig is. De SDK&#39;s die door Adobe Pass Authentication worden geleverd en onderhouden, zijn uitgerust met functies die buiten de box vallen en bedrijfsregels implementeren die in het geval van de REST API door de programmeurs moeten worden geïmplementeerd en onderhouden. In de onderstaande tabel met verantwoordelijkheden van programmeurs beschrijven we de beperkingen van de huidige REST API die door programmeurs moeten worden aangepakt.



### Server-naar-server vs clientgebaseerde Pros en Cons

Een server-aan-Server architectuur verstrekt een manier om het grootste deel van de authentificatie en vergunning verwante logica in één enkele logische eenheid of implementatie te consolideren.  Deze aanpak heeft voor- en nadelen.  De pros omvatten:

* Enkelvoudige implementatie voor authentificatie en vergunning bedrijfslogica.
* Vermijd de noodzaak om die logica op elk gesteund platform uit te voeren gebruikend die platforms inheemse hulpmiddelen.
* De mogelijkheid om mogelijkheden bij te werken zonder dat clients moeten worden bijgewerkt met alle bijbehorende vereisten (bijvoorbeeld updates van App Store).
* **Eenvoudiger** uitbreiden en aangepaste authN- en authZ-mogelijkheden (bijv. D2C toevoegen).
* Direct beheer van het bijbehorende verkeer voor meer controle, kwaliteit en controle.



Opnieuw, zijn de cons vermeld in de verantwoordelijkheden van de Programmer, maar omvatten het volgende:

* SSO moet voor elke cliënt voor platforms zonder Platform SSO worden uitgevoerd.
* Programmeurs moeten indien nodig MVPD-specifieke logica implementeren.
* Alle platforms die REST API gebruiken delen één enkele configuratie die eigenschappen zoals authentificatie TTLs regeert.



### Verbonden apparaten

Voor de meeste aangesloten apparaten moet de REST API op de een of andere manier worden gebruikt, aangezien een SDK niet beschikbaar is. Het aangesloten apparaat zal of REST API direct gebruiken, of met een server-aan-server oplossing integreren die REST API gebruikt.

## Taken van de programmeur {#programmer-responsibilities}

Het volgende is op zowel server-aan-server als Verbonden toepassingen van het Apparaat van toepassing.

| **Functionaliteit** | **Verantwoordelijk voor de verwerking van de functionaliteit** | **Beschrijving van de beperkingen van de huidige Clientless API en verschillen met native SDK&#39;s** |
| --- | --- | --- |
| Configuratie-instellingen toegepast per platform | Adobe | Eén **hoofdbeperking** bij het gebruik van REST API op alle platforms (inclusief mobiele apparaten zoals iOS en Android) is dat de configuratie-instellingen die overeenkomen met REST API in ons TVE-dashboard-configuratieprogramma worden toegepast op alle apparaten (zelfs als er een iOS-apparaat is waarop een native toepassing wordt uitgevoerd die boven op onze REST API staat). Deze beperking **kan breken** de overeengekomen TTL&#39;s en de overeengekomen platforminstellingen met de MVPD&#39;s - indien deze per platform verschillen. [1](#1) |
| Single Sign-On | Programmeurs | SSO is met behulp van de REST-API alleen beschikbaar op platforms die ondersteuning bieden voor Platform SSO (bijvoorbeeld Apple, Roku, Amazon), terwijl SSO niet kan worden gegarandeerd voor andere platforms wanneer de REST-API wordt gebruikt. De SDK&#39;s slaan gegevens in cache op verschillende manieren in de site of in de app. Dit betekent dat de gebruiker zich één keer aanmeldt op een site/app en al is aangemeld op deelnemende sites, zonder dat gebruikersinteractie nodig is. [2](#2) |
| Eén aanmelding | Programmeurs | In een native SSO-scenario van SDK, wordt het afmelden bij één deelnemende toepassing overal vanaf de gebruiker afgemeld. Op de huidige REST API steunen wij geen SLO, zal het afmelden van één toepassing de gebruiker slechts voor die bepaalde toepassing afmelden. |
| Caching | Programmeurs | De REST API implementaties zullen hun eigen caching mechanisme voor zaken overeengekomen gegevenspunten moeten uitvoeren. De SDK&#39;s plaatsen automatisch verschillende gegevensposten in de cache, waarbij rekening wordt gehouden met verschillende bedrijfsregels. Bijvoorbeeld, worden de gebruikersmeta-gegevens in het voorgeheugen ondergebracht met zelfde TTL zoals het authentificatietoken, terwijl sommige punten programmatically van caching (preflight) kunnen worden uitgesloten. |
| Gedetailleerd mechanisme voor foutrapportage | Programmeurs | De REST API is vooral gebaseerd op HTTP-foutcodes voor het rapporteren van toepassingsfouten, terwijl SDK&#39;s een gedetailleerd mechanisme voor foutmeldingen hebben dat de toepassingsontwikkelaars helpt beter te begrijpen wat er gebeurt. |
| Herstel van toepassingsfouten (opnieuw proberen op fout, lusdetectie enz.) | Programmeurs | REST API-implementaties moeten hun eigen herstelsystemen voor toepassingen bouwen, terwijl implementaties boven op SDK&#39;s profiteren van het foutherstellingssysteem van SDK&#39;s: herstel van tijdelijke netwerkfouten door bepaalde netwerkaanroepen opnieuw te proberen met logica om ‘lussen’ te voorkomen. |
| Verificatie per aanvrager | Programmeurs | Sommige MVPDs vereisen authentificatie voor elke plaats/app, of om bedrijfsredenen, of wegens technische uitdagingen. De SDK&#39;s dwingen dit automatisch af op basis van de TVE-dashboardconfiguratie. De REST API-implementatoren moeten dit zelf implementeren om geen inbreuk te maken op bedrijfsovereenkomsten, of om een volledige vergunning te kunnen verkrijgen voor die MVPD&#39;s die hun verificatiegegevens per toepassing regelen. |
| Passieve verificatie | Programmeurs | Sommige MVPDs steunen &quot;passieve&quot;authentificatie, waar de gebruiker niet wordt vereist om de geloofsbrieven in te gaan, en een poging om de gebruiker voor authentiek te verklaren wordt gemaakt automatisch. Dit is vooral nuttig voor MVPDs die &quot;Authentificatie per aanvrager&quot;als vereiste ook hebben. In een dergelijk geval is de door de SDK&#39;s ingeschakelde UX naadloos, waarbij de gebruiker slechts eenmaal in een toepassing verifieert en de SDK &quot;passieve&quot; verificatie voor andere toepassingen in het ecosysteem uitvoert. De gebruiker ziet niet de extra passieve vraag, zal hij eenvoudig reeds voor authentiek verklaard zijn, terwijl MVPD het doel bereikt om afzonderlijke authentificatiesessies voor elke toepassing te hebben. |
| Impliciete en uniforme apparaatdetectie | Programmeurs | SDK&#39;s detecteren automatisch het apparaattype en verzenden die informatie standaard. REST API-implementaties zijn vaak geschikt voor het verzenden van verschillende informatie, afhankelijk van de implementator, waardoor de bedrijfsregels en statistieken van verschillende sites worden scheefgetrokken of afgebroken. **De programmeurs moeten ervoor zorgen dat zij ons correcte apparateninformatie hebben verzonden** op elk van hun apps. Voor server aan serverimplementaties, REST api identificeert correct het IP adres van de eindgebruiker, tenzij de extra stappen worden genomen. Het IP-adres is belangrijk in bepaalde gebruiksgevallen, zoals fraudepreventie of HBA. |
| Tamper proof TempPass | Programmeurs | Het afdwingen van TempPass is gebaseerd op stabiele apparaat identiteitskaart SDK&#39;s gebruiken hardwareinfo- en vingerafdruktechnieken om het apparaat te identificeren. Dit mechanisme is niet openbaar, en dus veiliger en botsingsvrij. Voor REST API-implementaties moeten de programmeurs hun eigen algoritmen implementeren voor apparaatidentificatie/vingerafdrukken. |
| Apparaatbinding | Programmeurs | Tokens die met een toepassing via SDK op een apparaat worden gegenereerd, zijn niet overdraagbaar, waardoor het voor een kwaadwillige gebruiker moeilijk wordt om zijn tokens te delen en andere gebruikers toegang te bieden. Dit is gebaseerd op hetzelfde apparaat-id mechanisme als de &#39;tamper proof TempPass&#39;. Voor REST API, blijven de tokens in de wolk, wat betekent dat twee apparaten met zelfde deviceIDs die de zelfde vraag maken de zelfde reacties/toegang zullen krijgen. Programmeurs moeten ervoor zorgen dat zij een robuust, veilig en botsingsvrij mechanisme hebben om deviceIDs te produceren/toe te wijzen. |
| Voorspelbare en gecertificeerde reeks functies | Programmeurs | De bestaande reeks functies in elke SDK is consistent, voorspelbaar en volledig gecertificeerd en getest. Sommige bedrijfsvereisten worden afgedwongen via SDK&#39;s, waardoor het voor de programmeur riskant wordt om contracten te schenden terwijl de REST API wordt gebruikt. Hoewel de REST API flexibeler kan zijn, garandeert de Adobe dat de SDK-implementaties zakelijke beslissingen en instellingen van het TVE-dashboard afdwingen. In het geval van Clientless, voert elke programmeur zijn eigen ondergroep van de bedrijfsregels uit die zijn toepassingen op een bepaald tijdstip aanpast. |


1. Als onderdeel van ons nieuwe One API-initiatief zijn we van plan deze beperking op te heffen en regels per platform toe te passen op basis van de apparaatidentificatie.

2. De Adobe blijft met alle belangrijke platforms werken om Platform SSO uit te voeren dat met onze REST API kan worden gebruikt. Ons One API-initiatief biedt SSO-ondersteuning voor toepassingen die zijn geïmplementeerd met behulp van native SDK&#39;s en toepassingen die zijn geïmplementeerd met behulp van REST API.

## Minimale apparaatvereisten {#min_reqs}

Om de Adobe Pass Authentication REST API te kunnen gebruiken, moeten apparaten voldoen aan de technische minimumeisen die in de REST API-sectie van het [Adobe Pass-verificatieplatform / Apparaat / Vereisten voor tools](#general_clientless_reqs).
