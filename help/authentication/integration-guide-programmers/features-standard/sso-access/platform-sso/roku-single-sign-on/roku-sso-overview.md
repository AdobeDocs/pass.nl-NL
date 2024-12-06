---
title: Overzicht van Roku SSO
description: Info Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Overzicht van Roku SSO {#overview}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

In dit document worden de gegevens beschreven die nodig zijn om te profiteren van Single Sign-On (SSO)-mogelijkheden op Roku-apparaten. Adobe Pass-verificatie werkt samen met Roku om de gebruikerservaring bij aanmelding te verbeteren en Single Sign-On te vereenvoudigen voor tv-abonnees overal op tv.

De oplossing is gebaseerd op de REST-API van de Adobe Pass-verificatie, zodat de meeste programmeurs hun toepassingen niet hoeven bij te werken om te profiteren van Single Sign-On.

## Roku SSO inschakelen {#enable-roku-sso}

Roku SSO wordt toegelaten door gebrek tenzij de programmeur of MVPD verzoek om SSO om worden onbruikbaar gemaakt.

Elke programmeur kan SSO op het platform van Roku voor specifieke integratie toelaten/onbruikbaar maken.

### Wat een programmeur moet doen om Roku SSO te laten werken {#make-roku-sso-work}

Voor de toepassingen van Programmers die REST API op cliëntapparaten uitvoeren, zal Roku SSO zonder enige verandering werken. Het besturingssysteem Roku voegt op elk verzoek twee HTTP-headers toe aan de eindpunten van de Adobe Pass-verificatie.

Voor de toepassingen die van Programmers een Server aan de oplossing van de Server voor REST api uitvoeren, zou de programmeur het team van Roku moeten contacteren en om een configuratie vragen waar deze twee kopballen naar hun domein op alle api stromen zullen worden verzonden.

Door de gebruiker opgegeven abonnee-id die moet worden gebruikt in plaats van de apparaat-id die door de toepassing wordt doorgegeven om SSO voor alle toepassingen (en voor alle apparaten) te garanderen.

Neem voor specifieke informatie over de indeling van de benodigde koppen contact op met uw Adobe.

### Mogelijke problemen {#possible-issues}

Programmeurs moeten controleren of hun huidige implementaties op basis van de REST API van de Adobe geen belemmering vormen voor Roku&#39;s platform-SSO. Zie hieronder een lijst met mogelijke problemen en hoe deze moeten worden opgelost.

| Probleem | Mogelijke oorzaak | Mogelijke oplossingen |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Geen Roku SSO-header verzonden naar Adobe | HTTP gebruiken in plaats van HTTPS voor aanroepen naar Adobe Pass Authentication-domeinen | HTTPS gebruiken |
| MVPD-logo niet weergegeven/niet bijgewerkt voor SSO-tokens | Gebruikersinterface is afhankelijk van lokale opslag | Toepassingen moeten de gebruikersinterface (en lokale opslag, indien nodig) bijwerken na verificatie |
| Afmelden geactiveerd op geen AuthZ | Toepassingsontwerp | De toepassing moet worden bijgewerkt om zich nooit achter de schermen af te melden |

## Veelgestelde vragen {#faq}

* **Hoe zal SSO werken?**

  SSO werkt in alle programmeertoepassingen die door Adobe Pass-verificatie worden aangedreven op alle Roku-apparaten die aan dezelfde Roku-gebruiker zijn gekoppeld. Niet zullen alle MVPDs Roku SSO toestaan.


* **zal er om het even welke verandering in de authentificatie TTLs zijn?**

  Het eerste geldige authentificatietoken zal voor het uitvoeren van SSO worden gebruikt en, in dit geval, zullen alle andere toepassingen die door SSO zullen voor authentiek worden verklaard zelfde TTL gebruiken tot het verloopt. Wanneer u dus van de ene toepassing naar de andere navigeert, deelt de tweede toepassing de TTL van de eerste toepassing die wordt geverifieerd.


* **zal andere functionaliteit van de Adobe werken zoals vroeger?**

  Alle Adobe Pass-verificatiefuncties werken zoals voorheen.


* **is er een opt-in/opt-out proces van de Programmer die van SSO op het platform van Roku profiteert?**

  Dit wordt een configuratiewijziging in het TVE-dashboard van de Adobe. Elke programmeur kan SSO op het platform van Roku voor specifieke integratie toelaten/onbruikbaar maken.
