---
title: Standaardmetagegevenskenmerken
description: Standaardmetagegevenskenmerken
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Standaardmetagegevenskenmerken {#std-metadata-attributes}

Deze pagina is bedoeld om een limitatieve lijst te verstrekken van metagegevenskenmerken die de dienst van de Controle van de Valuta kan verwerken en die als basis voor beleid kunnen worden gebruikt dat kan worden uitgevoerd. De standaardkenmerken voor metagegevens kunnen als volgt worden gecategoriseerd:

* Attributen inbegrepen door ontwerp (verzonden op elke vraag van de zittingsinitialisatie, aangezien zij in de weg URL worden vereist). Zonder deze waarden kan geen geldige aanroep worden uitgevoerd.
* Attributen van metagegevens: waarden die als formuliergegevens moeten worden doorgegeven tijdens de initialisatieaanroep van de sessie (als de waarden van het back-endbeleid vereist zijn).

## Kenmerken die door het ontwerp worden vereist {#attr-req-by-design}

De Gelijktijdige Controle API dwingt cliënten om de volgende waarden als deel van om het even welke geldige initialisatievraag te verzenden: [ vraag van de zittingsinitiatie ](/help/concurrency-monitoring/technical/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Veldnaam | Voorbeeldwaarde | Waar wordt het gebruikt? | Verkregen van |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Koptekst van autorisatie | Zendesk-ticket bij integratie |
| mvpdName | Voorbeeld_MVPD | URI-pad | Adobe Pass-verificatie van configuratiepunt wanneer de gebruiker de MVPD selecteert |
| accountId | 12345 | URI-pad | De Authentificatie upstreamUserID meta-gegevens van de Gebruiker na gebruikerslogin [ Meta-gegevens upstreamUserID - de Authentificatie van Adobe Pass ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Kenmerken van metagegevens {#metadata-attr}

De velden in de onderstaande tabel kunnen door programmeurs en MVPD&#39;s worden gebruikt om beleid te maken dat in Gelijktijdige bewaking zal worden geïmplementeerd.

Met [ API v2.0 ](https://streams-stage.adobeprimetime.com/swagger-ui/index.html), als om het even welk van deze attributen door het bepaalde beleid wordt vereist, zal een zittingspoging zonder dat attribuut in een 400 Onjuiste Verzoek resulteren.


| Entiteit | Kenmerknaam | Gegevenstype | Beschrijving | Externe referentie (ex: EIDR, OATC) | Voorbeeldwaarde | Validatieregels |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Media Company | programmerName | string | De naam van de programmeur |                                                   | ProgrammerX |                                                                                   |
| Bron | kanaal | string | Het tv-kanaal |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | De &quot;vriendelijke&quot; of door de consument leesbare titel die voor deze inhoud moet worden aangeboden | [ EIDR 2.0 de Verwijzing van de Gebieden van Gegevens ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | opsomming | Een waarde die het algemene type van inhoud beschrijft die door TveItem wordt vertegenwoordigd. De opgesomde waarden zijn: movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent trailer | [ OATC Meta-gegevens voer Aanbevolen Praktijk ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Het veld moet overeenkomen met een van de items in de opsomming |
|                 | contentType | string | In dit veld wordt bepaald of de gevraagde inhoud live of VOD is | NVT | live, vod | live of vod |
|                 | genre | string | Het genre van de inhoud die wordt gestreamd. Beschrijft het algemene programmeringstype | [ Aanbevolen Praktijk van de Meta-gegevens van 0} OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | Comedy | Geldig type genre |
|                 | duur | getal | De duur van het media-item in seconden | [ OATC Meta-gegevens voer Aanbevolen Praktijk ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | nummerreeks |
| Apparaat/browser | deviceId | string | De unieke apparaat-id. | [ Eigenschappen van de Atlas van het Apparaat ](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | De vriendelijke naam van dit apparaat. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | string | De marketingnaam (of de klantvriendelijke naam) voor een apparaat | [ Eigenschappen van de Atlas van het Apparaat ](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | geldige marketingnaam |
|                 | mobileDevice | boolean | True (waar) als het apparaat bestemd is voor gebruik onderweg | [ Eigenschappen van de Atlas van het Apparaat ](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | string | De modelnaam van het apparaat, de browser of een andere component | [ Eigenschappen van de Atlas van het Apparaat ](https://deviceatlas.com/device-data/properties){target=_blank} | tablet, telefoon, xbox. set-top box | geldige modelnaam van apparaat |
|                 | osName | string | Het besturingssysteem waarop het apparaat wordt uitgevoerd | [ Atlas van het Apparaat - OS vooraf bepaalde bezitswaarden ](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Other Note: U moet met een gebruikersnaam en wachtwoord in de Atlas van het Apparaat worden aangemeld om de bezitswaarden te bekijken | de verwachte waarde is een van de waarden in de vooraf gedefinieerde eigenschappen van de apparaatatlas |
|                 | browserName | string | De naam of het type browser op het apparaat | [ Atlas van het Apparaat - Browser vooraf bepaalde bezitswaarden ](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | De naam of het type van browser op het apparaat.  Nota: U moet met een gebruikersbenaming en een wachtwoord in de Atlas van het Apparaat worden aangemeld om de bezitswaarden te bekijken | de verwachte waarde is een van de waarden in de vooraf gedefinieerde eigenschappen van de apparaatatlas |
|                 | browserVersion | string | De browserversie op het apparaat | [ Eigenschappen van de Atlas van het Apparaat ](https://deviceatlas.com/device-data/properties){target=_blank} | De browserversie op het apparaat |                                                                                   |
| Toepassing | applicationName | string | De gebruiksvriendelijke of door de consument leesbare naam van de toepassing | NVT | Sample_Application |                                                                                   |
|                 | applicationId | string | De toepassings-id die een clienttoepassing op unieke wijze identificeert. | NVT | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | Het native platform van de toepassing | NVT | ios, android |                                                                                   |
|                 | applicationVersion | string | Deze waarde kan voor analysedoeleinden worden gebruikt | NVT | 1.0, 2.0 |                                                                                   |
| Onderwerp | accountId | string | Rekeningnummer van het onderwerp Concurrency Monitoring (binnen het toepassingsgebied van de MVPD) | NVT | testaccount |                                                                                   |
|                 | contractType | string | premie, basis. De klanten zijn vrij om dit als douanemetagegevens toe te voegen en het binnen hun eigen gebieden te gebruiken | NVT | premie, basis |                                                                                   |
| Gebruiker | name | string | Sommige MVPDs verstrekken informatie met betrekking tot de specifieke gebruiker die inhoud speelt. | NVT |                                                                                                                                                         |                                                                                   |
|                 | hba | boolean | Geeft aan of de gebruiker de stream probeert te starten vanaf zijn thuislocatie | NVT | true, false | true of false |
| Locatie | continent | string | Het continent waar de deviceID die het afspeelverzoek verzendt, afkomstig is van | NVT | Noord-Amerika | geldige continent-naam |
|                 | land | string | Het land waar de deviceID die het afspeelverzoek verzendt, afkomstig is van | NVT | VS | geldige landnaam |
|                 | state | string | De status waar de deviceID die de afspeelaanvraag verzendt, afkomstig is van | NVT | CA | geldige statusnaam |
|                 | stad | string | De plaats waar de deviceID die het afspeelverzoek verzendt, afkomstig is van | NVT | Cupertino | geldige plaatsnaam |
|                 | postcode | getal | De postcode waar de deviceID die de afspeelaanvraag verzendt, afkomstig is van | NVT | 95014 | geldige postcode |
| Streamen | streamId | string | Gegenereerd door de CM-service, niet onder controle van de klant. Wordt impliciet gebruikt wanneer regels van het type maxstreams worden bepaald. | NVT | NVT | NVT |
|                 | streamCDN | string | Hiermee wordt de CDN aangegeven waaruit de stream is opgehaald | NVT | NVT | NVT |

## Voorbeelden van het gebruik van metagegevenskenmerken voor het maken van beleid {#examples-metadata-attr}

De standaardvelden voor metagegevens kunnen worden gebruikt voor het definiëren van serverbeleid op basis van hun veldwaarden:

* U kunt een beleid zodanig configureren dat het alleen van toepassing is op specifieke veldwaarden (bijvoorbeeld een specifiek iOS-beleid: waarbij `osType` is `iOS` )
* U kunt het aantal afzonderlijke waarden voor een bepaald veld beperken. Hier volgen enkele voorbeelden:
   * niet meer dan X verschillende apparaten: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * niet meer dan X duidelijke postcodes: `HAVING DISTINCT COUNT(zipcode) <= 3`
* U kunt het aantal actieve stromen per gebiedswaarde beperken. Hier volgen enkele voorbeelden:
   * niet meer dan X actieve streams voor één apparaattype: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * niet meer dan X actieve streams voor streams met live inhoud: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Contacteer het team van de Controle Concurrency door [ het creëren van een kaartje in Zendesk ](mailto:tve-support@adobe.com) en wijs op welk beleid u wilt uitgevoerd hebben.

Hier volgen meer voorbeelden van beleid en integratiecookbooks:

* [Beleidsbeslissingspunt](/help/concurrency-monitoring/technical/cm-policy-decision-point.md)
* [ API Console - de Gelijktijdige Controle van Adobe ](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
