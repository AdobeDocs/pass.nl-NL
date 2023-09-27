---
title: Bijlage B "Tips voor foutopsporing"
description: Bijlage B "Tips voor foutopsporing"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Bijlage B: Tips voor foutopsporing {#appendix-b-debugging-tips}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Tijdelijke gegevens wissen {#clearing-temporary-data}

Met Adobe Pass-verificatie worden tijdelijke gegevens opgeslagen, zoals browsercache, LSO&#39;s-cache en cookies. Het wissen van tijdelijke gegevens is belangrijk om ervoor te zorgen dat u bij het testen een schone lei krijgt.

- [De browsercache en cookies wissen](#clearing-the-browser-cache-and-cookies)
- [LSO&#39;s-cache wissen](#clearing-lsos-cache)


## De browsercache en cookies wissen {#clearing-the-browser-cache-and-cookies}

Het is afhankelijk van de browser, maar in Firefox: &quot;Tools&quot; -\> &quot;Clear Recent History...&quot; -\> Op &quot;Time range to clear:&quot; selecteert u &quot;Alles&quot; en op &quot;Details&quot;: controleer de &quot;Cookies&quot; en &quot;Cache&quot; -\> Klik op &quot;Clear Now&quot;.


## LSO&#39;s-cache wissen {#clearing-lsos-cache}

Toegang krijgen tot de [Flash Player Help](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Selecteer de ```entitlement.\*``` (afhankelijk van wat er wordt getest) en klik op Website verwijderen.


## Foutopsporingsgereedschappen {#tools}

Adobe Pass Authentication-engineers gebruiken de volgende tools voor foutopsporing:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (werkt met de foutopsporingsversie van de Flash Player)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
