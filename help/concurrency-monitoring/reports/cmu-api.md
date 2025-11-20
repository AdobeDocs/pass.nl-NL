---
title: API-overzicht
description: API-overzicht
exl-id: 3fe6f6d8-5b2f-47e5-a8da-06fb18a5d46b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '2043'
ht-degree: 0%

---

# API voor gelijktijdige bewaking {#cmu-api-usage}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## API-overzicht {#api-overview}

Het gebruik van de Controle van de gelijktijdige (CMU) wordt uitgevoerd als WOLAP (Web-based [&#x200B; Online Analytische Verwerking &#x200B;](http://en.wikipedia.org/wiki/Online_analytical_processing)) project. CMU is een generiek zaken-meldend Web API die door een gegevenspakhuis wordt gesteund. Het doet dienst als de vraagtaal van HTTP die typische verrichtingen toelaat OLAP om RESTfully worden uitgevoerd.


>[!NOTE]
>
>De CMU-API is niet algemeen beschikbaar. Neem contact op met uw Adobe-vertegenwoordiger voor vragen over beschikbaarheid.

CMU API verstrekt een hiërarchische mening van de onderliggende kubussen OLAP. Elk middel ([&#x200B; afmeting &#x200B;](/help/concurrency-monitoring/reports/cm-usage-reports.md#dimensions-2-filter-metrics) in de afmetinghiërarchie, in kaart gebracht als segment van de weg URL) produceert rapporten met (bijeengevoegde) [&#x200B; metriek &#x200B;](/help/concurrency-monitoring/reports/cm-usage-reports.md#monitor-metrics) voor de huidige selectie. Elke bron wijst naar de bovenliggende bron (voor roll-up) en de bijbehorende subbronnen (voor boor-down). Segmenteren en dicing worden bereikt via parameters van queryreeksen die de afmetingen vastzetten op specifieke waarden of bereiken.

De REST API verstrekt de beschikbare gegevens binnen een tijdsinterval dat in het verzoek wordt gespecificeerd (die terug naar standaardwaarden als niets wordt verstrekt), volgens de afmetingspad, verstrekte filters, en geselecteerde metriek. Het tijdbereik wordt niet toegepast op rapporten die geen tijdafmetingen bevatten (jaar, maand, dag, uur, minuut, seconde).

Het eindpunt URL wortelweg zal de algemene bijeengevoegde metriek binnen één enkel verslag, samen met de verbindingen aan de beschikbare boor-down opties terugkeren. De API-versie wordt toegewezen als het volgende segment van het URI-eindpuntpad. Bijvoorbeeld, https://mgmt.auth.adobe.com/cmu/ *v2* betekent dat de cliënten tot versie 2 van WOLAP zullen toegang hebben.

De beschikbare URL-paden kunnen worden gevonden via koppelingen in het antwoord. Geldige URL-paden worden vastgehouden om een pad in de onderliggende boor-down structuur toe te wijzen dat (pre)geaggregeerde metriek bevat. Een weg in de vorm /afmetingen1/dimensie2/dimensie3 zal op een pre-samenvoeging van die drie dimensies (het equivalent van een SQL componentenGROEP DOOR dimensie1, dimensie2, dimensie3) wijzen. Als een dergelijke pre-samenvoeging niet bestaat en het systeem het niet kan onmiddellijk berekenen, zal API een 404 niet Gevonden reactie terugkeren.

### Boor-down Boom {#drill-down-tree}

De volgende boor-down bomen illustreren de afmetingen (middelen) beschikbaar in CMU 2.0:

**Afmetingen beschikbaar aan de HUIDIGEN van CM**

![](../assets/new_breakdown.png)

Een `GET` naar het API-eindpunt van `https://mgmt.auth.adobe.com/cmu/v2` retourneert een representatie met:

* Koppelingen naar de beschikbare basisverdiepingspaden:

  ```html
  <link rel="drill-down" href="/cmu/v2/dimensionA"/>
  <link rel="drill-down" href="/cmu/v2/dimensionB"/>
  ```

* Een samenvatting (samengevoegde waarden) voor alle metriek (in het standaardinterval, aangezien geen parameters van het vraagkoord worden verstrekt, zie hieronder).

Na een boor-benedenweg (stap voor stap): /afmetingenA/jaar/maand/dag/dimensiX wint de volgende reactie terug:

* Koppelingen naar de opties voor diepteY en DimensieZ
* Een rapport met dagelijkse aggregaten voor elke waarde van dimensieX

### **Filters**

Met uitzondering van de datum-/tijddimensies, kan elke dimensie die beschikbaar is voor de huidige projectie (dimensiepad) worden gefilterd door de naam ervan als parameter voor queryreeksen te gebruiken.

De volgende filteropties zijn beschikbaar:

* **evenaart** filters worden verstrekt door de afmetingsnaam aan een bepaalde waarde in het vraagkoord te plaatsen.
* **IN** de filters kunnen worden gespecificeerd door de zelfde dimensie-naam parameter veelvoudige tijden met verschillende waarden toe te voegen: dimensie=value1&amp;dimensie=value2
* **niet evenaart** filters moeten &quot;!&quot; gebruiken symbool na de naam van de dimensie die resulteert in de &#39;!=&#39; &quot;operator&quot;: dimensie!=value
* **NIET IN** filters vereisen &quot;!=&#39;-operator moet meerdere keren worden gebruikt, één keer voor elke waarde in de set: dimensie!=value1&amp;dimensie!=value2&amp;...


Er is ook een speciaal gebruik voor de afmetingsnamen in het vraagkoord: Als de afmetingsnaam als parameter van het vraagkoord zonder waarde wordt gebruikt, zal dit API instrueren om een projectie terug te keren die die afmeting in het rapport omvat.

Voorbeeld-CMU-query&#39;s:

| URL | SQL Equivalent |
|:---|:---|
| /dimensie1/dimensie2/dimensie3?dimensie1=waarde1 | SELECTEER * van projectie WAAR dimensie1 = &quot;waarde1&quot;GROEP DOOR dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1=waarde1&amp;dimensie1=waarde2 | SELECTEER * vanuit projectie WAAR dimensie1 IN (&#39;value1&#39;, &#39;value2&#39;) GROEP BY dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1!=value1 | SELECTEER * vanuit projectie WAAR dimensie1 &lt;> &#39;waarde1&#39; GROEP BY dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1!=value1&amp;Dimensie2!=value2 | SELECTEER * vanuit projectie WAAR dimensie1 NIET IN (&#39;value1&#39;, &#39;value2&#39;) GROEP BY dimensie1, dimensie2, dimensie3 |
| Ervan uitgaande dat er geen direct pad is: /dimensie1/dimensie3 maar er is een pad: /dimensie1/dimensie2/dimensie3 </br></br> /dimensie1?dimensie3 | SELECTEREN * vanuit projectiegroep BY dimensie1,dimensie3 |

>[!NOTE]
>
>Geen van deze filtertechnieken werkt voor datum-/tijdafmetingen. De enige manier om datum-/tijdafmetingen te filteren is het begin en eind van de (hieronder beschreven) vraagkoordparameters aan de vereiste waarden te plaatsen.

De volgende parameters van het vraagkoord hebben gereserveerde betekenis voor API (en daarom kunnen zij niet als afmetingsnamen worden gebruikt, of anders geen het filtreren zou voor zulk een afmeting mogelijk zijn).

Parameters voor door CMU API gereserveerde queryreeks:

| Parameter | Optioneel | Beschrijving | Standaardwaarde | Voorbeeld |
|:--------------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| access_token | Ja | Als de IMS OAuth-beveiliging is ingeschakeld, kan het IMS-token worden doorgegeven als een standaardtoken voor Vergunning onder of als een parameter voor de queryreeks. | Geen | access_token=XXXXXX |
| dimensienaam | Ja | Elke naam van een dimensie - deze bevindt zich in het huidige URL-pad of in een geldig subpad. De waarde wordt behandeld als een filter voor gelijke waarden. Als er geen waarde is opgegeven, wordt de opgegeven dimensie ook in de uitvoer opgenomen, zelfs als deze niet is opgenomen of grenst aan het huidige pad | Geen | someDimension=someValue&amp;someOtherDimension |
| end | Ja | Eindtijd van het rapport in millis | Huidige tijd van de server | end=2012-07-30 |
| format | Ja | Gebruikt voor inhoudonderhandeling (met het zelfde effect maar lagere belangrijkheid dan de weg &quot;uitbreiding&quot; - zie hieronder). | Geen: tijdens de onderhandelingen over de inhoud worden de andere strategieën uitgeprobeerd | format=json |
| limiet | Ja | Maximumaantal rijen dat moet worden geretourneerd | Standaardwaarde die door de server in de zelfkoppeling wordt gerapporteerd als er geen limiet is opgegeven in de aanvraag | limit=1500 |
| cijfers | Ja | Lijst met door komma&#39;s gescheiden metrische namen die moeten worden geretourneerd. Deze lijst moet worden gebruikt voor het filteren van een subset van de beschikbare metriek (om de laadgrootte te reduceren) en ook voor het afdwingen van de API om een projectie te retourneren die de gevraagde metriek bevat (in plaats van de standaard optimale projectie). | Alle metriek beschikbaar voor de huidige projectie zal worden teruggekeerd voor het geval deze parameter niet wordt verstrekt. | metriek=m1,m2 |
| start | Ja | De tijd van het begin voor het rapport als ISO8601; de server zal het resterende deel invullen als slechts een prefix wordt verstrekt: b.v., zal start=2012 in start=2012-01-01 :00: 00:00 resulteren | Gerapporteerd door de server in de zelfverbinding; de server probeert redelijke gebreken te verstrekken die op geselecteerde tijdgranulariteit worden gebaseerd | start=2012-07-15 |


GET is momenteel de enige beschikbare HTTP-methode. Ondersteuning voor OPTIONS-/HEAD-methoden is mogelijk in toekomstige versies beschikbaar.



## CMU API-statuscodes {#cmu-api-status-codes}

| Statuscode | Reden | Beschrijving |
|:-----------|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 | OK | Het antwoord zal &quot;roll-up&quot;en &quot;boor-down&quot;verbindingen (indien van toepassing) bevatten. Het rapport zal als attribuut van het middel worden teruggegeven: een genestelde &quot;rapport&quot;element/bezit. |
| 400 | Ongeldig verzoek | De antwoordinstantie bevat een tekstbericht waarin wordt uitgelegd wat er mis is met het verzoek.  Een 400 beschadigde verzoekstatus gaat vergezeld van een verklarende tekst in het antwoordlichaam (duidelijk/tekstmedia type) dat nuttige informatie over de cliëntfout verstrekt. Naast de triviale scenario&#39;s zoals ongeldige datumformaten of filters toegepast op niet bestaande dimensies, zal het systeem ook weigeren om op vragen te antwoorden die een massaal volume van gegevens vereisen dat wordt teruggekeerd of geaggregeerd op de vlucht. |
| 401 | Ongeautoriseerd | Wordt veroorzaakt door een aanvraag die niet de juiste OAuth-headers bevat om de gebruiker te verifiëren |
| 403 | Verboden | Geeft aan dat het verzoek niet is toegestaan in de huidige beveiligingscontext. Dit gebeurt wanneer de gebruiker is geverifieerd, maar geen toegang heeft tot de gevraagde informatie |
| 404 | Niet gevonden | Vindt plaats als de aanvraag een ongeldig URL-pad bevat. Dit zou nooit moeten voorkomen als de cliënt &quot;boor-down&quot;/&quot;roll-up&quot;verbindingen volgt die met 200 reacties worden verstrekt |
| 405 | Methode niet toegestaan | Geeft aan dat een niet-ondersteunde methode is gebruikt in de aanvraag. Hoewel momenteel alleen de GET-methode wordt ondersteund, kunnen toekomstige versies HEAD of OPTIONS toestaan |
| 406 | Niet acceptabel | Geeft aan dat de client een niet-ondersteund mediatype heeft aangevraagd |
| 500 | Interne serverfout | &quot;Dit mag nooit gebeuren&quot; |
| 503 | Service niet beschikbaar | Signaleert een fout binnen de toepassing of zijn gebiedsdelen |

## Gegevensindelingen {#data-formats}

De gegevens zijn beschikbaar in de volgende indelingen:

* JSON (standaard)
* XML
* CSV
* HTML (voor demo-doeleinden)


De volgende strategieën voor inhoudonderhandeling kunnen door klanten worden gebruikt (de prioriteit wordt gegeven door de positie in de lijst - eerst dingen):

1. Een &quot;bestandsextensie&quot; die is toegevoegd aan het laatste segment van het URL-pad: bijvoorbeeld /cmu/v2/tenant/year/month/day.xml. Als de URL een queryreeks bevat, moet de extensie vóór het vraagteken komen: `/cmu/v2/tenant/year/month/day.csv?mvpd=SomeMVPD`
1. Een query-tekenreeksparameter voor indeling: bijv., `/cmu/report?format=json`
1. De standaard HTTP Accept-header: bijvoorbeeld, `Accept: application/xml`

Zowel ondersteunen de parameter &quot;extension&quot; als de parameter query de volgende waarden:

* xml
* json
* csv
* html

Als geen mediatype wordt opgegeven door een van de strategieën, produceert de API standaard JSON-inhoud.

## Hypertext Application Language (HAL) {#hypertext-app-lang}

Voor JSON en XML wordt de payload gecodeerd als HAL, zoals hier beschreven: `http://stateless.co/hal_specification.html` .

Het werkelijke rapport (een geneste tag/eigenschap genaamd &quot;report&quot;) bestaat uit de feitelijke lijst met records die alle geselecteerde/toepasselijke afmetingen en meetgegevens met hun waarden bevatten, en die als volgt zijn gecodeerd:

### JSON {#json}

```js
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML {#xml}

```xml
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report>
```

Voor XML- en JSON-indelingen is de volgorde van de velden (afmetingen en metriek) in een record niet opgegeven, maar consistent (de volgorde is in alle records hetzelfde). Klanten mogen echter niet vertrouwen op een bepaalde volgorde van de velden in een record.

De middelverbinding (het &quot;zelf&quot;rel in JSON en het &quot;href&quot;middelattribuut in XML) bevat de huidige weg en het vraagkoord dat voor het gealigneerde rapport wordt gebruikt. De vraagkoord zal alle impliciete en expliciete parameters openbaren, zodat de nuttige lading uitdrukkelijk het gebruikte tijdinterval, de impliciete filters (als om het even welk) zal wijzen, etc. De rest verbindingen binnen het middel zullen alle beschikbare segmenten bevatten die kunnen worden gevolgd om neer in de huidige gegevens te boren. Er wordt ook een koppeling voor roll-up toegevoegd die naar het bovenliggende pad verwijst (indien aanwezig). De `href` -waarde voor de boor-down/roll-up koppelingen bevat alleen het URL-pad (de queryreeks is niet inbegrepen, dus deze moet zo nodig door de client worden toegevoegd). Merk op dat niet alle parameters van het vraagkoord die (of geïmpliceerd) door het huidige middel worden gebruikt voor &quot;roll-up&quot;of &quot;boor-down&quot;verbindingen van toepassing zullen zijn (bijvoorbeeld, kunnen de filters niet op sub of super-middelen van toepassing zijn).

Voorbeeld (ervan uitgaande dat er één metrische waarde is die clients wordt genoemd en dat er een pre-aggregatie voor `year/month/day/...` is):

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.xml`

  ```xml
  <resource href="/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
    <links>
      <link rel="roll-up" href="/cmu/v2/year"/>
      <link rel="drill-down" href="/cmu/v2/year/month/day"/>
    </links>
    <report>
      <record month="6" year="2012" clients="205"/>
      <record month="7" year="2012" clients="466"/>
    </report>
  </resource>
  ```

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.json`

  ```js
  {
    "_links" : {
      "self" : {
        "href" : "/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
      },
      "roll-up" : {
        "href" : "/cmu/v2/year"
      },
      "drill-down" : {
        "href" : "/cmu/v2/year/month/day"
      }
    },
    "report" : [ {
      "month" : "6",
      "year" : "2012",
      "clients" : "205"
    }, {
      "month" : "7",
      "year" : "2012",
      "clients" : "466"
    } ]
  }
  ```

### CSV {#csv}

In de CSV-gegevensindeling worden geen koppelingen of andere metagegevens (behalve de koptekstrij) inline verschaft. In plaats daarvan worden de metagegevens van de selectie opgenomen in de bestandsnaam, die als volgt wordt weergegeven:

```html
report__<start-date>_<end-date>_<filter-values,...>.csv
```

CSV zal een kopbalrij en dan de rapportgegevens als verdere rijen bevatten. De koptekstrij bevat alle afmetingen gevolgd door alle meetgegevens. De soortorde van de rapportgegevens zal in de orde van de dimensies worden weerspiegeld. Daarom als de gegevens door D1 en dan door D2 worden gesorteerd, zal de kopbal CSV als: `D1, D2, ...metrics....` kijken

De volgorde van de velden in de koptekstrij komt overeen met de sorteervolgorde van de tabelgegevens.

Voorbeeld: https://mgmt.auth.adobe.com/cmu/v2/year/month.csv maakt een bestand met de naam ```report__2012-07-20_2012-08-20_1000.csv``` met de volgende inhoud:

| Jaar | Maand | Clients |
|:----:|:-----:|:-------:|
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## Gegevensversheid {#data-freshness}

Hoewel het verzoek een Laatst-Gewijzigde kopbal bevat, wijst het **NIET** op de tijd toen het rapport in het lichaam het laatst werd bijgewerkt. De algemene verslagen worden op regelmatige basis berekend, met de volgende regels:

* als de tijdgranulariteit **jaar** of **maand** is, dan wordt het rapport bijgewerkt om de 2 dagen
* als de tijdgranulariteit **dag** is, dan wordt het rapport bijgewerkt om de 3 uren
* als de tijdgranulariteit **uur** is, dan wordt het rapport bijgewerkt elk uur
* als de tijdgranulariteit **minuut** is, dan wordt het rapport bijgewerkt elke minuut

Het **activiteitsniveau** en **gelijktijdig niveau** rapporten worden bijgewerkt elke dag, ongeacht de tijdgranulariteit.

## GZIP-compressie {#gzip-compression}

Adobe raadt u ten zeerste aan ondersteuning van gzip in te schakelen voor clients die CMU-rapporten ophalen. Dit zal de responsgrootte aanzienlijk verminderen, wat op zijn beurt de responstijd vermindert. (De compressieverhouding voor CMU-gegevens ligt in het bereik 20-30.)

Als u gzip-compressie in uw client wilt inschakelen, stelt u de header Accepteren-coderen: als volgt in:

```
Accept-Encoding: gzip, deflate
```

## Gerelateerde informatie {#related-information}

* [CMU-overzicht](/help/concurrency-monitoring/reports/cm-usage-reports.md)
