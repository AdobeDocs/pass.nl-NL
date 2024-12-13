---
title: Fout bij rapporteren
description: Fout bij rapporteren
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3012'
ht-degree: 0%

---

# (Verouderd) Fout bij rapporteren {#error-reporting}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht {#overview}

Foutrapportage in Adobe Pass-verificatie wordt momenteel op twee verschillende manieren geïmplementeerd:

* **Geavanceerde Fout die** meldt implementor registreert een foutencallback in het geval van [ AccessEnabler JavaScript SDK ](#accessenabler-javascript-sdk) of voert een genoemde interfacemethode uit &quot;`status`&quot;in het geval van [ AccessEnabler iOS/tvOS SDK ](#accessenabler-ios-tvos-sdk) en [ AccessEnabler Android SDK ](#accessenabler-android-sdk), om geavanceerde foutenrapportering te ontvangen. De fouten worden gecategoriseerd in **Informatie**, **Waarschuwing**, en **de types van Fout**. Dit het melden systeem is asynchroon **, in die** er geen garantie van de orde is waarin de veelvoudige fouten **zullen worden teweeggebracht.**  Voor details op het geavanceerde fout die systeem melden, zie [ Geavanceerde Fout die ](#advanced-error-reporting) sectie meldt.

* **Oorspronkelijke Fout die -** een statisch rapporterend systeem meldt waarin de foutenmeldingen tot specifieke callback functies worden overgegaan wanneer de specifieke verzoeken ontbreken. De fouten worden gegroepeerd in generische, authentificatie, en toestemmingstypes. Voor de lijst van fout die op in het originele systeem wordt gemeld, zie de [ Oorspronkelijke Fout die ](#original-error-reporting) sectie meldt.


## Geavanceerde foutrapportage {#advanced-error-reporting}

* [AccessEnabled JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabled Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>De oude [ Oorspronkelijke Fout die ](#original-error-reporting) API meldt zal blijven werken aangezien het vroeger deed, de geavanceerde fout meldt breekt niet de functionaliteit, maar de originele fout meldt zal geen updates meer ontvangen. Alle nieuwe fouten en updates zullen aan het geavanceerde fout rapporteringssysteem gebeuren.

### AccessEnabled JavaScript SDK {#accessenabler-javascript-sdk}

Het nieuwe fout rapporterend systeem wordt verlaten facultatief, daarom kan de implementor een callback van de foutenmanager uitdrukkelijk registreren om geavanceerde foutenrapporten te ontvangen. Het systeem bevat de mogelijkheid om meerdere callbacks met foutmeldingen dynamisch te registreren en de registratie ervan ongedaan te maken. Bovendien kunt u om het even welke nieuwe foutencallbacks registreren zodra AccessEnabler JavaScript SDK wordt geladen, zonder de behoefte om een andere initialisering (alvorens `setRequestor()`) uit te voeren, betekenend dat u de capaciteit hebt om geavanceerde rapporten over initialisatie en configuratiefouten te ontvangen.


#### Implementatie {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


De callback-functie van de fouthandler ontvangt één object (een kaart) met de volgende structuur:

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. Binding {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Koppelt een handler voor een gebeurtenis.

**`eventType`** - SLECHTS resulteert de &quot;`errorEvent`&quot;waarde in AccessEnabler JavaScript SDK die geavanceerde foutenrapporten callbacks teweegbrengt.

**`handlerName`** - een tekenreeks die de naam van de fouthandlerfunctie opgeeft.


Beide bind-parameters mogen alleen tekens uit de volgende set gebruiken: `[0-9a-zA-Z][-._a-zA-Z0-9]` . Dit betekent dat parameters moeten beginnen met een getal of letter en vervolgens alleen afbreekstreepjes, punten, onderstrepingstekens en alfanumerieke tekens mogen bevatten.  Bovendien mogen parameters niet langer zijn dan 1024 tekens.

**Voorbeeld** van bindende foutenmanagers:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

Vanwege technische beperkingen kunt u geen sluiting of anonieme functie binden. U moet de naam van de methode opgeven in de tweede parameter.


### 2. Binding opheffen {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Hiermee wordt een eerder gekoppelde gebeurtenishandler verwijderd.

**`eventType`** - SLECHTS resulteert de &quot;`errorEvent`&quot;waarde in AccessEnabler JavaScript SDK die geavanceerde foutenrapporten callbacks teweegbrengt.

**`handlerName`** - een tekenreeks die de naam van de fouthandlerfunctie opgeeft, indien deze null is of ontbreekt, worden alle gekoppelde handlers voor de opgegeven `eventType` verwijderd.

Beide bind-parameters mogen alleen tekens uit de volgende set gebruiken: `[0-9a-zA-Z][-._a-zA-Z0-9]` . Dit betekent dat parameters moeten beginnen met een getal of letter en vervolgens alleen afbreekstreepjes, punten, onderstrepingstekens en alfanumerieke tekens mogen bevatten.  Bovendien mogen parameters niet langer zijn dan 1024 tekens.

**Voorbeeld** van het verwijderen van één enkele foutenmanager:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Voorbeeld** verwijderend alle foutenmanagers:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

Het nieuwe systeem voor foutenrapportage is verplicht, daarom moet de uitvoerder expliciet voldoen aan het nieuwe &quot;EntitlementStatus&quot;-protocol van doelstelling C. Deze nieuwe benadering staat programmeurs toe om geavanceerde foutenrapportering te ontvangen.

#### Implementatie {#accessenab-ios-tvossdk-imp}

Een uitvoerder moet met het volgende **** protocol in overeenstemming zijn EntitlementStatus:

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Uw **status** functie zal één enkel voorwerp (een `NSDictionary`) met de volgende structuur ontvangen:

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Verklaring**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implementatie**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabled Android SDK {#accessenabler-android-sdk}

Het nieuwe systeem voor foutrapportage is verplicht, omdat de implementator expliciet moet voldoen aan het door de interface gedefinieerde protocol voor `IAccessEnablerDelegate` . Deze nieuwe benadering staat programmeurs toe om geavanceerde foutenrapportering te ontvangen.

#### Implementatie {#access-enablr-androidsdk-imp}

Een implementor moet de nieuwe `status` methode van de interface `IAccessEnablerDelegate` behandelen. De functie **`status`** ontvangt één **`AdvancedStatus`** -object met het volgende model:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Steekproef**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### AccessEnabler FireOS SDK {#accessenabler-fireos-sdk}


Het nieuwe systeem voor foutrapportage is verplicht, omdat de implementator expliciet moet voldoen aan het door de interface gedefinieerde protocol voor `IAccessEnablerDelegate` . Deze nieuwe benadering staat programmeurs toe om geavanceerde foutenrapportering te ontvangen.

#### Implementatie {#access-enab-fireos-sdk-}

Een implementor moet de nieuwe `status` methode van de interface `IAccessEnablerDelegate` behandelen. De functie **`status`** ontvangt één **`AdvancedStatus`** -object met het volgende model:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Steekproef**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Verwijzing naar geavanceerde foutcodes {#advanced-error-codes-reference}

In de volgende tabel worden de foutcodes weergegeven en beschreven die door de nieuwere API voor fouten worden weergegeven, samen met de voorgestelde handelingen die moeten worden uitgevoerd om deze te corrigeren:

| ID | Niveau | Beschrijving | Handelingen voor ontwikkelaars | Handelingen van gebruikers | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | Fout | Algemene Apple SSO-fout | De fout bevat een detailveld met de oorspronkelijke VSA-fout. | nvt | nvt | Ja | nvt |
| VSA203 | Info | De gebruiker besloot zich af te melden bij de toepassing terwijl de verificatie plaatsvond als resultaat van een aanmelding via de SSO van het platform. | Geef de gebruiker de opdracht om zich expliciet af te melden bij Instellingen -> Accounts -> TV Provider op tvOS. <br><br> Geef de gebruiker de opdracht om zich expliciet af te melden bij Settings -> TV Provider op iOS/iPadOS. | Meld u expliciet af bij Instellingen -> Accounts -> TV Provider op tvOS. <br> <br> Afmelden bij Instellingen -> TV-provider op iOS/iPadOS | nvt | Ja | nvt |
| VSA404 | Info | De machtiging Application Video Subscriber Account is onbepaald. | Stimuleer gebruikers die geen toestemming geven om abonnementsgegevens te openen door de voordelen van de Single Sign-On (SSO) gebruikerservaring uit te leggen. | De gebruiker kan zijn besluit wijzigen door naar de toepassingsinstellingen (toegang tot tv-provider) of naar het gedeelte van Settings -> TV-provider op iOS/iPadOS of Settings -> Accounts -> TV-provider op tvOS te gaan. | nvt | Ja | nvt |
| VSA503 | Info | Verzoek om metagegevens van account voor toepassingsvideo-abonnee is mislukt. | Het MVPD-eindpunt reageert niet. De toepassing kan terugvallen op de normale verificatiestroom. | nvt | nvt | Ja | nvt |
| 500 | Fout | Interne fout | Het gebruik AccessEnablerDebug en inspecteert zuivert logboeken (console.log output) om te bepalen wat verkeerd ging. | nvt | Ja | Ja | nvt |
| SEC403 | Fout | Domeinbeveiligingsfout. De aanvrager gebruikt een ongeldig domein. Alle domeinen die door een bepaalde identiteitskaart van de Aanvrager worden gebruikt moeten door Adobe worden gewhitelliseerd. | - De AccessEnabler alleen laden vanuit de lijst met toegestane domeinen <br> <br> - Neem contact op met de Adobe om de whitelist van het domein te beheren voor de gebruikte id van de aanvrager <br> <br> - iOS: controleer of u het juiste certificaat gebruikt en of de handtekening juist is gemaakt | nvt | nvt | Ja | nvt |
| SEC412 | Waarschuwing | [ Beschikbaar in Versie 2.5 ] identiteitskaart van het Apparaat past niet aan. Dit kan gebeuren wanneer het onderliggende platform zijn identiteitskaart van het Apparaat verandert. In dit geval worden de bestaande tokens gewist en wordt de gebruiker niet meer geverifieerd. Merk op dat dit rechtmatig gebeurt wanneer de gebruiker JS SDK gebruikt en zwerft (op JS is client-IP onderdeel van de apparaat-id). Anders zou dit een aanwijzing kunnen zijn van een poging tot fraude - een poging om tokens van een ander apparaat te kopiëren. | - Controleer het aantal waarschuwingen. Als ze zonder duidelijke reden pieken (geen recente browserupdates, nieuwe besturingssystemen), kan dit een aanwijzing zijn voor pogingen tot fraude.  <br> <br> - (Optioneel) Informeer de gebruiker dat hij zich opnieuw moet aanmelden. | Meld u opnieuw aan. | Ja | Ja | Ja van 3.2 |
| SEC420 | Fout | HTTP-beveiligingsfout bij communicatie met Adobe Pass-verificatieservers. Deze fout treedt doorgaans op wanneer spoofing/proxy&#39;s zijn geïnstalleerd. | - Laad `[https://]{SP_FQDN\}` in browser en keur manueel de SSL certificaten, bijvoorbeeld, **https://api.auth.adobe.com** of **https://api.auth-staging.adobe.com** goed <br> <br> - De proxycerts markeren als vertrouwd | Als dit voor een regelmatige gebruiker voorkomt, is het een aanwijzing van een mogelijke man-in-de-middenaanval! | Ja | Ja | Ja van 3.2 |
| CFG100 | Waarschuwing | Datum/tijd/tijdzone van de clientcomputer is niet correct ingesteld. Dit zal waarschijnlijk leiden tot verificatie-/vergunningsfouten. | - Informeer de gebruiker om de juiste tijd in te stellen. <br> <br> Voer handelingen uit om machtigingsstromen te voorkomen, aangezien deze waarschijnlijk mislukken. | Stel de juiste datum/tijd in. | Ja | Ja | Ja van 3.2 |
| CFG400 | Fout | Er is een ongeldige aanvrager-id opgegeven. | De ontwikkelaar MOET een geldige aanvrager-id opgeven. | nvt | Ja | Ja | Ja van 3.2 |
| CFG404 | Fout | De Adobe Pass-verificatieservers zijn niet gevonden. Dit kan in drie gevallen gebeuren: <br><br> - De ontwikkelaar heeft een ongeldige spoofing. <br><br> - De gebruiker heeft netwerkproblemen en kan de Adobe Pass-verificatiedomeinen niet bereiken. <br><br> -Adobe Pass-verificatieservers zijn onjuist geconfigureerd. <br><br>  **Nota:** op Firefox, zal CFG400 in plaats van CFG404 (browser beperking) verschijnen | - Spoofing controleren. <br><br> - Controleer de netwerk-/DNS-instellingen. <br><br> -Inform Adobe. | Controleer de netwerk-/DNS-instellingen. | Ja | Ja | Ja van 3.2 |
| CFG410 | Fout | AccessEnabler is te oud. | Informeer de gebruiker caches te wissen. | Wis de browsercache. | Ja | nvt | Ja van 3.2 |
| CFG5xx | Fout | Er treden interne fouten op bij de Adobe Pass-verificatieservers. xx kan een willekeurig getal zijn. | - Informeer de gebruiker dat de Adobe Pass-verificatie niet beschikbaar is. <br><br> - Adobe Pass-verificatie omzeilen. <br> <br> - Informeer Adobe. | Probeer het later. | Ja | Ja | Ja van 3.2 |
| N000 | Info | De gebruiker is niet geverifieerd. | nvt | Aanmelden. | Ja | Ja | Ja van 3.2 |
| N001 | Info | Een passieve authentificatiepoging werd begonnen op de achtergrond. Dit zal voor MVPDs gebeuren die met &quot;Authentificatie per Aanvrager&quot;wordt gevormd. Terwijl de gebruiker hopelijk automatisch voor authentiek zal worden verklaard, brengt dit een prestatiesboete op initialisering op. | U kunt desgewenst de gebruiker informeren of de gebruikersinterface tonen die de gebruiker waarschuwt dat &quot;het werk bezig is&quot;. | Wacht. | Ja | Ja | Ja van 3.2 |
| N003 | Info | De gebruiker selecteert de optie &quot;Andere tv-provider&quot; in de kiezer van Apple MVPD. | *displayProviderDialog* callback zal worden geroepen en de toepassing kon aan regelmatige authentificatiestroom terugvallen. | Selecteer reguliere MVPD en ga verder met het aanmeldingsscherm. | nvt | Ja | nvt |
| N004 | Info | De gebruiker selecteert een tv-provider die niet door de huidige aanvrager wordt ondersteund. | *displayProviderDialog* callback zal worden geroepen en de toepassing kon aan regelmatige authentificatiestroom terugvallen. | Selecteer reguliere MVPD en ga verder met het aanmeldingsscherm. | nvt | Ja | nvt |
| N005 | Info | De MVPD-kiezer is geannuleerd. | nvt | nvt | Ja | Ja | Ja van 3.2 |
| N010 | Waarschuwing | De gebruiker werd voor authentiek verklaard terwijl de authenticate-all degradatieregel op zijn plaats voor de geselecteerde MVPD was. | De gebruiker optioneel laten weten dat hij &quot;gratis&quot; toegang krijgt vanwege MVPD-problemen. | nvt | Ja | Ja | Ja van 3.2 |
| N011 | Info | De gebruiker is geverifieerd met TempPass. | - Informeer de gebruiker. <br> <br> - U kunt desgewenst een lijst met normale MVPD&#39;s presenteren. | Meld u optioneel aan bij uw gewone MVPD. | Ja | Ja | Ja van 3.2 |
| N111 | Waarschuwing | Verlopen TempPass. | - Informeer de gebruiker. <br> <br> - Presenteer een lijst van regelmatige MVPDs. <br> <br> - Verberg de optie TempPass. | Meld u aan bij uw normale MVPD. | Ja | Ja | Ja van 3.2 |
| N130 | Fout | **teken van de Authentificatie niet gevonden op zitting.** Dit kan een van de volgende oorzaken hebben: <br> <br> 1 Browser heeft (derde) cookies uitgeschakeld (niet van toepassing op AccessEnabler JavaScript SDK versie 4.x) <br> <br> 2. Browser heeft Enable cross-site tracking (Safari 11+) <br> <br> 3. Sessie is verlopen <br> <br> 4. De programmeur roept verificatie-API&#39;s op onjuiste wijze achter elkaar aan <br> <br> OPMERKING: deze foutcode is niet beschikbaar voor omleidingsstromen van volledige pagina&#39;s. | 1. Vraag de gebruiker of hij cookies van derden wil inschakelen <br> <br> 2. Gebruiker vragen om het bijhouden van meerdere sites uit te schakelen <br> <br> 3. Vraag de gebruiker om opnieuw te verifiëren <br> <br> 4. API&#39;s aanroepen in de juiste volgorde | 1. Cookies (van derden) inschakelen <br> <br> 2. Spatiëring tussen sites uitschakelen <br> <br> 3. Opnieuw verifiëren <br> <br> 4. NVT | Ja | Ja | Ja van 3.2 |
| N500 | Fout | Interne fout. <br> <br> Opmerking: dit is de &quot;Algemene verificatiefout&quot; en &quot;Interne verificatiefout&quot; van het oorspronkelijke foutsysteem. Deze fout zal uiteindelijk worden opgeheven. | Het gebruik AccessEnablerDebug en inspecteert zuivert logboeken (console.log output) om te bepalen wat verkeerd ging. | nvt | Ja | Ja | nvt |
| R401 | Fout | Er is een fout opgetreden tijdens het verkrijgen van een toegangstoken. <br> <br> Opmerking: dit is een onherstelbare fout. Vertel de gebruiker dat de toepassing niet beschikbaar is. | - iOS: controleer de softwareinstructie en aangepaste schema&#39;s in uw toepassing. <br> <br> - JavaScript: controleer de softwareinstructie in uw websitetoepassing. <br> <br> Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja van v4.0 | Ja vanaf v3.0 | Ja van 3.2 |
| R400 | Fout | De toepassing is niet geregistreerd. De software-instructie is ongeldig of is ingetrokken. <br> <br> Opmerking: dit is een onherstelbare fout. Vertel de gebruiker dat de toepassing niet beschikbaar is. | - iOS: controleer de softwareinstructie en aangepaste schema&#39;s in uw toepassing. <br> <br> - JavaScript: controleer de softwareinstructie in uw websitetoepassing. <br> <br> Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja van v4.0 | Ja vanaf v3.0 | Ja van 3.2 |
| REG500 | Fout | Registratiecode kan niet van server worden opgehaald. <br> <br> Opmerking: dit is een onherstelbare fout. Vertel de gebruiker dat de toepassing niet beschikbaar is. | Open een ticket met Zendesk en informeer de gebruiker dat het systeem tijdelijk niet beschikbaar is. | nvt | Ja van v4.0 | Ja vanaf v3.0 | Ja van 3.2 |
| REGCODE | Succes | De toepassing met de naam setSelectedProvider API op een tvOS-platform. | Instruct/vraag de gebruiker om een tweede apparaat (scherm) aan login te gebruiken gebruikend de verstrekte registratiecode. | Gebruik de regcode op een tweede apparaat (scherm) om verificatie te starten. | nvt | Alleen voor tvOS | nvt |
| Z010 | Waarschuwing | De gebruiker is geautoriseerd terwijl de regel voor alle authenticatie of alle autorisaties was ingesteld voor de geselecteerde MVPD. | De gebruiker optioneel laten weten dat hij &quot;gratis&quot; toegang krijgt vanwege MVPD-problemen. | nvt | Ja | Ja | Ja van 3.2 |
| Z011 | Info | Gebruiker is geautoriseerd met TempPass | De gebruiker optioneel informeren | nvt | Ja | Ja | Ja van 3.2 |
| Z100 | Fout | Autorisatie is mislukt omdat de gebruiker geen abonnement heeft op de gevraagde bron of om andere redenen die afkomstig zijn van de MVPD, bijvoorbeeld omdat de video niet overeenkomt met de instellingen voor Ouderlijk toezicht voor de gebruikersaccount | - Afspelen niet toestaan. <br> <br> - Informeer de gebruiker. <br> <br> - De &#39;message&#39;-sleutel in het foutbericht kan een gedetailleerder bericht van de MVPD bevatten. | nvt | Ja | Ja | Ja van 3.2 |
| Z110 | Fout | De autorisatie wordt geweigerd omdat MVPD herhaaldelijk weigert. Mogelijke poging tot fraude of DOS. | - Afspelen niet toestaan. <br> <br> - Informeer de gebruiker. | nvt | Ja | Ja | Ja van 3.2 |
| Z120 | Fout | Autorisatie geweigerd om technische redenen in de communicatie met de MVPD. Mogelijke netwerkfout. | - Afspelen niet toestaan. <br> <br> - Informeer de gebruiker dat de MVPD problemen ondervond en het later moet proberen. | Probeer het later. | Ja | Ja | Ja van 3.2 |
| Z130 | Fout | De autorisatie wordt geweigerd omdat een ongeldige / onjuist gevormde resource is gebruikt. | Controleer de resourcetekenreeks en corrigeer deze. Over het algemeen is deze fout het gevolg van een onjuist gevormde MRSS of het gebruik van een onbewerkte tekenreeks in plaats van MRSS. | nvt | Ja | Ja | Ja van 3.2 |
| Z169 | Fout | Autorisatie geweigerd omdat de afbraakregel authzNone is toegepast voor de opgegeven bron. | De gebruiker op de hoogte stellen | nvt | Ja | Ja | Ja van 3.2 |
| Z500 | Fout | Interne fout. <br> <br> Opmerking: dit is de verouderde &#39;Generic Authentication Error&#39; en &#39;Internal Authentication Error&#39;. Deze fout zal uiteindelijk worden opgeheven. | Het gebruik AccessEnablerDebug en inspecteert zuivert logboeken (console.log output) om te bepalen wat verkeerd ging. | nvt | Ja | Ja | Ja van 3.2 |
| P100 | Fout | Voorvertoning is mislukt. Waarschijnlijk is dit te wijten aan het aanvragen van een vergunning voor te veel middelen. | - Gebruik NIET meer dan het maximale aantal toegestane bronnen. <br> <br> - Neem contact op met de ondersteuning van Adobe Pass-verificatie om het maximumaantal toegestane bronnen te zoeken/in te stellen. | nvt | Ja vanaf v3.0 | Ja | Ja van 3.2 |
| IS2XX | Fout | Deze foutencodes zijn teruggekeerd wanneer de gegevens van de de eindreactie van de individualisatieserver een ongeldig formaat hebben of vereiste individualiseringsinformatie missen. | Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja vanaf v3.0 | nvt | nvt |
| IS4XX | Fout | Deze foutencodes zijn teruggekeerd in het geval van de mislukking 4XX van het individualisatieserver - is de de statuscode van HTTP van de reactie. | Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja vanaf v3.0 | nvt | nvt |
| IS5XX | Fout | Deze foutcodes worden geretourneerd in het geval van een eindpuntfout van de individualisatieserver 5XX - dit is de HTTP-statuscode van de reactie. | Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja vanaf v3.0 | nvt | nvt |
| IS0 | Fout | Deze code is teruggekeerd wanneer het individualisatieservereindpunt helemaal niet antwoordde, daarom heeft de verbinding uit getimede tijd | Een ticket openen met Zendesk en de gebruiker laten weten dat het systeem tijdelijk niet beschikbaar is | nvt | Ja vanaf v3.0 | nvt | nvt |
| LS011 | Waarschuwing | AccessEnabler gebruikt een vluchtige opslag vanwege problemen met LSO/LocalStorage en WebStorage (of onbeschikbaarheid). <br> <br> Verificatie/autorisatie blijft op de huidige pagina staan. Bij elke pagina die wordt geladen, moet de gebruiker worden geverifieerd. Gevormde TTLs wordt niet afgedwongen over paginaatherladingen. | - Informeer de gebruiker van beperkingen. <br> <br> - Informeer de gebruiker hoe u de beschikbare opslagruimte vergroot. <br> <br> - U kunt zich ook afmelden om de opslag te wissen. | - Meer opslagruimte. <br> <br> - Afmelden om opslag te wissen. | Ja | nvt | nvt |

<br>

## Oorspronkelijke foutrapportage {#original-error-reporting}

In deze sectie wordt het oorspronkelijke systeem voor foutrapportage beschreven, samen met de oorspronkelijke foutcodes. In het oorspronkelijke systeem voor foutrapportage geeft de AccessEnabler fouten door aan deze twee callback-functies: `setAuthenticationStatus()` na een aanroep van `checkAuthentication()`; `tokenRequestFailed()` , na de mislukking van een aanroep van `checkAuthorization()` of `getAuthorization()` .

De oorspronkelijke foutrapportage en status-API&#39;s werken nog steeds precies zo als voorheen. De oorspronkelijke API&#39;s voor foutrapportage worden echter niet bijgewerkt. Alle nieuwe fout die en updates op de oude fouten melden zullen slechts in de nieuwe [ Geavanceerde Fout die systeem ](#advanced-error-reporting) meldt worden weerspiegeld.


Voor voorbeelden van het gebruiken van het originele fout meldend systeem, zie de [ JavaScript API Verwijzing ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[ setAuthenticationStatus () ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) en [ tokenRequestFailed () ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg) functies, [ iOS/tvOS API Verwijzing ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md): [ setAuthenticationStatus () ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus) en [ tokentRequestFailed () {111, ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed) 2} Android API Verwijzing ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [ setAuthenticationStatus () ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) en [ tokenRequestFailed () ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).[

### Oorspronkelijke callback-foutcodes {#original-callback-error-codes}

| **Algemene Fouten** | |
|---|---|
| Interne fout | Er is een systeemfout opgetreden bij het verwerken van de aanvraag. |
| Provider niet geselecteerd | Vindt plaats wanneer de klant in het dialoogvenster voor providerselectie annuleert. |
| Fout: provider niet beschikbaar | Vindt plaats wanneer er geen providers beschikbaar zijn. |
|  |  |
| **de Fouten van de Authentificatie** | |
| Algemene verificatiefout | Wordt geretourneerd als de reden onbekend is of niet kan worden gepubliceerd. |
| Interne verificatiefout | Er is een systeemfout opgetreden bij het verifiëren. |
| Fout door gebruiker niet geverifieerd | Gebruiker is niet geverifieerd. |
|  |  |
| **de Fouten van de Vergunning** |  |
| Algemene autorisatiefout | Wordt geretourneerd als de reden onbekend is of niet kan worden gepubliceerd. |
| Interne autorisatiefout | Er is een systeemfout opgetreden bij het autoriseren. |
| Fout: gebruiker niet geautoriseerd | De klant is niet geautoriseerd om de gevraagde inhoud te bekijken. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
