---
title: WKWebView-ondersteuning op iOS SDK 3.1+
description: WKWebView-ondersteuning op iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# (Verouderd) WKWebView-ondersteuning op iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

**wegens Apple die UIWebView op iOS afleidt, hebben wij iOS SDK 3.1 met steun voor WKWebView bijgewerkt.**

## Compatibiliteit {#compatibility}

Vanaf iOS SDK versie 3.1 kunnen implementoren nu onderling WKWebView of UIWebView gebruiken. Aangezien UIWebView door Apple wordt afgekeurd, zouden apps aan WKWebView moeten migreren, om kwesties met toekomstige versies van iOS te vermijden.

Merk op dat de migratie eenvoudig zou impliceren omschakelend de klasse UIWebView met WKWebView, is er geen specifiek werk dat met betrekking tot Adobe AccessEnabler moet worden gedaan.

## Bekende problemen {#known-issues}

Adobe AccessEnabler gebruikte een verborgen interne instantie UIWebView om &quot;[ passieve authentificatie ](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)&quot;voor bepaalde MVPDs uit te voeren. De &quot;passieve&quot;stroom was nuttig voor MVPDs die authentificatie voor elke aanvrager identiteitskaart vereisen, en van deze stroom profiteerde die Programmers die zelfde teamidentiteitskaart over veelvoudige toepassingen van iOS gebruikten om een ervaring SSO (Adobe SSO) te simuleren. Deze functie wordt momenteel gebruikt door een beperkt aantal MVPD&#39;s.

De eigenschap gebruikte een gedrag van UIWebView die Adobe toestond om de authentificatiecookies te vangen en hen tijdens de &quot;passieve&quot;stroom opnieuw te spelen. WKWebView introduceert sterkere veiligheid die Adobe verhindert om de koekjes te vangen die bij login worden geplaatst en hen opnieuw te spelen gebruikend een verborgen geval van WKWebView. Wegens deze veiligheidsverbetering en overwegende dat de &quot;passieve&quot;stroom slechts een zeer beperkte reeks MVPDs in een zeer specifiek implementatiescenario (veelvoudige toepassingen die zelfde teamidentiteitskaart gebruiken) bevoordeelde, schrapte de Adobe de &quot;passieve authentificatieeigenschap&quot;voor MVPDs gebruikend webviews voor authentiek verklaren.

De eigenschap is nog aanwezig voor MVPDs die wordt gevormd om SFSafariViewController te gebruiken, maar merk op dat in dit geval de &quot;passieve&quot;authentificatie aan de gebruiker zichtbaar zal zijn aangezien SFSafariViewController niet op een &quot;verborgen&quot;manier kan worden gebruikt.
