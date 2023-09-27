---
title: Tijdelijke controle opnieuw instellen op iOS
description: Tijdelijke controle opnieuw instellen op iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Tijdelijke controle opnieuw instellen op iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

De iOS Demo-app bevat een speciaal scherm voor het opnieuw instellen van de Temp Pass-TTL. De volgende informatie is vereist voor het opnieuw instellen.

- **Omgeving:** Geeft het Adobe Pay-TV-voldoende servereindpunt aan dat de netwerkaanroep Temp-controle ontvangt. Mogelijke waarden: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Geen** (*mgmt.auth.adobe.com*) of **Aangepast** (gereserveerd voor interne Adobe tests).
- **OAuth2 Dragertoken:** het token OAuth2 is nodig om de programmeur te autoriseren voor Adobe van betaaltelevisie. Een dergelijk token kan worden verkregen van het specifieke Pay-TV-verificatieeindpunt OAuth2 (bijvoorbeeld *curl-u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accstoken?gift\_type=client\_credentials&quot;*).
- **Id aanvrager:** de unieke id voor de huidige programmeur. Deze waarde wordt gelezen vanuit het hoofdscherm van de Demo-app (het veld voor de aanvrager).
- **ID Temperatuur-controle:** de unieke id voor de Temp Pass MVPD.
- **Apparaat-id:** hashed Device ID berekend door de Demo-app.
- **Algemene toets:** Sommige MVPD&#39;s van de Controle van Temp (d.w.z. de volgende verlengbare functionaliteit van de Voldoende Controle van Temperatuur) steunen een generische sleutel voor het terugstellen van de Voldoende Voldoende Controle van Temperatuur (naast Apparaat ID).

Alle bovenstaande parameters (behalve de *Algemene toets*) zijn verplicht. Hier is een voorbeeld van parameters en de bijbehorende netwerkvraag die door de Toepassing van de Demo zal worden uitgevoerd (het voorbeeld is in de vorm van een *curl *command):

- **Omgeving:** Geen (*mgmt.auth.adobe.com*)
- **OAuth2 Dragertoken:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Programma-id:** REF
- **ID Temperatuur-controle:** TempPassREF
- **Apparaat-id:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639 91
- **Algemene toets:** null (geen waarde opgegeven)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

er wordt een HTTP-aanvraag voor DELETE ingediend bij de **/reset** eindpunt, het overgaan van *OAuth2 Dragertoken* in de machtigingsheader en de *Apparaat-id*, *Id van aanvrager* en *Temperatuurcontrole-id (MVPD-id)* als parameters.

Als de programmeur een waarde voor de *Algemene toets*, wordt een andere HTTP-aanroep uitgevoerd (dit keer naar de **/reset/generic** (eindpunt), doorgeven *Algemene toets* in de *key* request parameter.

Stel bijvoorbeeld de instelling *Algemene toets* aan een e-mailadresknoeiboel (voor MVPDs van de Pas van Temperatuur die dit soort functionaliteit steunen) zal de volgende vraag van HTTP opleveren (e-mail is `user@domain.com` de SHA-256-hash is `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Het is belangrijk om te benadrukken dat het opnieuw instellen van de Pas van de Temp in de Demo App het zelfde effect voor een Programma app op het zelfde apparaat kan niet hebben. Dit komt doordat de apparaat-id (zoals berekend door de demo-app en AccessEnabler) mogelijk niet voor alle toepassingen op het apparaat gelijk is:

- iOS 6 en lager: de apparaat-id wordt berekend met het MAC-adres (dat uniek is voor alle apps). Als u de Temp-controle opnieuw instelt in de Demo-app, wordt deze opnieuw ingesteld in alle andere programmeertoepassingen op het apparaat.

- iOS 7 en hoger: de apparaat-id wordt berekend op basis van de waarde IDFV (ID for Vendor), die uniek is voor alle toepassingen die hetzelfde voorvoegsel voor de bundel-id hebben (dat wil zeggen alle componenten behalve de laatste). Aangezien de demo-app en een programmeerapp verschillende bundel-id&#39;s hebben, heeft het opnieuw instellen van de Temp-controle in de demo-app geen effect op een programmeertoepassing.

Het laatste gebruik-geval (iOS 7 en hoger) is het gemeenschappelijkst zodat zien hoe de Programmeurs de Pas van Temp voor hun apps in deze situatie kunnen terugstellen. Er zijn verschillende opties:

1. Geef de code vanuit de demo-app door aan de programmeertoepassing. De *TempPassResetViewController* en *DeviceIdDemoApp* bevatten de kernlogica voor het opnieuw instellen van Temp-controle en deze kunnen eenvoudig worden gewijzigd en opgenomen in de programmeerapp.

1. De HTTP-aanvraag voor het opnieuw instellen van de Temp-controle uitvoeren met *krullen*. De parameter device\_Id kan worden verkregen door de IDFV van de programmeerapp te berekenen en er een SHA-256-hash op toe te passen (voorbeeldcode in het deelvenster *DeviceIdDemoApp* klasse).

1. U kunt de voorinstelling vanuit de demo-app uitvoeren door de gehashte IDFV van de app van de programmeur op te geven als de *Algemene toets*. Dit zal in twee netwerkvraag resulteren: voor het terugstellen van de Pas van Temp voor de Bodemapp (irrelevant voor programmeur) en voor het terugstellen van de Pas van Temp voor de app van de Programmer.

Alle bovenstaande opties zijn vergelijkbaar. Het is aan de programmeur om er een te kiezen, afhankelijk van het gemak van de implementatie.
