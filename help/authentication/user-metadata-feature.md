---
title: Metagegevens gebruiker
description: Metagegevens gebruiker
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# Metagegevens gebruiker {#user-metadata}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>
</br>

## Inleiding {#intro}

De eigenschap van Meta-gegevens van de Gebruiker laat programmeurs toe om tot verschillende types van gebruiker-specifieke gegevens toegang te hebben die door MVPDs worden gehandhaafd.  De meta-gegevenstypes van de gebruiker omvatten postcodes, ouderlijke classificaties, gebruiker - IDs, en meer.  *Gebruiker* metagegevens vormen een uitbreiding van de eerder beschikbare *static* meta-gegevens (het teken van de Authentificatie TTL, het teken van de Vergunning TTL, en identiteitskaart van het Apparaat).


Belangrijke punten van metagegevens van gebruiker:

- Voldoet aan de aanvraag van de programmeur tijdens de verificatie- en machtigingsstromen
- Waarden worden opgeslagen in de token
- De waarden kunnen worden genormaliseerd als de verschillende MVPDs gegevens in verschillende formaten verstrekken
- Sommige parameters kunnen worden gecodeerd met de sleutel van de programmeur (bijvoorbeeld postcode)
- Specifieke waarden worden door de Adobe beschikbaar gesteld via een configuratiewijziging

## Gebruikersmetagegevens verkrijgen {#obtaining}

De meta-gegevens van de gebruiker zijn beschikbaar aan Programmeurs via AccessEnabler `getMetadata()` en via de `/usermetadata` in de Clientless API.  Raadpleeg de API-documentatie van het platform voor meer informatie over het gebruik `getMetadata()` en de bijbehorende callback `setMetadataStatus()` (of voor de eindpunten en parameters die in Clientless API worden gebruikt).

Programmeurs krijgen meta-gegevens door een sleutel voor het type meta-gegevens te verstrekken zij willen verkrijgen: `getMetadata(key)`.

De metagegevens worden als volgt geretourneerd: `setMetadataStatus(key, encrypted, data)`:

| Parameter | Type | Beschrijving |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | String | Geeft het type gevraagde metagegevens aan |
| `encrypted` | Boolean | Een Booleaanse markering die aangeeft of de &quot;waarde&quot; al dan niet gecodeerd is. Als dit &quot;waar&quot;is dan zal de &quot;waarde&quot;eigenlijk een Gecodeerde vertegenwoordiging van het Web JSON van de daadwerkelijke waarde zijn. |
| `data` | Object | Een JSON-object dat de weergave van de metagegevens bevat |



De structuur van de gegevensparameter, de waarden variëren tussen types:

| Sleutel | Type waarde | Monster | Beschrijving |
| --- | --- | --- | --- |
| `zip` | JSON-array | \[&quot;77754&quot;, &quot;12345&quot;\] | Postcode |
| `householdID` | JSON-tekenreeks | &quot;1o7241p&quot; | Huishoudelijke identificatiecode. Als MVPD geen subaccounts ondersteunt, is dit hetzelfde als `userID` |
| `maxRating` | JSON-object | {MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;,  <br>  URL: &quot;http://manage.my/parental&quot; } | Maximale ouderlijke classificatie voor de gebruiker |
| `userID` | JSON-tekenreeks | &quot;1o7241p&quot; | De gebruikers-id. In het geval waarin een MVPD subaccounts ondersteunt en de gebruiker niet de hoofdaccount is, `userID` verschilt van `householdID`. |
| `channelID` | JSON-array | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Een lijst met kanalen die een gebruiker mag bekijken |
| `is_hoh` | JSON-tekenreeks | &quot;1&quot; | Een markering die aangeeft of een gebruiker het hoofd van een huishouden is |
| `encryptedZip` | JSON-tekenreeks | &quot;&quot; | Met Comcast wordt de postcode gecodeerd |
| `typeID` | JSON-tekenreeks | Primair | Een vlag die identificeert als de gebruikersrekening primaire/secundaire rekening is |
| `primaryOID` | JSON-tekenreeks | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Huishoudelijke identificatiecode. Indien `typeID` is Primair, bevat de waarde van `userID` |
| hba_status | Boolean | &quot;true&quot; &quot;false&quot; | Een booleaanse vlag die erop wijst of Op huis-Gebaseerde Authentificatie voor een bepaalde integratie wordt toegelaten |
| allowMirroring | Boolean | &quot;true&quot; &quot;false&quot; | Geeft aan of schermspiegeling is toegestaan voor dit apparaat |

>[!NOTE]
>
> **Opmerking:** Als de gegevensparameter gecodeerd is, zoals gewoonlijk het geval is voor de **zip-toets** De representatie van de metagegevenssleutel is dan een JSON-tekenreeks in plaats van een Array of Object.


**Belangrijk:** De daadwerkelijke Metagegevens van de Gebruiker beschikbaar aan een Programmer hangt van af wat MVPD ter beschikking stelt.  Er moeten juridische overeenkomsten worden gesloten met MVPD&#39;s voordat gevoelige gebruikersmetagegevens (zoals zipcode) beschikbaar worden gesteld in de productieomgeving.

</br>


| Naam | Details | Vereist codering | Opmerkingen |
| --- | --- | --- | --- |
| Gebruikersnaam | Zoals verstrekt door het MVPD | Nee | Dit is de waarde die dan door Adobe wordt gehakt en in het media teken en in sendTrackingData () callback wordt blootgesteld.<br><br>De hashing in deze zaak werd om historische redenen uitgevoerd<br><br>Deze id kan een ID voor een huishouden of een subaccount zijn. Dit wordt typisch niet gespecificeerd, is het enkel identiteitskaart verbonden aan login die op het ogenblik werd gebruikt (die primaire één of subaccount kan zijn) |
| Upstream-gebruikersnaam | Door het MVPD verstrekt om alleen voor Gelijktijdige monitoringstromen te worden gebruikt | Nee | Deze waarde wordt gebruikt bij het afdwingen van equivalentielimieten voor MVPD- en Programmer-sites en -apps. <br><br>De id kan ook beleid voor gelijktijdige bewaking bevatten<br><br>Voor de meeste MVPD&#39;s is deze waarde gelijk aan de gebruikersnaam |
| Gebruikersnaam voor huishouden | Door het MVPD verstrekt voor de meeste ouderlijke controlestromen | Nee | ID waarmee programmeurs inzicht kunnen krijgen in het gebruik van huishoudens in plaats van subaccounts.<br><br>Het wordt soms gebruikt als Ouderlijke vervanger van Controles als de ware classificaties niet beschikbaar zijn (Als de gebruiker met de huisrekening werd het programma geopend kunnen zij letten op, anders verklaarde inhoud niet zou worden getoond)<br><br>Er is veel variatie tussen de MVPD&#39;s voor de manier waarop dit wordt vertegenwoordigd: huishoudelijke gebruikers-id, hoofd-id van het huishouden, hoofd van de huishoudelijke vlag enz. |
| Hoofd van de huishoudens | Markering die aangeeft of de lopende rekening al dan niet het hoofd van een huishouden is | Nee | zie hierboven |
| Type-id / Primaire id | ID&#39;s van huishoudelijke rekeningen | Nee | AT&amp;T-specifieke indicatoren voor het hoofd van het huishouden.<br><br>Type ID = Markering die aangeeft of de gebruikersaccount een primaire/secundaire account is<br><br>Primaire OID = Huishoudelijke identificatiecode. Als TypeID Primair is, bevat deze de waarde van de gebruikersnaam |
| Max. waardering | De maximaal toegestane score voor de huidige account | Nee | Hiermee kunnen programmeurs inhoud filteren die niet geschikt is voor account.<br><br>Heeft MPAA- of VCHIP-ratings |
| Kanaal omhoog | Lijst met beschikbare kanalen in het gebruikerspakket | Nee | Gebruikt om diverse kanalen van portalen snel toe te staan/te verwijderen die veelvoudige netwerken bijeenvoegen</br></br> *Houd er rekening mee dat Preflight-autorisatie over het algemeen flexibeler is voor deze toepassing en in plaats daarvan moet worden gebruikt* <br><br>De specificatie OLCA staat voor dit als AttributeStatement in de reactie van AuthN toe |
| HBA-status | Geeft aan of verificatie via HBA heeft plaatsgevonden | Nee |     |
| Postcode | Postcode facturering gebruiker | Ja | Wordt gebruikt voor uitzendings- of tekengebeurtenissen<br><br>Kan ook de AuthZ-reactie voor snelle updates ontvangen<br><br>Gevoelige gegevens, vereist juridische overeenkomsten van de MVPD |
| Gecodeerde postcode | Postcode facturering gebruiker (Comcast) | Ja | Als hierboven maar versleuteld door Comcast |
| Taal | Taalinstellingen gebruiker | Nee | Wordt gebruikt om berichten weer te geven volgens de voorkeuren van de gebruiker |
| Spiegelen toestaan | Geeft aan of schermspiegeling is toegestaan voor dit apparaat | Nee |     |



## Beschikbare metagegevens {#available_metadata}

In de volgende tabel wordt de huidige status van metagegevens van gebruikers in het Adobe Pass-verificatiesysteem beschreven:


|     | **Juridisch **<br><br>**Overeenkomst **<br><br>**Ondertekend (alleen ZIP)** | **Gebruikersnaam **<br><br>**op AuthN** | **Postcode **<br><br>**op AuthN/Z** | **Classificatie **<br><br>**op AuthN/Z** | **Huishoudelijk **<br><br>**ID op AuthN/Z** | **Kanaal-id op AuthN** | **Hoofd van de huishoudens op AuthN** | **Type-id op AuthN** | **Primaire OID op AuthN** | Taal | Upstream gebruiker-id **op AuthN** | HBA-status | OnNet | inHome | Spiegelen op AuthZ toestaan | **Notities** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Formele naam** | nvt | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | taal | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Voor AuthN - OiosamlMetadataParser moet worden veranderd zodat alle parsers dit nieuwe toegelaten attribuut hebben <br>2.  Voor AuthZ - Er is geen generische manier, omdat de vergunningsimplementatie MVPD-specifiek is |
| **Versleuteling vereist** | nvt | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| **Gevoelig** | nvt | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** |     |     |
| **Adobe-idP** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja** | **Ja** | **Ja** | **Ja** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Er is geen juridische overeenkomst nodig. Deze kan worden ingeschakeld. |
| **Synacor** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja** | **Ja** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Juridische overeenkomst die niet alle proxy-VPD&#39;s bestrijkt.**   <br>Dit is algemene steun voor Synacor - misschien niet opgebouwd tot al hun MVPDs. |
| Dish | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Deelt de zelfde lijst zoals alle Synacor MVPDs, plus upstreamUserID. |
| Comcast | **Nee** | **Ja** | **Nee** | **Ja (alleen AuthZ)** | **Ja (alleen AuthZ)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Ja** | **Nee** | **Nee** | **Nee** |     |
| **AT&amp;T** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Ja** | **Ja** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Juridische overeenkomst ondertekend. |
| **Cablevision** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Juridische overeenkomst ondertekend. |
| **HTC** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| **Proxy Massilon** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Juridische overeenkomst ondertekend. |
| **Proxywissing** | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthZ)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Juridische overeenkomst ondertekend. |
| Rogers | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| RCN | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| Handvest | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| Verizon | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Ja** | **Nee** | **Nee** | **Nee** |     |
| Eastlink | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| Proxy GLDS | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| DTV | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| COX | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| Cogeco | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** |     |
| Videotron | **Nee** | **Ja** | **Ja (alleen AuthN)** | **Nee** | **Ja*** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | Hiermee wordt de huishoudelijkeID beschikbaar gemaakt met dezelfde waarde als de gebruikerID |
| Spectrum | **Ja** | **Ja** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Ja (alleen AuthN)** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Ja** | **Nee** | **Nee** | **Ja** |     |
| **Alle overige **<br><br>**MVPD&#39;s** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Nee** | **Ja** | **Nee** | **Nee** | **Nee** | **Nee** | **Er is nog geen juridische overeenkomst, gevoelige metagegevens zijn niet beschikbaar voor Production.**  <br>Voor alle MVPD&#39;s is de gebruikersnaam beschikbaar zonder extra werk. |


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
