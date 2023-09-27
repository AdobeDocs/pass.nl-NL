---
title: Verbeterde foutcodes
description: Verbeterde foutcodes
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 40aeba47c293f2cc4569f01a6fb1ca4bfbcd0de6
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# Verbeterde foutcodes {#enhanced-error-codes}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

In dit document wordt de lijst met API-foutcodes en aanvullende foutinformatie beschreven die naar de toepassingen wordt geretourneerd.

Om de Verbeterde Codes van de Fout in de toepassing van Programmers te gebruiken, moet een verzoek aan het team van de Steun worden ingediend om het met een configuratieverandering toe te laten.

## Behandeling van antwoordfout {#response-error-handling}

In de meeste scenario&#39;s bevat de Adobe Pass Authentication API aanvullende foutinformatie in de responsiestructuur om **betekenisvolle context** waarom een bepaalde fout is opgetreden en/of mogelijke oplossingen om het probleem automatisch op te lossen.  *In sommige specifieke gevallen met verificatie- of uitlogstromen kunnen de Adobe Pass-verificatieservices echter een HTML-reactie of een leeg lichaam retourneren. Raadpleeg de API-documentatie voor meer informatie.*

Terwijl bepaalde types van fouten automatisch kunnen worden behandeld (zoals het opnieuw proberen van een vergunningsverzoek in het geval van een netwerkonderbreking of gebruiker vereisen om opnieuw voor authentiek te verklaren als hun zitting is verlopen), zouden andere types configuratieveranderingen of de interactie van het team van de klantenzorg kunnen vereisen. Het is belangrijk voor Programmeurs om volledige fouteninformatie in dergelijke gevallen te verzamelen en te verstrekken.

De Adobe Pass-verificatie-API retourneert HTTP-statuscodes in het bereik 400-500 om fouten of fouten aan te geven. Elke HTTP-statuscode heeft bepaalde implicaties:

- De 4xx-foutcodes impliceren dat de fout door de client wordt gegenereerd en dat de client extra werk moet doen om de fout te verhelpen (bijvoorbeeld een toegangstoken verkrijgen voordat beschermde API&#39;s worden aangeroepen of een vereiste parameter wordt opgegeven)

- De 5xx-foutcodes impliceren dat de fout wordt gegenereerd door de server en dat de server extra werk moet doen om deze te herstellen.

De aanvullende foutinformatie wordt opgenomen in het veld &quot;Fout&quot; in de antwoordinstantie.

<table>
<thead>
  <tr>
    <th>Naam</th>
    <th>Type</th>
    <th>Voorbeeld</th>
    <th>Beschrijving</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>fout</td>
    <td><i>object</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://tve.helpdocsonline.com/errors<br>&nbsp;&nbsp;&nbsp;&nbsp;/network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://tve.helpdocsonline.com/errors/network_connection_failure&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Verwijst naar verzamelde inzameling of foutenvoorwerpen terwijl het proberen om het verzoek te voltooien.</td>
  </tr>
</tbody>
</table>

</br>

Adobe Pass API&#39;s die meerdere items verwerken (API voor voorafgaande toestemming, enz.) geven mogelijk aan of de verwerking voor een bepaald item is mislukt en met succes voor andere items is uitgevoerd met behulp van foutgegevens op itemniveau. In dit geval worden de ***&quot;error&quot;*** object bevindt zich op itemniveau en de responstekst kan meerdere items bevatten ***&quot;fouten&quot;*** objecten - raadpleeg de API-documentatie.

</br>

**Voorbeeld met gedeeltelijk succes en fout op itemniveau**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://tve.helpdocsonline.com/errors/network_connection_failure",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Elk foutobject heeft de volgende parameters:

| Naam | Type | Voorbeeld | Beperkt | Beschrijving |
|---|---|----|:---:|---|
| *status* | *integer* | *403* | &amp;controleren; | De HTTP-statuscode van de respons zoals beschreven in RFC 7231 (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 Ongeldig verzoek</li><li>401 Niet-toegelaten</li><li>403 Niet toegestaan</li><li>404 Niet gevonden</li><li>405 Methode niet toegestaan</li><li>409 Conflict</li><li>410 Gone</li><li>412 Voorwaarde is mislukt</li><li>429 Te veel verzoeken</li><li>500 Interval-serverfout</li><li>503 Service niet beschikbaar</li></ul> |
| *code* | *string* | *network_connection_failure* | &amp;controleren; | De standaard Adobe Pass-verificatiefoutecode. De volledige lijst met foutcodes is hieronder opgenomen. |
| *message* | *string* | *Kan geen contact opnemen met uw tv-provider services* | | Het leesbare bericht van de mens dat aan het eind - gebruiker kan worden getoond. |
| *details* | *string* | *Uw abonnementspakket bevat niet het kanaal &quot;Live&quot;* | | In sommige gevallen wordt een gedetailleerd bericht verstrekt door de MVPD toestemmingseindpunten of door de programmeur door degradatieregels. <p> Merk op dat als geen douanebericht van de partnerdiensten werd ontvangen dan dit gebied niet op de foutengebieden zou kunnen aanwezig zijn. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | Een URL die een koppeling bevat naar meer informatie over waarom deze fout is opgetreden en mogelijke oplossingen. <p>De URI vertegenwoordigt een absolute URL en mag niet worden afgeleid van foutcode. Afhankelijk van de foutcontext kan een andere URL worden opgegeven. Bijvoorbeeld, zal de zelfde bad_request foutencode verschillende URL&#39;s voor authentificatie en vergunningsdiensten opbrengen. |
| *traceren* | *string* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Een unieke id voor deze reactie die kan worden gebruikt wanneer contact wordt opgenomen met ondersteuning om specifieke problemen in complexere scenario&#39;s te identificeren. |
| *action* | *string* | *opnieuw proberen* | &amp;controleren; | Aanbevolen maatregelen om de situatie te verhelpen: <ul><li> *none* - Helaas is er geen vooraf bepaalde actie om dit probleem op te lossen. Dit kan wijzen op een onjuiste aanroep van de openbare API</li><li>*configuratie* - Een configuratiewijziging is nodig via het TVE-dashboard of door contact op te nemen met de ondersteuningsafdeling. </li><li>*registratie van aanvragen* - De toepassing moet zich opnieuw registreren. </li><li>*verificatie* - De gebruiker moet verifiëren of opnieuw verifiëren. </li><li>*autorisatie* - De gebruiker moet toestemming voor de specifieke bron verkrijgen. </li><li>*aantasting* - Er dient enige vorm van afbraak te worden toegepast. </li><li>*opnieuw proberen* - Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen</li><li>*retry-after* - Het opnieuw indienen van het verzoek na de aangegeven periode zou de kwestie kunnen oplossen.</li></ul> |

</br>

**Opmerkingen:**

- ***Beperkt*** kolom *Hiermee wordt aangegeven of de desbetreffende veldwaarde een eindige set vertegenwoordigt* (bijvoorbeeld bestaande HTTP-statuscodes voor &quot;*status*&quot;). In de toekomst kunnen updates van deze specificatie waarden toevoegen aan de beperkte lijst, maar worden bestaande waarden niet verwijderd of gewijzigd. Onbeperkte velden kunnen doorgaans gegevens bevatten, maar er kunnen beperkingen gelden om een redelijke grootte te garanderen.

- Elke reactie van de Adobe zal &quot;Adobe-verzoek-identiteitskaart&quot;bevatten die het cliëntverzoek door onze diensten van HTTP identificeert. De &quot;**traceren**&quot; het veld vult dat aan en zij dienen samen te worden gerapporteerd.

## HTTP-statuscodes en foutcodes {#http-status-codes-and-error-codes}

De inconsistenties tussen verschillende foutcodes en de bijbehorende HTTP-statuscodes zijn het gevolg van de achterwaartse compatibiliteitsvereisten met oudere SDK&#39;s en toepassingen (bijvoorbeeld *onbekend\_toepassing* resulteert in een onjuiste aanvraag van 400 aanvragen terwijl *onbekend\_software\_statement* rendement 401 niet toegestaan). Het oplossen van deze inconsistenties zal in toekomstige herhalingen gericht zijn.

## Handelingen en foutcodes {#actions-and-error-codes}

Voor de meeste foutcodes kunnen meerdere acties in aanmerking komen als paden naar het oplossen van het probleem in kwestie of zelfs meerdere acties zijn vereist om ze automatisch te corrigeren. We hebben ervoor gekozen de fout met de hoogste waarschijnlijkheid aan te geven. De **handelingen** kan in drie categorieën worden gesplitst:

1. degenen die proberen om de verzoekcontext te bevestigen (opnieuw proberen, opnieuw proberen-na)
1. degenen die proberen de gebruikerscontext binnen de toepassing (toepassing-registratie, authentificatie, vergunning) te bevestigen
1. die de integratiecontext tussen een toepassing en een identiteitsprovider proberen te herstellen (configuratie, degradatie)

Voor de eerste categorie (opnieuw proberen en opnieuw proberen-na), eenvoudig zou het opnieuw proberen van het zelfde verzoek genoeg kunnen zijn om de kwestie op te lossen. In gevallen van APIs die veelvoudige punten behandelen, zou de toepassing het verzoek moeten herhalen en slechts die punten met &quot;retry&quot;of &quot;retry-after&quot;actie omvatten. Voor &quot;*retry-after*&quot; actie, een &quot;<u>Opnieuw proberen na</u>De header geeft aan hoeveel seconden de toepassing moet wachten voordat de aanvraag wordt herhaald.

Voor de tweede en derde categorie is de daadwerkelijke uitvoering van de actie in hoge mate afhankelijk van de toepassingsfuncties. Bijvoorbeeld &quot;*aantasting*&quot; kan worden geïmplementeerd als &quot;switch to 15 minuten temporary pass to allow users playback of the content&quot; of als &quot;automatic tool to apply AUTHN-ALL or AUTHZ-ALL degradatie for its integration with the specified MVPD&quot;. Vergelijkbaar met &quot;*verificatie*&quot;Actie kan een passieve verificatie (back-kanaalverificatie) op een tablet en een volledige tweede schermverificatiestroom op aangesloten tv&#39;s activeren. Daarom hebben we ervoor gekozen om volledige URL&#39;s een schema en alle parameters te geven.

## Foutcodes {#error-codes}

In de onderstaande tabel met fouten worden de mogelijke foutcodes, de bijbehorende berichten en mogelijke acties weergegeven.

| Handeling | Foutcode | HTTP-statuscode | Beschrijving |
|---|---|---|---|
| **none** | *authentication_deny_by_mvpd* | 403 | MVPD heeft een &quot;Weigeren&quot;besluit teruggegeven toen het verzoeken van vergunning voor de gespecificeerde middel. |
|  | *authentication_deny_by_parental_controls* | 403 | MVPD heeft een &quot;Weigeren&quot;besluit wegens ouderlijke controlemontages voor het gespecificeerde middel teruggegeven. |
|  | *authentication_deny_by_programmer* | 403 | De degradatieregel die door de Programmer wordt toegepast handhaaft een &quot;ontkent&quot;besluit voor de huidige gebruiker. |
|  | *bad_request* | 400 | De API-aanvraag is ongeldig of heeft een onjuiste indeling. Controleer de API-documentatie om de vereisten voor de aanvraag te bepalen. |
|  | *individualization_service_unavailable* | 503 | De aanvraag is mislukt omdat de individualisatieservice niet beschikbaar is. |
|  | *internal_error* | 500 | De aanvraag is mislukt vanwege een interne serverfout. |
|  | *invalid_client_time* | 400 | Datum/tijd/tijdzone van de clientcomputer is niet correct ingesteld. Dit zal waarschijnlijk leiden tot verificatie-/vergunningsfouten. |
|  | *invalid_custom_scheme* | 400 | Het opgegeven aangepaste schema dat in de toepassingsregistratie wordt gebruikt, wordt niet herkend. Controleer de TVE-dashboardconfiguratie op de juiste aangepaste schemawaarden. |
|  | *invalid_domain* | 400 | De aanvrager gebruikt een ongeldig domein. Alle domeinen die door een bepaalde identiteitskaart van de Aanvrager worden gebruikt moeten door Adobe worden gewhitelliseerd. |
|  | *invalid_header* | 400 | De aanvraag is mislukt omdat deze een ongeldige koptekst bevat. Controleer de API-documentatie om te bepalen welke headers geldig zijn voor uw aanvraag en of er beperkingen zijn voor de waarde ervan. |
|  | *invalid_http_method* | 405 | De HTTP-methode die aan de aanvraag is gekoppeld, wordt niet ondersteund. Controleer de API documentatie om de gesteunde methodes van HTTP voor uw verzoek te bepalen. |
|  | *invalid_parameter_value* | 400 | De aanvraag is mislukt omdat deze een ongeldige parameter of parameterwaarde bevat. Controleer de API-documentatie om te bepalen welke parameters geldig zijn voor uw aanvraag en of er beperkingen zijn voor de waarde ervan. |
|  | *invalid_resource_value* | 400 | De aanvraag is mislukt omdat een ongeldige of onjuist gevormde bron is gebruikt. Controleer de API-documentatie om te bepalen hoe complexe bronnen voor uw aanvraag moeten worden gecodeerd en of er beperkingen zijn voor de waarde en/of grootte van de bronnen. |
|  | *invalid_registration_code* | 404 | De opgegeven registratiecode is niet langer geldig of is verlopen. |
|  | *invalid_service_configuration* | 500 | De aanvraag is mislukt vanwege een onjuiste serviceconfiguratie. |
|  | *missing_authentication_header* | 400 | De aanvraag is mislukt omdat deze niet de vereiste verificatieheader voor de specifieke API bevat. |
|  | *missing_resource_mapping* | 400 | Er is geen overeenkomstige afbeelding voor de opgegeven bron. Neem contact op met de ondersteuning om de vereiste toewijzing te herstellen. |
|  | *preauthentication_deny_by_mvpd* | 403 | MVPD heeft een &quot;Weigeren&quot;besluit teruggegeven toen het verzoeken van pre-vergunning voor de gespecificeerde middel. |
|  | *preauthentication_deny_by_programmer* | 403 | De degradatieregels die door Programmer worden toegepast dwingen een &quot;ontkennen&quot;besluit voor de huidige gebruiker af. |
|  | *registration_code_service_unavailable* | 503 | De aanvraag is mislukt omdat de service voor registratiecode niet beschikbaar is. |
|  | *service_unavailable* | 503 | Het verzoek is mislukt omdat de authenticatie- of autorisatiedienst niet beschikbaar is. |
|  | *access_token_unavailable* | 400 | De aanvraag is mislukt vanwege een onverwachte fout tijdens het ophalen van het toegangstoken. Controleer de configuratie van het TVE-dashboard op beschikbare softwareinstructies en geregistreerde aangepaste schema&#39;s. |
|  | *unsupported_client_version* | 400 | Deze versie van de Adobe Pass Authentication SDK is te oud en wordt niet meer ondersteund. Raadpleeg de API-documentatie voor de stappen die nodig zijn om een upgrade naar de nieuwste versie uit te voeren. |
| **configuratie** | *network_required_ssl* | 403 | Er is een SSL verbindingsprobleem voor de dienst van de doelpartner. Neem contact op met het ondersteuningsteam. |
|  | *te_many_resources* | 403 | De autorisatie- of voorafgaande autorisatieaanvraag is mislukt omdat er te veel bronnen zijn gevraagd. Neem contact op met het ondersteuningsteam om de beperkingen voor autorisatie en autorisatie correct te configureren. |
|  | *unknown_programmer* | 400 | De programmeur of serviceprovider wordt niet herkend. Gebruik het TVE-dashboard om de opgegeven programmeur te registreren. |
|  | *unknown_application* | 400 | De toepassing wordt niet herkend. Gebruik het TVE-dashboard om de opgegeven toepassing te registreren. |
|  | *unknown_integration* | 400 | De integratie tussen de opgegeven programmeur en identiteitsprovider bestaat niet. Gebruik het TVE-dashboard om de vereiste integratie te maken. |
|  | *unknown_software_statement* | 401 | De softwareverklaring verbonden aan het toegangstoken wordt niet erkend. Neem contact op met het ondersteuningsteam voor meer informatie over de status van de software-instructie. |
| **registratie van aanvragen** | *access_token_expired* | 401 | Het toegangstoken is verlopen. De toepassing moet het toegangstoken vernieuwen, zoals aangegeven in de API-documentatie. |
|  | *invalid_access_token_signature* | 401 | De handtekening voor het toegangstoken is niet meer geldig. De toepassing moet het toegangstoken vernieuwen, zoals aangegeven in de API-documentatie. |
|  | *invalid_client_id* | 401 | De bijbehorende client-id wordt niet herkend. De toepassing moet het registratieproces van de toepassing volgen, zoals aangegeven in de API-documentatie. |
| **verificatie** | *authentication_session_expired* | 410 | De huidige verificatiesessie is verlopen. De gebruiker moet opnieuw met gesteund MVPD voor verdere verificatie verifiëren. |
|  | *authentication_session_missing* | 401 | De verificatiesessie die aan deze aanvraag is gekoppeld, kan niet worden opgehaald. De gebruiker moet opnieuw met gesteund MVPD voor verdere verificatie verifiëren. |
|  | *authentication_session_invalidate* | 401 | De verificatiesessie is ongeldig gemaakt door de identiteitsprovider. De gebruiker moet opnieuw met gesteund MVPD voor verdere verificatie verifiëren. |
|  | *authentication_session_publisher_mismatch* | 400 | De autorisatieaanvraag is mislukt omdat de aangegeven MVPD voor de autorisatiestroom anders is dan die welke de authenticatiesessie heeft afgegeven. De gebruiker moet met gewenste MVPD opnieuw voor authentiek verklaren om verder te gaan. |
|  | *authentication_deny_by_hba_policies* | 403 | MVPD heeft een &quot;Weigeren&quot;besluit op huis-gebaseerd authentificatiebeleid teruggegeven. De huidige authentificatie werd verkregen gebruikend een op huis-gebaseerde authentificatiestroom (HBA) maar het apparaat is niet meer thuis wanneer het verzoeken van om toestemming voor het gespecificeerde middel. De gebruiker moet opnieuw met gesteund MVPD voor verdere verificatie verifiëren. |
|  | *identity_not_recognized_by_mvpd* | 403 | Het verzoek om toestemming is mislukt omdat de identiteit van de gebruiker niet door het MVPD is erkend. |
| **autorisatie** | *authentication_expired* | 410 | De vorige autorisatie voor de opgegeven resource is verlopen. De gebruiker moet een nieuwe vergunning verkrijgen om verder te gaan. |
|  | *authentication_not_found* | 404 | Er is geen autorisatie gevonden voor de opgegeven resource. De gebruiker moet een nieuwe vergunning verkrijgen om verder te gaan. |
|  | *device_identifier_mismatch* | 403 | De opgegeven apparaat-id komt niet overeen met de identificatie van het autorisatieapparaat. De gebruiker moet een nieuwe vergunning verkrijgen om verder te gaan. |
| **opnieuw proberen** | *network_connection_failure* | 403 | Er was een verbindingsmislukking met de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|  | *network_connection_timeout* | 403 | Er was een verbindingsonderbreking met de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|  | *network_receive_error* | 403 | Er was een gelezen fout terwijl het terugwinnen van de reactie van de bijbehorende partnerdienst. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
|  | *maximum_execute_time_over* | 403 | De aanvraag is niet binnen de maximaal toegestane tijd voltooid. Het opnieuw proberen van het verzoek zou de kwestie kunnen oplossen. |
| **retry-after** | *te_many_Requests* | 429 | Er zijn te veel verzoeken verzonden binnen een bepaald interval. De toepassing kan het verzoek na de voorgestelde periode opnieuw proberen. |
|  | *user_rate_limit_over* | 429 | Een bepaalde gebruiker heeft binnen een bepaald interval te veel aanvragen ingediend. De toepassing kan het verzoek na de voorgestelde periode opnieuw proberen. |
