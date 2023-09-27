---
title: Voorvoegsel
description: JavaScript vooraf autoriseren
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 0%

---

# Voorvoegsel {#js-preauthorize}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#preauth-overview}

De methode van de preauthorize API moet door toepassingen worden gebruikt om pre-vergunningsbesluiten voor één of meerdere middelen te verkrijgen. De aanvraag van de API voor voorafgaande toestemming moet worden gebruikt voor UI-tips en/of het filteren van inhoud. Een daadwerkelijke vergunning API verzoek moet worden gemaakt alvorens gebruikerstoegang tot de gespecificeerde middelen toe te staan.

Als er een onverwachte fout optreedt (bijvoorbeeld netwerkprobleem en MVPD-autorisatiepunt niet beschikbaar) wanneer een aanvraag voor een voorafgaande API wordt verwerkt door de Adobe Pass-verificatieservices, wordt er een of meer afzonderlijke foutinformatie opgenomen voor de betrokken bronnen als onderdeel van het resultaat van de eerdere API-reactie.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Omschrijving:** Deze methode moet door toepassingen worden gebruikt om de (informatieve) beslissingen van geverifieerde gebruikers vooraf te verkrijgen van de Adobe Pass-verificatieservice om specifieke beveiligde bronnen weer te geven, met als voornaamste doel de gebruikersinterface van de toepassing te versieren (bijvoorbeeld om de toegangsstatus met vergrendelings- en ontgrendelingspictogrammen aan te geven).

**Beschikbaarheid:** v4.4.0+

**Parameters:**

* `PreauthorizeRequest`: Object Builder dat wordt gebruikt om de aanvraag te definiëren
* `AccessEnablerCallback`: callback gebruikt om de API-reactie te retourneren
* `PreauthorizeResponse`: Object dat wordt gebruikt om de inhoud van de API-reactie te retourneren

### class PreAuthzeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Hiermee stelt u de lijst met bronnen in waarvoor u voorafgaande autorisatiebeslissingen wilt verkrijgen.
* Het is verplicht deze in te stellen voor het gebruik van de voorafgaande autorisatie-API.
* Elk element in de lijst moet een Koord zijn die of de waarde van middelidentiteitskaart of het mediaRSS fragment vertegenwoordigen dat met MVPD moet worden overeengekomen.
* Deze methode stelt de informatie alleen in in de context van de huidige `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van deze methodeaanroep.

* Een daadwerkelijke `PreauthorizeRequest` u kunt een blik hebben `PreauthorizeRequestBuilder`s methode:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` middelen. De lijst van middelen waarvoor u pre-vergunningsbesluiten wilt verkrijgen.
* `@returns {PreauthorizeRequestBuilder}` De verwijzing naar hetzelfde `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van de methodeaanroep.
* Het doet dit om het creëren van methode het ketenen toe te staan.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Hiermee stelt u de functies in die u wilt uitschakelen bij het verkrijgen van beslissingen vóór de autorisatie.
* Deze functie stelt de informatie alleen in in de context van de huidige `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van deze functieaanroep.
* Een daadwerkelijke `PreauthorizeRequest` u kunt een blik hebben `PreauthorizeRequestBuilder`s functie:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` functies. De reeks functies die u wilt uitschakelen.
* `@returns` De verwijzing naar hetzelfde `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van de functieaanroep.
* Het doet dit om het creëren van functie ketting toe te staan.

#### build(): preAuthzeRequest {#preauth-req}

* Hiermee maakt en haalt u de referentie van een nieuwe `PreauthorizeRequest` objectinstantie.
* Deze methode instantieert een nieuw `PreauthorizeRequest` object telkens wanneer het wordt aangeroepen.
* Deze methode gebruikt de vooraf ingestelde waarden in de context van de huidige `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van deze methodeaanroep.
* Houd er rekening mee dat deze methode geen bijwerkingen veroorzaakt.
* De staat van de SDK of de staat van de SDK wordt derhalve niet gewijzigd `PreauthorizeRequestBuilder` objectinstantie, de ontvanger van deze methodeaanroep.
* Het betekent dat de opeenvolgende vraag van deze methode voor de zelfde ontvanger tot verschillende nieuwe zal leiden `PreauthorizeRequest` objectinstanties, maar met dezelfde informatie, voor het geval de waarden op de `PreauthorizeRequestBuilder` waar niet gewijzigd tussen de vraag.
* Als u geen van de verschafte informatie (bronnen en caching) hoeft bij te werken, kunt u de instantie PreauthorizeRequest opnieuw gebruiken voor meerdere toepassingen van de API voor voorafgaande toestemming.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;t> {#interface-access-enablr-callback}

#### onResponse(resultaat: T); {#on-response-result}

* De callback van de reactie die door SDK wordt geroepen toen de preauthorize API verzoek werd vervuld.
* Het resultaat is een geslaagd resultaat of een foutresultaat met een status.
* `@param {T} result`

#### onFailed(result: T); {#on-failure-result}

* De mislukte callback die door SDK wordt geroepen wanneer het preauthorize API verzoek kon niet worden onderhouden.
* Het resultaat is een mislukkingsresultaat dat een status bevat.
* `@param {T} result`

### klasse PreauthorizeResponse {#preauth-response-class}

#### status van het publiek: status; {#public-status}

* Retourneert: aanvullende status (status) informatie in het geval van een fout.
* Houd mogelijk een `null` waarde.

#### besluiten van het publiek: Besluit[]; {#public-decisions}

* Retourneert: De lijst met voorafgaande autorisatiebeslissingen. Eén besluit voor elke bron.
* De lijst kan leeg zijn in het geval van een fout.

### klassestatus {#class-status}

#### status van het publiek: nummer; {#public-status-numbr}

* De HTTP-antwoordstatuscode zoals beschreven in RFC 7231.
* Kan 0 zijn voor het geval dat de `Status` is afkomstig van de SDK in plaats van de Adobe Pass-verificatieservices.

#### openbare code: nummer; {#public-code-numbr}

* De standaardfoutcode voor Adobe Pass Authentication-services.
* Kan een lege tekenreeks of een `null` waarde.

#### openbare boodschap: tekenreeks; {#public-msg-string}

* Het gedetailleerde bericht dat in sommige gevallen wordt verstrekt door de MVPD toestemmingseindpunten of door de degradatieregels van de Programmer.
* Kan een lege tekenreeks of een `null` waarde.

#### publieke details: tekenreeks; {#public-details-strng}

* Bevat een gedetailleerd bericht dat in sommige gevallen wordt verstrekt door de MVPD toestemmingseindpunten of door de degradatieregels van de Programmer.
* Kan een lege tekenreeks of een `null` waarde.


#### public helpUrl: string; {#public-help-url-string}

* De URL die een koppeling bevat naar meer informatie over de oorzaak van deze fout en de mogelijke oplossingen.
* Kan een lege tekenreeks of een `null` waarde.

#### public trace: string; {#public-trace-string}

* De unieke id voor deze reactie, die kan worden gebruikt wanneer contact wordt opgenomen met ondersteuning om specifieke problemen in complexere scenario&#39;s te identificeren.
* Kan een lege tekenreeks of een `null` waarde.

#### public action: string; {#public-action-string}

* De aanbevolen maatregelen om de situatie te verhelpen.
   * **none**: Helaas is er geen vooraf gedefinieerde actie om dit probleem op te lossen. Dit kan wijzen op een onjuiste aanroep van de openbare API
   * **configuratie**: Een configuratiewijziging is nodig via het TVE-dashboard of door contact op te nemen met de technische ondersteuning.
   * **registratie van aanvragen**: De toepassing moet zich opnieuw registreren.
   * **verificatie**: De gebruiker moet verifiëren of opnieuw verifiëren.
   * **autorisatie**: De gebruiker moet toestemming voor de specifieke bron verkrijgen.
   * **aantasting**: Er moet een of andere vorm van afbraak worden toegepast.
   * **opnieuw proberen**: Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen
   * **retry-after**: Het opnieuw proberen van het verzoek na de aangegeven periode zou de kwestie kunnen oplossen.
* Kan een lege tekenreeks of een `null` waarde.

### klasse-besluit {#class-decision}

#### public id: string; {#public-id-string}

* De bron-id waarvoor het besluit is genomen.

#### publiek toegestaan : Boolean ; {#public-auth-boolean}

* De waarde van de markering die aangeeft of de beslissing succesvol is of niet.

#### openbare fout: status; {#public-error-status}

* Aanvullende status (status)-informatie voor het geval er een fout optreedt. Houd mogelijk een `null` waarde.

## Voorbeeld van implementatie van client {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Scenario-voorbeelden {#scenario-examples}

### Scenario 1: alle gevraagde middelen zijn goedgekeurd {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Uitgeschakeld</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Scenario 2: Sommige gevraagde middelen zijn toegestaan. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Uitgeschakeld</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Besluiten&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;geautoriseerd&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorised&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;geautoriseerd&quot;: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;Besluiten&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;geautoriseerd&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthentication_deny_by_mvpd&quot;,
    &quot;message&quot;: &quot;The MVPD has returned a \&quot;Deny\&quot; Decision when request pre-Authorisation for the specified resource.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;geautoriseerd&quot;: true
    },
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 3: Geen van de gevraagde middelen werd toegestaan. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Uitgeschakeld</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Besluiten&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorised&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorised&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorised&quot;: false
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Ingeschakeld</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Besluiten&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthentication_deny_by_mvpd&quot;,
    &quot;message&quot;: &quot;The MVPD has returned a \&quot;Deny\&quot; Decision when request pre-Authorisation for the specified resource.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthentication_deny_by_mvpd&quot;,
    &quot;message&quot;: &quot;The MVPD has returned a \&quot;Deny\&quot; Decision when request pre-Authorisation for the specified resource.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execute_time_over&quot;,
    &quot;message&quot;: &quot;The request did not complete in the maximum allowed time. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    }
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 4: Onjuist verzoek van de cliënt - geen gespecificeerde middelen. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Uitgeschakeld/Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;The request failed due to an internal error.&quot;,
    &quot;details&quot;: &quot;Required String[] parameter &#39;resource&#39; is not present&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;besluiten&quot;: [
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 5: Onjuist cliëntverzoek - lege gespecificeerde middelen. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Uitgeschakeld/Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;The resource parameter is missing&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;besluiten&quot;: [
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 6: De fout van het netwerk. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;Besluiten&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_receive_error&quot;,
    &quot;message&quot;: &quot;Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;geautoriseerd&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_receive_error&quot;,
    &quot;message&quot;: &quot;Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    }
    ]
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 7: De stroom van de pre-autorisatie werd aangehaald zonder een geldige zitting AuthN.

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Uitgeschakeld/Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;The authentication session associated with this request could not be retrieve. De gebruiker moet met gesteund MVPD opnieuw voor authentiek verklaren om verder te gaan.&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    },
    &quot;besluiten&quot;: [
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Scenario 8: de stroom van de preAutorisate werd aangehaald alvorens de reeksRequestor vraag werd voltooid

<table>
<thead>
  <tr>
    <th>Markering voor verbeterde foutcode</th>
    <th>Antwoord</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Uitgeschakeld/Ingeschakeld</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;The request is yet not configured which is a condition for using any API than the setRequestor API.&quot;,
    &quot;action&quot;: &quot;retry&quot;
    },
    &quot;besluiten&quot;: [
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
