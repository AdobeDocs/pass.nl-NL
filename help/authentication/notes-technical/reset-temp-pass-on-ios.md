---
title: Tijdelijke controle opnieuw instellen op iOS
description: Tijdelijke controle opnieuw instellen op iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# Tijdelijke controle opnieuw instellen op iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

De iOS Demo-app bevat een speciaal scherm voor het opnieuw instellen van de Temp Pass-TTL. De volgende informatie is vereist voor het opnieuw instellen.

- **Milieu:** specificeert het Adobe betaal-TV het eindpunt van de omslagserver dat de het netwerkvraag van de Pas van het Teruggestelde Temperatuur zal ontvangen. Mogelijke waarden: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Versie** (*mgmt.auth.adobe.com*) of **Douane** (gereserveerd voor het interne testen van de Adobe).
- **OAuth2 Token van de Teller:** het token OAuth2 is noodzakelijk voor het autoriseren van de programmeur voor Adobe Pay-TV-verificatie. Zulk een teken kan van het specifieke Pay-TV authentificatie OAuth2 eindpunt (b.v. *worden verkregen krull -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_access?gift\_type=client\_credentials&quot;*).
- **identiteitskaart van de Aanvrager:** unieke identiteitskaart voor huidige Programmer. Deze waarde wordt gelezen vanuit het hoofdscherm van de Demo-app (het veld voor de aanvrager).
- **identiteitskaart van de Pas van de Temperatuur:** unieke identiteitskaart voor MVPD van de Pas van Temperatuur.
- **identiteitskaart van het Apparaat:** hashed identiteitskaart van het Apparaat die door de Demo App wordt gegevens verwerkt.
- **Algemene Sleutel:** sommige MVPDs van de Pas van Temperatuur (d.w.z. de volgende verlengbare functionaliteit van de Voldoende Controle van Temperatuur) steunt een generische sleutel voor het terugstellen van de Voldoende Pas van Temperatuur (naast identiteitskaart van het Apparaat).

Alle bovengenoemde parameters (behalve de *Algemene Sleutel*) zijn verplicht. Hier is een voorbeeld van parameters en de bijbehorende netwerkvraag die door de Toepassing van de Demo zal worden uitgevoerd (het voorbeeld is in de vorm van een *curl *command):

- **Milieu:** Versie (*mgmt.auth.adobe.com*)
- **OAuth2 Token van de Drager:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **identiteitskaart van de Programmer:** REF
- **identiteitskaart van de Controle van het Temperatuur:** TempPassREF
- **identiteitskaart van het Apparaat:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e671fa 639991
- **Algemene Sleutel:** ongeldig (geen waarde verstrekt)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

Een verzoek van HTTP van DELETE zal aan het **worden gemaakt/teruggesteld** eindpunt, die *Token OAuth2 van de Drager* in de kopbal van de Toestemming en *identiteitskaart van het Apparaat*, *identiteitskaart van de Vraag* overgaan en *identiteitskaart van de Controle van Temperatuur (identiteitskaart MVPD)* als parameters.

Als de Programmer een waarde voor *Generic Zeer belangrijke* levert, zal een andere vraag van HTTP (dit keer aan het **/reset/generic** eindpunt) worden uitgevoerd, die de *Algemene Sleutel* binnen de *sleutel* verzoekparameter overgaan.

Bijvoorbeeld, plaatsend *Algemene Sleutel* aan een e-mailadresknoeiboel (voor
MVPD&#39;s van Temp-controle die dit soort functionaliteit ondersteunen) leveren de
na HTTP-aanroep (de e-mail is `user@domain.com` zijn SHA-256
hash is `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Het is belangrijk om te benadrukken dat het opnieuw instellen van de Pas van de Temp in de Demo App het zelfde effect voor een Programma app op het zelfde apparaat kan niet hebben. Dit komt doordat de apparaat-id (zoals berekend door de demo-app en AccessEnabler) mogelijk niet voor alle toepassingen op het apparaat gelijk is:

- iOS 6 en lager: de apparaat-id wordt berekend met het MAC-adres (dat uniek is voor alle apps). Als u de Temp-controle opnieuw instelt in de Demo-app, wordt deze opnieuw ingesteld in alle andere programmeertoepassingen op het apparaat.

- iOS 7 en hoger: de apparaat-id wordt berekend op basis van de waarde IDFV (ID for Vendor), die uniek is voor alle toepassingen die hetzelfde voorvoegsel voor de bundel-id hebben (dat wil zeggen alle componenten behalve de laatste). Aangezien de demo-app en een programmeerapp verschillende bundel-id&#39;s hebben, heeft het opnieuw instellen van de Temp-controle in de demo-app geen effect op een programmeertoepassing.

Het laatste gebruik-geval (iOS 7 en hoger) is het gemeenschappelijkst zodat zien hoe de Programmeurs de Pas van Temp voor hun apps in deze situatie kunnen terugstellen. Er zijn verschillende opties:

1. Geef de code vanuit de demo-app door aan de programmeertoepassing. De {*klassen 0} TempPassResetViewController* en *DeviceIdDemoApp bevatten de kernlogica voor het terugstellen van de Pas van Temp, en zij kunnen gemakkelijk worden gewijzigd en in programmeur worden omvat app.*

1. Uitvoer het verzoek van HTTP voor het terugstellen van de Pas van Temperatuur met *krullen* uit. De apparaat \_Id parameter kan worden verkregen door IDFV van de programmeur uit te werken app en een hash SHA-256 over het toe te passen (steekproefcode in de *DeviceIdDemoApp* klasse).

1. Eenvoudig voer het terugstellen van de Bodemapp uit door hashed IDFV van de app van de Programmer als *Algemene Sleutel* te specificeren. Dit zal in twee netwerkvraag resulteren: voor het terugstellen van de Pas van Temp voor de Bodemapp (irrelevant voor programmeur) en voor het terugstellen van de Pas van Temp voor de app van de Programmer.

Alle bovenstaande opties zijn vergelijkbaar. Het is aan de programmeur om er een te kiezen, afhankelijk van het gemak van de implementatie.
