---
title: REST API Cookbook (client-naar-server)
description: Testen van API-cookboekclient naar server.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 0%

---

# (Verouderd) REST API Cookbook (client-naar-server) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht {#overview}

Dit document bevat stapsgewijze instructies voor het engineeringteam van een programmeur om een &quot;slim apparaat&quot; (spelconsole, smart TV-app, set top box, enz.) te integreren met Adobe Pass-verificatie via REST API-services. Deze client-to-server-benadering, waarbij REST API&#39;s worden gebruikt in plaats van een client-SDK, maakt bredere ondersteuning mogelijk van verschillende platforms waarvoor het ontwikkelen van een aanzienlijk aantal unieke SDK&#39;s niet haalbaar is. Voor een breed technisch overzicht van hoe de Klantloze oplossing werkt, zie het [ Klantloze Technische Overzicht ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md).


Deze aanpak vereist twee componenten (streaming app en AuthN app) om de vereiste stromen te voltooien: opstarten, registreren, autoriseren en weergavemedia in de streaming app en de verificatiestroom in uw AuthN-app.

### Draaimechanisme

De Adobe Pass Authentificatie REST API wordt geregeerd door a [ het Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Componenten {#components}

In een werkende cliënt-aan-server oplossing zijn de volgende componenten betrokken:



| Type | Component | Beschrijving |
| --- | --- | --- |
| Streaming apparaat | Streaming-app | De programmeertoepassing die op het het stromen apparaat van de gebruiker verblijft en voor authentiek verklaarde video speelt. |
| | \[Optioneel\] AuthN-module | als het Streamen Apparaat een Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Module AuthN verantwoordelijk voor het voor authentiek verklaren van de gebruiker op MVPD IdP. |
| \[Optioneel\] AuthN-apparaat | AuthN App | als het Streaming Apparaat geen Agent van de Gebruiker (d.w.z. Browser van het Web) heeft, is de Toepassing AuthN een Toepassing van het Web van de Programmer die van een afzonderlijk apparaat van de gebruiker gebruikend Webbrowser wordt betreden. |
| Adobe-infrastructuur | Adobe Pass Service | De dienst die met de Dienst van MVPD IdP en AuthZ integreert en authentificatie en vergunningsbesluiten verstrekt. |
| MVPD-infrastructuur | MVPD IdP | Een eindpunt van MVPD dat op referentie-gebaseerde authentificatiedienst verleent om de identiteit van hun gebruiker te bevestigen. |
| | MVPD AuthZ Service | Een eindpunt van MVPD dat vergunningsbesluiten verstrekt die op gebruikersabonnementen, ouderlijke controles, enz. worden gebaseerd. |

## Stromen{#flows}

### Dynamische clientregistratie (DCR)

Adobe Pass gebruikt DCR om clientcommunicatie tussen een programmeertoepassing of server en de Adobe Pass-services te beveiligen. De stroom DCR is afzonderlijk en wordt beschreven in het [ Dynamische Overzicht van de Registratie van de Cliënt ](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) documentatie.


### Streaming (slim apparaat) App Flows

![](../../../../assets/smart-device-app-flow.png)

#### Stroom opstarten

1. Uw app start en laadt de eerste gebruikersinterface.

2. Vraag een apparaat-id op of genereer deze.

3. Geef een vraag van de Controle-authentificatie uit om te zien of is het apparaat reeds voor authentiek verklaard.  Bijvoorbeeld: [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. Als de `checkauthn` vraag slaagt, ga aan de Stroom van de Vergunning vanaf Stap 2 te werk.  Start de Registratiestroom als dit mislukt.



#### Registratiestroom

1. U krijgt een registratiecode en URL waarmee uw gebruiker toegang krijgt tot de tweede toepassing voor schermaanmelding en u kunt deze aan de gebruiker presenteren:

   a. Verzend een verzoek van de POST naar de Dienst van de Code van de Registratie van de Adobe, die een gehakt apparatenidentiteitskaart en een &quot;Registratie URL&quot;overgaat.  Bijvoorbeeld: [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b. De geretourneerde registratiecode en URL aan de gebruiker presenteren.

   c. Geef de gebruiker de opdracht om naar een apparaat te schakelen dat geschikt is voor internet, naar de URL te navigeren en vervolgens de registratiecode in te voeren.



#### Autorisatiestroom

1. De gebruiker keert van het tweede scherm terug app en drukt op de &quot;Doorgaan&quot; knoop op uw apparaat. U kunt ook een opiniepeilingsmechanisme implementeren om de verificatiestatus te controleren, maar de Adobe Pass-verificatie raadt de methode voor de knop Doorgaan aan tijdens de opiniepeiling. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Bijvoorbeeld: [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. Verzend een verzoek van de GET naar de de vergunningsdienst van de Authentificatie van Adobe Pass om vergunning in werking te stellen. Bijvoorbeeld: `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Als de reactie op succes wijst: De gebruiker heeft een geldig teken AuthN EN de gebruiker wordt gemachtigd om op de gevraagde media (er is een geldig teken AuthZ voor deze gebruiker) te letten.

* Als de reactie op mislukking wijst: Onderzoek de geworpen Uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:

   * Als het een fout AuthN was dan herstart de Stroom van de Registratie.

   * Als het een AuthZ-fout was, is de gebruiker niet geautoriseerd om de gevraagde media te bekijken en moet een foutbericht worden weergegeven aan de gebruiker.

   * Als er een andere fout is opgetreden (verbindingsfout, netwerkfout, enz.), geeft u een geschikt foutbericht weer aan de gebruiker.



#### Media-stroom weergeven

1. Mediakeuzen presenteren. De gebruiker selecteert de media die u wilt weergeven.

2. Zijn de media beveiligd?

   a. Uw app controleert of de media is beveiligd.

   b. Als het medium is beveiligd, wordt de autorisatie door uw app gestart
(AuthZ) Stroom hierboven.

   c. Als het medium niet is beveiligd, kunt u het medium voor de
gebruiker.

3. De media afspelen.


### AuthN (tweede scherm) App Flow

![](../../../../assets/secnd-screen-authn-flow.png)

1. Krijg een lijst van MVPDs voor deze gebruiker. Bijvoorbeeld: [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. Start de verificatiestroom.  Bijvoorbeeld: [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. Controleer of de verificatie is gelukt. Bijvoorbeeld:[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. Stuur de gebruiker terug naar de toepassing van het slimme apparaat om de autorisatiestroom te voltooien.

## Single Sign-On voor partners {#partner-sso}

Sommige apparaten verstrekken specifieke steun voor Partner Enige Sign-On (SSO):

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Platform Single Sign-On {#platform-sso}

Sommige apparaten bieden specifieke ondersteuning voor Platform Single Sign-On (SSO):

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)
* [Roku SSO](../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)

## TempPass en Promotional TempPass voor REST API {#temppass}

Voor TempPass- en Promotional TempPass-implementaties waarbij de gebruiker geen referenties hoeft in te voeren, kan verificatie rechtstreeks in de streamingtoepassing worden geïmplementeerd.

**om deze API te gebruiken moet de Streaming App van de uniciteit van apparatenidentiteitskaart verzekeren aangezien dit voor het identificeren van het teken, samen met de facultatieve extra gegevens wordt gebruikt.**


![](../../../../assets/temp-pass-promo-temppass.png)
