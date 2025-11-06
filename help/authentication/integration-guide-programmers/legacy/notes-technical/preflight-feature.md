---
title: Preflight-functie, Hoe kan ik het probleem inschakelen, oplossen of bepalen
description: Preflight-functie, Hoe kan ik het probleem inschakelen, oplossen of bepalen
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# (Verouderd) Preflight-functie: Het probleem inschakelen, oplossen of bepalen {#preflight-feature}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

Er is een wijziging opgetreden in de manier waarop Adobe Pass Authentication preAuthorizeResources berekent. De preAuthorization API heeft een nieuwe implementatie. Deze implementatie vervangt de oude oplossing, die inhoudt dat alleen meerdere toelatingseisen worden gesteld.
De externe interface voor de preAuthorization API is onveranderd, worden geen updates vereist in de toepassing van de Programmer.

Er zijn drie manieren waarop de Preflight-bronnen worden berekend:

* **Ver en sluit zich aan bij methode aan MVPD**: dit impliceert Adobe die veelvoudige vergunningsvraag aan MVPD (de cliënt zal nog één Preflight vraag moeten maken niettemin) maken.
* **Kanaal lijn-omhoog**: MVPD stelt de kanaallijn voor de het programma geopende gebruiker in de authentificatiereactie van SAML bloot en Adobe keert de erkende middelen terug die op dat worden gebaseerd. De SAML authN-respons in de SAML-tracer moet die lijst onthullen.
* **Multikanaalvergunning**: De cliënt &amp; de authentificatie van Adobe allebei maken één enkele vraag aan MVPD voor een reeks middelen.

Ongeacht MVPD, zal de toepassing van de Cliënt één enkele vraag aan het Preflight eindpunt (checkPreauthorisedResources API) maken, die een reeks resourceIDs overgaat. Op basis van een van de bovenstaande manieren die door MVPD worden ondersteund, retourneert Adobe de vooraf geautoriseerde resourceID&#39;s.

Als Preflight is gebaseerd op de vork &amp; sluit zich aan bij methode, dan controleert het achterste eind van de Authentificatie van Adobe Pass een waarde die voor de &quot;maximum pre-autorisatievraag&quot;in zijn configuratie wordt geplaatst. Dit is geconfigureerd door Adobe.

De standaardwaarde voor &#39;max prepermission call&#39; config is &#39;5&#39;, wat betekent dat bij max. 5 resources in Preflight voor de vork &amp; join MVPD&#39;s kunnen worden verzonden. Het overgaan van meer dan 5 middelen zal in een uitzondering resulteren en een ongeldige lijst zal zijn teruggekeerd. Dit is het verwachte gedrag. Wij kunnen dit aan om het even welke waarde vormen als de MVPD kanaallijn of multikanaalvergunning niet steunt, maar slechts na het raadplegen van hen aangezien de veelvoudige vork &amp; sluit zich aan bij vergunningsvraag hun ladingstijden zal verhogen.

Dit zijn dus de dingen die u moet zoeken wanneer u Preflight voor een MVPD inschakelt/oplost:

* De methode die de MVPD ondersteunt (vork &amp; join, channel line-up of multi-channel).
* Als alleen vork &amp; join wordt ondersteund, moet aan de programmeur worden gevraagd hoeveel resourceID&#39;s het in de Preflight-aanroep zal verzenden.
* De MVPD moet worden geraadpleegd en moet weten wat de gevolgen zijn van het maken van een &#39;n&#39; aantal aanvragen voor vergunningen voor fork &amp; join. Daarna, moet de waarde in config worden gevormd als het groter is dan 5.

**Beperking**

Merk op dat wij geen resourceID terug van de Preflight vraag voor sommige MVPDs zoals AT&amp;T &amp; TWC krijgen als om het even welke resourceIDs vals identiteitskaart of een niet erkende identiteitskaart in de lijst van resourceIDs is die zij in de preflight vraag verzenden hoewel wij geldige &amp; gemachtigde middelen in die lijst ook hebben.
