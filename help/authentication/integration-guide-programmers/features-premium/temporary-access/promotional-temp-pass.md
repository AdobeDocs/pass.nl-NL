---
title: Tijdelijke controle voor speciale acties
description: Tijdelijke controle voor speciale acties
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
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

De Pass van de Bevorderende Temperatuur wordt ontworpen om voor het runnen van promotiecampagnes te worden gebruikt waar een gebruiker, na het verstrekken van geldige identiteitsinformatie (bijvoorbeeld, e-mailadres) aan de Programmer, a **vooraf bepaald aantal verschillende titels van VOD voor een vooraf bepaalde periode** zal kunnen verbruiken.

>[!IMPORTANT]
>
>Adobe slaat geen Persoonlijk Identificeerbare Informatie (PII) op. Daarom moet de programmeur een knoeiboel over de unieke gebruiker plaatsen verstrekte informatie over de Authentificatie APIs van Adobe Pass.

De Bevorderende Controle van het Temperatuur wordt voortgebouwd bovenop de ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) eigenschap van de Voldoende Controle 0} van het Temperatuur, betekenend omvat het alle functionaliteit van de Voldoende Controle van het Temperatuur.[

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

**de Bevorderende eigenschap van de Controle van de Temperatuur kan op de volgende eigenschappen worden gevormd:**

* Door de gebruiker opgegeven informatietoets (bijvoorbeeld e-mail)
* Aantal bronnen dat de gebruiker mag gebruiken
* TTL - het tijdinterval waarin de gebruiker het gevormde aantal middelen mag verbruiken

### Metagegevens gebruiker {#user-metadata}

Om de implementatie van de toepassing van de Programmer te vergemakkelijken, wordt de volgende **informatie van gebruikersmeta-gegevens blootgesteld** op de Volgorde van de Temperatuur van de Bevordering, met overeenkomstige sleutels (om de sleutels te activeren, contact tve-support@adobe.com):

* **remaining_resources**: het aantal resterende middelen die de huidige gebruiker gerechtigd is te verbruiken
* **used_assets**: de lijst van middelen die de huidige gebruiker reeds heeft verbruikt
* **expiration_date**: de vervaldatum van de huidige gebruiker

### Hoe wordt de weergavetijd berekend? {#compute-viewing-time}

De tijd dat een tijdelijke controle geldig blijft, heeft geen betrekking op de hoeveelheid tijd die een gebruiker besteedt aan het weergeven van inhoud in de toepassing van de programmeur. Op het aanvankelijke gebruikersverzoek om vergunning via de Volgorde van de Temperatuur van de Bevordering, wordt een vervaltijd berekend door de aanvankelijke huidige verzoektijd aan TTL (duurtijdinterval) toe te voegen die door de Programmer wordt gespecificeerd.

### Verificatie en autorisatie {#authn-authz}

Voor de stromen van de Controle van de Temperatuur van de Bevordering, authentificatie en de vergunning communiceren niet met daadwerkelijke MVPD, **zodat zullen alle vergunningsverzoeken** slagen zolang aan al deze voorwaarden wordt voldaan:

* verificatietokens zijn geldig voor opgegeven bronnen
* aantal **used_assets** is lager dan de grens die door de Programmer wordt geplaatst
* **expiration_date** waarde is na huidige datum.

### Preflight-gedrag {#preflight-beh}

Wanneer een Preflight- of pre-autorisatieverzoek wordt ingediend voor een MVPD voor een tijdelijke controle van promoties, bevat de corresponderende geretourneerde Preflight-reactie de volledige lijst met bronnen van de Preflight-aanvraag als Preflight geslaagd.

De logica achter dit amendement is dat de toelatingsvoorwaarden voor een tijdelijke controle van promoties gebaseerd zijn op een beperking van tijd en hulpbronnenaantal in plaats van op specifieke middelen. Meer specifiek, zolang de tijdbeperking wordt voldaan en zolang de middelgrens niet wordt overschreden, zullen de geroepen middelen worden toegelaten.

### SSO {#sso}

SSO wordt niet toegelaten voor instanties van de Volwassening van de Temperatuur van de Bevordering, die met &quot;Authentificatie per Aanvrager&quot;wordt gevormd toegelaten. Dit betekent dat wanneer de gebruiker van toepassing A aan een andere toepassing B overschakelt die allebei met de zelfde Volgorde van de BevorderingsTemperatuur geïntegreerd zijn, de gebruiker niet automatisch zal worden aangemeld.

### Afmelden {#logout}

Alle tokens op een apparaat worden bij het afmelden verwijderd. Daarom zou het overschakelen van de Bevorderingstemperatuur Pass aan een regelmatige gebruiker-geselecteerde MVPD niet op deze implementatie moeten baseren. De aanbeveling is om de `setSelectedProvider(null)` functie te gebruiken om de toepassingsstaat te ontruimen en dan de authentificatiestroom opnieuw te beginnen, die een betere gebruikerservaring heeft.

### Stroomdiagram voor promotietemperatuur {#promo-tempass-flowdia}

![ Bevorderend het Stroomdiagram van de Pas van de Temperatuur ](../../../assets/promo-temp-pass-flow.png)

*Cijfer: De stroom van de Controle van de Tijdelijke Bevordering*

## Tijdelijke controle voor bevordering uitvoeren {#impl-promo-tempass}

Voor een tijdelijke controle van de promotietest zijn de volgende functies aan de clientzijde vereist:

* **informatie van het herkenningsteken van de Gebruiker, bijvoorbeeld propagatie van het e-mailadres** (verzendend het e-mailadres van de gebruiker op de authentificatie en de vergunningsstromen). De e-mail wordt vereist door de Authentificatie van Adobe Pass om de authentificatie en toestemmingstkens te binden (gelijkend op het geval van `device_ID`, die op alle vraag wordt vereist).
* **de authentificatie van de Kracht** - toestaand de Programmer om een authentificatiestroom te dwingen wanneer de gebruiker reeds voor authentiek verklaard is. Deze functionaliteit wordt vereist om een gebruikersmeta-gegevens te dwingen verfrissen zich (de sleutel van gebruikersmeta-gegevens **used_assets** bevat het aantal beschikbare middelen) telkens als app is begonnen. Omdat de gebruiker zich op meerdere apparaten kan aanmelden, zijn de metagegevens van de gebruiker die tijdens het opstarten van de app op het apparaat aanwezig zijn, onbetrouwbaar en moeten we deze bijwerken om de huidige status voor die specifieke gebruiker (geïdentificeerd door e-mailadres) te weerspiegelen.


>[!IMPORTANT]
>De gedwongen authentificatie is mogelijk slechts op iOS en Android.
>Adobe Pass-verificatie beschikt niet over een ingebouwd mechanisme om het gratis streamen te stoppen nadat de X minuten zijn verstreken. De Authentificatie van Adobe Pass zal ophouden uitgevend **vergunning** en **korte media** tokens zodra de gebruiker de vrije middelen van Y verbruikt. Het is aan de programmeurs om de toegang te beperken zodra de Promotional Temp Pass verloopt.

## Beveiliging {#security}

>[!IMPORTANT]
>Adobe slaat geen Persoonlijk Identificeerbare Informatie (PII) op. Daarom moet de programmeur een knoeiboel over de unieke gebruiker plaatsen verstrekte informatie over de Authentificatie APIs van Adobe Pass.

**Hashing van informatie van het gebruikersidentificatie**

De Adobe adviseert gebruikend de **SHA-2** familie of zijn specifieke **SHA-256**, **SHA-512** functies op gegevens alvorens het wordt verzonden naar Adobe.

Bijvoorbeeld, **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a3 32b18b88d09069fb7&quot;**.

## Tijdelijke controle voor speciale acties herstellen of wissen {#reset-promo-tempass}

Bepaalde bedrijfsregels vereisen een regelmatige zuivering van de Volgorde van de BevorderingsTemperatuur. Om dit te doen, voorziet de Authentificatie van Adobe Pass Programmers van a *openbare* Web API, hieronder beschreven:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocol: **https**</li><li>Host:<ul><li>Versie: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Pad: **/reset-tempass/v2/reset**</li><li>De parameters van de vraag: **device_id=all&amp;request_or_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>kopballen: ApiKey: **1232293681726481**</li> <li>Reactie:<ul><li>Succes: **HTTP 204**</li><li>Mislukking: **HTTP 400** voor onjuiste verzoeken, **HTTP 401** als ApiKey niet gespecificeerd, **HTTP 403** als ApiKey ongeldig is</li></ul></li></ul> |

Boven de vereisten om de Pas van Temperatuur te zuiveren, gebruikt de Bevorderende Controle van Temperatuur de knoeiboel over informatie van het gebruikersherkenningsteken die als **wordt verzonden generic_data** op authentificatie en vergunning voor het zuiveren.

De hash wordt verzonden, niet de hele JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Ondersteunde clients {#supported-clients}

| Adobe Pass-verificatieclients | Tijdelijke controle voor speciale acties | Gereedschap herstellen | Ondersteunt toegewezen antwoordcode/clientfout |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | JA | JA | JA (vanaf versie 3.0.0) |
| IOS voor native client | JA | JA | JA (vanaf v 1.10) |
| Android voor native client | JA | JA | JA |
| Clientloze API | JA | JA | NEE |


## Beperkingen {#limitations}

In dit gedeelte worden de beperkingen beschreven die gelden voor de huidige implementatie van de Promotional Temp Pass.

### Zonder clip {#lim-clientless}

**Slimme apparaten zonder Unieke identiteitskaart van het Apparaat**

Niet alle toepassingen voor slimme apparaten kunnen een unieke apparaat-id leveren. Bij gebrek aan één, kan de Authentificatie van Adobe Pass UUID gebruiken die door de Dienst van de Code van de Registratie van de Adobe als Unieke Apparaat ID wordt geproduceerd. Dit betekent dat als de gebruiker zich afmeldt, de verificatie- en autorisatietokens worden verwijderd. Wanneer de gebruiker opnieuw probeert te verifiëren, kan de gebruiker dit keer met andere gebruikersgegevens (bijvoorbeeld een e-mail) opnieuw autoriseren. Adobe raadt aan een UI-flow toe te voegen die een gebruiker niet toestaat het systeem voor de gek te houden en logica toe te voegen om te bepalen of het een nieuwe gebruiker betreft die een proefversie of een bestaande proefversie aanvraagt.

**terugstellend/het zuiveren de Pas van Temperatuur**

Tijdelijke controle opnieuw instellen voor client is niet beschikbaar in de specifieke gevallen van Xbox 360 en Xbox One, omdat deze platforms extra parsering van apparaat-id&#39;s vereisen, niet mogelijk in het gereedschap Temperatuur-controle opnieuw instellen.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
