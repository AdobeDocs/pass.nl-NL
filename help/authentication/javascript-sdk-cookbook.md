---
title: JavaScript SDK Cookbook
description: JavaScript SDK Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# JavaScript SDK Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#intro}

In dit document worden de workflows beschreven die de toepassing op hoofdniveau van een programmeur implementeert voor een JavaScript-integratie met de Adobe Pass-verificatieservice. De koppelingen naar de JavaScript API-naslaggids worden overal weergegeven.

Let ook op het volgende: [Gerelateerde informatie](#related) bevat een koppeling naar een set JavaScript-codevoorbeelden.

## Machtigingsstromen {#entitlement}

1. [Vereisten](#prereq)
2. [Stroom opstarten](#startup)
3. [Verificatiestroom](#authn)
4. [Autorisatiestroom](#authz)
5. [Media-stroom weergeven](#logout)

</br>

![](assets/javascript-flows.png)


## Vereisten {#prereq}

**Afhankelijkheden:**

- Adobe Pass Authentication Library (AccessEnabler), werkt u samen met uw Adobe Pass Authentication Account Manager om dit te regelen.
- Geldige aanvraag-ID voor Adobe Pass-verificatie, werk dit samen met uw accountmanager voor Adobe Pass-verificatie.

Maak uw callback-functies:

- `entitlementLoaded`
</br>

**Trigger** AccessEnabler heeft geladen en gebeëindigd initialiserend.

- `displayProviderDialog(mvpds)`

  **Trigger** `getAuthentication(),` alleen als de gebruiker geen provider (een MVPD) heeft geselecteerd en nog niet is geverifieerd. De parameter mvpds is een array van providers die beschikbaar zijn voor de gebruiker.

- `setAuthenticationStatus(status, errorcode)`

  **Trigger**
   - `checkAuthentication()`elke keer.
   - `getAuthentication()` alleen als de gebruiker al is geverifieerd en een provider heeft geselecteerd.

  De geretourneerde status is geslaagd of mislukt. De foutcode beschrijft het type fout.

- `createIFrame(width, height)`

  **Trigger** `setSelectedProvider(providerID)`, alleen als de geselecteerde provider is geconfigureerd voor weergave in een IFrame.

  >[!NOTE]
  >
  >Een leverancier wordt gevormd om zijn authentificatiescherm als of omleiding of in een iFrame terug te geven, en de programmeur moet van beide rekenschap geven.

- `sendTrackingData(event, data)`

  **Triggers:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  De `event` parameter geeft aan welke machtigingsgebeurtenis heeft plaatsgevonden; de `data` parameter is een lijst van waarden met betrekking tot de gebeurtenis.
- `setToken(token, resource)`
  **Trigger** `checkAuthorization()`en `getAuthorization()` na een geslaagde machtiging om een bron te bekijken.   De `token` parameter is het kortstondige media token; de `resource` parameter is de inhoud die de gebruiker mag bekijken.

- `tokenRequestFailed(resource, code, description)`
  **Trigger**`checkAuthorization()` en`getAuthorization()`  na een niet-succesvolle toelating.\
  De `resource` parameter is de inhoud die de gebruiker probeerde te bekijken; de `code` parameter is de foutcode die aangeeft welk type fout is opgetreden; de `description` parameter beschrijft de fout die aan de foutencode wordt geassocieerd.

- `selectedProvider(mvpd)`

  **Trigger** [`getSelectedProvider()`](#$getSelProv `mvpd` parameter geeft informatie over de provider die door de gebruiker is geselecteerd.

- `setMetadataStatus(metadata, key, arguments)`

  **Trigger** `getMetadata().`\
  De `metadata` parameter bevat de specifieke gegevens die u hebt aangevraagd. De sleutelparameter is de sleutel die wordt gebruikt in de `getMetadata()`en de `arguments` parameter is hetzelfde woordenboek als dat waaraan is doorgegeven `getMetadata()`.


## 2. Opstartstroom

**I. Laad de JavaScript van AccessEnabler:**

**Voor staging-profiel**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

of...

**Voor productieprofiel**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Triggers:** Wanneer de initialisatie is voltooid, roept Adobe Pass-verificatie uw `entitlementLoaded()` callback-functie. Dit is het ingangspunt aan de communicatie van uw toepassing met AccessEnabler.


**II.** Bellen `setRequestor()`om de identiteit van de programmeur vast te stellen; `requestorID` en (optioneel) een array met eindpunten voor Adobe Pass-verificatie.

**Triggers:** Geen, maar schakelt `displayProviderDialog()` te worden opgeroepen wanneer dat nodig is.


**III.** Bellen `checkAuthentication()` om op een bestaande authentificatie te controleren zonder volledig in werking te stellen [verificatiestroom].  Als deze vraag slaagt, kunt u rechtstreeks aan te werk gaan `authorization flow`.  Zo niet, ga dan door naar de `authentication flow`.

**Afhankelijkheid:** Een geslaagde oproep tot `setRequestor()`(dit gebiedsdeel is ook op alle verdere vraag van toepassing).

**Triggers:** `setAuthenticationStatus()` callback

</br>

## 3. Verificatiestroom</span>


**Afhankelijkheid:** Een geslaagde oproep tot `setRequestor()`(dit gebiedsdeel is ook op alle verdere vraag van toepassing).


Bellen `getAuthentication()` om de authentificatiestatus OF te krijgen om de stroom van de leveranciersauthentificatie teweeg te brengen.

**Trigers:**

- `displayProviderDialog()`als de gebruiker nog niet is geverifieerd
- `setAuthenticationStatus()` indien de verificatie reeds heeft plaatsgevonden

De voltooiing van de authentificatiestroom wordt bereikt wanneer AccessEnabler roept `setAuthenticationStatus()`with `isAuthenticated == 1`.

## 4. Vergunningsstroom {#authz}

**Afhankelijkheden:**

- Een geslaagde oproep tot `setRequestor()` (dit gebiedsdeel is ook op alle verdere vraag van toepassing).
- Geldige ResourceID(s) overeengekomen met de MVPD(s). Merk op dat ResourceIDs het zelfde zou moeten zijn als die gebruikt op andere apparaten of platforms, en zal het zelfde over MVPDs zijn.

Bellen `getAuthorization()` en geef ResourceID voor de gevraagde media door. Een geslaagde vraag zal een Korte Symbolische waarde van Media terugkeren, die bevestigt dat de gebruiker wordt gemachtigd om de gevraagde media te bekijken.

- Als de vraag overgaat: De gebruiker heeft een geldig teken AuthN en de gebruiker wordt gemachtigd om op de gevraagde media te letten.
- Als de vraag ontbreekt: Onderzoek de geworpen Uitzondering om zijn type (AuthN, AuthZ, of iets anders) te bepalen:
- Als de vraag een fout AuthN toen was begin de Stroom AuthN opnieuw.
- Als de vraag een fout AuthZ toen was is de gebruiker niet geautoriseerd om op de gevraagde media te letten en één of ander soort foutenmelding zou aan de gebruiker moeten worden getoond.
- Als er een andere fout is opgetreden (verbindingsfout, netwerkfout, enz.) geeft u vervolgens een geschikt foutbericht weer aan de gebruiker.

Gebruik de token-verificateur van Media om de shortMediaToken te valideren die door een geslaagde gebruiker is geretourneerd `getAuthorization()` vraag.


**Afhankelijkheid:** De token-verificateur voor korte media (die is opgenomen in de bibliotheek AccessEnabler)

- Als de validatie slaagt: geef de gewenste media voor de gebruiker weer/afspelen.
- Als het ontbreekt: De token AuthZ was ongeldig, zou de mediaaanvraag moeten worden geweigerd en zou een foutbericht aan de gebruiker moeten worden getoond.

## 5. Mediastroom weergeven {#logout}

- De gebruiker selecteert de media die u wilt weergeven.
   - Is media beveiligd?
      - Uw app controleert of het medium is beveiligd:
         - Als de media is beveiligd, start uw app de bovenstaande workflow voor autorisatie (AuthZ).
         - Als het medium niet is beveiligd, gaat u verder met de Media-doorloop weergeven.
         - Media afspelen

## De bezoeker-id configureren {#visitorID}

Een [Experience Cloud bezoekerID](https://experienceleague.adobe.com/docs/id-service/using/home.html) waarde is vanuit analytisch oogpunt van groot belang . Zodra een EC bezoekerID-waarde is ingesteld, zal de SDK deze informatie samen met elke netwerkaanroep verzenden en zal de Adobe Pass Authentication-service deze informatie verzamelen. Op deze manier kunt u de analysegegevens van de Adobe Pass Authentication-service correleren met andere analytische rapporten die u van andere toepassingen of websites hebt. Informatie over het instellen van de EG-bezoekerID vindt u [hier](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Deze functionaliteit wordt ondersteund vanaf JS SDK versie 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
