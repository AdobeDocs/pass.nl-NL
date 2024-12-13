---
title: Android SDK Cookbook
description: Android SDK Cookbook
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1704'
ht-degree: 0%

---

# (Verouderd) Android SDK Cookbook {#android-sdk-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

## Inleiding {#intro}

In dit document worden de workflows voor machtigingen beschreven die de toepassing op hoofdniveau van een programmeur kan implementeren via de API&#39;s die worden weergegeven door de Android AccessEnabler-bibliotheek.


De Adobe Pass-verificatieoplossing voor Android is uiteindelijk verdeeld in twee domeinen:

- Het domein UI - dit is de bovenste toepassingslaag die UI uitvoert en de diensten gebruikt die door de bibliotheek AccessEnabler worden verleend om toegang tot beperkte inhoud te verlenen.
- Het domein AccessEnabler - dit is waar de machtigingswerkschema&#39;s in de vorm van worden uitgevoerd:
   - De vraag van het netwerk die aan de backendservers van de Adobe wordt gemaakt
   - Zakelijke en logische regels met betrekking tot de workflows voor verificatie en autorisatie
   - Beheer van diverse bronnen en verwerking van workflowstatus (zoals de tokencache)

Het doel van het domein AccessEnabler is alle ingewikkeldheid van de machtigingswerkschema&#39;s te verbergen, en aan de hogere laagtoepassing (door de bibliotheek AccessEnabler) een reeks eenvoudige machtigingsprimitieven te verstrekken waarmee u de machtigingswerkschema&#39;s uitvoert:

1. Stel de identiteit van de aanvrager in.

1. Controleer en krijg authentificatie tegen een bepaalde identiteitsleverancier.

1. Controleer en krijg vergunning voor een bepaalde bron.

1. Afmelden.

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

     Wordt geactiveerd door `setRequestor()` en retourneert een geslaagde of mislukte bewerking.\
     Het succes wijst erop u met machtigingsvraag kunt te werk gaan.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Wordt alleen geactiveerd door `getAuthentication()` als de gebruiker geen provider (MVPD) heeft geselecteerd en nog niet is geverifieerd.\
     De parameter `mvpds` is een array van providers die beschikbaar zijn voor de gebruiker.

   - [setAuthenticationStatus(status, errorCode)`](#$setAuthNStatus)

     Wordt telkens geactiveerd door `checkAuthentication()` .\
     Wordt alleen geactiveerd door `getAuthentication()` als de gebruiker al is geverifieerd en een provider heeft geselecteerd.

     De geretourneerde status is geslaagd of mislukt. De foutcode beschrijft het type fout.

   - [navigateToUrl(url)](#$navigateToUrl)

     Wordt geactiveerd door `getAuthentication()` nadat de gebruiker een MVPD selecteert. De parameter `url` biedt de locatie van de MVPD-aanmeldingspagina.

   - [` sendTrackingData(event, data)`](#$sendTrackingData)

     Wordt geactiveerd door `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()` .\
     De parameter `event` geeft aan welke gebeurtenis entitlement heeft plaatsgevonden. De parameter `data` is een lijst met waarden die betrekking hebben op de gebeurtenis.

   - [setToken(token, resource)`](#$setToken)

     Wordt geactiveerd door `checkAuthorization()` en `getAuthorization()` nadat u een resource hebt bekeken.\
     De parameter `token` is het kortstondige media-token; de parameter `resource` is de inhoud die de gebruiker mag bekijken.

   - [` tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

     Wordt geactiveerd door `checkAuthorization()` en `getAuthorization()` nadat de autorisatie is mislukt.\
     De parameter `resource` is de inhoud die de gebruiker probeerde te bekijken. De parameter `code` is de foutcode die aangeeft welk type fout is opgetreden. De parameter `description` beschrijft de fout die aan de foutcode is gekoppeld.

   - [` selectedProvider(mvpd)`](#$selectedProvider)

     Wordt geactiveerd door `getSelectedProvider()` .\
     De parameter `mvpd` biedt informatie over de provider die door de gebruiker is geselecteerd.

   - [` setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

     geactiveerd door `getMetadata().`\
     De parameter `metadata` bevat de specifieke gegevens die u hebt aangevraagd. De parameter `key` is de sleutel die wordt gebruikt in de aanvraag `getMetadata()` en de parameter `arguments` is hetzelfde woordenboek dat is doorgegeven aan `getMetadata()` .

   - [&quot;preauthorisedResources(resources)&quot;](#$preauthResources)

     Wordt geactiveerd door `checkPreauthorizedResources()` .\
     De parameter `authorizedResources` geeft de bronnen weer die de gebruiker mag bekijken.


![](../../../../assets/android-entitlement-flows.png)


### B. Startstroom {#startup_flow}

1. Start de toepassing op het hoogste niveau.
1. Adobe Pass-verificatie starten

   a. Vraag [`getInstance`](#$getInstance) aan om één instantie van Adobe Pass Authentication AccessEnabler te maken.

   - **Afhankelijkheid:** Eigen de Authentificatie van Adobe Pass
Android Library (AccessEnabler)

   b. Vraag ` setRequestor()` om de identificatie van de Programmer te vestigen; ga in de 1} van de Programmer en (facultatief) een serie van de eindpunten van de Authentificatie van Adobe Pass over.`requestorID`

   - **Afhankelijkheid:** Geldige VraagID van de Authentificatie van Adobe Pass\
     (Gebruik hiervoor Adobe Pass Authentication Account Manager.)

   - **Trekkers:** setRequestorComplete () callback

   | OPMERKING |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | Er kunnen geen aanvragen voor een machtiging worden ingevuld totdat de identiteit van de aanvrager volledig is vastgesteld. Dit betekent in feite dat setRequestor() nog steeds wordt uitgevoerd, alle volgende aanvragen voor machtigingen (bijvoorbeeld `checkAuthentication()` ) worden geblokkeerd.<br><br> u hebt twee implementatieopties: Zodra de informatie van de verzoeksidentificatie wordt verzonden naar de achterste server, kan de toepassingslaag UI één van de twee volgende benaderingen kiezen:<br><br> 1.  Wacht op het teweegbrengen van `setRequestorComplete()` callback (deel van de afgevaardigde AccessEnabler).  Deze optie biedt de meeste zekerheid dat `setRequestor()` is voltooid en wordt daarom aangeraden voor de meeste implementaties.<br> 2.  Ga verder zonder te wachten op het activeren van de callback van `setRequestorComplete()` en geef aanvragen voor machtigingen af. Deze vraag (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorisedResource, getMetadata, logout) wordt een rij gevormd door de bibliotheek AccessEnabler, die de daadwerkelijke netwerkvraag na `setRequestor(). ` zal maken Deze optie kan nu en dan worden onderbroken als bijvoorbeeld, de netwerkverbinding instabiel is. |

1. Vraag [ checkAuthentication () ](#$checkAuthN) om een bestaande authentificatie te controleren zonder de volledige stroom van de Authentificatie in werking te stellen.   Als deze vraag slaagt, kunt u aan de stroom van de Vergunning direct te werk gaan.  Zo niet, ga dan door naar de verificatiestroom.

   - **Afhankelijkheid:** Een succesvolle vraag aan `setRequestor()` (dit gebiedsdeel is eveneens op alle verdere vraag van toepassing).

   - **Trekkers:** setAuthenticationStatus () callback

### C. Verificatiestroom {#authn_flow}

1. Vraag [`getAuthentication()`](#$getAuthN) om de authentificatiestroom in werking te stellen, of bevestiging te krijgen dat de gebruiker reeds voor authentiek verklaard is.\
   **Trekkers:**
   - De callback setAuthenticationStatus(), als de gebruiker al is geverifieerd.  In dit geval, ga direct aan de [ Stroom van de Vergunning ](#authz_flow) te werk.
   - De callback displayProviderDialog(), als de gebruiker nog niet is geverifieerd.

1. Geef de gebruiker de lijst met providers weer die naar `displayProviderDialog()` is verzonden.

1. Nadat de gebruiker een provider selecteert, verkrijgt u de URL van de MVPD van de gebruiker via de callback van `navigateToUrl()` .  Open een WebView en richt die controle WebView aan URL.

1. Via WebView die in de vorige stap wordt geconcretiseerd, landt de gebruiker op de MVPD login pagina en input login geloofsbrieven. Verscheidene omleidingsverrichtingen vinden binnen WebView plaats.

   **Nota:** Op dit punt, heeft de gebruiker de kans om de authentificatiestroom te annuleren. Als dit voorkomt, is uw UI laag verantwoordelijk voor het informeren van AccessEnabler over deze gebeurtenis door `setSelectedProvider()` met `null` als parameter te roepen. Dit staat AccessEnabler toe om het interne staat op te schonen en de Stroom van de Authentificatie terug te stellen.

1. Wanneer de gebruiker zich met succes heeft aangemeld, detecteert de toepassingslaag het laden van een aangepaste omleidings-URL (d.w.z.: `http://adobepass.android.app`). Deze aangepaste URL is in feite een ongeldige URL die niet is bedoeld voor het laden van de WebView. Het is een signaal dat de Stroom van de Authentificatie heeft voltooid, en dat WebView moet worden gesloten.

1. Sluit de controle WebView en vraag `getAuthenticationToken()`, die AccessEnabler opdraagt om het authentificatietoken van de backendserver terug te winnen.

1. [ Facultatieve ] Vraag [`checkPreauthorizedResources(resources)`](#$checkPreauth) om te controleren welke middelen de gebruiker aan mening wordt gemachtigd. De parameter `resources` is een array met beveiligde bronnen die is gekoppeld aan het verificatietoken van de gebruiker.\
   **Trekkers:** `preAuthorizedResources()` callback\
   **het punt van de Uitvoering:** na de voltooide Stroom van de Authentificatie

1. Ga door naar de machtigingsstroom als de verificatie is gelukt.



### D. Vergunningsstroom {#authz_flow}

1. Vraag [ getAuthorization () ](#$getAuthZ) om de vergunning in werking te stellen
stroom.

   Afhankelijkheid: geldige ResourceID(&#39;s) overeengekomen met de MVPD(s).

   **Nota:** ResourceIDs zou het zelfde moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn.

1. Verificatie en autorisatie valideren.

   - Als de `getAuthorization()` vraag slaagt: De gebruiker heeft geldige tokens AuthN en AuthZ (de gebruiker wordt voor authentiek verklaard en gemachtigd om de gevraagde media te bekijken).
   - Als `getAuthorization()` ontbreekt: Onderzoek de geworpen uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
      - Als het een authentificatie (AuthN) fout was dan herstart de authentificatiestroom.
      - Als het een autorisatiefout (AuthZ) was, dan is de gebruiker niet geautoriseerd om de gevraagde media te bekijken en een soort foutbericht zou aan de gebruiker moeten worden getoond.
      - Als er een ander type fout is opgetreden (verbindingsfout, netwerkfout, enz.), geeft u een geschikt foutbericht weer voor de gebruiker.

1. Valideer de token voor korte media.\
   Gebruik de Adobe Pass Authentication Media Token Verifier-bibliotheek om het kortstondige mediatoken te verifiëren dat door de bovenstaande `getAuthorization()` oproep wordt geretourneerd:

   - Als de validatie is gelukt: de gevraagde media afspelen voor de gebruiker.
   - Als de validatie mislukt: de AuthZ-token was ongeldig, wordt de mediaquery geweigerd en wordt een foutbericht weergegeven aan de gebruiker.

1. Ga terug naar de normale stroom van de toepassing.

### E. Mediastroom weergeven {#media_flow}

1. De gebruiker selecteert de media die u wilt weergeven.
2. Zijn de media beveiligd?  Uw toepassing controleert of het geselecteerde medium is beveiligd:
- Als de geselecteerde media beschermd is, begint uw toepassing de [ Stroom van de Vergunning ](#authz_flow) hierboven.
- Als het geselecteerde medium niet is beveiligd, speelt u de media voor de gebruiker af.



### F. Afmeldingsstroom {#logout_flow}

1. Roep [`logout()`](#$logout) aan om de gebruiker af te melden.\
   AccessEnabler wist alle in het cachegeheugen opgeslagen waarden en tokens voor de huidige MVPD voor de huidige aanvrager en ook voor aanvragers met Single Sign On. Na het ontruimen van het geheime voorgeheugen, richt AccessEnabler een servervraag om de server-zijzittingen schoon te maken.  Merk op dat aangezien de servervraag in SAML kon resulteren die aan IdP (dit staat voor de zittingsschoonmaak op de kant IdP toe) wordt omgeleid, deze vraag moet alle omleidingen volgen. Om deze reden, moet deze vraag binnen een controle worden behandeld WebView.

   a. Na het zelfde patroon zoals het authentificatiewerkschema, doet het domein AccessEnabler een verzoek aan de UI toepassingslaag (via `navigateToUrl()` callback) om tot een controle te leiden WebView en die controle op te dragen om URL van het logout eindpunt op de backendserver te laden.

   b. Opnieuw, moet UI de activiteit van de controle controleren WebView en het ogenblik ontdekken wanneer de controle, aangezien het door verscheidene omleidingen gaat, de douane URL van de toepassing laadt (d.w.z.: `http://adobepass.android.app/`). Zodra deze gebeurtenis plaatsvindt, sluit de UI toepassingslaag WebView en het logout proces volledig is.

   **Nota:** de logout stroom verschilt van de authentificatiestroom in die zin dat de gebruiker niet wordt vereist om met WebView op om het even welke manier in wisselwerking te staan. De toepassingslaag UI gebruikt een WebView om ervoor te zorgen dat alle omleidingen worden gevolgd. Aldus is het mogelijk (en geadviseerd) om de controle WebView onzichtbaar (d.w.z. verborgen) tijdens het logout proces te maken.



### Gebruikersstromen voor Login met veelvoudige MVPDs en Logout {#user_flows}

[ hier ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) u hebt een document beschrijvend het gedrag wanneer het gebruiken van veelvoudige MVPDs en wat gebeurt wanneer de gebruiker zich uit een toepassing afmeldt.

Het beschreven gedrag is beschikbaar als u Android SDK versie >= 2.0.0 gebruikt.
