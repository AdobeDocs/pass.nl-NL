---
title: Tijdelijke controle voor speciale acties
description: Tijdelijke controle voor speciale acties
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: c88fba76b1805dd88fccb68b13bde11b1b8ea181
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# Tijdelijke controle voor speciale acties {#promotional-temp-pass}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht van functies {#feature-summary}

Met de tijdelijke controle voor speciale acties kunnen programmeurs tijdelijke toegang tot hun beveiligde inhoud bieden aan gebruikers die geen accountgegevens bij een MVPD hebben.

De tijdelijke promotiecampagne is ontworpen om voor het runnen van promotiecampagnes te worden gebruikt waar een gebruiker, na het verstrekken van geldige identiteitsinformatie (bijvoorbeeld, e-mailadres) aan de programmeur, een **vooraf gedefinieerd aantal verschillende VOD-titels voor een vooraf gedefinieerde periode**.

>[!IMPORTANT]
>
>Adobe slaat geen Persoonlijk Identificeerbare Informatie (PII) op. Daarom moet de programmeur een knoeiboel over de unieke gebruiker plaatsen verstrekte informatie over de Authentificatie APIs van Adobe Pass.

De tijdelijke controle van de bevordering wordt gebouwd bovenop [Temperatuurcontrole](/help/authentication/temp-pass.md) -functie, wat betekent dat deze alle functies voor het doorstaan van temperaturen bevat.

Zodra het maximum vooraf bepaalde aantal titels VOD of de vooraf bepaalde periode worden overschreden, zal die gebruiker tot inhoud op het zelfde apparaat of door de zelfde informatie van het gebruikersherkenningsteken (bijvoorbeeld, e-mailadres) niet kunnen toegang hebben tot de toestemmingstokens worden ontruimd van de server van de Authentificatie van Adobe Pass.

>[!NOTE]
>
>Temperatuurcontrole maakt deel uit van het Premium Workflow-pakket. Neem contact op met je Adobe Pass-verkoper als je deze functie wilt gebruiken.

## Vergelijking van Temperatuur- en Promotiemaxima {#tp-ptp-comparison}

| Temperatuurcontrole | Tijdelijke controle voor speciale acties |
|----------------------------------|----------------------------------------------------------------------------------------|
| Toegang tot inhoud <ul><li>op tijd gebaseerd</li></ul> | Toegang tot inhoud <ul><li>op tijd gebaseerd</li><li>op basis van het aantal middelen</li></ul> |
| Toegangsbeveiliging gebaseerd op <ul><li>apparaat-id</li></ul> | Beveiliging gebaseerd op <ul><li>apparaat-id</li><li>hash over de opgegeven informatie over de gebruikersidentificatie (bijvoorbeeld e-mail)</li></ul> |
| Clientfout-API beschikbaar | Clientfout-API beschikbaar |
| Herstellen/leegmaken beschikbaar | Herstellen/leegmaken beschikbaar |

## Details onderdeel {#feature-details}

Met deze functie kunnen gebruikers promotionele inhoud openen vanaf een specifiek apparaat (telefoon en tablet) nadat ze unieke informatie hebben opgegeven, zoals het e-mailadres, in de toepassing van de programmeur.

De programmeur zal een knoeiboel over PII van de gebruiker op de authentificatie en vergunning APIs verstrekken. Deze knoeiboel zal samen met apparaatIdentiteitskaart in het produceren van een unieke sleutel worden gebruikt om de gebruiker en het apparaat te identificeren.

Op basis van apparaat-id en de informatie die de gebruiker heeft verstrekt en de onderstaande logica volgt, bepaalt Adobe Pass-verificatie of de gebruiker zich in een nieuwe of bestaande proefversie bevindt:

* nieuwe knoeiboel over user-provided informatie (bijvoorbeeld, e-mail), nieuwe apparaatId => nieuwe proef
* bestaande knoeiboel over user-provided informatie (bijvoorbeeld, e-mail), nieuwe apparaatId => bestaande proef (met bestaande knoeiboel over gebruiker verstrekte informatie (bijvoorbeeld, e-mail))
* nieuwe knoeiboel over user-provided informatie (bijvoorbeeld, e-mail), bestaande apparaatId => bestaande proef (met bestaande apparaatId)
* bestaande hash over door de gebruiker opgegeven informatie (bijvoorbeeld e-mail), bestaande apparaat-id => bestaande proefversie

>[!NOTE]
>De bevestiging en het hashing van de user-provided informatie wordt behandeld door Programmer, niet door Adobe.

**De functie Tijdelijke controle voor speciale acties kan worden geconfigureerd op basis van de volgende eigenschappen:**

* Door de gebruiker opgegeven informatietoets (bijvoorbeeld e-mail)
* Aantal bronnen dat de gebruiker mag gebruiken
* TTL - het tijdinterval waarin de gebruiker het gevormde aantal middelen mag verbruiken

### Metagegevens gebruiker {#user-metadata}

Om de uitvoering van de toepassing van de programmeur te vergemakkelijken: **metagegevens van de gebruiker worden weergegeven** op de Pass van de Bevorderingstemperatuur, met overeenkomstige sleutels (om de sleutels te activeren, contacteer tve-support@adobe.com):

* **remaining_resources**: het aantal resterende bronnen dat de huidige gebruiker mag gebruiken
* **used_assets**: de lijst met bronnen die de huidige gebruiker al heeft verbruikt
* **expiration_date**: de vervaldatum van de huidige gebruiker

### Hoe wordt de weergavetijd berekend? {#compute-viewing-time}

De tijd dat een tijdelijke controle geldig blijft, heeft geen betrekking op de hoeveelheid tijd die een gebruiker besteedt aan het weergeven van inhoud in de toepassing van de programmeur. Op het aanvankelijke gebruikersverzoek om vergunning via de Volgorde van de Temperatuur van de Bevordering, wordt een vervaltijd berekend door de aanvankelijke huidige verzoektijd aan TTL (duurtijdinterval) toe te voegen die door de Programmer wordt gespecificeerd.

### Verificatie en autorisatie {#authn-authz}

Voor de stromen van de Pass van de Promotie Temp, communiceren de authentificatie en de vergunning niet met een daadwerkelijke MVPD; **alle vergunningsaanvragen zullen dus slagen** zolang aan al deze voorwaarden is voldaan:

* verificatietokens zijn geldig voor opgegeven bronnen
* aantal **used_assets** lager is dan de door de programmeur vastgestelde limiet
* **expiration_date** waarde is na huidige datum.

### Preflight-gedrag {#preflight-beh}

Wanneer een Preflight- of pre-autorisatieverzoek wordt ingediend voor een MVPD voor een tijdelijke controle van promoties, bevat de corresponderende geretourneerde Preflight-reactie de volledige lijst met bronnen van de Preflight-aanvraag als Preflight geslaagd.

De logica achter dit amendement is dat de toelatingsvoorwaarden voor een tijdelijke controle van promoties gebaseerd zijn op een beperking van tijd en hulpbronnenaantal in plaats van op specifieke middelen. Meer specifiek, zolang de tijdbeperking wordt voldaan en zolang de middelgrens niet wordt overschreden, zullen de geroepen middelen worden toegelaten.

### SSO {#sso}

SSO wordt niet toegelaten voor instanties van de Volwassening van de Temperatuur van de Bevordering, die met &quot;Authentificatie per Aanvrager&quot;wordt gevormd toegelaten. Dit betekent dat wanneer de gebruiker van toepassing A aan een andere toepassing B overschakelt die allebei met de zelfde Volgorde van de BevorderingsTemperatuur geïntegreerd zijn, de gebruiker niet automatisch zal worden aangemeld.

### Afmelden {#logout}

Alle tokens op een apparaat worden bij het afmelden verwijderd. Daarom zou het overschakelen van de Bevorderingstemperatuur Pass aan een regelmatige gebruiker-geselecteerde MVPD niet op deze implementatie moeten baseren. De aanbeveling is om de `setSelectedProvider(null)` om de status van de toepassing te wissen en vervolgens de verificatiestroom opnieuw te starten, wat een betere gebruikerservaring heeft.

### Stroomdiagram voor promotietemperatuur {#promo-tempass-flowdia}

![Stroomdiagram voor promotietemperatuur](assets/promo-temp-pass-flow.png)

*Afbeelding: Tijdelijke doorstroming voor promotiedoeleinden*

## Tijdelijke controle voor bevordering uitvoeren {#impl-promo-tempass}

Voor een tijdelijke controle van de promotietest zijn de volgende functies aan de clientzijde vereist:

* **Informatie over gebruikers-id, bijvoorbeeld propagatie van e-mailadressen** (het verzenden van het e-mailadres van de gebruiker op de authentificatie en de vergunningsstromen). De e-mail wordt vereist door de Authentificatie van Adobe Pass om de authentificatie en toestemmingstkens (gelijkend op het geval van te binden `device_ID`, vereist voor alle vraag).
* **Verificatie forceren** - de programmeur toestaan om een authentificatiestroom te dwingen wanneer de gebruiker reeds voor authentiek wordt verklaard. Deze functionaliteit is vereist om de metagegevens van de gebruiker te vernieuwen (de metagegevens van de gebruiker) **used_assets** bevat het aantal beschikbare bronnen) telkens wanneer de app wordt gestart. Omdat de gebruiker zich op meerdere apparaten kan aanmelden, zijn de metagegevens van de gebruiker die tijdens het opstarten van de app op het apparaat aanwezig zijn, onbetrouwbaar en moeten we deze bijwerken om de huidige status voor die specifieke gebruiker (geïdentificeerd door e-mailadres) te weerspiegelen.


>[!IMPORTANT]
>Verificatie via forceren is alleen mogelijk op iOS en Android.
>Adobe Pass-verificatie beschikt niet over een ingebouwd mechanisme om het gratis streamen te stoppen nadat de X minuten zijn verstreken. De Adobe Pass-verificatie houdt op met uitgeven **autorisatie** en **korte media** tokens zodra de gebruiker de vrije middelen van Y verbruikt. Het is aan de programmeurs om de toegang te beperken zodra de Promotional Temp Pass verloopt.

## Beveiliging {#security}

>[!IMPORTANT]
>Adobe slaat geen Persoonlijk Identificeerbare Informatie (PII) op. Daarom moet de programmeur een knoeiboel over de unieke gebruiker plaatsen verstrekte informatie over de Authentificatie APIs van Adobe Pass.

**Onderbreking van de informatie van de gebruikersidentificatie**

Adobe raadt u aan de **SHA-2** familie of specifieke **SHA-256**, **SHA-512** functies op gegevens voordat deze naar de Adobe worden verzonden.

Bijvoorbeeld: **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## Tijdelijke controle voor speciale acties herstellen of wissen {#reset-promo-tempass}

Bepaalde bedrijfsregels vereisen een regelmatige zuivering van de Volgorde van de BevorderingsTemperatuur. Om dit te doen, voorziet de Authentificatie van Adobe Pass Programmeurs van een *publiek* web-API, zoals hieronder beschreven:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocol: **https**</li><li>Host:<ul><li>Geen: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Pad: **/reset-tempass/v2/reset**</li><li>Parameters query: **device_id=all&amp;request_or_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>kopteksten: ApiKey: **1232293681726481**</li> <li>Reactie:<ul><li>Geslaagd: **HTTP 204**</li><li>Fout: **HTTP 400** voor onjuiste verzoeken, **HTTP 401** als ApiKey niet is opgegeven, **HTTP 403** als ApiKey ongeldig is</li></ul></li></ul> |

Naast de vereisten voor het leegmaken van de Temperatuur-controle wordt in de controle voor promotietemperatuur de hash gebruikt voor de identificatiegegevens van de gebruiker die worden verzonden als **generic_data** inzake de echtheidscontrole en de zuiveringsvergunning.

De hash wordt verzonden, niet de hele JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Ondersteunde clients {#supported-clients}

| Adobe Pass-verificatieclients | Tijdelijke controle voor speciale acties | Gereedschap herstellen | Ondersteunt toegewezen antwoordcode/clientfout |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | JA | JA | JA (vanaf versie 3.0.0) |
| IOS voor native client | JA | JA | JA (vanaf v 1.10) |
| Systeemeigen clientAndroid | JA | JA | JA |
| Clientloze API | JA | JA | NEE |


## Beperkingen {#limitations}

In dit gedeelte worden de beperkingen beschreven die gelden voor de huidige implementatie van de Promotional Temp Pass.

### Zonder clip {#lim-clientless}

**Slimme apparaten zonder unieke apparaat-id**

Niet alle toepassingen voor slimme apparaten kunnen een unieke apparaat-id leveren. Bij gebrek aan één, kan de Authentificatie van Adobe Pass UUID gebruiken die door de Dienst van de Code van de Registratie van de Adobe als Unieke Apparaat ID wordt geproduceerd. Dit betekent dat als de gebruiker zich afmeldt, de verificatie- en autorisatietokens worden verwijderd. Wanneer de gebruiker opnieuw probeert te verifiëren, kan de gebruiker dit keer met andere gebruikersgegevens (bijvoorbeeld een e-mail) opnieuw autoriseren. Adobe raadt aan een UI-flow toe te voegen die een gebruiker niet toestaat het systeem voor de gek te houden en logica toe te voegen om te bepalen of het een nieuwe gebruiker betreft die een proefversie of een bestaande proefversie aanvraagt.

**Temperatuurcontrole opnieuw instellen/leegmaken**

Tijdelijke controle opnieuw instellen voor client is niet beschikbaar in de specifieke gevallen van Xbox 360 en Xbox One, omdat deze platforms extra parsering van apparaat-id&#39;s vereisen, niet mogelijk in het gereedschap Temperatuur-controle opnieuw instellen.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
