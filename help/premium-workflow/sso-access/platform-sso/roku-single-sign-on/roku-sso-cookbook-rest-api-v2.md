---
title: Roku SSO Cookbook (REST API V2)
description: Roku SSO Cookbook (REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO Cookbook (REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De Adobe Pass Authentication REST API V2 biedt ondersteuning voor Platform Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op RokuOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan het bestaande [ REST API V2 Overzicht ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) dat een mening op hoog niveau en het document verstrekt dat beschrijft hoe te om [ Enige sign-on uit te voeren gebruikend de stromen van de platformidentiteit ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Single Sign-On gebruiken om identiteitsstromen van platforms te gebruiken {#cookbook}

Adobe Pass-verificatie werkt samen met Roku om de gebruikerservaring bij aanmelding te verbeteren en Single Sign-On (SSO) te vereenvoudigen voor tv-abonnees overal op tv.

### Vereisten {#prerequisites}

Alvorens met Roku enig teken-op het gebruiken van de stromen van de platformidentiteit te werk te gaan, zorg ervoor dat SSO Roku wordt toegelaten. Roku SSO wordt toegelaten door gebrek tenzij de programmeur of MVPD verzoek om SSO om worden onbruikbaar gemaakt.

Elke Programmeur kan Enige Sign-On (SSO) op het platform van Roku voor specifieke integratie door het [ Adobe Pass TVE Dashboard ](https://experience.adobe.com/pass/authentication) toelaten of onbruikbaar maken.

### Workflow {#workflow}

**Cliënt-aan-Server**

Voor programmeertoepassingen die een client-naar-server architectuur gebruiken om REST API V2 te integreren, functioneert Roku SSO naadloos zonder enige wijzigingen.

RokuOS voegt automatisch twee HTTP-headers toe aan alle aanvragen die naar de eindpunten van de Adobe Pass-verificatie worden verzonden.

**Server-aan-Server**

Voor programmeertoepassingen die een server-aan-server architectuur gebruiken om REST API V2 te integreren, moet de programmeur met het team van Roku coördineren om deze kopballen te vormen om in alle API stromen te worden omvat die aan hun domein worden gericht.

Om cross-application en cross-device SSO toe te laten, zou roku-Geleide abonnee identiteitskaart in plaats van apparatenidentiteitskaart moeten worden gebruikt wanneer overgegaan door de toepassing.

Raadpleeg de volgende documentatie voor meer informatie:

* [Koptekst - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Koptekst - AP-apparaat-id](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Neem contact op met uw Adobe-vertegenwoordiger voor specifieke informatie over de indeling van de benodigde headers.

### Veelgestelde vragen {#faqs}

* **Hoe zal SSO werken?**

  SSO werkt in alle programmeertoepassingen die door Adobe Pass-verificatie worden aangedreven op alle Roku-apparaten die aan dezelfde Roku-gebruiker zijn gekoppeld. Niet zullen alle MVPDs Roku SSO toestaan.


* **zal er om het even welke verandering in de authentificatie TTLs zijn?**

  Het eerste geldige authentificatietoken zal voor het uitvoeren van SSO worden gebruikt en, in dit geval, zullen alle andere toepassingen die door SSO zullen voor authentiek worden verklaard zelfde TTL gebruiken tot het verloopt. Wanneer u dus van de ene toepassing naar de andere navigeert, deelt de tweede toepassing de TTL van de eerste toepassing die wordt geverifieerd.


* **zal andere functionaliteit van Adobe zoals vroeger werken?**

  Alle Adobe Pass-verificatiefuncties werken zoals voorheen.


* **is er een opt-in/opt-out proces van de Programmer die van SSO op het platform van Roku profiteert?**

  Dit wordt een configuratiewijziging in het Adobe TVE-dashboard. Elke programmeur kan SSO op het platform van Roku voor specifieke integratie toelaten of onbruikbaar maken.


* **wat enkele gemeenschappelijke kwesties zijn?**

  Programmeurs moeten controleren of hun huidige implementaties op basis van de Adobe REST API geen belemmering vormen voor Roku&#39;s platform-SSO.

  Zie hieronder een lijst met mogelijke problemen en hoe deze moeten worden opgelost.

| Probleem | Mogelijke oorzaak | Mogelijke oplossingen |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Geen Roku SSO-header verzonden naar Adobe | HTTP gebruiken in plaats van HTTPS voor aanroepen naar Adobe Pass Authentication-domeinen | HTTPS gebruiken |
| MVPD-logo niet weergegeven/niet bijgewerkt voor SSO-tokens | Gebruikersinterface is afhankelijk van lokale opslag | Toepassingen moeten de gebruikersinterface (en lokale opslag, indien nodig) bijwerken na verificatie |
| Afmelden geactiveerd op geen AuthZ | Toepassingsontwerp | De toepassing moet worden bijgewerkt om zich nooit achter de schermen af te melden |
