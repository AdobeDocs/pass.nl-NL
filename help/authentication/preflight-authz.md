---
title: Preflight-autorisatie
description: Preflight-autorisatie
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# Preflight-autorisatie {#preflight-authorization}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Overzicht {#overview}

Deze functie biedt een lichte controle op de autorisatie voor meerdere bronnen. Het doel van deze lichtgewichtcontrole is om de interface te versieren (die bijvoorbeeld de toegangsstatus met vergrendelings- en ontgrendelpictogrammen aangeeft). Preflight-autorisatie is zo licht en efficiënt mogelijk, zodat één API-aanroep de machtigingsstatus oplevert voor een lijst met bronnen. Merk op dat deze eigenschap niet gebiedend op het toestaan van een middel is.

A `getAuthorization(resource)` of `checkAuthorization(resource)` De vraag MOET nog worden gemaakt alvorens playback toe te staan.

Preflight-autorisatie biedt ook ondersteuning voor een ander gebruiksgeval, waarbij de programmeur autorisatie voor meerdere bron-id&#39;s moet aanvragen om één item met media-inhoud af te spelen. De programmeur kan een eerste preflight controle op de vereiste middelen uitvoeren, en afhankelijk van de reactie, zou vroeg kunnen ontbreken als de bedrijfsvoorwaarden niet worden vervuld.

Voor een lijst van MVPDs die Preflight vergunning steunen, zie [Preflight-machtiging voor MVPD](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) pagina.

>[!NOTE]
>
> Merk op dat het gebruik van deze eigenschap voor MVPDs die geen volledige Preflight vergunningssteun hebben vooraf met Adobe &amp; MVPDs moet worden overeengekomen. Het gebruik van Preflight-vergunningen voor deze MVPD&#39;s valt onder het &quot;worst case-scenario&quot; dat wordt beschreven [hier](/help/authentication/mvpd-preflight-authz.md#intro) en kan leiden tot prestatieproblemen en een trage responstijd. </br>
> Houd er ook rekening mee dat gebruik van een Preflight-autorisatie met **meer dan 5 middelen moeten uitdrukkelijk door de Adobe worden goedgekeurd**.

## Preflight-API van AccessEnabler {#AE_pre_api}

AccessEnabler stelt een API/callback functiepaar bloot om Preflight vergunning uit te voeren. De API-aanroep heeft één parameter die bestaat uit een lijst met bronnen. De callback functie neemt één enkele parameter die de daadwerkelijke erkende middelen vertegenwoordigt. De volgende voorbeelden zijn in ActionScript, maar de vraag is beschikbaar in alle vloeren van AccessEnabler.

### checkPreauthorisedResources(Array:resources):void {#checkPreauthRes}

Roep deze functie op het voorwerp AccessEnabler om vergunningsstatus voor een lijst van middelen te verzoeken.

De parameter resources is de lijst met middelen waarvoor de machtiging moet worden gecontroleerd. Elk element in de lijst moet een tekenreeks zijn die de bron-id vertegenwoordigt. Voor de bron-id gelden dezelfde beperkingen als voor de bron-id in de `getAuthorization()` de vraag, dat wil zeggen, het wordt overeengekomen over waarde die tussen Programmer en MVPD, of een mediaRSS fragment wordt gevestigd. Merk op dat de Authentificatie van Adobe Pass middelen op geen enkele manier beheert, behalve een dunne mediatielaag die middelformaten afhankelijk van kan omzetten wat MVPD eigenlijk steunt.

### preauthorisedResources(Array:authorisedResources) {#preauthRes}

Dit is een callback functie die in de toepassing van de hoger-laag van de Programmer moet worden uitgevoerd. AccessEnabler zal deze functie na het berekenen van de erkende middelenlijst roepen.


### JS-voorbeeld

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Implementatiegegevens {#details}

- [Preflight met ChannelID](#preflight_using_channelID)
- [POST / Voorkeur geven](#post)
- [Opslag](#storage)

De API-aanroep probeert een lijst met geoorloofde bronnen voor de huidige gebruiker in de lokale opslag van de client te vinden in de cache. Als er geen lijst in de cache is, wordt een HTTPS-aanroep naar de AdobePass-servers uitgevoerd om de lijst op te halen.

Het caching mechanisme verbetert prestatiestijden op verdere vraag door de netwerkvraag volledig over te slaan. Bovendien kan de lijst in de cache vooraf worden ingevuld als onderdeel van het verificatieproces.  (Zie voor informatie over het instellen van dit scenario [Integratie van Preflight-autorisatie](/help/authentication/authz-usecase.md#preflight_authz_int) in het gedeelte Autorisatie van de MVPD Integration Guide).

Bovendien kan de in cache opgeslagen lijst met bronnen potentieel worden gebruikt om de machtigingsstroom te optimaliseren, in die zin dat als er een lijst met bronnen in cache bestaat, `checkAuthorization()` kan het controleren alvorens een netwerkvraag te doen. Als de bron niet voorkomt in de lijst met vooraf geautoriseerde bronnen, kan de controle mislukken zonder dat u de Adobe Pass-verificatieservers hoeft aan te roepen.


### Preflight met ChannelID {#preflight_using_channelID}

Vanaf Adobe Pass Authentication 2.4.1 Release werkt de Preflight-flow als volgt:

1. Tijdens de verificatie wordt met Adobe Pass-verificatie het `channelIID` element van de reactie van SAML van MVPD, en gebruikt deze waarde om het `authorizedResources` -element in het verificatietoken.
1. Binnen de `checkPreauthorizedResources()` API-functie, Adobe Pass-verificatie controleert of de `authorizedResources` element is ingesteld.
1. Als de `authorizedResources` -element is ingesteld, leest Adobe Pass Authentication die waarde en voert een doorsnede uit tussen de lijst met bronnen in de `authorizedResources` en de lijst met middelen die zijn ontvangen van `checkPreauthorizedResources()` parameter.  Het resultaat van deze doorsnede is de definitieve lijst van vooraf gemachtigde middelen.
1. Als de `authorizedResources` -element niet is ingesteld, voert u de eerder geïmplementeerde flow uit, waarin de lijst met bronnen wordt ontvangen van `checkPreauthorizedResources()` wordt doorgegeven aan PreAuthorizationServlet. Deze servlet voert de vergunningsvraag aan eindpunten MVPD uit en keert de lijst van vooraf erkende middelen terug.

### Voorbeeld van Preflight met ChannelID

In het onderstaande voorbeeld ziet u een voorbeeldkanaallijn. De naam van het kenmerk dat wordt gebruikt om de lijst met kanalen te verzenden, kan verschillen van de ene MVPD tot de andere:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


De `authorizedResources` element van het authentificatieelement verschijnt als volgt:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

De programmeur voert het `checkPreauthorizedResources()` API-aanroep, waarbij de volgende lijst met parameters wordt doorgegeven:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- fbc-fox

De huidige Preflight-implementatie voert de doorsnede uit met de lijst met bronnen van de `authorizedResources` -element en retourneert deze lijst:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**Opmerking:** Let op: de doorsnede is niet hoofdlettergevoelig.



### POST / Voorkeur geven {#post}

Deze vraag zal automatisch door AccessEnabler worden uitgevoerd wanneer geen caching, geautoriseerd, middellijst bestaat.


#### Verzoek {#req}

| Parameter | Type | Vereist | Beschrijving |
| --- | --- | --- | --- |
| `authentication_token` | string | JA | Het verificatietoken. |
| `resource_id` | string | JA | Eén resource. Dit kan meerdere keren worden opgegeven, één keer voor elk element van de array resources dat in de `checkPreauthorizedResources()` API-aanroep. |


**Opmerking:** Het maximumaantal gevraagde middelen is configureerbaar.
**Maximale standaardwaarde is 5.**


#### Antwoord {#resp}

De reactie die servlet vóór autorisatie terugstuurt heeft het volgende formaat:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Opslag {#storage}

Een lijst van vooraf gemachtigde middelen die AccessEnabler van de Dienstverlener krijgt. Deze lijst met bronnen:

- Wordt samen met AuthN en AuthZ-tokens opgeslagen
- Is geldig zolang de gebruiker op de zelfde website is of tot het teken AuthN verloopt
- Wordt telkens opnieuw opgehaald wanneer de gebruiker op een nieuwe geïntegreerde Adobe Pass-verificatiewebsite landt

Elke ingang bevat middelidentiteitskaart die de gebruiker voor vooraf wordt gemachtigd.

Bijvoorbeeld:


| Resource ID | Geautoriseerd |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Deze lijst heeft de naam &quot;cache voorafgaand aan autorisatie&quot;.

#### Stroom {#flow}

1. Programma&#39;s app/site maakt een `checkPreauthorizedResources(resourceList)` vraag.
1. AccessEnabler verifieert het authentificatietoken voor erkende middelen:
   1. Als het authentificatietoken geoorloofde middelen bevat - deze lijst is gebiedend en geen vraag zou moeten worden gemaakt om deze informatie te krijgen. De middelen van resourceList worden gezocht binnen de lijst van erkende middelen op het authentificatietoken en slechts die die werden gevonden zijn teruggekeerd door `preauthorizedResources()` callback.
   1. Als het verificatietoken GEEN geautoriseerde bronnen bevat - `resourceList` wordt vergeleken met de lijst met bronnen in de cache voorafgaand aan de autorisatie.
      1. Als de lijst de zelfde middelen bevat - dit betekent dat een vraag aan de server reeds werd gemaakt en de reactie is reeds in het voorvergunningsgeheime voorgeheugen. Alleen bronnen die zijn geautoriseerd, worden geretourneerd door de `preauthorizedResources()` callback.
      1. Als de lijst NIET de zelfde middelen bevat - de cliënt moet een vraag aan de server richten om de vergunningsstaat voor de middelen in resourceList te verkrijgen. De reactie zal in het voorvergunningsgeheime voorgeheugen worden opgehaald en worden opgeslagen, volledig die de oude middelen vervangen. Alleen bronnen die zijn geautoriseerd, worden geretourneerd door de `preauthorizedResources()` callback.


#### Ophalen lijst {#listRetrieve}

Wanneer een `checkPreauthorizedResources()` wordt geroepen, wordt de lijst van middelen om voor vergunning te verifiëren gecontroleerd tegen het voorvergunningsgeheime voorgeheugen. Als de lijst de zelfde reeks middelen bevat, wordt geen vraag aan Service Provider gemaakt, aangezien alle middelen nodig voor het teweegbrengen van `preauthorizedResources()` callback is reeds in het geheime voorgeheugen.


#### logout() {#logout}

De cache voor voorafgaande toestemming wordt bij het afmelden geleegd.


## Afhankelijkheden {#depends}

De prestaties van de Preflight-API zijn afhankelijk van specifieke MVPD-implementaties.  Zie voor implementatieopties [Integratie van Preflight-autorisatie](/help/authentication/authz-usecase.md#preflight_authz_int) in het gedeelte Autorisatie van de MVPD Integration Guide.


## Beveiliging {#security}

De client-API&#39;s zijn beschikbaar voor alle programmeurs.

De implementatie gebruikt HTTPS als vervoer, maar om een lichtere vraag te hebben, worden geen extra veiligheidsmaatregelen gebruikt (geen het ondertekenen, geen FAXS).

**Let op:** Gebruik deze API NIET op een gebiedende manier om te bepalen of een gebruiker toegang tot een beschermde bron zou moeten worden verleend. Het doel van deze API is UI decoration en/of preflighting bedrijfsbesluiten. De `getAuthorization()` en `checkAuthorization()` De vraag zou altijd moeten worden gemaakt alvorens playback toe te staan.


## Compatibiliteit {#compat}

Deze functie wordt ondersteund in alle Folivors van de AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (in AuthN-flow op het tweede scherm).

Preflight-autorisatie biedt geen ondersteuning voor vooraf autoriseerde bronnen die CDATA-secties bevatten. De focus van het huidige Preflight-systeem is het filteren op kanaalniveau. De middelen met secties CDATA zijn waarschijnlijk activa-vlakke middelen. Preflight biedt ondersteuning voor eenvoudige `mrss` bronnen voor voorafgaande toestemming op kanaalniveau, zolang deze geen CDATA bevatten.

## Integratie met andere functies {#integ_w_other_features}

Een mogelijke optimalisering kan op de vergunningsstroom voorkomen om snel te ontbreken als het middel niet in de lijst van vooraf gemachtigde middelen is.

In deze optimalisering, integreert het server Preflight eindpunt met het mechanisme van de Afbraak.

Als een regel &quot;AuthN allen&quot;op zijn plaats voor MVPD en de Aanvrager is, zal het preflight eindpunt eenvoudig terugspiegelen alle middelen die in het verzoek worden ontvangen.

Het zelfde is waar als &quot;AuthZ allen&quot;voor minstens één van de middelen huidig in het verzoek wordt toegelaten.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
