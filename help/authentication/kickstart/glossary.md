---
title: Verklarende woordenlijst
description: Verklarende woordenlijst
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Verklarende woordenlijst {#glossary}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## AccessEnabled {#accessEnabler}

De clientcomponent van Adobe Pass Authentication. Adobe Pass Authentication biedt een AccessEnabler-bibliotheek voor elk ondersteund platform.

## AuthN {#authn}

Wordt gebruikt als steno voor &quot;verificatie&quot;, zoals in &quot;AuthN Token&quot; of &quot;AuthN Flow&quot;.


## AuthN Token{#authn-token}

Verificatietoken, gegenereerd door Adobe Pass-verificatie nadat een gebruiker is geverifieerd met een MVPD. Afhankelijk van het integratieplatform van de programmeur wordt het token opgeslagen op het apparaat van de gebruiker of op de Adobe Pass-verificatieservers.

## AuthZ {#authz}

Wordt gebruikt als steno voor &quot;autorisatie&quot;, zoals in &quot;AuthZ Token&quot; of &quot;AuthZ Flow&quot;.

## AuthZ Token {#authz-token}

Verificatietoken, gegenereerd door Adobe Pass-verificatie nadat een gebruiker is gemachtigd om beveiligde inhoud weer te geven. Het teken AuthZ wordt opgeslagen op de servers van de Authentificatie van Adobe Pass, en wordt gebruikt om a [ Korte-levende Token van Media ](#short-lived-token) te produceren.

## Kanaal-id (afgekeurd) {#channel_id}

Voormalige term voor bron-id.

## Clientloze API {#clientless-api}

Een integratie van de Authentificatie van Adobe Pass die de Webdiensten in plaats van de component van de cliënt AccessEnabler gebruikt.

## Apparaat-id {#device-id}

Identificeert een apparaat (zoals een telefoon, tablet, enz.) uniek binnen de Authentificatie van Adobe Pass. Deze id is verkregen/opgegeven door de toepassing van de programmeur.


## Entitlement Flow{#entitlement_flow}

De term die in de documentatie van de Authentificatie van Adobe Pass wordt gebruikt om naar het volledige proces te verwijzen om een apparaat/gebruiker met de Authentificatie van Adobe Pass te registreren, een gebruiker met een MVPD voor authentiek te verklaren, een middel voor een gebruiker te machtigen, en een gebruiker te registreren.


## GUID {#guid}

Zie [ identiteitskaart van de Gebruiker ](#user-id).

## IdP {#idp}

Identificeer Leverancier; synoniem met MVPD in de context van de rol van MVPD in een integratie van de Authentificatie van Adobe Pass. (Klanten moeten hun identiteit verifiëren via de aanmeldingspagina van hun Pay TV-provider.)

## Verificator mediatokens {#media-token-verifier}

Een door de Adobe verschafte bibliotheek die door programmeurs wordt gebruikt om het kortstondige mediatoken te verifiëren dat door de Verificatie van Adobe Pass wordt gegenereerd nadat een machtigingsstroom met succes is voltooid.

## MVPD {#mvpd}

Multi-channel Video Programming Distributor; synoniem met &quot;Pay TV Provider&quot;.

## MVPD-id {#mvpd-id}

Zie [ identiteitskaart van de Gebruiker ](#user-id).

## Partner-id {#partner-id}

Een herkenningsteken dat de Adobe tot MVPDs overgaat, die het gebruikt om namens de Authentificatie van Adobe Pass om authentificatie te identificeren. Soms wordt het gebruikt voor het vormen van hun UIs voor bepaalde Programmers, soms is het het zelfde over alle Programmers, hangt het van de behoeften van MVPD af.

## Tv-provider betalen {#pay-tv-provider}

Synoniem met [ MVPD ](#mvpd).

## Programmeur {#programmer}

Gelijk aan &#39;contentprovider&#39;, &#39;account&#39;, &#39;kanaal&#39;, &#39;serviceprovider&#39;, &#39;merk&#39; enzovoort.

## Proxy MVPD {#proxy-mvpd}

Een MVPD die de identiteitsdiensten voor andere MVPDs verleent; direct geïntegreerd met de Authentificatie van Adobe Pass.

## Proxied MVPD {#proxied-mvpd}

Een MVPD die geen directe integratie met Adobe SP heeft, maar door een Volmacht MVPD geïntegreerd.

## Id van aanvrager {#requestor-id}

Identificeert uniek a [ Programmer ](#programmer) (een rekening, een merk, een kanaal, of een bezit) binnen de Authentificatie van Adobe Pass. Deze id wordt bepaald door de programmeur en de Adobe tijdens de eerste installatie van de account. Op het Web, wordt identiteitskaart van de Aanvrager geassocieerd met een reeks gewhitelisteerde domeinen; om het even welke vraag die een identiteitskaart van een buitendomein gebruikt zal worden geweigerd. Programmeurs gebruiken de aanvrager-id ook voor analyses. Meestal is er slechts één aanvrager-id per programmeur. Een andere functie met betrekking tot de id van de aanvrager is dat de programmeur een openbaar certificaat aan de Adobe moet overleggen, aangezien de API-aanroep setRequestor verwacht dat gecodeerde gegevens worden verzonden en gebruikt om de programmeur te verifiëren in het Adobe Pass-verificatiesysteem.

## Resource ID {#resource-id}

Een koord of het middel mRSS dat a [ Programmer ](#programmer) aan MVPDs identificeert. Het wordt overeengekomen tussen de Programmer en MVPDs; De Authentificatie van Adobe Pass gaat identiteitskaart van het Middel door onaangetast, zodat moet het voor alle MVPDs het zelfde zijn. Een programmeur kan veelvoudige middel IDs gebruiken zolang MVPDs zich van bewust is wat elke identiteitskaart vertegenwoordigt.

## SessionGUID {#sessionGUID}

Zie [ identiteitskaart van de Gebruiker ](#user-id).

## Token voor kortstondige media {#short-lived-token}

Deze token wordt gegenereerd door Adobe Pass Authentication zodra het machtigingsproces voor een bepaalde gebruiker met succes is voltooid. Het token wordt doorgegeven aan de programmeur, die de Verificatietoken van Adobe Pass-verificatie gebruikt op het token voor kortlevende media om de beveiliging van het machtigingsproces te controleren.

## Slim apparaat {#smart-device}

Een term die wordt gebruikt in de Adobe Pass Authentication-documentatie voor verwijzingen naar set-top boxes, spelconsoles en smart TV&#39;s. Dit zijn apparaten die voorzien van een netwerkmogelijkheden maar niet geschikt zijn om Web-pagina&#39;s terug te geven.

## SP{#sp}

De Dienstverlener; dit verwijst gewoonlijk naar de *rol* van SP, die door de Authentificatie van Adobe Pass wordt gespeeld, handelend namens een Programmer in een integratie met [ MVPD ](#mvpd).

## Temperatuurcontrole {#temp-pass}

Een functie waarmee programmeurs tijdelijk gratis toegang kunnen bieden tot betaalinhoud. De toegang is per-Aanvrager, voor een programmeur-gespecificeerde periode.

## TTL {#ttl}

Tijd om te leven. Dit is de opgegeven tijdsduur dat een token geldig is.

## TVE {#tve}

Televisie overal.

## Gebruikersnaam {#user-id}

Hiermee wordt de gebruiker van de app van een programmeur op unieke wijze geïdentificeerd, maar wordt de gebruiker van de MVPD geregistreerd. Beschikbaar in verschillende formulieren voor verschillende gebruiksgevallen. Zie [ Begrijpend Gebruiker IDs in het Overzicht van de Programmer ](/help/authentication/kickstart/programmer-overview.md#user-ids).

## Lijst van gewenste personen {#whitelist}

Een lijst met domeinen die als legitiem zijn aangewezen voor communicatie met Adobe Pass-verificatie.

## XSTS-token {#xsts-token}

Een beveiligingstoken dat door Microsoft for Xbox console-app-ontwikkeling wordt uitgegeven, wordt gebruikt bij de Xbox/Adobe Pass-verificatie-integratie.
