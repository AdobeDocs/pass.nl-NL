---
title: Metagegevens gebruiker
description: Metagegevens gebruiker
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Metagegevens gebruiker {#user-metadata}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>
</br>

## Inleiding {#intro}

De eigenschap van Meta-gegevens van de Gebruiker laat programmeurs toe om tot verschillende types van gebruiker-specifieke gegevens toegang te hebben die door MVPDs worden gehandhaafd.  De meta-gegevenstypes van de gebruiker omvatten postcodes, ouderlijke classificaties, gebruiker - IDs, en meer.  *meta-gegevens van de Gebruiker 1} is een uitbreiding aan de eerder beschikbare* statische *meta-gegevens (het teken van de Authentificatie TTL, het teken van de Toestemming TTL, en identiteitskaart van het Apparaat).*


Belangrijke punten van metagegevens van gebruiker:

- Voldoet aan de aanvraag van de programmeur tijdens de verificatie- en machtigingsstromen
- Waarden worden opgeslagen in de token
- De waarden kunnen worden genormaliseerd als de verschillende MVPDs gegevens in verschillende formaten verstrekken
- Sommige parameters kunnen worden gecodeerd gebruikend de sleutel van de Programmer (b.v. zip code), zie [ Certificaat van Metagegevens van de Gebruiker voor encryptie ](user-metadata-certificate.md) voor de generatie van het encryptiecertificaat
- Specifieke waarden worden door de Adobe beschikbaar gesteld via een configuratiewijziging

## Gebruikersmetagegevens verkrijgen {#obtaining}

De meta-gegevens van de gebruiker is beschikbaar aan Programmeurs via de functie AccessEnabler `getMetadata()` en via het `/usermetadata` eindpunt in Clientless API.  Raadpleeg de API-documentatie van het platform voor meer informatie over het gebruik van `getMetadata()` en de bijbehorende callback `setMetadataStatus()` (of voor de eindpunten en parameters die worden gebruikt in de API zonder client).

Programmeurs krijgen metagegevens door een sleutel op te geven voor het type metagegevens dat ze willen verkrijgen: `getMetadata(key)` .

De metagegevens worden als volgt geretourneerd: `setMetadataStatus(key, encrypted, data)`

| Parameter | Type | Beschrijving |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | String | Geeft het type gevraagde metagegevens aan |
| `encrypted` | Boolean | Een Booleaanse markering die aangeeft of de &quot;waarde&quot; al dan niet gecodeerd is. Als dit &quot;waar&quot;is dan zal de &quot;waarde&quot;eigenlijk een Gecodeerde vertegenwoordiging van het Web JSON van de daadwerkelijke waarde zijn. |
| `data` | Object | Een JSON-object dat de weergave van de metagegevens bevat |



De structuur van de gegevensparameter, de waarden variëren tussen types:

| Sleutel | Type waarde | Monster | Beschrijving |
| --- | --- | --- | --- |
| `zip` | JSON-array | \[&quot;77754&quot;, &quot;12345&quot;\] | Postcode |
| `householdID` | JSON-tekenreeks | &quot;1o7241p&quot; | Huishoudelijke identificatiecode. Als de MVPD geen subaccounts ondersteunt, is dit gelijk aan `userID` |
| `maxRating` | JSON-object | {MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | Maximale ouderlijke classificatie voor de gebruiker |
| `userID` | JSON-tekenreeks | &quot;1o7241p&quot; | De gebruikers-id. Als een MVPD subaccounts ondersteunt en de gebruiker niet de hoofdaccount is, zal `userID` anders zijn dan `householdID` . |
| `channelID` | JSON-array | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Een lijst met kanalen die een gebruiker mag bekijken |
| `is_hoh` | JSON-tekenreeks | &quot;1&quot; | Een markering die aangeeft of een gebruiker het hoofd van een huishouden is |
| `encryptedZip` | JSON-tekenreeks | &quot;&quot; | Met Comcast wordt de postcode gecodeerd |
| `typeID` | JSON-tekenreeks | Primair | Een vlag die identificeert als de gebruikersrekening primaire/secundaire rekening is |
| `primaryOID` | JSON-tekenreeks | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Huishoudelijke identificatiecode. Als `typeID` Primair is, bevat deze de waarde van `userID` |
| hba_status | Boolean | &quot;true&quot; &quot;false&quot; | Een booleaanse vlag die erop wijst of Op huis-Gebaseerde Authentificatie voor een bepaalde integratie wordt toegelaten |
| allowMirroring | Boolean | &quot;true&quot; &quot;false&quot; | Geeft aan of schermspiegeling is toegestaan voor dit apparaat |

>[!NOTE]
>
> **Nota:** als de gegevensparameter wordt gecodeerd, zoals gewoonlijk het geval voor de **zip sleutel** is, dan zal de vertegenwoordiging van de meta-gegevenssleutel een Koord JSON in plaats van een Serie of een Voorwerp zijn.


**Belangrijk:** de daadwerkelijke Meta-gegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD beschikbaar maakt.  Er moeten juridische overeenkomsten worden gesloten met MVPD&#39;s voordat gevoelige gebruikersmetagegevens (zoals zipcode) beschikbaar worden gesteld in de productieomgeving.

</br>


| Naam | Details | Vereist codering | Opmerkingen |
| --- | --- | --- | --- |
| Gebruikersnaam | Zoals verstrekt door het MVPD | Nee | Dit is de waarde die dan door Adobe wordt gehakt en in het media teken en in sendTrackingData () callback wordt blootgesteld.<br><br> het hashing in dit geval werd gedaan om historische redenen <br><br> Deze identiteitskaart kon huisidentiteitskaart of een sub-rekening identiteitskaart zijn. Dit wordt typisch niet gespecificeerd, is het enkel identiteitskaart verbonden aan login die op het ogenblik werd gebruikt (die primaire één of subaccount kan zijn) |
| Upstream-gebruikersnaam | Door het MVPD verstrekt om alleen voor Gelijktijdige monitoringstromen te worden gebruikt | Nee | Deze waarde wordt gebruikt bij het afdwingen van equivalentielimieten voor MVPD- en Programmer-sites en -apps. <br><br> identiteitskaart kan gelijktijdig controlebeleid bevatten evenals <br><br> voor de meeste MVPDs is deze waarde gelijk aan identiteitskaart |
| Gebruikersnaam voor huishouden | Door het MVPD verstrekt voor de meeste ouderlijke controlestromen | Nee | ID waarmee programmeurs inzicht kunnen krijgen in het gebruik van huishoudens in plaats van subaccounts.<br><br> het wordt soms gebruikt als Ouderlijke vervanger van Controles als ware classificaties niet beschikbaar zijn (als de gebruiker met de huisrekening het programma werd geopend kunnen zij kijken, anders verklaarde inhoud niet worden getoond) <br><br> Er is veel variatie over MVPDs voor hoe dit wordt vertegenwoordigd - huisgebruiker - identiteitskaart, hoofd van huisidentiteitskaart, hoofd van huisvlag enz. |
| Hoofd van de huishoudens | Markering die aangeeft of de lopende rekening al dan niet het hoofd van een huishouden is | Nee | zie hierboven |
| Type-id / Primaire id | ID&#39;s van huishoudelijke rekeningen | Nee | AT&amp;T-specifieke indicatoren voor het hoofd van het huishouden.<br><br> identiteitskaart van het Type = Vlag die identificeert als de gebruikersrekening primaire/secundaire rekening <br><br> Primaire OID = Huishoudelijke herkenningsteken is. Als TypeID Primair is, bevat deze de waarde van de gebruikersnaam |
| Max. waardering | De maximaal toegestane score voor de huidige account | Nee | Hiermee kunnen programmeurs inhoud filteren die niet geschikt is voor account.<br><br> heeft ratings MPAA of VCHIP |
| Kanaal omhoog | Lijst met beschikbare kanalen in het gebruikerspakket | Nee | Gebruikt om diverse kanalen van portals snel toe te staan/te verwijderen die veelvoudige netwerken </br></br> bijeenvoegen *gelieve nota te nemen dat de Preflight vergunning over het algemeen meer flexibiliteit voor dit geval toestaat en in plaats daarvan* <br><br> zou moeten worden gebruikt de specificatie OLCA staat voor dit als AttributeStatement in de reactie AuthN toe |
| HBA-status | Geeft aan of verificatie via HBA heeft plaatsgevonden | Nee |     |
| Postcode | Postcode facturering gebruiker | Ja | Gebruikt voor het Uitzenden of het Sporteren van gebeurtenissen <br><br> kan ook met de Reactie AuthZ voor snelle updates <br><br> Gevoelige gegevens worden verstrekt, behoeften juridische overeenkomsten MVPD |
| Gecodeerde postcode | Postcode facturering gebruiker (Comcast) | Ja | Als hierboven maar versleuteld door Comcast |
| Taal | Taalinstellingen gebruiker | Nee | Wordt gebruikt om berichten weer te geven volgens de voorkeuren van de gebruiker |
| Spiegelen toestaan | Geeft aan of schermspiegeling is toegestaan voor dit apparaat | Nee |     |



## Beschikbare metagegevens {#available_metadata}

In de volgende tabel wordt de huidige status van metagegevens van gebruikers in het Adobe Pass-verificatiesysteem beschreven:


|     | **<br><br>** Juridische Overeenkomst 1} **<br><br>** Ondertekend (zip slechts) **** | **identiteitskaart van de Gebruiker **<br><br>**op AuthN** | **Zipcode **<br><br>**op AuthN/Z** | **Rating **<br><br>**op AuthN/Z** | **Huishoudelijke **<br><br>**identiteitskaart op AuthN/Z** | **identiteitskaart van het Kanaal op AuthN** | **Hoofd van Huishouden op AuthN** | **identiteitskaart van het Type op AuthN** | **Primaire OID op AuthN** | Taal | Upstream UserID **op AuthN** | HBA-status | OnNet | inHome | Spiegelen op AuthZ toestaan | **Nota&#39;s** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Formele Naam** | nvt | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | taal | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Voor AuthN - OiosamlMetadataParser moet worden veranderd zodat alle parsers dit nieuwe toegelaten attribuut <br>  hebben.  Voor AuthZ - Er is geen generische manier, omdat de vergunningsimplementatie MVPD-specifiek is |
| **Vereiste Encryptie** | nvt | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| **Gevoelig** | nvt | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |     |     |
| **Adobe IdP** | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja** | **ja** | **ja** | **ja** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Er is geen juridische overeenkomst nodig. Deze kan worden ingeschakeld. |
| **Synacor** | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Juridische overeenkomst die niet alle proxy MVPDs behandelt.**   <br> dit is generische steun voor Synacor - misschien niet opgebouwd tot al hun MVPDs. |
| Dish | **Nr** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Deelt de zelfde lijst zoals alle Synacor MVPDs, plus upstreamUserID. |
| Comcast | **Nr** | **ja** | **Nr** | **ja (slechts AuthZ)** | **ja (slechts AuthZ)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** |     |
| **AT&amp;T** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **ja** | **ja** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| **Cablevision** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| **HTC** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| **Massilon van de Volmacht** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| **Uitklapt van de Volmacht** | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthZ)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| Rogers | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| RCN | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| Handvest | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| Verizon | **Nr** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** |     |
| Eastlink | **Nr** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| Proxy GLDS | **Nr** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| DTV | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| COX | **Nr** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| Cogeco | **Nr** | **ja** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |     |
| Videotron | **Nr** | **ja** | **ja (slechts AuthN)** | **Nr** | **ja*** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Hiermee wordt de huishoudelijkeID beschikbaar gemaakt met dezelfde waarde als de gebruikerID |
| Spectrum | **ja** | **ja** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **ja** |     |
| **Alle Andere **<br><br>**MVPDs** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Geen wettelijke overeenkomst nog, gevoelige meta-gegevens niet beschikbaar voor Productie.** <br> voor alle MVPDs, is de identiteitskaart van de Gebruiker beschikbaar zonder extra werk. |


De lijst met gebruikermetagegevenstypen wordt uitgebreid wanneer nieuwe typen beschikbaar worden gemaakt en worden toegevoegd aan het Adobe Pass-verificatiesysteem.

## Codevoorbeelden {#code_samples}

- [Codevoorbeeld 1](#code_sample1)
- [Codevoorbeeld 2 (Mock getMetadata)](#code_sample2)


### Codevoorbeeld 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Codevoorbeeld 2 (Mock getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
