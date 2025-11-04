---
title: REST API Cookbook (Server-to-Server)
description: Testen van API-cookboekserver op de server.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# (Verouderd) REST API Cookbook (Server-to-Server) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht {#overview}

Het doel van dit kookboekdocument is om beste praktijken voor het uitvoeren van de Authentificatie van Adobe Pass te detailleren gebruikend de server-aan-Server architectuur.  Het verstrekt basisvereisten, geleidelijke stroomimplementatie en algemene overwegingen voor productiemilieu&#39;s en verrichting.

### Draaimechanisme

De Adobe Pass Authentificatie REST API wordt geregeerd door a [&#x200B; het Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


## Componenten {#components}

In een werkende server-aan-server oplossing zijn de volgende componenten betrokken:


| Type | Component | Beschrijving |
| --- | --- | --- |
| Streaming apparaat | Streaming-app | De programmeertoepassing die op het het stromen apparaat van de gebruiker verblijft en voor authentiek verklaarde video speelt. |
| | \[Optioneel\] AuthN-module | als het Streamen Apparaat een Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Module AuthN verantwoordelijk voor het voor authentiek verklaren van de gebruiker op MVPD IdP. |
| \[Optioneel\] AuthN-apparaat | AuthN App | als het Streaming Apparaat geen Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Toepassing AuthN een Toepassing van het Web van de Programmer die van een afzonderlijk apparaat van de gebruiker gebruikend Webbrowser wordt betreden. |
| Programmeringsinfrastructuur | Programmeringsservice | Een service die het streamingapparaat koppelt aan de Adobe Pass-service om verificatie- en autorisatiebeslissingen te verkrijgen. |
| Adobe-infrastructuur | Adobe Pass Service | De dienst die met de Dienst van MVPD IdP en AuthZ integreert en authentificatie en vergunningsbesluiten verstrekt. |
| MVPD-infrastructuur | MVPD IdP | Een eindpunt van MVPD dat op referentie-gebaseerde authentificatiedienst verleent om de identiteit van hun gebruiker te bevestigen. |
| | MVPD AuthZ Service | Een eindpunt van MVPD dat vergunningsbesluiten verstrekt die op gebruikersabonnementen, ouderlijke controles, enz. worden gebaseerd. |

## Stromen {#flows}

### Dynamische clientregistratie (DCR)


Adobe Pass gebruikt DCR om clientcommunicatie tussen een programmeertoepassing of server en de Adobe Pass-services te beveiligen. De stroom DCR is afzonderlijk en beschreven in het [&#x200B; Dynamische Overzicht van de Registratie van de Cliënt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.


### Verificatie (authN)

De authentificatiestroom wordt gebruikt om een gebruiker toe te staan om zich te identificeren
aan hun MVPD om te bepalen of de gebruiker een geldig account heeft.

1. De gebruiker start de Streaming Device-app en probeert zich aan te melden of beveiligde inhoud weer te geven.
2. De toepassing Streaming Device (Streaming Device) vraagt de programmeerservice of het apparaat al is geverifieerd.
3. De programmeerservice registreert de toepassing met DCR.
4. De Dienst van de Programmer controleert de Streaming status van Device AUthN door de dienst van Adobe Pass **checkauthn** API te roepen.
5. Voor het geval waar de **controleauteur** vraag de status terugkeert die het Apparaat van de Gebruiker voor authentiek wordt verklaard, dan kan app aan de stroom van de Vergunning te werk gaan.
6. Voor het geval waar de **controleauteur** vraag de status terugkeert die het Apparaat van de Gebruiker NIET voor authentiek wordt verklaard, dan zou app op een gebruikersverzoek moeten wachten om login.
7. Wanneer de gebruiker om direct login (b.v. selecteert login knoop) of onrechtstreeks login (b.v. selecteert beschermde inhoud wanneer niet reeds voor authentiek verklaard), doet de Streaming Apparaat app een verzoek aan de Dienst van de Programmer om gebruikersauthentificatie in werking te stellen. De dienst van de Programmer verzoekt en ontvangt een unieke registratiecode (regcode) door de Dienst van Adobe Pass **te roepen regcode** API.
8. De dienst van de Programmer wint ook de lijst van huidige MVPDs en attributen terug door de Dienst van Adobe Pass **config** API te roepen. Opmerking: deze API kan ook eerder in de flow en cache worden aangeroepen.
9. De programmeerservice retourneert de regcode naar de app Streaming Device en de verwerkte MVPD-lijst die is aangevraagd in stap \#7. Opmerking: de bewerkte MVPD-lijstindeling wordt opgegeven door de programmeur en kan worden gefilterd om specifieke MVPD&#39;s (dat wil zeggen allow- of block-lists) expliciet toe te staan of te blokkeren.
10. Als het apparaat van AuthN (d.w.z. &quot;tweede scherm&quot;), of door keus of behoefte (d.w.z. het Streaming Apparaat steunt geen Agent van de Gebruiker) verschillend is, dan zou het Streaming Apparaat de regcode en URI voor de gebruiker moeten tonen om tot de Toepassing van AuthN toegang te hebben. De gebruiker typt URI in de Agent van de Gebruiker op het Apparaat AuthN om de Toepassing te lanceren AuthN, en typt dan regcode in die toepassing. Als het Streaming Apparaat het zelfde als het Apparaat AuthN is, dan kan regcode programmatically tot de Module worden overgegaan AuthN.
11. De module AuthN stelt de gebruikersauthentificatie met MVPD door een Plukker van MVPD te tonen in werking. Nadat de gebruiker MVPD selecteert, verklaart de Module AuthN **&#x200B;**&#x200B;met regcode voor authentiek, die de Agent van de Gebruiker aan MVPD IDP opnieuw richt. Wanneer de gebruiker met succes met de MVPD voor authentiek verklaart, wordt de Agent van de Gebruiker opnieuw geleid door de Dienst van Adobe Pass, waar de succesvolle authentificatie met regcode wordt geregistreerd, en dan opnieuw gericht terug naar de Module AuthN.
12. Als het streamingapparaat verschilt van het AuthN-apparaat, geeft het AuthN-apparaat een succesvol verificatiebericht weer aan de gebruiker en worden de stappen uitgevoerd (bijvoorbeeld &quot;Success\!! U kunt nu terugkeren naar uw gameconsole om door te gaan met \[..\]&quot;). Als het streamingapparaat hetzelfde is als het AuthN-apparaat, kan het streamingapparaat de voltooiing van de verificatie programmatisch detecteren.



Het volgende diagram illustreert de authentificatiestroom:

![](/help//authentication/assets/authn-flow.png)

### Autorisatie (authZ)

De machtigingsstroom wordt gebruikt om te bepalen of een gebruiker toegang heeft tot gevraagde inhoud.

1. Telkens wanneer de gebruiker beveiligde inhoud probeert te bekijken op de Streaming Device-app, roept de Streaming Device-app de Programmer Service aan om de inhoud te identificeren en om toestemming en informatie te vragen die nodig zijn om de stream te starten.
1. De dienst van de Programmer roept Adobe Pass **&#x200B;**&#x200B;API goedkeurt die identiteitskaart van het Middel samen met andere vereiste parameters overgaat. De dienst van Adobe roept de Dienst van MVPD AuthZ met identiteitskaart van het Middel en ontvangt en vergunningsbesluit dat dan aan de Dienst van de Programmer wordt teruggegeven. Dit vergunningsbesluit zal door de Dienst van Adobe Pass voor een configureerbare periode in het voorgeheugen ondergebracht worden. Op verdere **machtigt** vraag van de Dienst van de Programmer aan de Dienst van Adobe Pass, zal de caching waarde zijn teruggekeerd zolang het geldig is.
1. Als de vergunning wordt verleend, zou de Dienst van de Programmer Adobe Pass **/tokens/media** API moeten roepen, die een ondertekend media teken zal terugkeren. De programmeerservice moet het mediatoken valideren met behulp van de JAR (Media Token Verifier Library). Indien geldig, zou de Dienst van de Programmer toestemming en nodig moeten terugkeren om de stroom (b.v. stroom URL) te beginnen die in stap \#1 wordt gevraagd.
1. Als de vergunning wordt ontkend, **goedkeurt** vraag zal een foutencode en een beschrijving aan de Dienst van de Programmer terugkeren. De dienst van de Programmer zou de foutencode en de beschrijving (of een programma gewijzigd bericht) aan het verzoek in stap \#1 moeten terugkeren.

In het volgende diagram wordt de stroom van de autorisatie weergegeven:

![](/help//authentication/assets/authz-flow.png)

### Afmelden

Met de afmeldingsstroom kan de gebruiker de huidige identiteit verwijderen
is gekoppeld aan de toepassing.

1. Wanneer de gebruiker om logout (d.w.z. verwijder van het apparaat de huidige MVPD-account verbonden aan de toepassing), roept de app Streaming Device de Programmer Service op om het apparaat af te melden.
1. De dienst van de Programmer zou de Adobe Pass **logout** API moeten roepen.

Het volgende diagram illustreert de logout flow:

![](/help//authentication/assets/logout-flow.png)

### \[Optioneel\] Voorafgaande toestemming (ook bekend als voor de vlucht)

Voortoestemming kan worden gebruikt om van een reeks middelen snel te bepalen degenen een gebruiker toegang zou kunnen hebben.  Het resultaat van deze vraag wordt typisch gebruikt om UI voor een individuele gebruiker aan te passen.

1. Zodra de gebruiker voor authentiek wordt verklaard, kan het Steaming Apparaat de Dienst van de Programmer roepen om de inhoud te verzoeken waarop de gebruiker gerechtigd is te stromen.

1. De dienst van de Programmer zou Adobe Pass **moeten roepen pre goedkeurt** API met een lijst van Middel IDs, die een eenvoudig koord zijn dat typisch een kanaal vertegenwoordigt een gebruiker zou kunnen gerechtigd zijn om te stromen. *Nota: Momenteel, machtigt* **&#x200B;**&#x200B;**&#x200B; *vraag vooraf om de lijst tot vijf (5) Middel IDs te beperken. Wanneer meer dan vijf middelen nodig zijn, machtigt het veelvoud* &#x200B;**&#x200B;**&#x200B;** *vraag kan worden gemaakt, of de vraag kan worden gevormd om meer dan vijf middelen met een overeenkomst van MVPDs goed te keuren. Implementors zouden in mening moeten houden de kosten van a* **&#x200B;**&#x200B;** *vraag zowel aan de middelen van MVPD evenals de reactietijd aan de Programmer vooraf machtigen en hun gebruik van de vraag goed structureren.*

1. De **pre autoriseert** vraag zal aan de Dienst van de Programmer met een voorwerp JSON antwoorden die een WAAR of VALS waarde voor elke identiteitskaart van het Middel in het verzoek bevatten die erop wijst of de gebruiker aan het bijbehorende kanaal of niet gerechtigd is. *Nota: Als een MVPD geen antwoord voor bepaalde identiteitskaart van het Middel (b.v. wegens netwerkfouten of onderbrekingen) verstrekt, zal de waarde aan VALS in gebreke blijven.*

1. De dienst van de Programmer zou **&#x200B;**&#x200B;vraagreactie moeten gebruiken preauthorize om tot een programmeur-bepaalde douanerespons aan het Streaming Apparaat te leiden, typisch om de presentatie aan de gebruiker te personaliseren die op hun rechten wordt gebaseerd.

In het volgende diagram wordt de stroom voorafgaand aan de autorisatie weergegeven:

![](/help//authentication/assets/preauthz-flow.png)


### Metagegevens \[optioneel\]

U kunt metagegevens gebruiken om gebruikersgegevens op te halen die door de MVPD worden gedeeld.
Voorbeelden hiervan kunnen gebruikers-id, postcode enzovoort zijn.

1. Zodra de gebruiker voor authentiek wordt verklaard, kan de Dienst van de Programmer Adobe Pass **gebruikersmeta-gegevens** API roepen om informatie over de voor authentiek verklaarde gebruiker te verzoeken.

1. De reactie omvat alle metagegevens die beschikbaar zijn voor de opgegeven gebruiker. De specifieke gebieden worden gevormd afzonderlijk voor elke integratie van Programmer/MVPD.

In het volgende diagram wordt de stroom voorafgaand aan de autorisatie weergegeven:



![](/help//authentication/assets/user-metadata-api-preauthz.png)



## Omgevingen en functionele vereisten{#environments}



Een programmeur moet ten minste twee omgevingen maken: een voor productie en een of meer voor staging.


### Productie

De productieomgeving moet in hoge mate beschikbaar zijn en op passende wijze worden geschaald voor grote of onverwachte pieken (bv. live sporten, breken
nieuws).



De Adobe Pass-service wordt uitgevoerd op meerdere datacenters die geografisch over de hele VS zijn verspreid.  Om de beste reactietijd (d.w.z. laagste latentie) van de dienst van Adobe Pass te bereiken, zou de Programmer ook een gelijkaardige geografisch verspreide dienst moeten creëren
infrastructuur.


De dienst van de Programmer zou het DNS geheime voorgeheugen tot maximaal 30 s moeten beperken voor het geval Adobe verkeer moet terugleiden. Dit kan zich voordoen als een datacenter niet beschikbaar is.


De programmeur zou de openbare IP waaier van het productiemilieu moeten verstrekken. Deze worden ingevoerd in een lijst met toegestane IP&#39;s in de Adobe Pass-infrastructuur voor toegang en worden beheerd door het gebruiksbeleid van Adobe voor Fair API.

### Staging

De het opvoeren omgeving kan minimaal zijn, maar zou alle systeemcomponenten en bedrijfslogica moeten omvatten. Het moet op dezelfde wijze functioneren als de productie en het moet mogelijk maken om emissies buiten de productie te testen. In het ideale geval kan de testomgeving worden verbonden met de Adobe Pass-testomgevingen voor gebruik door de programmeur en door Adobe wanneer dat nodig is, zodat we hulp kunnen bieden bij het testen en oplossen van problemen.

### Functionele vereisten

De dienst van de Programmer moet nauwkeurige apparatenidentificatieinformatie over het apparaat overgaan waarvoor zij de stromen uitvoeren. Bovendien, moet de dienst van de Programmer IP van het apparaat overgaan waarvoor zij de stromen (in x-door:sturen-voor kopbal) samen met de haven van de verbindingsbron (op het gebied van apparateninfo) uitvoeren:

    **X-Door:sturen-voor: \&lt;client\_ip \>* 
    
     waar \ &lt;client\_ip \> het cliënt openbare IP adres 
    
    
    
     is de kopbal moet op **regcode* en &#x200B;** autorze* vraag 
    
     Voorbeelden worden toegevoegd:
    
     POST /REGgie/v1/{req\_id}/regcode HTTP/1.1 
    
     x-Door:sturen-voor:203.45.101.20 
    
    
    
     GET /api/v1/autoriseer HTTP/1.1 
    
     x-Door:sturen-voor:203.45.101.20 



De programmeerservice moet gegevens en opmaak verzenden die vereist zijn voor afzonderlijke MVPD&#39;s of geïntegreerde apps (bijvoorbeeld apparaat-IP, bronpoort, apparaatinformatie, MRSS, optionele gegevens zoals ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->


De dienst van de Programmer moet authN en authZ TTLs respecteren wanneer het caching en maakt authN of authZ zittingen onbruikbaar wanneer op de hoogte gebracht.

De programmeur moet certificaten onderhouden die met Adobe worden gedeeld.
