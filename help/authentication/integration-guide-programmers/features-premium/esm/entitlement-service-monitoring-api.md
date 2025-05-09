---
title: Entitlement Service Monitoring API
description: Entitlement Service Monitoring API
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 0%

---

# Entitlement Service Monitoring API {#entitlement-service-monitoring-api}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Controleer voordat u de afbraakAPI gebruikt of aan de volgende voorwaarden is voldaan:
>
> * Haal de cliëntgeloofsbrieven zoals die in [ worden beschreven terug cliëntgeloofsbrieven ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
> * Haal het toegangstoken zoals die in [ wordt beschreven terug toegangstoken ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
>
> Verwijs naar de [ Dynamische documentatie van het Overzicht van de Registratie van de Cliënt ](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) voor meer informatie over hoe te om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

## API-overzicht {#api-overview}

De Controle van de Dienst van het recht (ESM) wordt uitgevoerd als WOLAP (Web-based [ Online Analytische Verwerking ](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank} ) project. ESM is een generisch zaken-meldend Web API die door een gegevenspakhuis wordt gesteund. Het doet dienst als de vraagtaal van HTTP die typische verrichtingen toelaat OLAP om RESTfully worden uitgevoerd.

>[!NOTE]
>
>De ESM API is niet algemeen beschikbaar. Neem contact op met uw Adobe voor vragen over beschikbaarheid.

ESM API verstrekt een hiërarchische mening van de onderliggende kubussen OLAP. Elk middel ([ afmeting ](#esm_dimensions) in de afmetinghiërarchie, in kaart gebracht als segment van de weg URL) produceert rapporten met (bijeengevoegde) [ metriek ](#esm_metrics) voor de huidige selectie. Elke bron wijst naar de bovenliggende bron (voor roll-up) en de bijbehorende subbronnen (voor boor-down). Segmenteren en dicing worden bereikt via parameters van queryreeksen die de afmetingen vastzetten op specifieke waarden of bereiken.

De REST API verstrekt de beschikbare gegevens binnen een tijdsinterval dat in het verzoek wordt gespecificeerd (die terug naar standaardwaarden als niets wordt verstrekt), volgens de afmetingspad, verstrekte filters, en geselecteerde metriek. Het tijdbereik wordt niet toegepast op rapporten die geen tijdafmetingen bevatten (jaar, maand, dag, uur, minuut, seconde).

Het eindpunt URL wortelweg zal de algemene bijeengevoegde metriek binnen één enkel verslag, samen met de verbindingen aan de beschikbare boor-down opties terugkeren. De API-versie wordt toegewezen als het volgende segment van het URI-eindpuntpad. `https://mgmt.auth.adobe.com/esm/v3` betekent bijvoorbeeld dat de clients toegang hebben tot WOLAP versie 3.

De beschikbare URL-paden kunnen worden gevonden via koppelingen in het antwoord. Geldige URL-paden worden vastgehouden om een pad in de onderliggende boor-down structuur toe te wijzen dat (pre)geaggregeerde metriek bevat. Een pad in de vorm `/dimension1/dimension2/dimension3` geeft een pre-aggregatie van deze drie dimensies weer (het equivalent van een SQL `clause GROUP` BY `dimension1` , `dimension2` , `dimension3` ). Als een dergelijke pre-samenvoeging niet bestaat en het systeem het niet kan onmiddellijk berekenen, zal API een 404 niet Gevonden reactie terugkeren.

## Boor-down Boom {#drill-down-tree}

De volgende boor-benedenbomen illustreren de afmetingen (middelen) beschikbaar in ESM 3.0 voor [ Programmers ](#progr-dimensions) en [ MVPDs ](#mvpd-dimensions).


### Dimensionen beschikbaar voor programmeurs {#progr-dimensions}

#### Dag

![](../../../assets/esm-progr-dimensions-day.png)

#### Uur

![](../../../assets/esm-progr-dimensions-hour.png)

#### Minute

![](../../../assets/esm-progr-dimensions-minute.png)

### Dimensionen die beschikbaar zijn voor MVPD&#39;s {#mvpd-dimensions}

![](../../../assets/esm-mvpd-dimensions.png)

Een GET naar het API-eindpunt van `https://mgmt.auth.adobe.com/esm/v3` retourneert een representatie met:

* Koppelingen naar de beschikbare basisverdiepingspaden:

   * `<link rel="drill-down" href="/v3/dimensionA"/>`

   * `<link rel="drill-down" href="/v3/dimensionB"/>`

* Een overzicht (geaggregeerde waarden) voor alle metriek (in de standaardwaarde)
interval, aangezien geen parameters van het vraagkoord worden verstrekt, zie hieronder).


Na een boor-down weg (stap voor stap):
`/dimensionA/year/month/day/dimensionX` haalt het volgende op
reactie:

* Koppelingen naar de opties voor &quot;`dimensionY`&quot; en &quot;`dimensionZ`&quot; voor de keuzelijst

* Een rapport met dagelijkse aggregaten voor elke waarde van `dimensionX`


### Filters

Met uitzondering van de datum-/tijddimensies, kan elke dimensie die beschikbaar is voor de huidige projectie (dimensiepad) worden gefilterd door de naam ervan als parameter voor queryreeksen te gebruiken.

De volgende filteropties zijn beschikbaar:

* **evenaart** filters worden verstrekt door de afmetingsnaam aan een bepaalde waarde in het vraagkoord te plaatsen.

* **IN** de filters kunnen worden gespecificeerd door de zelfde dimensie-naam parameter veelvoudige tijden met verschillende waarden toe te voegen: dimensie=value1\&amp;dimensie=value2

* **niet evenaart** filters moeten &quot;\!&quot; gebruiken symbool na de naam van de dimensie die resulteert in de &#39;\!=&#39; &quot;operator&quot;: dimensie\!=value

* **NIET IN** filters vereisen &quot;\!=&#39; operator moet meerdere keren worden gebruikt, één keer voor elke waarde in de set: dimensie\!=value1\&amp;Dimensie\!=value2&amp;...

Er is ook een speciaal gebruik voor de afmetingsnamen in het vraagkoord: Als de afmetingsnaam als parameter van het vraagkoord zonder waarde wordt gebruikt, zal dit API instrueren om een projectie terug te keren die die afmeting in het rapport omvat.

### Voorbeeld-ESM-query&#39;s

| *URL* | *SQL Equivalent* |
|---|---|
| /dimensie1/dimensie2/dimensie3?dimensie1=waarde1 | SELECTEER * vanuit projectie WAAR dimensie1 = &#39;waarde1&#39; </br> GROEP BY dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1=waarde1&amp;dimensie1=waarde2 | SELECTEER * vanuit projectie WAAR dimensie1 IN (&#39;value1&#39;, &#39;value2&#39;) </br> GROEP BY dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1!=value1 | SELECTEREN * vanuit projectie WAAR dimensie1 &lt;> &#39;value1&#39; | </br> GROEP BY dimensie1, dimensie2, dimensie3 |
| /dimensie1/dimensie2/dimensie3?dimensie1!=value1&amp;Dimensie2!=value2 | SELECTEER * vanuit projectie WAAR dimensie1 NIET IN (&#39;value1&#39;, &#39;value2&#39;) | </br> GROEP BY dimensie1, dimensie2, dimensie3 |
| Ervan uitgaande dat er geen direct pad is: /afmetingen1/dimensie3 </br> , maar dat er wel een pad is: /afmetingen1/dimensie2/dimensie3 </br> </br> /dimensie1?afmetingen3 | SELECTEREN * vanuit projectiegroep BY dimensie1, dimensie3 |

>[!NOTE]
>
>Geen van deze filtertechnieken werkt voor `date/time` -afmetingen. De enige manier om `date/time` -afmetingen te filteren, is door de parameters van de queryreeks `start` en `end` (hieronder beschreven) in te stellen op de vereiste waarden.

De volgende parameters van het vraagkoord hebben gereserveerde betekenis voor API (en daarom kunnen zij niet als afmetingsnamen worden gebruikt, of anders geen het filtreren zou voor zulk een afmeting mogelijk zijn).

### Door ESM API gereserveerde parameters voor queryreeks

| Parameter | Optioneel | Beschrijving | Standaardwaarde | Voorbeeld |
| --- | ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ---- | --- |
| access_token | Ja | Het token DCR kan worden doorgegeven als een standaard token voor Vergunninghouder. | Geen | access_token=XXXXXX |
| dimensienaam | Ja | Elke naam van een dimensie - deze bevindt zich in het huidige URL-pad of in een geldig subpad. De waarde wordt behandeld als een filter voor gelijke waarden. Als er geen waarde is opgegeven, wordt de opgegeven dimensie ook in de uitvoer opgenomen, zelfs als deze niet is opgenomen of grenst aan het huidige pad | Geen | someDimension=someValue&amp;someOtherDimension |
| end | Ja | Eindtijd van het rapport in millis | Huidige tijd van de server | end=2024-07-30 |
| format | Ja | Gebruikt voor inhoudonderhandeling (met het zelfde effect maar lagere belangrijkheid dan de weg &quot;uitbreiding&quot; - zie hieronder). | Geen: tijdens de onderhandelingen over de inhoud worden de andere strategieën uitgeprobeerd | format=json |
| limiet | Ja | Maximumaantal rijen dat moet worden geretourneerd | Standaardwaarde die door de server in de zelfkoppeling wordt gerapporteerd als er geen limiet is opgegeven in de aanvraag | limit=1500 |
| cijfers | Ja | Lijst met door komma&#39;s gescheiden metrische namen die moeten worden geretourneerd. Deze lijst moet worden gebruikt voor het filteren van een subset van de beschikbare metriek (om de laadgrootte te reduceren) en ook voor het afdwingen van de API om een projectie te retourneren die de gevraagde metriek bevat (in plaats van de standaard optimale projectie). | Alle metriek beschikbaar voor de huidige projectie zal worden teruggekeerd voor het geval deze parameter niet wordt verstrekt. | metriek=m1,m2 |
| start | Ja | De tijd van het begin voor het rapport als ISO8601; de server zal het resterende deel invullen als slechts een prefix wordt verstrekt: b.v., zal start=2024 in start=2024-01-01 :00: 00:00 resulteren | Gerapporteerd door de server in de zelfverbinding; de server probeert redelijke gebreken te verstrekken die op geselecteerde tijdgranulariteit worden gebaseerd | start=2024-07-15 |

De enige beschikbare HTTP-methode die momenteel beschikbaar is, is GET.

## ESM API-statuscodes {#esm-api-status-codes}

| Statuscode | Reden | Beschrijving |
|---|---|---|
| 200 | OK | Het antwoord zal &quot;roll-up&quot;en &quot;boor-down&quot;verbindingen (indien van toepassing) bevatten. Het rapport zal als attribuut van het middel worden teruggegeven: een genestelde &quot;rapport&quot;element/bezit. |
| 400 | Ongeldig verzoek | De antwoordinstantie bevat een tekstbericht waarin wordt uitgelegd wat er mis is met het verzoek. </br> </br> Een slechte verzoekstatus van 400 gaat vergezeld van een verklarende tekst in het antwoordlichaam (duidelijk/tekstmedia type) dat nuttige informatie over de cliëntfout verstrekt. Naast de triviale scenario&#39;s zoals ongeldige datumformaten of filters toegepast op niet bestaande dimensies, zal het systeem ook weigeren om op vragen te antwoorden die een massaal volume van gegevens vereisen dat wordt teruggekeerd of geaggregeerd op de vlucht. |
| 401 | Ongeautoriseerd | Wordt veroorzaakt door een aanvraag die niet de juiste OAuth-headers bevat om de gebruiker te verifiëren |
| 403 | Verboden | Geeft aan dat het verzoek niet is toegestaan in de huidige beveiligingscontext. Dit gebeurt wanneer de gebruiker is geverifieerd, maar geen toegang heeft tot de gevraagde informatie |
| 404 | Niet gevonden | Vindt plaats als de aanvraag een ongeldig URL-pad bevat. Dit zou nooit moeten voorkomen als de cliënt &quot;boor-down&quot;/&quot;roll-up&quot;verbindingen volgt die met 200 reacties worden verstrekt |
| 405 | Methode niet toegestaan | Geeft aan dat een niet-ondersteunde methode is gebruikt in de aanvraag. Hoewel momenteel alleen de methode GET wordt ondersteund, kunnen toekomstige versies HEAD of OPTIONS toestaan |
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

1. Een &quot;bestandsextensie&quot; die is toegevoegd aan het laatste segment van het URL-pad: bijvoorbeeld `/esm/v3/media-company/year/month/day.xml` . Als de URL een queryreeks bevat, moet de extensie vóór het vraagteken komen: `/esm/v3/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Een query-tekenreeksparameter voor indeling: bijv., `/esm/report?format=json`
1. De standaard HTTP Accept-header: bijvoorbeeld, `Accept: application/xml`

Zowel ondersteunen de parameter &quot;extension&quot; als de parameter query de volgende waarden:

* xml
* json
* csv
* html

Als geen mediatype wordt opgegeven door een van de strategieën, produceert de API standaard JSON-inhoud.

## Toepassingstaal Hypertext {#hypertext-application-language}

Voor JSON en XML wordt de payload gecodeerd als HAL, zoals hier beschreven: <http://stateless.co/hal_specification.html> .

Het werkelijke rapport (een geneste tag/eigenschap genaamd &quot;report&quot;) bestaat uit de feitelijke lijst met records die alle geselecteerde/toepasselijke afmetingen en meetgegevens met hun waarden bevatten, en die als volgt zijn gecodeerd:

### JSON

```JSON
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

### XML

```XML
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report
```

Voor XML- en JSON-indelingen is de volgorde van de velden (afmetingen en metriek) in een record niet opgegeven, maar consistent (de volgorde is in alle records hetzelfde). Klanten mogen echter niet vertrouwen op een bepaalde volgorde van de velden in een record.

De middelverbinding (het &quot;zelf&quot;rel in JSON en het &quot;href&quot;middelattribuut in XML) bevat de huidige weg en het vraagkoord dat voor het gealigneerde rapport wordt gebruikt. De vraagkoord zal alle impliciete en expliciete parameters openbaren, zodat de nuttige lading uitdrukkelijk het gebruikte tijdinterval, de impliciete filters (als om het even welk) zal wijzen, etc. De rest verbindingen binnen het middel zullen alle beschikbare segmenten bevatten die kunnen worden gevolgd om neer in de huidige gegevens te boren. Er wordt ook een koppeling voor roll-up toegevoegd die naar het bovenliggende pad verwijst (indien aanwezig). De `href` -waarde voor de boor-down/roll-up koppelingen bevat alleen het URL-pad (de queryreeks is niet inbegrepen, dus deze moet zo nodig door de client worden toegevoegd). Merk op dat niet alle parameters van het vraagkoord die (of geïmpliceerd) door het huidige middel worden gebruikt voor &quot;roll-up&quot;of &quot;boor-down&quot;verbindingen van toepassing zullen zijn (bijvoorbeeld, kunnen de filters niet op sub of super-middelen van toepassing zijn).

Voorbeeld (ervan uitgaande dat er één metrische waarde is met de naam `clients` en dat er een vooraggregatie voor `year/month/day/...` is):

* https://mgmt.auth.adobe.com/esm/v3/year/month.xml

```XML
   <resource href="/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v3/year"/>
   <link rel="drill-down" href="/esm/v3/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2024" clients="205"/>
   <record month="7" year="2024" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v3/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21"
          },
          "roll-up" : {
            "href" : "/esm/v3/year"
          },
          "drill-down" : {
            "href" : "/esm/v3/year/month/day"
          }
        },
        "report" : [ {
          "month" : "6",
          "year" : "2024",
          "clients" : "205"
        }, {
          "month" : "7",
          "year" : "2024",
          "clients" : "466"
        } ]
      }
  ```

### CSV

In de CSV-gegevensindeling worden geen koppelingen of andere metagegevens (behalve de koptekstrij) inline verschaft. In plaats daarvan worden de metagegevens van de selectie opgenomen in de bestandsnaam, die als volgt wordt weergegeven:

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

CSV zal een kopbalrij en dan de rapportgegevens als verdere rijen bevatten. De koptekstrij bevat alle afmetingen gevolgd door alle meetgegevens. De soortorde van de rapportgegevens zal in de orde van de dimensies worden weerspiegeld. Als de gegevens daarom worden gesorteerd op `D1` en vervolgens op `D2` , ziet de CSV-header er als volgt uit: `D1, D2, ...metrics...` .

De volgorde van de velden in de koptekstrij komt overeen met de sorteervolgorde van de tabelgegevens.


Voorbeeld: https://mgmt.auth.adobe.com/esm/v3/year/month.csv maakt een bestand met de naam `report__2024-07-20_2024-08-20_1000.csv` met de volgende inhoud:


| Jaar | Maand | Clients |
| ---- | :---: | ------- |
| 2024 | 6 | 580 |
| 2024 | 7 | 231 |

## Gegevensversheid {#data-freshness}

De geslaagde HTTP-reacties bevatten een `Last-Modified` -header die aangeeft op welk tijdstip het rapport in de hoofdtekst voor het laatst is bijgewerkt. Het gebrek aan een Laatst-Gewijzigde kopbal wijst erop dat de rapportgegevens in real time worden gegevens verwerkt.

Gewoonlijk worden grove korrelige gegevens minder vaak bijgewerkt dan fijnkorrelige gegevens (bijv. waarden per minuut of uurwaarden kunnen actueler zijn dan de dagelijkse waarden, vooral voor metriek die niet kan worden berekend op basis van kleinere granulariteiten, zoals unieke tellingen).

## GZIP-compressie {#gzip-compression}

De Adobe adviseert sterk dat u gzip steun in cliënten toelaat die ESM- rapporten halen. Dit zal de responsgrootte aanzienlijk verminderen, wat op zijn beurt de responstijd vermindert. (De compressieverhouding voor ESM-gegevens ligt in het bereik 20-30.)

Als u gzip-compressie wilt inschakelen in uw client, stelt u de header `Accept-Encoding:` als volgt in:

* Accepteren-coderen: gzip, deflate
