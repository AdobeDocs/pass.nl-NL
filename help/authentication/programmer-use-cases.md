---
title: Gebruiksgevallen voor programmeerprogramma's
description: Gebruiksgevallen voor programmeerprogramma's
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: 189d0e5c83d1bc171c6ff3057f22dbf4055ea6b1
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Gebruiksgevallen voor programmeerprogramma&#39;s {#programmer-use-cases}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

In dit document wordt een overzicht gegeven van de Gebruiksgevallen voor Integratie van Programmer die worden ondersteund door Adobe Pass Authentication. U kunt deze pagina controleren voordat u begint met een integratieproject om te zien welke functies momenteel worden ondersteund.

## Gebruik hoofdletters {#use-cases}


### Basisintegratie: Federale verificatie en autorisatie voor één kanaalnetwerk {#basic-integration}

**Prioriteit** - Hoog

**Breakdown** - Enige Programmer-Germerkte TVE app met 1 Netwerk van het Kanaal binnen de ervaring wordt ontvangen die

Hierdoor kunnen programmeurs premiuminhoud in hun eigen TVE-app* aanbieden met een gefederaliseerde machtigingscontrole op de MVPD. De requestID moet worden uitgelijnd op het merk van de toepassing die de inhoud aan de viewer aanbiedt. In dit scenario, is er een 1 tot 1 verhouding tussen de identiteitskaart van de Aanvrager van de Authentificatie van Adobe Pass en identiteitskaart van het Middel die voor recht wordt geverifieerd.

>[!NOTE]
>
>De TVE-app wordt in dit document gebruikt om gezamenlijk te verwijzen naar de verschillende typen toepassingen (web-apps, mobiele apps, enz.) ondersteund door Adobe Pass-verificatie. De onderstaande kolom Platforms kan details bevatten over ondersteunde platforms voor specifieke toepassingen.

#### Gevallen van specifiek gebruik (algemeen voor de meeste integraties) {#sp-use-cases-basic-int}

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Hoog | MVPD Discovery from Programmer TVE App | De gebruiker start op de TVE-app met branding van de programmeur en wordt gevraagd zijn MVPD-provider te selecteren. | Web (SWF/JS)                    Mobiel (iOS/Android)                   Clientloze API (voor tweede scherm) |                                           |
| Hoog | Federatieve verificatie van de TVE-app van de programmeur | De gebruiker begint op de merknaam van de programmeur TVE-app en nadat hij zijn MVPD-provider heeft geselecteerd, gaat de gebruiker naar de aanmeldingspagina van de MVPD zelf om zijn of haar gegevens in te voeren. | Web (SWF/JS)                    Mobiel (iOS/Android) |                                           |
| Hoog | Autorisatie vanuit de TVE-app van programmeur | Nadat de gebruiker voor authentiek wordt verklaard, kan de TVE app van de Programmer backchannel vergunningsverzoeken aan MVPD indienen om de rechten van de gebruiker te controleren. Meestal wordt hiermee alleen gecontroleerd of het Channel Network deel uitmaakt van het MVPD-abonnementspakket van gebruikers.                                  In dit geval komen de id van de aanvrager en de bron-id overeen met 1:1. | Alle platforms |                                           |
| Medium | Afmelden bij TVE-app van programmeur | Hiermee kan de gebruiker zich afmelden en de Adobe Pass Authentication AuthN/AuthZ-tokens wissen. In veel gevallen, registreert dit ook de gebruiker uit MVPD. Maar MVPDs varieert in of dit wordt gesteund. De Adobe Pass-verificatiesessie en tokens worden altijd gewist. | Alle platforms behalve standaard XBox | Verscheidene MVPDs steunt dit niet. |
| Hoog | Single Sign-On voor sites en toepassingen | Hiermee kan de gebruiker de aanmeldingssessie delen tussen sites en apps zonder dat hij of zij zich opnieuw hoeft aan te melden. | Alle platforms behalve de API zonder client | Vereist minstens SDK 1.7 voor sommige MVPDs. |

### Eén TVE-app die meerdere kanaalnetwerken host {#single-app-multi-channel}

**Prioriteit** - Hoog

Laat de Programmer toe om verscheidene kanaalnetwerken van inhoud op de zelfde branded bestemming voor hun kijkers samen te voegen.

#### Specifieke gebruiksgevallen {#sp-use-cases-singl-tve-app}

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hoog | Verificatie via afzonderlijk kanaal | De gebruiker kan inhoud van verschillende kanaalnetwerken in dezelfde TVE-app bekijken. De programmeur kan vergunningsvraag maken die voor elk kanaalnetwerk specifiek is om de rechten van de gebruikers te bevestigen. | Alle platforms | Alle MVPD&#39;s ondersteunen dit nu in een of andere vorm. |
| Laag | Preflight-autorisatiezoekopdracht | Hierdoor kan de programmeur in één API-aanroep controleren welke kanalen de gebruiker in zijn pakket heeft. Dit wordt gedaan vóór daadwerkelijke vraag AuthZ om inhoud uit het gebruikersinterface te filtreren die de gebruiker geen toegang heeft tot. |               | De meeste MVPDs stelt deze gegevens nog niet als Attributen van de Gebruiker bloot, zodat maakt de Adobe eigenlijk vraag AuthZ om het te krijgen. Ook, zijn de meeste MVPDs beperkt tot 5 tegelijkertijd, omdat zij geen veelvoudige kanalen in één enkele vraag steunen.                             Het is zeer belangrijk om te verifiëren hoeveel kanalen de programmeur moet preflight controle. Wat het aantal ook is, we zullen moeten controleren of het prima is met de MVPD&#39;s. De meeste MVPD&#39;s ondersteunen momenteel niet meer dan vijf kanalen (derde kwartaal, 2013). |

### Toelating op bedrijfsniveau {#asset-level-authz}

**Prioriteit** - Laag

**Uitsplitsing** - geef een herkenningsteken van Activa op het verzoek van de Vergunning over

**Platforms** - Alle platforms

#### Specifieke gebruiksgevallen {#sp-use-cases-asset-lvl-authz}

Laat MVPD toe om activa niveau analyses op elke vraag te krijgen AuthZ. Dit heeft het nadeel van het negeren van het geheime voorgeheugen van AuthZ van de Authentificatie van Adobe Pass.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| Laag | Een element-id doorgeven bij een vergunningsaanvraag | Laat MVPD toe om activa niveau analyses op elke vraag te krijgen AuthZ.  Heeft het nadeel van het negeren van de Adobe Pass Authentication AuthZ cache. | Alle platforms | Slechts één MVPD steunt dit momenteel. |




### Ouderlijke controles {#parental-controls}

**Prioriteit** - Laag

Hiermee schakelt u beperkingen voor MVPD-gebruikersaccounts in voor de TVE-app van de programmeur.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| Laag | Inhoud filteren op basis van gebruikerskenmerken | Hiermee kan de programmeur de maximaal toegestane score voor een gebruiker controleren voordat de lijst met beschikbare inhoud voor de gebruiker wordt weergegeven. | Web (Flash/JS)                    Mobiel (iOS/Android) | Slechts werkt met één MVPD momenteel. |
| Laag | Inhoud-beoordelingen in de AuthZ-aanvraag doorgeven | Laat de programmeur toe om de specifieke classificatie van de inhoud over te gaan de gebruiker als deel van het verzoek AuthZ aan MVPD wil letten                             Verwant aan #3, aangezien de ratings typisch op het activaniveau zijn. | Alle platforms | Slechts werkt met één MVPD momenteel. |

#### Aanpassing van de MVPD-integratie per merk programmeur {#mvpd-int-cust-prog-brand}

**Prioriteit** - Medium

Hiermee wordt aangepaste ervaring ingeschakeld tijdens AuthN- of AuthZ-foutberichten.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Medium | Geef de Identificatiecode van de Serviceleverancier in het AuthN-verzoek door. | Laat specifieke branding op de MVPD login pagina specifiek voor de dienstverlener toe. Schakel ook automatisch de standaardinstelling in zodat deze overeenkomt met het publiek, zoals Spaans voor Univisie. | Alle platforms | Varieert door MVPD. Sommigen steunen dit niet. |
| Medium | Aangepaste foutberichten op AuthZ-respons | Laat Programmer of merkspecifieke foutenmeldingen van MVPD toe die specifieke bericht voor upsell met een verbinding kunnen omvatten die het pakket bevordert. | Web, Android, iOS | Varieert door MVPD. Sommigen steunen dit niet. |


### Gebruiksscenario&#39;s van aangesloten apparaten {#connected-devices}

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medium | XBox LiveID SSO voor apps en consoles | Hiermee kan de gebruiker een AuthN-sessie delen tussen apps en tussen verschillende spelconsoles - gekoppeld aan hun LiveID-account. | Native XBox-SDK | De meeste MVPDs houdt niet van dit omdat het typische model het teken aan het apparaat moet binden - niet aan de gebruiker.                             We raden deze aanpak niet meer aan als dat mogelijk is. |
| Hoog | Verbonden apparaat met tokens gebonden aan de toepassings-id op het apparaat | Hiermee kan de programmeur de MVPD-machtiging in de token binden aan de appID op het apparaat waarvoor deze is uitgegeven. | Clientloze API | Hierdoor wordt het aangesloten apparaat nauwkeuriger uitgelijnd op de standaardimplementatie voor tokens.                             De verbetering moet nog een apparaat-brede identiteitskaart zijn. |

### Apparaatspecifieke lengte van AuthN TTL {#authn-ttl-length}

Schakel TVE-machtiging in voor speciale gebeurtenissen die mogelijk geen bronnen zijn in de MVPD-machtigingsdatabase, zoals normale kanalen.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Hoog | Verschillende TTL-waarden per platform instellen | Laat de Programmeur toe om een verschillende lengte van TTL voor Web, mobiele en aangesloten apparaten te vestigen. Adobe Pass-verificatie ondersteunt momenteel de mogelijkheid om 3 verschillende TTL-waarden te hebben:                                Web (Flash)                    Mobiel/HTML5                    Clientless - Verbonden apparaten |           | Sommige MVPDs plaatsen dynamisch TTL. De Adobe kan deze dynamische montages indien nodig met voeten treden, gebruikend de configuratiemontages. |

### Speciale toepassingen op basis van gebeurtenissen {#special-event}

**Prioriteit** - Laag

Schakel TVE-machtiging in voor speciale gebeurtenissen die mogelijk geen bronnen zijn in de MVPD-machtigingsdatabase, zoals normale kanalen.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| Laag | Meerdere kanalen als proxy voor een gebeurtenis | Dit is gebeurd voor de Olympische Spelen, waar de abonnee twee verschillende kanalen in zijn pakket moest hebben om toegang te krijgen. In dit geval, creeerde de Authentificatie van Adobe Pass een nieuwe resourceID, en had alle MVPDs de afbeelding aan de specifieke kanalen op hun eind doen.  Dat werkte prima met genoeg geavanceerd bericht. Dit was belangrijk omdat de meeste MVPDs geen veelvoudige middelvraag steunt. | Alle platforms | Ondersteund door alle MVPD&#39;s met de juiste kennisgeving. |
| Laag | Speciale nieuwe gebeurtenistoepassing, bestaande kanaalbronnen gebruiken | Dit is gedaan voor Madness van maart. De inhoudsprovider heeft een nieuwe app gemaakt met een nieuwe aanvraag-id. Alle MVPDs nodig om steun voor nieuwe requestID in hun systeem toe te voegen. De resourceIDs was normale kanalen.  Sommige MVPD&#39;s moesten ook de kanalen als geldig onder de nieuwe aanvrager in kaart brengen, zodat was meer tijd nodig voor die gevallen. | Alle platforms | Ondersteund door alle MVPD&#39;s met de juiste kennisgeving. |
| Laag | Bestaande aanvragerID, resourceID | Dit is gedaan voor het Masters golf weekend toernooi. Het was slechts een klein evenement voor een paar dagen, en de stramienen hadden hun eigen mobiele app die in orde was om de inhoud weer te geven. De programmeur was van plan om voor het verkeer van de Authentificatie van Adobe Pass te betalen en enkel hun standaard aanvragerID en resourceID te gebruiken. De enige truc was dat de programmeur een mobiel cert voor het ondertekenen van de aanvragerID met de meesters had, en dat aan hun configuratie als hun reservecert voor dat weekend heeft toegevoegd. | Alle platforms | Geen invloed op MVPD&#39;s |

### Integratie van inhoudsservers {#content-server-integration}

**Prioriteit** - Medium

Validatie van mediatoken inschakelen voordat de videostream naar de clientspeler wordt uitgebracht.

| Prioriteit | Hoofdletters gebruiken | Beschrijving | Platforms | MVPD-notities |
|---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| Hoog | Programmer Federated Player - met machtiging op paginaniveau | Adobe Pass-verificatie-API&#39;s worden uitgevoerd in JavaScript op de pagina en het token wordt doorgegeven aan de speler. Token kan op een aantal manieren worden doorgegeven aan de validatieservice:                                 Param ophalen op de URL van de validatieservice                    URL-parameter die wordt doorgegeven in queryreeks van de URL van de stream                    Externe interface-API                    FlashVars |           |            |
| Medium | Programmer Federated Player - met interne Player Authorization | Adobe Pass-verificatie-API&#39;s worden uitgevoerd in het ActionScript in de SWF van de speler, zodat het token beschikbaar is voor de speler vanaf de callback. |           |            |
| Hoog | Gesynchroniseerde speler - Gehost op MVPD-portal met machtiging op paginaniveau met een iFrame om de speler te buigen | Vergelijkbaar met de speler met machtiging op paginaniveau, maar met de spelerpagina die in het MVPD-portaal wordt geplaatst. Verificatie moet afzonderlijk plaatsvinden in het MVPD-portaal. |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
