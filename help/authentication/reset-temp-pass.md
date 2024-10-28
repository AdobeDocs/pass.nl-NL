---
title: Tijdelijke controle opnieuw instellen
description: Tijdelijke controle opnieuw instellen
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 32060798501abe2bf7314474d55cf844efb3f7b1
workflow-type: tm+mt
source-wordcount: '336'
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

Om **een specifieke PasPas van Temperatuur voor een apparaat of alle apparaten** terug te stellen, verstrekt de Authentificatie van Adobe Pass Programmeurs van het a *openbare* Web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protocol:** HTTPS
* **Gastheer:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Weg:** /reset-tempass/v3/reset
* **de parameters van de Vraag:** device_id (wanneer geen waarde wordt verstrekt, gebreken aan allen), requestor_id (vereist), mvpd_id (vereist), appId, deviceUser, milieu
* **Kopballen:** Vergunning: Drager &lt;access_token_here>
* **Reactie:**
   * Succes - HTTP 204
   * Mislukt:xw
      * HTTP 400 voor een onjuiste aanvraag
      * HTTP 401 als de toegang wordt ontkend. De cliënt MOET om een nieuw access_token verzoeken.
      * HTTP 403 als de client-id geen aanvragen meer mag uitvoeren. Er moeten nieuwe clientreferenties worden gegenereerd.


Bijvoorbeeld:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Om **de Flexibele PasPas van Temperatuur voor een generische sleutel of alle sleutels** terug te stellen, verstrekt de Authentificatie van Adobe Pass Programmeurs van het a *openbare* Web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protocol:** HTTPS
* **Gastheer:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Weg:** /reset-tempass/v3/reset/generic
* **de parameters van de Vraag:** sleutel (wanneer geen waarde wordt verstrekt, gebreken aan allen), requestor_id (vereist), mvpd_id (vereist), milieu
* **Kopballen:** Vergunning: Drager &lt;access_token_here>
* **Reactie:**
   * Succes - HTTP 204
   * Fout:
      * HTTP 400 voor een onjuiste aanvraag
      * HTTP 401 als de toegang wordt ontkend. De cliënt MOET om een nieuw access_token verzoeken.
      * HTTP 403 als de client-id geen aanvragen meer mag uitvoeren. Er moeten nieuwe clientreferenties worden gegenereerd.


Bijvoorbeeld, plaatsend *Algemene Sleutel* aan een e-mailadresknoeiboel (voor
MVPD&#39;s van Temp-controle die dit soort functionaliteit ondersteunen) leveren de
na HTTP-aanroep (de e-mail is `user@domain.com` zijn SHA-256
hash is `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
