---
title: Standaardmetagegevenskenmerken
description: Standaardmetagegevenskenmerken
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---


# Standaardmetagegevenskenmerken {#std-metadata-attributes}

Deze pagina is bedoeld om een limitatieve lijst te verstrekken van metagegevenskenmerken die de dienst van de Controle van de Valuta kan verwerken en die als basis voor beleid kunnen worden gebruikt dat kan worden uitgevoerd. De standaardkenmerken voor metagegevens kunnen als volgt worden gecategoriseerd:

* Attributen inbegrepen door ontwerp (verzonden op elke vraag van de zittingsinitialisatie, aangezien zij in de weg URL worden vereist). Zonder deze waarden kan geen geldige aanroep worden uitgevoerd.
* Attributen van metagegevens: waarden die als formuliergegevens moeten worden doorgegeven tijdens de initialisatieaanroep van de sessie (als de waarden van het back-endbeleid vereist zijn).

## Kenmerken die door het ontwerp worden vereist {#attr-req-by-design}

Met de API voor gelijktijdige bewaking worden clients gedwongen de volgende waarden te verzenden als onderdeel van een geldige initialisatieaanroep: [uitnodigingen voor het starten van de sessie](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Veldnaam | Voorbeeldwaarde | Waar wordt het gebruikt? | Verkregen van |
|-------------|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Koptekst van autorisatie | Zendesk-ticket bij integratie |
| mvpdName | Sample_MVPD | URI-pad | De Authentificatie van Adobe Pass van config eindpunt wanneer de gebruiker MVPD selecteert |
| accountId | 12345 | URI-pad | Metagegevens van de Adobe Pass-verificatie upstreamUser-id na gebruikersaanmelding [Gebruikersmetagegevens upstreamUser-id - Adobe Pass-verificatie](/help/authentication/user-metadata-feature.md) |


## Kenmerken van metagegevens {#metadata-attr}

De velden in de onderstaande tabel kunnen door programmeurs en MVPD&#39;s worden gebruikt om beleid te maken dat in Gelijktijdige bewaking zal worden geïmplementeerd.

Met [API v2.0](http://docs.adobeptime.io/cm-api-v2/), als om het even welk van deze attributen door het bepaalde beleid wordt vereist, zal een zittingspoging zonder dat attribuut in een 400 Onjuiste Verzoek resulteren.


| Entiteit | Kenmerknaam | Gegevenstype | Beschrijving | Externe referentie (ex: EIDR, OATC) | Voorbeeldwaarde | Validatieregels |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Media Company | programmerName | string | De naam van de programmeur |                                                   | ProgrammerX |                                                                                   |
| Bron | kanaal | string | Het tv-kanaal |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | De &quot;vriendelijke&quot; of door de consument leesbare titel die voor deze inhoud moet worden aangeboden | [EIDR 2.0 - Referentie gegevensvelden](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | opsomming | Een waarde die het algemene type van inhoud beschrijft die door TveItem wordt vertegenwoordigd. De opgesomde waarden zijn: movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent trailer | [Aanbevolen praktijk voor OATC-metagegevens](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Het veld moet overeenkomen met een van de items in de opsomming |
|                 | contentType | string | In dit veld wordt bepaald of de gevraagde inhoud live of VOD is | NVT | live, vod | live of vod |
|                 | genre | string | Het genre van de inhoud die wordt gestreamd. Beschrijft het algemene programmeringstype | [Aanbevolen OATC-metagegevensfeed](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Praktijk | Comedy | Geldig type genre |
|                 | duur | getal | De duur van het media-item in seconden | [Aanbevolen praktijk voor OATC-metagegevens](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | nummerreeks |
| Apparaat/browser | deviceId | string | De unieke apparaat-id. | [Eigenschappen van apparaatatlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | De vriendelijke naam van dit apparaat. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | string | De marketingnaam (of de klantvriendelijke naam) voor een apparaat | [Eigenschappen van apparaatatlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | geldige marketingnaam |
|                 | mobileDevice | boolean | True (waar) als het apparaat bestemd is voor gebruik onderweg | [Eigenschappen van apparaatatlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | string | De modelnaam van het apparaat, de browser of een andere component | [Eigenschappen van apparaatatlas](https://deviceatlas.com/device-data/properties){target=_blank} | tablet, telefoon, xbox. set-top box | geldige modelnaam van apparaat |
|                 | osName | string | Het besturingssysteem waarop het apparaat wordt uitgevoerd | [Apparaataturen - vooraf gedefinieerde eigenschapswaarden van het besturingssysteem](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Other Note: U moet zijn aangemeld met een gebruikersnaam en wachtwoord in Device Atlas om de eigenschapswaarden weer te geven | de verwachte waarde is een van de waarden in de vooraf gedefinieerde eigenschappen van de apparaatatlas |
|                 | browserName | string | De naam of het type browser op het apparaat | [Apparaataturen - Vooraf gedefinieerde eigenschapswaarden voor browser](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | De naam of het type van browser op het apparaat.  Nota: U moet met een gebruikersbenaming en een wachtwoord in de Atlas van het Apparaat worden aangemeld om de bezitswaarden te bekijken | de verwachte waarde is een van de waarden in de vooraf gedefinieerde eigenschappen van de apparaatatlas |
|                 | browserVersion | string | De browserversie op het apparaat | [Eigenschappen van apparaatatlas](https://deviceatlas.com/device-data/properties){target=_blank} | De browserversie op het apparaat |                                                                                   |
| Toepassing | applicationName | string | De gebruiksvriendelijke of door de consument leesbare naam van de toepassing | NVT | Sample_Application |                                                                                   |
|                 | applicationId | string | De toepassings-id die een clienttoepassing op unieke wijze identificeert. | NVT | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | Het native platform van de toepassing | NVT | ios, android |                                                                                   |
|                 | applicationVersion | string | Deze waarde kan voor analysedoeleinden worden gebruikt | NVT | 1.0, 2.0 |                                                                                   |
| Onderwerp | accountId | string | De rekeningnummer van het onderwerp &quot;Concurrency Monitoring&quot; (binnen het toepassingsgebied van het MVPD) | NVT | testaccount |                                                                                   |
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

* U kunt een beleid vormen om slechts op specifieke gebiedswaarden van toepassing te zijn (bijvoorbeeld een specifiek beleid van iOS: waar `osType` is `iOS`)
* U kunt het aantal afzonderlijke waarden voor een bepaald veld beperken. Hier volgen enkele voorbeelden:
   * niet meer dan X verschillende inrichtingen: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * niet meer dan X duidelijke postcodes: `HAVING DISTINCT COUNT(zipcode) <= 3`
* U kunt het aantal actieve stromen per gebiedswaarde beperken. Hier volgen enkele voorbeelden:
   * maximaal X actieve streams voor één apparaattype: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * niet meer dan X actieve stromen voor stromen van levende inhoud: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Neem via [een ticket maken in Zendesk](mailto:tve-support@adobe.com) en geef aan welk beleid u wilt toepassen.

Hier volgen meer voorbeelden van beleid en integratiecookbooks:

* [Beleidsbeslissingspunt](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API-console - Gelijktijdige bewaking Adoben](http://docs.adobeptime.io/cm-api-v2/)
