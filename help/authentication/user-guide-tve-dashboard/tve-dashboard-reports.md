---
title: Rapporten
description: Leer hoe de gegevens worden samengevoegd in TVE-dashboardrapporten.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Rapporten {#Reports}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De **sectie van Rapporten** van het Dashboard TVE verleent toegang tot samengevoegde gegevens voor TTL AuthN, TTL AuthZ, en SSO rapporten. Deze rapporten omvatten uw kanaalintegratie met verschillende MVPDs over alle [ platforms ](#platforms).

De rapporten laten u aan filtergegevens en verzamelen inzichten over [ specifieke Kanalen of MVPDs ](#selecting-specific-channels-mvpds). U kunt rapporten ook exporteren in een CSV-bestand voor verdere analyse.

## Rapporten weergeven {#view-reports}

Voer de volgende stappen uit om een specifiek rapport weer te geven.

1. Selecteer het **lusje van Rapporten** in het linkerpaneel.
1. Selecteer een van de volgende tabbladen om de geaggregeerde gegevens van de opgenomen kanalen en MVPD&#39;s weer te geven en te exporteren:
   * [AuthN TTL-rapporten](#authn-ttl-reports)
   * [TTL-rapporten van AuthZ](#authz-ttl-reports)
   * [SSO-rapporten](#sso-reports)

   ![ Type van rapporten ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Type van rapporten*

### AuthN TTL-rapporten {#authn-ttl-reports}

De rapporten van AuthN TTL, die ook als Tijd-aan-Levende Authentificatie (TTL) worden bedoeld, tonen de duur waarvoor de authentificatietokens voor uw Integraties van Kanalen met diverse MVPDs over alle [ platforms ](#platforms) worden gevormd. Deze rapporten staan u toe om de hoeveelheid tijd te inspecteren een gebruiker voor een specifieke MVPD en een platform voor authentiek verklaard blijft. De duurwaarden worden voorgesteld in gebruikersvriendelijke formaten zoals, **dagen**, **uren**, **notulen**, en **seconden**. De tabel AuthN TTL-rapporten biedt horizontaal en verticaal schuiven voor verschillende schermgrootten.

U kunt gegevens voor [ specifieke kanalen of MVPDs ](#selecting-specific-channels-mvpds) ook bekijken en downloaden.

![ de Rapporten van AuthN TTL van de Uitvoer ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*de Rapporten van AuthN TTL van de Uitvoer*

>[!IMPORTANT]
>
> **die door MVPD** wordt geplaatst placeholder wordt gebruikt wanneer MVPD de waarde AuthN TTL eerder dan de configuratie van de Authentificatie van Adobe Pass afdwingt.

Selecteer **de rapporten van de Uitvoer** om de gegevens als Csv- dossier op uw lokale machine te bewaren.

### AuthZ TTL-rapporten {#authz-ttl-reports}

De rapporten van AuthZ TTL, die ook als Vergunning tijd-aan-Levend (TTL) worden bedoeld, tonen de duur van het toestemmingstoken dat voor uw Integraties van Kanalen met diverse MVPDs over alle [ platforms ](#platforms) wordt gevormd. Deze rapporten staan u toe om de hoeveelheid tijd te inspecteren een gebruiker geautoriseerd blijft om inhoud voor een specifiek MVPD en platform te letten. De duurwaarden worden voorgesteld in gebruikersvriendelijke formaten zoals, **dagen**, **uren**, **notulen**, en **seconden**. De tabel AuthZ TTL-rapporten biedt horizontaal en verticaal schuiven voor verschillende schermgrootten.

U kunt de gegevens voor [ specifieke kanalen of MVPDs ](#selecting-specific-channels-mvpds) ook bekijken en downloaden.

![ Uitvoer AuthZ TTL Rapporten ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Uitvoer AuthZ TTL Rapporten*

>[!IMPORTANT]
>
> **die door MVPD** wordt geplaatst placeholder wordt gebruikt wanneer MVPD de waarde AuthZ TTL eerder dan de configuratie van de Authentificatie van Adobe Pass afdwingt.

Selecteer **de rapporten van de Uitvoer** om de gegevens als Csv- dossier op uw lokale machine te bewaren.

### SSO-rapporten {#sso-reports}

De rapporten SSO, die ook als enige sign-on worden bedoeld, tonen de enige sign-on status voor uw Integraties van Kanalen met diverse MVPDs over alle [ platforms ](#platforms) wordt gevormd. Deze rapporten staan u toe om de verwachte ervaring van SSO van de gebruikersauthentificatie voor een specifieke MVPD en een platform te inspecteren. De waarden worden voorgesteld in gebruikersvriendelijke formaten zoals, **Uitgeschakelde SSO**, **Toegelaten SSO**, en **onzekere SSO**. De tabel SSO-rapporten bevat horizontaal en verticaal schuiven om verschillende schermgrootten mogelijk te maken.

U kunt gegevens voor [ specifieke kanalen of MVPDs ](#selecting-specific-channels-mvpds) ook bekijken en downloaden.

![ de Rapporten van SSO van de Uitvoer ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*de Rapporten van SSO van de Uitvoer*

>[!IMPORTANT]
>
> **SSO onzekere** placeholder wijst erop dat Enige Sign-On (SSO) wordt toegelaten en potentieel operationeel. De onderstaande instellingen kunnen echter een belemmering vormen voor SSO-verificatie, zoals in de volgende voorbeelden wordt uitgelegd:
>
> * Instellingen voor gebruikersplatformen: de optie om cookies van derden te blokkeren.
> * Besluiten van gebruikers: de gebruikers weigeren platformtoegang tot hun abonnement op een tv-provider.
> * MVPD-instellingen: MVPD vraagt verificatie voor elk kanaal.

Selecteer **de rapporten van de Uitvoer** om de gegevens als Csv- dossier op uw lokale machine te bewaren.

## Platforms {#platforms}

De [ Rapporten van AuthN TTL ](#authn-ttl-reports), [ Rapporten AuthZ TTL ](#authz-ttl-reports), en [ Sso- Rapporten ](#sso-reports) presenteren gegevens over diverse platforms, zoals:

* **Desktop**: De waarden van vertoningen die op de programmeerimplementaties via de Authentificatie JavaScript SDK van Adobe Pass worden toegepast.

* **Mobiel**

  **iOS**: De waarden van vertoningen die gebruikend de Authentificatie iOS SDK van Adobe Pass worden toegepast.

  **Android**: De waarden van vertoningen die door de Authentificatie van Adobe Pass Android SDK worden toegepast.

  **anderen**: De waarden van vertoningen die gebruikend de VERANTWOORDELIJKE API van de Authentificatie van Adobe Pass worden toegepast die voor mobiele apparaten wordt ontwikkeld.

* **TVCD**

  **Roku**: De waarden van vertoningen die via de VERTONING API van de Authentificatie van Adobe Pass worden toegepast, die Roku identificeren als apparatentype.

  **FireTV**: De waarden van vertoningen die door de Authentificatie van Adobe Pass worden toegepast FireTV SDK.

  **AppleTV**: De waarden van vertoningen die via de Authentificatie tvOS SDK van Adobe Pass worden toegepast.

  **anderen**: De waarden van vertoningen die gebruikend de VERTONING API van de Authentificatie van Adobe Pass voor TV-Verbonden apparaten worden toegepast.

* **Platform niet geïdentificeerd**: De waarden van vertoningen die op programmeerimplementaties worden toegepast wanneer de diensten van de Authentificatie van Adobe Pass een onbekend apparatentype ontdekken.

Meer over leren hoe te om het gewenste apparatentype, zoals **Roku** met de Authentificatie van Adobe Pass REST APIs of SDKs te delen, bekijk het mechanisme van [ het overgaan cliëntinformatie ](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> De samengevoegde gegevens zijn gebaseerd op de specifieke configuratie van elke Adobe Pass-verificatieomgeving. Wanneer u overschakelt tussen verschillende TVE-dashboardomgevingen, verwacht u variaties in de gegevens in de verschillende rapporten. Verwijs naar de [ milieu&#39;s van de Authentificatie van Adobe Pass ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md) om meer te weten.

## Specifieke kanalen en MVPD&#39;s selecteren {#selecting-specific-channels-mvpds}

[ AuthN TTL Rapporten ](#authn-ttl-reports), [ AuthZ TTL Rapporten ](#authz-ttl-reports), en [ SSO meldt ](#sso-reports) huidige gegevens voor **Alle Kanalen** integraties met **Alle MVPDs** door gebrek.

>[!NOTE]
>
> Als u **Alle Kanalen** of **Alle MVPDs** in de respectieve dropdown menu&#39;s schrapt, wordt een bericht getoond om een selectie te maken om zinvolle rapporten te bekijken.

Een rapport genereren voor specifieke kanalen:

1. Selecteer het **Opgenomen drop-down menu van Kanalen** bij de bovenkant van het geselecteerde rapport.

   ![ Included het dropdown menu van Kanalen ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Included het dropdown menu van Kanalen*

1. Deselecteer **Alle Kanalen**.

1. Selecteer de vereiste kanalen van **Inclusief Kanalen** dropdown menu waarvoor u gegevens wilt produceren.

>[!NOTE]
>
> Om opties beschikbaar in het **Included MVPDs** dropdown menu te hebben, moet u minstens één kanaal in het **Opgenomen kanalen** dropdown menu selecteren.

Om een rapport voor specifieke MVPDs te produceren:

1. Selecteer **Included MVPDs** dropdown menu bij de bovenkant van het geselecteerde rapport.

   ![ Included MVPDs dropdown menu ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Included MVPDs dropdown menu*

1. Deselecteer **Alle MVPDs**.

1. Selecteer vereiste MVPDs van **Included MVPDs** dropdown menu waarvoor u gegevens wilt produceren.
