---
title: Opmerkingen bij de release Adobe Pass Authentication 2.64
description: Opmerkingen bij de release Adobe Pass Authentication 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.64 {#authn-264-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#ss-web-clients}

* [Buildnummer](#build-no-264)
* [Nieuwe functies](#new-featres-264)
* [Bugfixes](#bug-fixes-264)

### Buildnummer {#build-no-264}

Adobe Pass-verificatie: adobe-pass-**2,64**

Releasedatum: **08-11-2022 - 11-10-2022**

### Nieuwe functies {#new-featres-264}

* Infrastructuurupdates, gericht op het verminderen van de reactietijden van de server, die algemene prestaties van het systeem verbeteren.
* Verbeteringen van het nieuwe platform-identificatiemechanisme.
* Mogelijkheid om ongevraagde authentificatiereacties van MVPDs te blokkeren waar de &quot;in_response_to&quot;parameter van de bewering van SAML mist.

### Bugfixes {#bug-fixes-264}

* Oplossing voor onjuiste opmaak van sommige verouderde TempPass-tokens.
* Minder belangrijke kwesties op de tweede stroom van de het schermauthentificatie werden gericht.
