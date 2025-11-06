---
title: JavaScript SDK Cookbook
description: JavaScript SDK Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# (Verouderd) JavaScript SDK Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#intro}

In dit document worden de workflows voor machtigingen beschreven die de toepassing op hoofdniveau van een programmeur implementeert voor JavaScript-integratie met de Adobe Pass-verificatieservice. Koppelingen naar de JavaScript API-naslaggids worden in de hele reeks weergegeven.

Merk ook op dat de [&#x200B; Verwante sectie van de Informatie &#x200B;](#related) a omvat
een koppeling maken naar een set JavaScript-codevoorbeelden.

## Machtigingsstromen {#entitlement}

1. [Vereisten](#prereq)
2. [Stroom opstarten](#startup)
3. [Verificatiestroom](#authn)
4. [Autorisatiestroom](#authz)
5. [Media-stroom weergeven](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Vereisten {#prereq}

**Afhankelijkheden:**

- Adobe Pass Authentication Library (AccessEnabler), werkt u samen met uw Adobe Pass Authentication Account Manager om dit te regelen.
- Geldige aanvraag-ID voor Adobe Pass-verificatie, werk dit samen met uw accountmanager voor Adobe Pass-verificatie.

Maak uw callback-functies:

- `entitlementLoaded`
</br>

**Trekker:** AccessEnabler heeft geladen en gebeëindigd initialiserend.

- `displayProviderDialog(mvpds)`

  **Trekker:** `getAuthentication(),` slechts als de gebruiker geen leverancier (een MVPD) heeft geselecteerd, en nog niet voor authentiek verklaard
De parameter mvpds is een array van providers die beschikbaar zijn voor de gebruiker.

- `setAuthenticationStatus(status, errorcode)`

  **Trekker:**
   - `checkAuthentication()` telkens.
   - `getAuthentication()` alleen als de gebruiker al is geverifieerd en een provider heeft geselecteerd.

  De geretourneerde status is geslaagd of mislukt. De foutcode beschrijft het type fout.

- `createIFrame(width, height)`

  **Trekker:** `setSelectedProvider(providerID)`, slechts als de geselecteerde leverancier aan vertoning in een IFrame wordt gevormd.

  >[!NOTE]
  >
  >Een leverancier wordt gevormd om zijn authentificatiescherm als of omleiding of in een iFrame terug te geven, en de programmeur moet van beide rekenschap geven.

- `sendTrackingData(event, data)`

  **Trekkers:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  De parameter `event` geeft aan welke gebeurtenis entitlement heeft plaatsgevonden. De parameter `data` is een lijst met waarden die betrekking hebben op de gebeurtenis.
- `setToken(token, resource)`
  **Trekker:** `checkAuthorization()` en `getAuthorization()` na een succesvolle vergunning om een middel te bekijken.   De parameter `token` is het kortstondige media-token; de parameter `resource` is de inhoud die de gebruiker mag bekijken.

- `tokenRequestFailed(resource, code, description)`
  **Trekker:**`checkAuthorization()` en `getAuthorization()` na een onsuccesvolle vergunning.\
  De parameter `resource` is de inhoud die de gebruiker probeerde te bekijken. De parameter `code` is de foutcode die aangeeft welk type fout is opgetreden. De parameter `description` beschrijft de fout die aan de foutcode is gekoppeld.

- `selectedProvider(mvpd)`

  **Trigger:** [`getSelectedProvider()`] (#$getSelProv de `mvpd` parameter verstrekt informatie over de leverancier die door wordt geselecteerd
de gebruiker.

- `setMetadataStatus(metadata, key, arguments)`

  **Trekker:** `getMetadata().`\
  De `metadata` parameter verstrekt de specifieke gegevens u vroeg; de belangrijkste parameter is de sleutel die in het `getMetadata()` verzoek wordt gebruikt; en de `arguments` parameter is het zelfde woordenboek dat tot `getMetadata()` werd overgegaan.


## &#x200B;2. Opstartstroom

**I. Laad JavaScript AccessEnabler:**

**voor het Opvoeren Profiel**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

of...

**voor het Profiel van de Productie**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Trekkers:** wanneer de initialisering volledig is, Adobe Pass
verificatie roept de callback-functie van `entitlementLoaded()` aan. Dit is het ingangspunt aan de communicatie van uw toepassing met AccessEnabler.


**II.** Vraag `setRequestor()` om te vestigen
identiteit van de programmeur; geef de programmeur `requestorID` door en
(Optioneel) Een array met eindpunten voor Adobe Pass-verificatie.

**Trekkers:** niets, maar laat `displayProviderDialog()` toe om worden geroepen wanneer nodig.


**III.** Vraag `checkAuthentication()` om voor een bestaande authentificatie te controleren zonder de volledige [ authentificatiestroom ] in werking te stellen.  Als deze aanroep slaagt, kunt u rechtstreeks naar de `authorization flow` gaan.  Als dat niet het geval is, gaat u verder naar `authentication flow` .

**Afhankelijkheid:** Een succesvolle vraag aan `setRequestor()` (dit gebiedsdeel is eveneens op alle verdere vraag van toepassing).

**Trekkers:** `setAuthenticationStatus()` callback

</br>

## &#x200B;3. Verificatiestroom </span>


**Afhankelijkheid:** Een succesvolle vraag aan `setRequestor()` (dit gebiedsdeel is eveneens op alle verdere vraag van toepassing).


Vraag `getAuthentication()` om de authentificatiestatus OF te krijgen om de stroom van de leveranciersauthentificatie teweeg te brengen.

**Trekkers:**

- `displayProviderDialog()` als de gebruiker nog niet voor authentiek is verklaard
- `setAuthenticationStatus()` als verificatie al heeft plaatsgevonden

De voltooiing van de authentificatiestroom wordt bereikt wanneer AccessEnabler `setAuthenticationStatus()` met `isAuthenticated == 1` roept.

## &#x200B;4. Vergunningsstroom {#authz}

**Afhankelijkheden:**

- Een geslaagde aanroep van `setRequestor()` (deze afhankelijkheid geldt ook voor alle volgende aanroepen).
- Geldige ResourceID(s) overeengekomen met de MVPD(s). Merk op dat ResourceIDs het zelfde zou moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn.

Roep `getAuthorization()` aan en geef ResourceID voor de gevraagde media door. Een geslaagde vraag zal een Korte Symbolische waarde van Media terugkeren, die bevestigt dat de gebruiker wordt gemachtigd om de gevraagde media te bekijken.

- Als de vraag overgaat: De gebruiker heeft een geldig teken AuthN en de gebruiker wordt gemachtigd om op de gevraagde media te letten.
- Als de vraag ontbreekt: Onderzoek de geworpen Uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
- Als de vraag een fout AuthN toen was begin de Stroom AuthN opnieuw.
- Als de vraag een fout AuthZ toen was is de gebruiker niet geautoriseerd om op de gevraagde media te letten en één of ander soort foutenmelding zou aan de gebruiker moeten worden getoond.
- Als er een andere fout is opgetreden (verbindingsfout, netwerkfout, enz.), geeft u een geschikt foutbericht weer aan de gebruiker.

Gebruik de Verifier van het Token van Media om shortMediaToken te bevestigen die van een succesvolle `getAuthorization()` vraag is teruggekeerd.


**Afhankelijkheid:** de Korte Symbolische Verifier van Media (inbegrepen met de
AccessEnabler-bibliotheek)

- Als de validatie slaagt: geef de gewenste media voor de gebruiker weer/afspelen.
- Als het ontbreekt: De token AuthZ was ongeldig, zou de mediaaanvraag moeten worden geweigerd en zou een foutbericht aan de gebruiker moeten worden getoond.

## &#x200B;5. Mediastroom weergeven {#logout}

- De gebruiker selecteert de media die u wilt weergeven.
   - Is media beveiligd?
      - Uw app controleert of het medium is beveiligd:
         - Als de media is beveiligd, start uw app de bovenstaande workflow voor autorisatie (AuthZ).
         - Als het medium niet is beveiligd, gaat u verder met de Media-doorloop weergeven.
         - Media afspelen

## De bezoeker-id configureren {#visitorID}

Het vormen a [&#x200B; Experience Cloud bezoekorID &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=nl-NL) waarde is zeer belangrijk van het analytische standpunt. Zodra een EC bezoekerID-waarde is ingesteld, zal de SDK deze informatie samen met elk netwerkgesprek verzenden en zal de Adobe Pass Authentication-service deze informatie verzamelen. Op deze manier kunt u de analysegegevens van de Adobe Pass Authentication-service correleren met andere analytische rapporten die u van andere toepassingen of websites hebt. De informatie over hoe te opstelling EC bezoekorID kan worden gevonden [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=nl-NL).


>[!NOTE]
>
>Deze functionaliteit wordt ondersteund vanaf JS SDK versie 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
