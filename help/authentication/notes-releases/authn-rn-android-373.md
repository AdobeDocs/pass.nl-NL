---
title: Opmerkingen bij de release Adobe Pass Authentication Android 3.7.3
description: Opmerkingen bij de release Adobe Pass Authentication Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Buildnummer {#build-number-373}

Adobe Pass-verificatie: Android 3.7.3

Datum van de versie: **09/19/2023**

## Overzicht van release {#release-overview-373}

* Wijzigingen voor ondersteuning voor Android 14 en toepassingen die gericht zijn op API-niveau 34
   * Voeg vlag toe die door [ Android 14 runtime-geregistreerde uitzendingen ontvangers ](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported) wordt vereist.
* ChromeCustomTabs herstellen; wordt niet geopend voor MVPD-aanmelding op emulator API 32+
   * Opmerking: als oplossing voor dit probleem op SDK &lt;3.7.3 is het openen van de Chrome-app op de emulator en het voltooien van de instelling voordat wordt geprobeerd MVPD-aanmelding uit te voeren

## Geen pakket {#release-package-373}

U kunt Android SDK v3.7.3 van [ hier downloaden ](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
