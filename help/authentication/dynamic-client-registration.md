---
title: Dynamische clientregistratie
description: Dynamische clientregistratie
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Dynamische clientregistratie {#dynamic-client-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Context {#context}

Om aan de moderne veiligheidspraktijken, de verbeterde vereisten van UX en van de platformeigenaars te richten, de Authentificatie van Adobe Pass Android SDK en SDK van iOS bewegen zich in de richting van goedkeuring [Aangepaste tabbladen van Android-chroom](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari view controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

De huidige AdobePass-implementatie gebruikt platformspecifieke webweergaven om de webomgeving te bieden voor de weergave van de MVPD-aanmeldingspagina. Deze webweergaven delen geen aanmeldingsbeheer met platformbrowsers, zodat de gebruiker geen wachtwoord kan gebruiken dat door de browser is opgeslagen bij het gebruik van een Adobe Pass-verificatietoepassing. Bovendien, om veiligheidsredenen, bewegen sommige platforms zich om de controlemechanismen WebView voor authentificatietaken te verwerpen. Zowel Google als Apple bieden alternatieve opties, zoals &quot;Aangepaste tabbladen Chrome&quot; en &quot;Safari View Controller&quot;. Dit zijn eigenlijk tabbladen voor eenmalig gebruik van hun respectievelijke browsers. Adobe Pass Authentication zal deze nieuwe componenten in 2018 goedkeuren.

## Details {#details}

Op dit moment zijn er twee manieren waarop Adobe Pass-verificatie toepassingen identificeert en registreert:

* browsergebaseerde clients worden geregistreerd via een toegestane domeinlijst
* native toepassingsclients, zoals iOS- en Android-toepassingen, worden geregistreerd via het mechanisme voor ondertekende aanvragers

Adobe Pass stelt een nieuw mechanisme voor clientregistratie voor om de nieuwe stromen voor Chrome Custom Tabs &amp; Safari View Controller aan te pakken. Dit mechanisme zal een veiligere en korrelige controle van uw toepassingen toestaan en het kan worden gebruikt om toepassingen op alle platforms te registreren.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
