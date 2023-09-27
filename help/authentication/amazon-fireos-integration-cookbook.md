---
title: Amazon FireOS Integration Cookbook
description: Amazon FireOS Integration Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1432'
ht-degree: 0%

---

# Amazon FireOS Integration Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


## Inleiding {#intro}

In dit document worden de workflows voor machtigingen beschreven die de toepassing op hoofdniveau van een programmeur kan implementeren via de API&#39;s die worden weergegeven in de Amazon FireOS AccessEnabler-bibliotheek.

De Adobe Pass-verificatieoplossing voor Amazon FireOS bestaat uiteindelijk uit twee domeinen:

- Het domein UI - dit is de bovenste toepassingslaag die UI uitvoert en de diensten gebruikt die door de bibliotheek AccessEnabler worden verleend om toegang tot beperkte inhoud te verlenen.
- Het domein AccessEnabler - dit is waar de machtigingswerkschema&#39;s in de vorm van worden uitgevoerd:
   - De vraag van het netwerk die aan de backendservers van de Adobe wordt gemaakt
   - Zakelijke en logische regels met betrekking tot de workflows voor verificatie en autorisatie
   - Beheer van diverse bronnen en verwerking van workflowstatus (zoals de tokencache)

Het doel van het domein AccessEnabler is alle ingewikkeldheid van de machtigingswerkschema&#39;s te verbergen, en aan de hogere laagtoepassing (door de bibliotheek AccessEnabler) een reeks eenvoudige machtigingsprimitieven te verstrekken waarmee u de machtigingswerkschema&#39;s uitvoert:

1. De identiteit van de aanvrager instellen
1. Verificatie aanvragen bij een bepaalde identiteitsprovider
1. Autorisatie voor een bepaalde bron controleren en ophalen
1. Afmelden

De het netwerkactiviteit van AccessEnabler vindt in een verschillende draad plaats zodat wordt de draad UI nooit geblokkeerd. Het communicatiekanaal in twee richtingen tussen de twee toepassingsdomeinen moet daarom een volledig asynchroon patroon volgen:

- De toepassingslaag UI verzendt berichten naar het domein AccessEnabler via de API vraag die door de bibliotheek AccessEnabler wordt blootgesteld.
- AccessEnabler antwoordt aan de laag UI door de callback methodes inbegrepen in het protocol AccessEnabler dat de laag UI met de bibliotheek AccessEnabler registreert.

## Machtigingsstromen {#entitlement}

1. [Vereisten](#prereqs)
1. [Stroom opstarten](#startup_flow)
1. [Verificatiestroom](#authn_flow)
1. [Autorisatiestroom](#authz_flow)
1. [Media-stroom weergeven](#media_flow)
1. [Afmelden](#logout_flow)



### A. Vereisten {#prereqs}

1. Maak uw callback-functies:
   - [setRequestorComplete()`](#$setRequestorComplete)

      - geactiveerd door `setRequestor()`, geeft succes of mislukking.     Het succes wijst erop u met machtigingsvraag kunt te werk gaan.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - geactiveerd door `getAuthentication()` alleen als de gebruiker geen provider (MVPD) heeft geselecteerd en nog niet is geverifieerd. De `mvpds` parameter is een array van providers die beschikbaar zijn voor de gebruiker.

   - [setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - geactiveerd door `checkAuthentication()` elke keer. geactiveerd door `getAuthentication()` alleen als de gebruiker al is geverifieerd en een provider heeft geselecteerd.

      - De teruggekeerde status wordt voor authentiek verklaard of niet voor authentiek verklaard, beschrijft de reden een authentificatiemislukking of een logout actie.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Wordt genegeerd in AmazonFireOS SDK, de methode wordt gebruikt op Android-platforms waar deze wordt geactiveerd door `getAuthentication()` nadat de gebruiker een MVPD selecteert.  De `url` parameter verstrekt de plaats van de MVPD login pagina.

   - [` sendTrackingData(event, data)`](#$sendTrackingData)

      - geactiveerd door `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
De `event` parameter geeft aan welke machtigingsgebeurtenis heeft plaatsgevonden;de `data` parameter is een lijst van waarden met betrekking tot de gebeurtenis.

   - [setToken(token, resource)`](#$setToken)

      - geactiveerd door `checkAuthorization()` en `getAuthorization()` na een geslaagde machtiging om een bron te bekijken.
      - De `token` parameter is het media-token van korte duur;de `resource` parameter is de inhoud die de gebruiker mag bekijken.

   - [` tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - geactiveerd door `checkAuthorization()` en `getAuthorization()` na een niet-succesvolle toelating.
      - De `resource` parameter is de inhoud die de gebruiker probeerde te bekijken; de `code` parameter is de foutcode die aangeeft welk type fout is opgetreden; de `description` parameter beschrijft de fout die aan de foutencode wordt geassocieerd.

   - [` selectedProvider(mvpd)`](#$selectedProvider)

      - geactiveerd door `getSelectedProvider()`.
      - De `mvpd` parameter geeft informatie over de provider die door de gebruiker is geselecteerd.

   - [` setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

      - geactiveerd door `getMetadata().`
      - De `metadata` parameter bevat de specifieke gegevens die u hebt aangevraagd; de `key` parameter is de sleutel die wordt gebruikt in de `getMetadata()` en de `arguments` parameter is hetzelfde woordenboek als dat waaraan is doorgegeven `getMetadata()`.

   - [&quot;preauthorisedResources(resources)&quot;](#$preauthResources)

      - geactiveerd door `checkPreauthorizedResources()`.
      - De `authorizedResources` parameter geeft de bronnen weer die de gebruiker mag bekijken.


![](assets/android-entitlement-flows.png)


### B. Startstroom {#startup_flow}

1. Start de toepassing op het hoogste niveau.
1. Adobe Pass-verificatie starten.

   1. Bellen [`getInstance`](#$getInstance) om één enkel geval van de Authentificatie AccessEnabler van Adobe Pass te creëren.

      - **Afhankelijkheid:** Adobe Pass Authentication Native Amazon FireOS-bibliotheek (AccessEnabler)

   1. Bellen` setRequestor()` vaststellen van de identiteit van de programmeur; doorgeven van de `requestorID` en (optioneel) een array met eindpunten voor Adobe Pass-verificatie.

      - **Afhankelijkheid:** Geldige Adobe Pass-verificatieaanvraag-id (werk dit samen met uw Adobe Pass-verificatieaccountmanager.)

      - **Triggers:** setRequestorComplete() callback

   Er kunnen geen aanvragen voor een machtiging worden ingevuld totdat de identiteit van de aanvrager volledig is vastgesteld. Dit betekent in feite dat setRequestor() nog steeds wordt uitgevoerd, alle volgende machtigingsaanvragen (bijvoorbeeld`checkAuthentication()`) zijn geblokkeerd.

   U hebt twee implementatieopties: wanneer de informatie van de aanvrager-identificatie naar de back-endserver is verzonden, kan de UI-toepassingslaag een van de volgende twee methoden kiezen:</p>

   1. Wacht op het in werking stellen van `setRequestorComplete()` callback (deel van de afgevaardigde AccessEnabler).  Deze optie biedt de meeste zekerheid dat `setRequestor()` voltooid, dus wordt het aanbevolen voor de meeste implementaties.
   1. Doorgaan zonder te wachten op de activering van de `setRequestorComplete()` callback en begin met het afgeven van aanvragen voor machtigingen. Deze vraag (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorisedResource, getMetadata, logout) wordt een rij gevormd door de bibliotheek AccessEnabler, die de daadwerkelijke netwerkvraag na het `setRequestor()`. Deze optie kan af en toe worden onderbroken als bijvoorbeeld de netwerkverbinding instabiel is.

1. Bellen [checkAuthentication()](#$checkAuthN) om op een bestaande authentificatie te controleren zonder de volledige stroom van de Authentificatie in werking te stellen.  Als deze vraag slaagt, kunt u aan de stroom van de Vergunning direct te werk gaan.  Zo niet, ga dan door naar de verificatiestroom.

- **Afhankelijkheid:** Een geslaagde oproep tot `setRequestor()` (dit gebiedsdeel is ook op alle verdere vraag van toepassing).

- **Triggers:** setAuthenticationStatus() callback

### C. Verificatiestroom {#authn_flow}

1. Bellen [`getAuthentication()`](#$getAuthN) om de verificatiestroom te starten of om te bevestigen dat de gebruiker al is geverifieerd.

   **Triggers:**

   - De callback setAuthenticationStatus(), als de gebruiker al is geverifieerd.  Ga in dit geval rechtstreeks door naar de [Autorisatiestroom](#authz_flow).
   - De callback displayProviderDialog(), als de gebruiker nog niet is geverifieerd.

1. De gebruiker de lijst met providers presenteren die is verzonden naar `displayProviderDialog()`.
1. Nadat de gebruiker een leverancier selecteert, zal een WebView de leverancierspagina voor gebruiker aan login openen

   >[!NOTE]
   >
   >Op dit punt, heeft de gebruiker de kans om de authentificatiestroom te annuleren. Als dit voorkomt, zal AccessEnabler het interne staat schoonmaken en zal de Stroom van de Authentificatie terugstellen.

1. Op een succesvolle login door de gebruiker, WebView zal sluiten.
1. call `getAuthenticationToken(),` dat AccessEnabler opdraagt om het authentificatietoken van de backendserver terug te winnen.
1. [Optioneel] Bellen [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker wordt gemachtigd om te bekijken. De `resources` parameter is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker.

   **Triggers:** `preAuthorizedResources()` callback\
   **Uitvoeringspunt:** Na de voltooide verificatiestroom

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.


### D. Vergunningsstroom {#authz_flow}

1. Bellen [`getAuthorization()`](#$getAuthZ) de vergunningsstroom in gang te zetten.

   Afhankelijkheid: geldige ResourceID(s) overeengekomen met de MVPD(s).

   **Opmerking:** ResourceIDs zou het zelfde moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn.

1. Verificatie en autorisatie valideren.

   - Als de `getAuthorization()` De vraag slaagt: De gebruiker heeft geldige tokens AuthN en AuthZ (de gebruiker wordt voor authentiek verklaard en gemachtigd om de gevraagde media te bekijken).
   - Indien `getAuthorization()` ontbreekt: Onderzoek de geworpen uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
      - Als het een authentificatie (AuthN) fout was dan herstart de authentificatiestroom.
      - Als het een autorisatiefout (AuthZ) was, dan is de gebruiker niet geautoriseerd om de gevraagde media te bekijken en een soort foutbericht zou aan de gebruiker moeten worden getoond.
      - Als er een ander type fout is opgetreden (verbindingsfout, netwerkfout, enz.) geeft u vervolgens een geschikt foutbericht weer aan de gebruiker.

1. Valideer de token voor korte media.

   Gebruik de Adobe Pass Authentication Media Token Verifier-bibliotheek om het kortstondige mediatoken te verifiëren dat door de `getAuthorization()` oproep hierboven:

   - Als de validatie is gelukt: de gevraagde media afspelen voor de gebruiker.
   - Als de validatie mislukt: de AuthZ-token was ongeldig, wordt de mediaquery geweigerd en wordt een foutbericht weergegeven aan de gebruiker.

1. Ga terug naar de normale stroom van de toepassing.

### E. Mediastroom weergeven {#media_flow}

1. De gebruiker selecteert de media die u wilt weergeven.
1. Zijn de media beveiligd?  Uw toepassing controleert of het geselecteerde medium is beveiligd:
   - Als de geselecteerde media is beveiligd, start uw toepassing de [Autorisatiestroom](#authz_flow) hierboven.
   - Als het geselecteerde medium niet is beveiligd, speelt u de media voor de gebruiker af.

### F. Afmeldingsstroom {#logout_flow}

1. Bellen [`logout()`](#$logout) om de gebruiker af te melden. AccessEnabler ontruimt alle caching waarden en tekenen die door de gebruiker voor huidige MVPD op alle aanvragers worden verkregen die login delen door Enig Teken. Na het ontruimen van het geheime voorgeheugen, richt AccessEnabler een servervraag om de server-zijzittingen schoon te maken.  Merk op dat aangezien de servervraag in SAML kon resulteren die aan IdP (dit staat voor de zittingsschoonmaak op de kant IdP toe) wordt omgeleid, deze vraag moet alle omleidingen volgen. Om deze reden, zal deze vraag binnen een controle WebView worden behandeld, onzichtbaar voor de gebruiker.

   **Opmerking:** De logout stroom verschilt van de authentificatiestroom in die zin dat de gebruiker niet wordt vereist om met WebView op om het even welke manier in wisselwerking te staan. Aldus is het mogelijk (en geadviseerd) om de controle WebView onzichtbaar (d.w.z.: verborgen) tijdens het logout proces te maken.
