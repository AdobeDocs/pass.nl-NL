---
title: Access Enabler Android SDK Single Sign-On (SSO) voor Android 10-apps
description: Access Enabler Android SDK Single Sign-On (SSO) voor Android 10-apps
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# (Verouderd) Access Enabler Android SDK Single Sign-On (SSO) voor Android 10-apps {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht

Single Sign-On (SSO) tussen Adobe Pass Authentication-aangedreven apps is beschikbaar op apparaten die Android OS gebruiken via Access Enabler Android SDK. Om Single Sign-On (SSO) op Android-apparaten aan te bieden, maken de Access Enabler Android SDK versie 3.2.1 (nieuwste versie) en eerdere versies gebruik van een gedeeld databasebestand dat is opgeslagen in een Android-opslagimplementatie en dat toegankelijk is voor alle Adobe Pass Authentication-apps.

Google in de nieuwste Android 10-release heeft echter enkele wijzigingen aangebracht &quot;om gebruikers meer controle over hun bestanden te geven en het bestand overzichtelijker te maken, zodat toepassingen die gericht zijn op Android 10 (API-niveau 29) en hoger, bereikbare toegang krijgen tot een extern opslagapparaat, of standaard opslagruimte binnen bereik. Dergelijke apps kunnen alleen hun toepassingsspecifieke map `\[...\]` zien.&quot; Meer details met betrekking tot deze Android 10 opslagveranderingen worden voorgesteld in [ Gegevens en de documentatie van de dossieropslag voor Android ](https://developer.android.com/training/data-storage/files/external-scoped).

Als gevolg van deze veranderingen kan Enige Sign-On (SSO) die door de versie van Android van de Toegang wordt aangeboden 3.2.1 (recentste) SDK worden aangeboden en de vorige versies kunnen op Android 10 apparaten worden beïnvloed zoals die in de volgende sectie wordt verklaard.****

## Gedrag

Afhankelijk van het gebruik van uw app **[!UICONTROL target SDK level]** of het gebruik van **android:requestLegacyExternalStorage** manifest attributen Single Sign-On (SSO) die door Android versie 3.2.1 SDK van de Toegang worden aangeboden (recentste) en vorige versies zullen momenteel als volgt gedragen:

- Uw app richt **Android 9 (API niveau 28)** of lager **- \>** Enige Sign-On (SSO) **zal** werken
- Uw app richt **Android 10** **(API niveau 29)** en plaatst **** de waarde van **requestLegacyExternalStorage aan waar** in duidelijk dossier van uw app **- \>** Enige Sign-On (SSO) **zal** werken
- Uw app richt **Android 10** **(API niveau 29)** en plaatst **niet** de waarde van **requestLegacyExternalStorage aan waar** in duidelijk dossier van uw app **- \>** Enige Sign-On (SSO) **zal niet** werken

>[!TIP]
>
> Alvorens Adobe Pass de Toegang van de Authentificatie Android SDK volledig compatibel is met scoped opslag, kunt u tijdelijk uit kiezen gebaseerd op het het doelSDK niveau van uw app of het requestLegacyExternalStorage duidelijke attribuut zoals die in openbare [ documentatie van Android ](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) wordt verklaard.
