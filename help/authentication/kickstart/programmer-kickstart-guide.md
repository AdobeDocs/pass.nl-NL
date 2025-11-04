---
title: Kickstart-handleiding voor programmeurs
description: Kickstart-handleiding voor programmeurs
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Kickstart-handleiding voor programmeurs {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Deze kickstart-handleiding is bedoeld voor inhoudsproviders (programmeurs) die Adobe® Pass Authentication willen integreren in hun websites of toepassingen.

In dit document worden de belangrijkste eerste stappen beschreven die moeten zorgen voor een soepele en efficiënte start van het integratieproces. Het is bedoeld om de verwachtingen te verduidelijken en richtsnoeren te geven over de manier waarop we met partners zullen samenwerken om succesvolle integratie te bereiken.

Adobe beschikt over een aantal bronnen waarmee u Adobe Pass-verificatie kunt integreren in uw website of toepassing. Gelieve te verwijzen naar **&quot;u&quot;** en **&quot;Adobe zal verstrekken&quot;** commentaren van elke hieronder sectie.

## Installatieproces {#setup-process}

Het installatieproces omvat onder andere de volgende stappen:

![ Adobe® het Proces van de Integratie van de Authentificatie van de Volwassing ](/help/authentication/assets/progr-flow-int-lifecycle.png)

*Adobe® het Proces van de Integratie van de Authentificatie van de Volwassing*

**u** tijdens de kickoff fase zult verstrekken:

* **dienstverlener (verzoekor herkenningsteken)**

  Dit is een tekenreeks waarmee het merk van de website of de toepassing die aanvragen voor Adobe Pass-verificatie indient, op unieke wijze wordt geïdentificeerd. De tekenreeks zelf is willekeurig, maar moet tussen Adobe en de programmeur worden overeengekomen

* **informatie van het Kanaal**

  Dit is een reeks tekenreeksen die wordt gebruikt om de inhoudskanalen te identificeren die door het servicebureau worden aangevraagd. In veel gevallen zijn het kanaal en de dienstverlener het zelfde. Eén id kan echter meerdere inhoudskanalen vertegenwoordigen. Deze koorden van de kanaalnaam zouden met de overeenkomstige kanalen van kabelTV moeten richten. Merk op dat sommige MVPDs deze waarde tijdens het authentificatie en/of goedkeuringsproces kan bevestigen.

* **namen van het Domein**

  Deze lijst bevat de domeinnamen die daadwerkelijk aan Adobe worden vermeld om de serviceprovider te vertegenwoordigen. Hiermee zorgt u ervoor dat alleen uw geautoriseerde domeinen via uw metagegevens toegang hebben tot Adobe Pass-verificatie. Zorg ervoor dat u domeinnamen opgeeft en duidelijk identificeert voor zowel productie- als testomgevingen, aangezien deze kunnen verschillen.

**u** via MVPD zult verstrekken:

* **Referentiesets**

  Dit zijn referenties die worden gebruikt om de gebruiker te verifiëren en autoriseren, of alleen om de gebruiker te verifiëren met de MVPD. Deze gegevens bestaan meestal uit een gebruikersnaam en wachtwoord die de MVPD u voor beide profielen (ophaling en productie) opgeeft.

* **herkenningstekens van het Middel**

  Dit zijn unieke id&#39;s voor de inhoudskanalen, shows, afleveringen of elementen die de serviceprovider wil beschermen. Deze identificatiecodes worden gebruikt om vergunningsbesluiten aan te vragen en moeten met de MVPD worden overeengekomen.

>[!IMPORTANT]
>
> De programmeur is verantwoordelijk voor de coördinatie met de MVPD om alle noodzakelijke zakelijke overeenkomsten af te ronden. Intussen zal Adobe Pass Authentication samenwerken met de MVPD om ervoor te zorgen dat de technische integratie correct tot stand komt:
>
> * **Nieuwe MVPD**
>
>     Als de MVPD niet geïntegreerd is met Adobe, moet er een aangepaste code worden ontwikkeld op basis van de specifieke MVPD-vereisten. Totdat deze ontwikkeling is voltooid, zal de MVPD niet beschikbaar zijn en kunnen de producttests met die MVPD niet worden uitgevoerd.
>
> * **Bestaande MVPD**
>
>     Als de MVPD al is geïntegreerd met Adobe, is het connectiviteitsproces aanzienlijk gestroomlijnd. In de meeste gevallen, kan de connectiviteit snel door configuratieaanpassingen eerder dan uitgebreide ontwikkeling worden gevestigd.
>
> Alle integraties vereisen gezamenlijke inspanningen op het gebied van kwaliteitsborging, inclusief tests door de MVPD, aangezien de eindgebruiker uiteindelijk een klant van de MVPD is. Het coördineren van testcycli hangt vaak af van de beschikbaarheid van MVPD-bronnen, wat mogelijke vertragingen kan veroorzaken.

## Toegang tot klantenondersteuning {#access-customer-support}

**Adobe zal** toegang tot ons systeem van de klantensteun via [ Zendesk ](https://tve.zendesk.com/home) verlenen. Als u toegang wilt tot Zendesk, moet u zich registreren en een account maken op https://tve.zendesk.com/home. Er is geen limiet voor het aantal gebruikers dat u kunt registreren. Zodra geregistreerd, kunt u commentaren op om het even welk voorgelegd kaartje bekijken en delen.

Het Adobe Pass-verificatieteam is beschikbaar als hulp bij vragen of technische problemen die u tijdens het integratieproces kunt tegenkomen. Gelieve te contacteren ons in [ tve-support@adobe.com ](mailto:tve-support@adobe.com).

## Toegang tot documentatie {#access-documentation}

**Adobe zal** toegang tot onze openbare documentatie via [ de Liga van de Ervaring van Adobe ](https://experienceleague.adobe.com/en/docs/pass/authentication/home) verlenen.

Het team van de Authentificatie van Adobe Pass verstrekt uitvoerige documentatie voor de beschikbare eigenschappen en APIs onder de [ Gids van de Integratie voor de sectie van Programmers ](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Verwijs naar de inhoudstafel onder deze sectie voor verbindingen aan gedetailleerde informatie over elk onderwerp.

## Toegang tot het testgereedschap {#access-testing-tool}

**Adobe zal** toegang tot ons APIs exploratiehulpmiddel via [ Adobe Developer ](https://developer.adobe.com/adobe-pass/) website verstrekken.

## Toegang tot hulpprogramma voor configuratiebeheer {#access-configuration-management-tool}

**Adobe zal** toegang tot een zelfbedieningshulpmiddel verstrekken om uw configuratie en gegevens via [ Dashboard van Adobe Pass te beheren TVE ](https://experience.adobe.com/pass/authentication).

Het team van de Authentificatie van Adobe Pass verstrekt uitvoerige documentatie voor het gebruik van het Dashboard van TVE onder de [ Gids van de Gebruiker voor het Dashboard van TVE ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) sectie. Verwijs naar de inhoudstafel onder deze sectie voor verbindingen aan gedetailleerde informatie over elk onderwerp.
