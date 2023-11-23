---
title: Tijdelijke controle opnieuw instellen
description: Tijdelijke controle opnieuw instellen
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Tijdelijke controle opnieuw instellen {#reset-temp-pass}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.
>
>Als u de API voor het doorgeven van tijdelijke gegevens wilt gebruiken, moet u:
>- Vraag het ondersteuningsteam om een softwareverklaring voor uw geregistreerde toepassing
>- verkrijg een toegangstoken dat op wordt gebaseerd [Dynamische clientregistratie](dynamic-client-registration.md)
> 

Om **een specifieke Temperatuurcontrole opnieuw instellen**, Adobe Pass-verificatie biedt programmeurs een *publiek* web-API:

- **Omgeving:** Geeft het Adobe Pay-TV-voldoende servereindpunt aan dat de netwerkaanroep Temp-controle ontvangt. Mogelijke waarden: **Prequal** (*mgmt-prequal.auth.adobe.com*), **Geen** (*mgmt.auth.adobe.com*) of **Aangepast** (gereserveerd voor interne Adobe tests).
- **OAuth2 Token van de Toegang:** het token OAuth2 is nodig om de programmeur te autoriseren voor Adobe van betaaltelevisie. Een dergelijke token kan worden verkregen van [Dynamische clientregistratie](dynamic-client-registration.md).
- **ID Temperatuur-controle:** de unieke id voor de Temp Pass MVPD die opnieuw moet worden ingesteld.(een programmeur kan veelvoudige MVPD van de Pas van Temperatuur gebruiken en wil specifieke terugstellen)
- **Algemene toets:** bepaalde Temp Pass MVPD&#39;s (d.w.z. [Tijdelijke doorloop voor speciale acties](promotional-temp-pass.md)).

Alle bovenstaande parameters (behalve de *Algemene toets*) zijn verplicht. Hier is een voorbeeld van parameters en de bijbehorende netwerkvraag (het voorbeeld is in de vorm van een *curl *command):

- **Omgeving:** Geen (*mgmt.auth.adobe.com*)
- **OAuth2 Token van de Toegang:** &lt;access_token> van [Dynamische clientregistratie](dynamic-client-registration.md)
- **Programma-id:** REF
- **ID Temperatuur-controle:** TempPassREF
- **Algemene toets:** null (geen waarde opgegeven)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

er wordt een HTTP-aanvraag voor DELETE ingediend bij de **/reset** eindpunt, het overgaan van *OAuth2 Token van de Toegang* in de machtigingsheader en de *Apparaat-id*, *Id van aanvrager* en *Temperatuurcontrole-id (MVPD-id)* als parameters.

Als de programmeur een waarde voor de *Algemene toets*, wordt een andere HTTP-aanroep uitgevoerd (dit keer naar de **/reset/generic** (eindpunt), doorgeven *Algemene toets* in de *key* request parameter.

Stel bijvoorbeeld de instelling *Algemene toets* aan een e-mailadresknoeiboel (voor MVPDs van de Pas van Temperatuur die dit soort functionaliteit steunen) zal de volgende vraag van HTTP opleveren (e-mail is `user@domain.com` de SHA-256-hash is `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Om **een specifieke Temperatuurcontrole opnieuw instellen voor alle apparaten**, Adobe Pass-verificatie biedt programmeurs een *publiek* web-API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>De bovenstaande URL vervangt de vorige API voor opnieuw instellen. De oude API voor opnieuw instellen (v1) wordt niet meer ondersteund.

- **Protocol:** HTTPS
- **Host:**
   - Release - mgmt.auth.adobe.com
   - Prequal - mgmt-prequal.auth.adobe.com
- **Pad:** /reset-tempass/v3/reset
- **Parameters query:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Kopteksten:** Toestemming: houder &lt;access_token_here>
- **Reactie:**
   - Succes - HTTP 204
   - Fout:
      - HTTP 400 voor een onjuiste aanvraag
      - HTTP 401 als de toegang wordt ontkend. De cliënt MOET om een nieuw access_token verzoeken.
      - HTTP 403 als de client-id niet langer aanvragen mag uitvoeren. Er moeten nieuwe clientreferenties worden gegenereerd.


Bijvoorbeeld:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
