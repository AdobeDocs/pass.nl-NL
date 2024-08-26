---
title: Tijdelijke controle opnieuw instellen
description: Tijdelijke controle opnieuw instellen
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Tijdelijke controle opnieuw instellen {#reset-temp-pass}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Controleer voordat u de API voor tijdelijke controle opnieuw instellen gebruikt of aan de volgende voorwaarden is voldaan:
>
> * Haal de cliëntgeloofsbrieven zoals die in [ worden beschreven terug cliëntgeloofsbrieven ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
> * Haal het toegangstoken zoals die in [ wordt beschreven terug toegangstoken ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
>
> Verwijs naar de [ Dynamische documentatie van het Overzicht van de Registratie van de Cliënt ](./dcr-api/dynamic-client-registration-overview.md) voor meer informatie over hoe te om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

Om **terug te stellen een specifieke Pas van Temperatuur**, verstrekt de Authentificatie van Adobe Pass Programmeurs van het a *openbare* Web API:

* **Milieu:** specificeert het Adobe betaal-TV het eindpunt van de omslagserver dat de het netwerkvraag van de Pas van het Teruggestelde Temperatuur zal ontvangen. Mogelijke waarden: **Prequal** (*beheer* prequal.auth.adobe.com*), **Versie** (*mgmt.auth.adobe.com*) of **Douane** (gereserveerd voor het interne testen van de Adobe).
* **Token van de Toegang OAuth2:** het token OAuth2 is noodzakelijk voor het autoriseren van de programmeur voor Adobe Pay-TV-verificatie. Zulk een teken kan worden verkregen zoals die in [ wordt beschreven ontvangt toegangstoken ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
* **identiteitskaart van de Voldoende Controle van het Temperatuur:** unieke identiteitskaart voor de Voldoende MVPD van het Temperatuur om worden teruggesteld.(een programmeur kan veelvoudige MVPD van de Pas van Temperatuur gebruiken en wil specifieke terugstellen)
* **Algemene Sleutel:** sommige Volgen MVPDs van de Temperatuur (d.w.z. [ Bevorderende tijdelijke pas ](promotional-temp-pass.md)).

Alle bovengenoemde parameters (behalve de *Algemene Sleutel*) zijn verplicht. Hier is een voorbeeld van parameters en de bijbehorende netwerkvraag (het voorbeeld is in de vorm van een *curl *command):

* **Milieu:** Versie (*mgmt.auth.adobe.com*)
* **Token van de Toegang OAuth2:** &lt;access_token> zoals die in [ wordt beschreven verkrijg toegangstoken ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie
* **identiteitskaart van de Programmer:** REF
* **identiteitskaart van de Controle van het Temperatuur:** TempPassREF
* **Algemene Sleutel:** ongeldig (geen waarde verstrekt)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

Een verzoek van HTTP van DELETE zal aan het **worden gemaakt/teruggesteld** eindpunt, die het *Token van de Toegang OAuth2* in de kopbal van de Toestemming en *identiteitskaart van het Apparaat*, *identiteitskaart van de Vraag* overgaan en *identiteitskaart van de Controle van Temperatuur (identiteitskaart MVPD)* als parameters.

Als de Programmer een waarde voor *Generic Zeer belangrijke* levert, zal een andere vraag van HTTP (dit keer aan het **/reset/generic** eindpunt) worden uitgevoerd, die de *Algemene Sleutel* binnen de *sleutel* verzoekparameter overgaan.

Bijvoorbeeld, plaatsend *Algemene Sleutel* aan een e-mailadresknoeiboel (voor
MVPD&#39;s van Temp-controle die dit soort functionaliteit ondersteunen) leveren de
na HTTP-aanroep (de e-mail is `user@domain.com` zijn SHA-256
hash is `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Om **een specifieke PasPas van Temperatuur voor alle apparaten** terug te stellen, verstrekt de Authentificatie van Adobe Pass Programmeurs van het a *openbare* Web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>De bovenstaande URL vervangt de vorige API voor opnieuw instellen. De oude API voor opnieuw instellen (v1) wordt niet meer ondersteund.

* **Protocol:** HTTPS
* **Gastheer:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Weg:** /reset-tempass/v3/reset
* **de parameters van de Vraag:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Kopballen:** Vergunning: Drager &lt;access_token_here>
* **Reactie:**
   * Succes - HTTP 204
   * Fout:
      * HTTP 400 voor een onjuiste aanvraag
      * HTTP 401 als de toegang wordt ontkend. De cliënt MOET om een nieuw access_token verzoeken.
      * HTTP 403 als de client-id niet langer aanvragen mag uitvoeren. Er moeten nieuwe clientreferenties worden gegenereerd.


Bijvoorbeeld:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
