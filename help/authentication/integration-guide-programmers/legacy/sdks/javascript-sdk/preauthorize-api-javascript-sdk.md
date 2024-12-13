---
title: Voorvoegsel
description: JavaScript vooraf autoriseren
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1488'
ht-degree: 0%

---

# (Verouderd) Vooraf autoriseren {#js-preauthorize}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht {#preauth-overview}

De methode van de preauthorize API moet door toepassingen worden gebruikt om pre-vergunningsbesluiten voor één of meerdere middelen te verkrijgen. De aanvraag van de API voor voorafgaande toestemming moet worden gebruikt voor UI-tips en/of het filteren van inhoud. Een daadwerkelijke vergunning API verzoek moet worden gemaakt alvorens gebruikerstoegang tot de gespecificeerde middelen toe te staan.

Als er een onverwachte fout optreedt (bijvoorbeeld netwerkprobleem en MVPD-autorisatiepunt niet beschikbaar) wanneer een API-aanvraag vooraf autoriseren wordt verwerkt door de Adobe Pass-verificatieservices, wordt een of meer afzonderlijke foutinformatie opgenomen voor de betrokken bronnen als onderdeel van het resultaat van de eerdere API-reactie.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Beschrijving:** Deze methode moet door toepassingen worden gebruikt om voor authentiek verklaarde gebruiker (informatieve) besluiten van de Dienst van de Authentificatie van Adobe Pass te verkrijgen om specifieke beschermde middelen te bekijken, voor het primaire doel om UI van de toepassing te versieren (b.v. die op toegangsstatus met slot en ontgrendelingspictogrammen wijzen).

**Beschikbaarheid:** v4.4.0+

**Parameters:**

* `PreauthorizeRequest`: Object Builder dat wordt gebruikt om de aanvraag te definiëren
* `AccessEnablerCallback`: callback gebruikt om de API-reactie te retourneren
* `PreauthorizeResponse`: Object dat wordt gebruikt om de inhoud van de API-reactie te retourneren

### class PreAuthzeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string []): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Hiermee stelt u de lijst met bronnen in waarvoor u voorafgaande autorisatiebeslissingen wilt verkrijgen.
* Het is verplicht deze in te stellen voor het gebruik van de voorafgaande autorisatie-API.
* Elk element in de lijst moet een tekenreeks zijn die de bron-id-waarde vertegenwoordigt of het media-RSS-fragment dat met de MVPD moet worden overeengekomen.
* Deze methode stelt de informatie alleen in in de context van de huidige objectinstantie `PreauthorizeRequestBuilder` , die de ontvanger is van deze methodeaanroep.

* Als u een werkelijke `PreauthorizeRequest` wilt maken, kunt u de methode van `PreauthorizeRequestBuilder` bekijken:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` resources. De lijst van middelen waarvoor u pre-vergunningsbesluiten wilt verkrijgen.
* `@returns {PreauthorizeRequestBuilder}` De verwijzing naar dezelfde `PreauthorizeRequestBuilder` -objectinstantie, de ontvanger van de methodeaanroep.
* Het doet dit om het creëren van methode het ketenen toe te staan.

#### disableFeatures (...features: string []): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Hiermee stelt u de functies in die u wilt uitschakelen bij het verkrijgen van beslissingen vóór de autorisatie.
* Deze functie stelt de informatie alleen in in de context van de huidige objectinstantie `PreauthorizeRequestBuilder` , die de ontvanger van deze functieaanroep is.
* Als u een werkelijke `PreauthorizeRequest` wilt maken, kunt u de functie van `PreauthorizeRequestBuilder` bekijken:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` -functies. De reeks functies die u wilt uitschakelen.
* `@returns` De verwijzing naar dezelfde `PreauthorizeRequestBuilder` -objectinstantie, de ontvanger van de functieaanroep.
* Het doet dit om het creëren van functie ketting toe te staan.

#### build(): preAuthzeRequest {#preauth-req}

* Maakt en haalt de referentie van een nieuwe objectinstantie `PreauthorizeRequest` op.
* Deze methode instantieert telkens wanneer een nieuw `PreauthorizeRequest` -object wordt aangeroepen.
* Deze methode gebruikt de waarden die vooraf zijn ingesteld in de context van de huidige objectinstantie `PreauthorizeRequestBuilder` , die de ontvanger is van deze methodeaanroep.
* Houd er rekening mee dat deze methode geen bijwerkingen veroorzaakt.
* daarom wijzigt de methode de status van de SDK of de status van de objectinstantie `PreauthorizeRequestBuilder` , die de ontvanger is van deze methodeaanroep, niet.
* Dit betekent dat opeenvolgende aanroepen van deze methode voor dezelfde ontvanger verschillende nieuwe `PreauthorizeRequest` -objectinstanties maken, maar dezelfde informatie hebben, voor het geval de waarden die zijn ingesteld op `PreauthorizeRequestBuilder` waar ze niet tussen de aanroepen zijn gewijzigd.
* Als u geen van de verschafte informatie (bronnen en caching) hoeft bij te werken, kunt u de instantie PreauthorizeRequest opnieuw gebruiken voor meerdere toepassingen van de API voor voorafgaande toestemming.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(resultaat: T); {#on-response-result}

* Callback van de reactie die door de SDK wordt aangeroepen toen de voorafgaande autorisatie-API-aanvraag werd uitgevoerd.
* Het resultaat is een geslaagd resultaat of een foutresultaat met een status.
* `@param {T} result`

#### onFailed(result: T); {#on-failure-result}

* De callback van de mislukking die door de SDK wordt geroepen wanneer het preauthorize API verzoek kon niet worden onderhouden.
* Het resultaat is een mislukkingsresultaat dat een status bevat.
* `@param {T} result`

### klasse PreauthorizeResponse {#preauth-response-class}

#### status van het publiek: status; {#public-status}

* Retourneert: aanvullende status (status) informatie in het geval van een fout.
* Plaats mogelijk een `null` -waarde.

#### openbare beslissingen: Besluit []; {#public-decisions}

* Retourneert: De lijst met voorafgaande autorisatiebeslissingen. Eén besluit voor elke bron.
* De lijst kan leeg zijn in het geval van een fout.

### klassestatus {#class-status}

#### status van het publiek: nummer; {#public-status-numbr}

* De HTTP-antwoordstatuscode zoals beschreven in RFC 7231.
* De waarde 0 kan zijn als de `Status` afkomstig is van de SDK in plaats van de Adobe Pass-verificatieservices.

#### openbare code: nummer; {#public-code-numbr}

* De standaardfoutcode voor Adobe Pass Authentication-services.
* Kan een lege tekenreeks of een `null` -waarde bevatten.

#### openbare boodschap: tekenreeks; {#public-msg-string}

* De gedetailleerde boodschap die in sommige gevallen wordt verstrekt door de eindpunten van de MVPD-autorisatie of door de regels voor de achteruitgang van programmeurs.
* Kan een lege tekenreeks of een `null` -waarde bevatten.

#### publieke details: tekenreeks; {#public-details-strng}

* Bevat een gedetailleerd bericht dat in sommige gevallen wordt verstrekt door de autorisatieeindpunten van MVPD of door de degradatieregels van Programmer.
* Kan een lege tekenreeks of een `null` -waarde bevatten.


#### public helpUrl: string; {#public-help-url-string}

* De URL die een koppeling bevat naar meer informatie over de oorzaak van deze fout en de mogelijke oplossingen.
* Kan een lege tekenreeks of een `null` -waarde bevatten.

#### public trace: string; {#public-trace-string}

* De unieke id voor deze reactie, die kan worden gebruikt wanneer contact wordt opgenomen met ondersteuning om specifieke problemen in complexere scenario&#39;s te identificeren.
* Kan een lege tekenreeks of een `null` -waarde bevatten.

#### public action: string; {#public-action-string}

* De aanbevolen maatregelen om de situatie te verhelpen.
   * **niets**: Jammer genoeg is er geen vooraf bepaalde actie om deze kwestie te verhelpen. Dit kan wijzen op een onjuiste aanroep van de openbare API
   * **configuratie**: Een configuratieverandering is nodig door TVE dashboard of door steun te contacteren.
   * **toepassing-registratie**: De toepassing moet zich opnieuw registreren.
   * **authentificatie**: De gebruiker moet voor authentiek verklaren of opnieuw voor authentiek verklaren.
   * **vergunning**: De gebruiker moet vergunning voor het specifieke middel verkrijgen.
   * **degradatie**: Één of andere vorm van degradatie zou moeten worden toegepast.
   * **herprobeer**: Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen
   * **retry-after**: Het opnieuw proberen van het verzoek na de vermelde periode zou de kwestie kunnen oplossen.
* Kan een lege tekenreeks of een `null` -waarde bevatten.

### klasse-besluit {#class-decision}

#### public id: string; {#public-id-string}

* De bron-id waarvoor het besluit is genomen.

#### publiek toegestaan : Boolean ; {#public-auth-boolean}

* De waarde van de markering die aangeeft of de beslissing succesvol is of niet.

#### openbare fout: status; {#public-error-status}

* Aanvullende status (status)-informatie voor het geval er een fout optreedt. Plaats mogelijk een `null` -waarde.

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
     &quot;besluiten&quot;: [
     
     &quot;identiteitskaart&quot;: &quot;RES01&quot;, 
     &quot;geautoriseerd&quot;: waar 
    , 
     
     &quot;identiteitskaart&quot;: &quot;RES02&quot;, 
     &quot;geautoriseerd&quot;: vals 
    , 
     {
     &quot;identiteitskaart&quot; RES: &quot;03&quot;, 
     &quot;geautoriseerd&quot;: waar 
     
    ] 
     
    
    &quot;

</td>
  </tr>

<tr>
    <td>Ingeschakeld</td>
    <td>

     &quot;JavaScript 
     {
     &quot;besluiten&quot;: [
     
     &quot;identiteitskaart&quot;: &quot;RES01&quot;, 
     &quot;geautoriseerd&quot;: waar 
    , 
     
     &quot;identiteitskaart&quot;: &quot;RES02&quot;, 
     &quot;geautoriseerd&quot;: vals, 
     &quot;fout&quot;: 
     &quot;status&quot;: 40 3, 
     &quot;code&quot;: &quot;preauthentication_deny_by_mvpd&quot;, 
     &quot;bericht&quot;: &quot;De MVPD heeft een &quot;Weigeren\&quot;besluit teruggegeven toen het verzoeken van pre-vergunning voor de gespecificeerde middel.&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;niets&quot;
     
    , 
     {
     &quot;identiteitskaart&quot;: &quot;RES03&quot;, 
     &quot;geautoriseerd&quot;: waar 
    , 
    ] 
     
    
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
     &quot;besluiten&quot;: [
     
     &quot;identiteitskaart&quot;: &quot;RES01&quot;, 
     &quot;geautoriseerd&quot;: vals 
    , 
     
     &quot;identiteitskaart&quot;: &quot;RES02&quot;, 
     &quot;geautoriseerd&quot;: vals 
    , 
     {
     &quot;identiteitskaart&quot; RES: 03&quot;,
     &quot;geautoriseerd&quot;: vals 
     
    ] 
     
    
    &quot;

</td>
  </tr>

<tr>
    <td>Ingeschakeld</td>
    <td>

     &quot;JavaScript 
    
     {
     &quot;besluiten&quot;: [
     {
     &quot;identiteitskaart&quot;: &quot;RES01&quot;, 
     &quot;geautoriseerd&quot;: vals, 
     &quot;fout&quot;: 
     &quot;status&quot;: 403, 
     &quot;code&quot;: &quot;preauthentication_deny_by_mvpd&quot;, 
     &quot;bericht&quot;: &quot;MVPD heeft a teruggekeerd Beslissing \ &quot;ontken\&quot;wanneer het verzoeken van pre-vergunning voor het gespecificeerde middel.&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;niets&quot;
     
    , 
     
     &quot;id&quot;: &quot;RES02&quot;, 
     &quot;geautoriseerd&quot;: vals, 
     fout&quot;: 
     &quot;status&quot;: 403, 
     &quot;code&quot;: &quot;prepermission_by_mvpd&quot;, 
     &quot;message&quot;: &quot;The MVPD has returned a \&quot;Deny\&quot; decisions when request pre-Authorisation for the specified resource.&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;none&quot;
     
    , 
     {
     &quot;id&quot;: &quot;RES03&quot;, 
     &quot;geautoriseerd&quot;: vals, 
     &quot;fout&quot;: 
     &quot;status&quot;: 403, 
     &quot;code&quot;: &quot;maximum_executing_time_over&quot;, 
     &quot;message&quot;: &quot;The request did not complete in the maximum allowed time. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;retry&quot;
     
     
    ] 
     
    
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
     &quot;status&quot;: 
     &quot;status&quot;: 400, 
     &quot;code&quot;: &quot;internal_error&quot;, 
     &quot;message&quot;: &quot;The request failed due to an internal error.&quot;, 
     &quot;details&quot;: &quot;Required String[] parameter &quot;resource&quot; is not present&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
     &quot;actie&quot;: &quot;niets&quot;
    , 
     &quot;besluiten&quot;: [] 
     
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
     &quot;status&quot;: 
     &quot;status&quot;: 412, 
     &quot;code&quot;: &quot;missing_resource&quot;, 
     &quot;bericht&quot;: &quot;De middelparameter mist&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;niets&quot;
    , 
     &quot;decisions&quot;: [] 
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
     &quot;besluiten&quot;: [
     
     &quot;identiteitskaart&quot;: &quot;RES01&quot;, 
     &quot;geautoriseerd&quot;: vals, 
     &quot;fout&quot;: 
     &quot;status&quot;: 
     &quot;code&quot;: &quot;network_receive_error&quot;, 
     &quot;bericht&quot;: &quot;Er was een gelezen fout terwijl het terugwinnen van de reactie de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;,
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;retry&quot;
     
    , 
     
     &quot;id&quot;: &quot;RES02&quot;, 
     &quot;toegelaten&quot;: vals, 
     &quot;fout&quot;: 
     &quot;status&quot;: 403, 
     &quot;code&quot;: &quot;network_receive_error&quot;, 
     &quot;bericht&quot;: &quot;Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen.&quot;, 
     &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
     &quot;actie&quot;: &quot;retry&quot;
     
     
    ] 
     
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
     &quot;status&quot;: 
     &quot;status&quot;: 0, 
     &quot;code&quot;: &quot;authentication_session_missing&quot;, 
     &quot;bericht&quot;: &quot;De authentificatiesessie verbonden aan dit verzoek kon niet worden teruggewonnen. De gebruiker moet met gesteunde MVPD opnieuw voor authentiek verklaren om verder te gaan.&quot;, 
     &quot;actie&quot;: &quot;authentificatie&quot;
    , 
     &quot;besluiten&quot;: [] 
     
    
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
     &quot;status&quot;: 
     &quot;status&quot;: 0, 
     &quot;code&quot;: &quot;requestor_not_configured&quot;, 
     &quot;bericht&quot;: &quot;De aanvrager is nog niet gevormd die een eerste vereiste voor het gebruiken van om het even welke API behalve setRequestor API.&quot; is, 
     &quot;actie&quot;: &quot;retry&quot;
    , 
     &quot;besluiten&quot;: [] 
     
    &quot;

</td>
  </tr>
</tbody>
</table>
