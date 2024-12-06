---
title: Amazon FireTV SSO - Handleiding voor de start van de programma's
description: Amazon FireTV SSO - Handleiding voor de start van de programma's
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Amazon FireTV SSO - Handleiding voor de start van de programma&#39;s {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Inleiding {#intro}

Dit document beschrijft de informatie nodig om de nieuwe **Adobe Pass Authentificatie van de Authentificatie te integreren vuurTV SDK** in uw fireTV toepassing. Deze nieuwe SDK haalt voordeel uit OS vlakke integratie op het platform van Amazon fireTV waarbij **Enige Onderteken** steun wordt aangeboden. Om te kunnen profiteren van Single Sign On is een kleine inspanning vereist van uw kant om uw toepassing van Clientless API naar de nieuwe fireTV SDK te migreren. Er zijn enkele wijzigingen in de verificatiestromen die hieronder worden beschreven.

## Architectuur op hoog niveau en integratie op besturingssysteemniveau {#high}

Om Single Sign On te bereiken tussen tv-toepassingen overal op Amazon fireTV-platform en om de algemene ervaring op dit platform te verbeteren, hebben we besloten onze kern-SDK op het niveau van het FireTV-besturingssysteem te integreren. De programmeurs zullen tegen een stompenbibliotheek moeten compileren die door Adobe wordt verstrekt. De eigenlijke functionaliteit wordt geleverd door de bibliotheek van de Adobe die aanwezig is in Amazon fireTV OS.

Totdat Amazon een FireTV-simulator aanbiedt die onze bibliotheek op besturingssysteemniveau bevat, is de ontwikkeling alleen mogelijk met echte FireTV-apparaten.

## Voordelen {#bene}

* Single Sign On tussen alle Adobe-tv-toepassingen overal op Amazon fireTV-platform met alle geïntegreerde MVPD&#39;s.
* Mogelijkheid om te profiteren van HBA (met ondersteunde MVPD&#39;s).
* Mogelijkheid om de nieuwste FireTV SDK te gebruiken zonder dat u uw toepassingen telkens opnieuw hoeft bij te werken wanneer een nieuwe SDK-versie wordt uitgebracht.
* Alle TVE-toepassingen profiteren van het gebruik van de gedeelde systeembibliotheek omdat er geen lokale kopie van de AccessEnabler-bibliotheek nodig is. Dit zorgt er ook voor dat alle toepassingen dezelfde SDK-versie gebruiken.
* Verificatie via één scherm: geen registratiecode en workflows via het tweede scherm.

## Migratie van een op Clientless API gebaseerde app naar een op FireTV SDK gebaseerde app {#migra1}

Voor het migreren van de Clientless API naar fireTV SDK moet u de codebase verwijderen die betrekking heeft op de Clientless API en de nieuwe FireTV SDK integreren.

Vergeleken met de op Clientless API gebaseerde app, met de nieuwe FireTV SDK wordt de verificatie naar het eerste scherm verplaatst en is er geen tweede schermverificatie meer nodig.

Hiervoor moeten programmeurs een MVPD-kiezer toevoegen aan hun apps, zodat gebruikers hun tv-provider rechtstreeks op het FireTV-apparaat kunnen kiezen. Op de selectie MVPD zal de gebruiker de MVPD login pagina op het brandTV apparaat worden getoond.

Draadframes van de gebruikersstromen die de regelmatige, HBA, en scenario&#39;s SSO op fireTV beschrijven kunnen bij [ Amazon Vuur TV - MVVPD Onderteken-binnen de Stroom van de Gebruiker worden gevonden ](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migratie van op Android SDK gebaseerde app naar op FireTV SDK gebaseerde app {#migra2}

Deze nieuwe fireTV SDK is zeer gelijkaardig aan onze bestaande SDK van Android en de huidige documentatie wij voor **hebben integreren onze SDK van Android** <!--http://tve.helpdocsonline.com/android-technical-overview--> kan worden gebruikt tot wij de documenten van FireTV SDK klaar hebben. Als u al Android-toepassingen hebt die gebruikmaken van de Android SDK, moet de integratie van fireTV SDK in uw FireTV-toepassing eenvoudig zijn.

Vergeleken met de bestaande Android SDK is het op fireTV SDK eenvoudiger om het verificatieproces te ontwikkelen, aangezien de taken voor het beheren/weergeven van de MVPD-aanmeldingspagina en het ophalen van het AuthN-token intern door de AccessEnabler-bibliotheek worden uitgevoerd.

## Veelgestelde vragen {#faq}

1. Hoe zal **SSO** werken?

   * SSO werkt in alle programmeertoepassingen die worden aangedreven door Adobe Pass Authentication en die de nieuwe FireTV SDK gebruiken op hetzelfde Amazon fireTV-apparaat
   * SSO tussen de apps van de Programmer die op Clientless REST API worden uitgevoerd en apps die op fireTV SDK **worden uitgevoerd zal NIET worden gesteund**

1. Wat is de MVPD-verslaggeving van fireTV SSO?

   * **Al MVPDs** die door de Authentificatie van Adobe Pass wordt geïntegreerd zal technisch SSO op fireTV SDK worden gesteund.

1. Naast het gebruiken van nieuwe SDK, welke andere **werkschemaveranderingen** zouden Programmeurs zich van bewust moeten zijn?

   * Programmeurs moeten een MVPD-kiezer implementeren voor FireTV-platform.

1. Zal er om het even welke verandering in de authentificatie **TTLs** zijn?

   * Er is geen verandering in gedrag betreffende authentificatie TTLs.
   * Het eerste geldige authentificatietoken zal voor het uitvoeren van SSO worden gebruikt en in dit geval zullen alle andere toepassingen die door SSO zullen voor authentiek worden verklaard zelfde TTL gebruiken tot het verloopt. Wanneer u dus van de ene toepassing naar de andere navigeert, deelt de tweede toepassing de TTL van de eerste toepassing die wordt geverifieerd.

1. Hoe de **degradatie API** werken?

   * Er zijn geen wijzigingen nodig voor de degradatie-API. De gebruikerservaring zal hetzelfde zijn als op Android-apparaten.

1. Hoe **TempPass** stromen worden beïnvloed?

   * TempPass-stromen zijn één scherm en gedragen zich net als op andere native apparaten.

1. Werkt andere functionaliteit voor Adoben zoals voorheen?

   * Alle Adobe Pass-verificatiefunctionaliteit werkt op fireTV net als op Android-apparaten.
