---
title: Verificatie met het OAuth 2.0-protocol
description: Verificatie met het OAuth 2.0-protocol
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Verificatie met het OAuth 2.0-protocol

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

Hoewel SAML nog het belangrijkste protocol is dat voor authentificatie door MVPDs van de VS en ondernemingen in het algemeen wordt gebruikt, is er een duidelijke tendens naar overschakeling naar OAuth 2.0 als primair authentificatieprotocol. Het OAuth 2.0-protocol (https://tools.ietf.org/html/rfc6749) is voornamelijk ontwikkeld voor consumentensites en werd snel aangenomen door internetreuzen zoals Facebook, Google en Twitter.

OAuth 2.0 is enorm succesvol en dit heeft bedrijven ertoe aangezet hun infrastructuur langzaam te verbeteren om het te steunen.



## Voordelen van de overgang naar OAuth 2.0 {#adv-oauth2}

Op een hoog niveau, biedt het protocol OAuth 2.0 de zelfde functionaliteit aan zoals het protocol SAML maar er zijn sommige belangrijke onderscheidingen.

Één van dit is het feit dat de verfrist symbolische stroom als manier kan worden gebruikt om authentificatie achter de scènes te verfrissen. Dit staat IdP (MVPDs in dit geval) toe om controle te handhaven terwijl nog het toestaan van een goede gebruikerservaring aangezien de gebruiker niet meer wordt vereist om zich vaak wegens veiligheidszorgen aan te melden.

Het protocol biedt ook meer flexibiliteit in termen van gegevens die worden blootgesteld aangezien een dienstverlener de tokens nu kan gebruiken om tot andere APIs toegang te hebben om extra info te krijgen. Dit resulteert op zijn beurt in een ‘chattier&#39;-protocol voor TVE-gebruiksgevallen, maar het biedt de flexibiliteit die nodig is voor complexe workflows.





## Voorschriften voor de overschakeling op OAuth 2.0 {#oauth-req}

Om authentificatie met OAuth 2.0 te steunen, moet een MVPD aan de volgende eerste vereisten voldoen:

In de eerste plaats moet het MVPD ervoor zorgen dat het de [Toekenning van autorisatiecode](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html) stroom.

Nadat het bevestigd heeft dat het de stroom steunt, moet MVPD ons de volgende informatie verstrekken:

* het eindpunt van de authentificatie
   * het eindpunt zal de vergunningscode verstrekken die later in ruil voor verfrist en toegangstoken zal worden gebruikt
* het /token eindpunt
   * dit zal verfrissen teken en toegangstoken verstrekken
   * het vernieuwingstoken moet stabiel zijn (het moet niet veranderen telkens als wij om een nieuw toegangstoken verzoeken
   * MVPD moet verscheidene actieve toegangstokens voor elk toestaan verfrist teken
   * this end point will also exchange a refresh token for an access token
* we hebben een **eindpunt voor gebruikersprofiel**
   * dit eindpunt zal userID verstrekken, die voor een rekening uniek moet zijn en geen Persoonlijk Identificeerbare Informatie zou moeten bevatten
* de **/logout** eindpunt (optioneel)
   * De Authentificatie van Adobe Pass zal aan dit eindpunt opnieuw richten, MVPD een omleiding terug URI verstrekken; op dit eindpunt, kan MVPD de koekjes op de cliëntmachine ontruimen of om het even welke gewenste logica voor logout toepassen
* het wordt ten zeerste aanbevolen ondersteuning te bieden voor geautoriseerde clients (client-apps die geen gebruikerautorisatiepagina activeren)
* wij zullen ook nodig hebben :
   * **clientID** en **clientgeheim** voor de integratieconfiguraties
   * **tijd om te leven** (TTL) waarden voor vernieuwt teken en toegangstoken
   * Wij kunnen MVPD van een callback van de Vergunning en logout callback URI voorzien. Ook, indien nodig, kunnen wij MVPDs van een lijst van IPs voorzien die in uw firewallmontages moeten worden gewhitelisteerd.


## Verificatiestroom {#authn-flow}

In de authentificatiestroom, zal de Authentificatie van Adobe Pass met MVPD op het protocol communiceren dat in de configuratie wordt geselecteerd. De OAuth 2.0-stroom wordt weergegeven in onderstaande afbeelding:



![Diagram om de stroom van de Authentificatie in de Authentificatie van de Adobe te tonen die met MVPD op het protocol communiceert dat in configuratie wordt geselecteerd.](assets/authn-flow.png)

**Afbeelding 1: OAuth 2.0-verificatiestroom**



## Verificatieverzoek en antwoord {#authn-req-response}

In een notendop, volgt de authentificatiestroom voor MVPDs ondersteunend het protocol OAuth 2.0 deze stappen:

1. De eindgebruiker navigeert aan de plaats van de Programmer en selecteert om met zijn geloofsbrieven MVPD aan login aan te melden
1. AccessEnabler die op de kant van de Programmer met wordt geïnstalleerd verzendt een verzoek van de Authentificatie in de vorm van een HTTP- verzoek naar het eindpunt van de Authentificatie van Adobe Pass, dat het eindpunt van de Authentificatie van Adobe Pass aan het MVPD toestemmingseindpunt opnieuw richt.
1. Het MVPD autorisatieeindpunt verzendt een vergunningscode naar het eindpunt van de Authentificatie van Adobe Pass
1. De Authentificatie van Adobe Pass gebruikt de ontvangen vergunningscode om te verzoeken verfrist token en een toegangstoken van het het symbolische eindpunt van MVPD
1. Een vraag om gebruikersinformatie &amp; meta-gegevens te halen kan naar het eindpunt van het gebruikersprofiel worden verzonden voor het geval dat de gebruikersinformatie niet inbegrepen in het teken is
1. Het verificatietoken wordt doorgegeven aan de eindgebruiker die nu met succes door de programmeersite kan bladeren

   >[!NOTE]
   >
   >Vernieuw teken wordt gebruikt om een nieuw toegangstoken te krijgen nadat het huidige toegangstoken ongeldig wordt of verloopt.


>[!IMPORTANT]
>
>Het vernieuwingstoken moet niet veranderen wanneer het voor een toegangstoken wordt geruild.

Deze beperking vloeit voort uit de cliëntstromen die de server niet toestaan om AuthNToken bij te werken die, voor het protocol OAuth 2.0, ook bevat verfrist teken.

Een typische vergunningsstroom voert een uitwisseling van toe vernieuwt teken dat in AuthNToken voor een toegangstoken wordt bewaard die later wordt gebruikt om de vergunningsvraag in de naam van de gebruiker uit te voeren die in de eerste plaats voor authentiek werd verklaard. Als de Server van de Vergunning (MVPD) het vernieuwt teken moest veranderen en oude ongeldig maakte, zullen wij niet geldig AuthNToken kunnen bijwerken. Daarom moeten MVPD&#39;s stabiele vernieuwingstokens ondersteunen om OAuth 2.0-integratie voor hen in te stellen.


## Migreren van SAML naar OAuth 2.0 {#saml-auth2-migr}

De migrerende integraties van SAML aan OAuth 2.0 zullen door Adobe en MVPD worden uitgevoerd. Er is geen behoefte aan enige technische verandering op de programmeerkant, hoewel de programmeur de cobranding op de MVPD login pagina zou kunnen willen controleren/testen. Vanuit het oogpunt van het MVPD zijn de eindpunten en andere informatie die in de eisen van Oauth 2.0 worden verlangd, vereist.

Om **SSO behouden**, zullen de gebruikers die reeds een authentificatietoken hebben dat via SAML wordt verkregen nog als voor authentiek worden beschouwd en hun verzoeken zullen door de oude integratie van SAML worden verpletterd.

Vanuit technisch oogpunt:

1. De Adobe zal een integratie OAuth 2.0 tussen programmeur en MVPD toelaten, ZONDER de integratie van SAML te schrappen.
1. Na het inschakelen zullen alle nieuwe gebruikers OAuth 2.0-stromen gebruiken.
1. De gebruikers reeds voor authentiek verklaard, die reeds een lokaal teken AuthN hebben dat onderwerpidentiteitskaart van SAML bevat, zullen automatisch door Adobe door de integratie van SAML worden verpletterd.
1. Voor de gebruikers in stap 3, zodra hun SAML geproduceerde teken AuthN verloopt, zal de Adobe hen als nieuwe gebruikers behandelen en zich als de gebruikers in stap 2 gedragen.
1. Adobe zal gebruikspatronen evalueren om te bepalen wanneer de integratie van SAML veilig kan worden gedeactiveerd.
