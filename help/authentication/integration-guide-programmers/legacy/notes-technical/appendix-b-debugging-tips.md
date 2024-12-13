---
title: Bijlage B "Tips voor foutopsporing"
description: Bijlage B "Tips voor foutopsporing"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# (Verouderd) Bijlage B: Tips voor foutopsporing {#appendix-b-debugging-tips}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Tijdelijke gegevens wissen {#clearing-temporary-data}

Met Adobe Pass-verificatie worden tijdelijke gegevens opgeslagen, zoals browsercache, LSO&#39;s-cache en cookies. Het wissen van tijdelijke gegevens is belangrijk om ervoor te zorgen dat u bij het testen een schone lei krijgt.

- [De browsercache en cookies wissen](#clearing-the-browser-cache-and-cookies)
- [ het Schrappen LSOs Geheime voorgeheugen ](#clearing-lsos-cache)


## De browsercache en cookies wissen {#clearing-the-browser-cache-and-cookies}

Het is afhankelijk van de browser, maar in Firefox: &quot;Tools&quot; -\> &quot;Clear Recent History...&quot; -\> Op &quot;Time range to clear:&quot; selecteert u &quot;Alles&quot; en op &quot;Details&quot;: controleer de &quot;Cookies&quot; en &quot;Cache&quot; -\> Klik op &quot;Clear Now&quot;.


## LSO&#39;s-cache wissen {#clearing-lsos-cache}

Heb toegang tot de [ hulp van de Flash Player ](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Selecteer ```entitlement.\*``` (afhankelijk van wat het wordt getest) en klik &quot;Website van de Schrapping&quot;.


## Foutopsporingsgereedschappen {#tools}

Adobe Pass Authentication-engineers gebruiken de volgende tools voor foutopsporing:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (werkt met de foutopsporingsversie van de Flash Player)
- Filter - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
