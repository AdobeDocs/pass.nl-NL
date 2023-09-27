---
title: Cookies-updates - vlaggen SameSite en Secure
description: Cookies-updates - vlaggen SameSite en Secure
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---

# Cookies-updates - vlaggen SameSite en Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


## Updates {#Updates}

In deze sectie worden de wijzigingen gemarkeerd die zijn aangebracht door de Chrome-browser en Adobe Pass-verificatie voor de verwerking van cookies van andere bedrijven.



### Chrome 80-updates {#Chrome}

Vanaf Chrome versie 80 (behalve versie 82), cookies die geen *SameSite* kenmerk wordt behandeld alsof het *SameSite=Lax*. Daarom moeten cookies die in een context naar andere sites moeten worden aangeleverd, expliciet de *SameSite=None*, en moet ook met de *Beveiligen* kenmerk en bezorgd over *HTTPS*. Meer informatie over deze updates is te vinden op de officiële chroompagina: <https://www.chromium.org/updates/same-site> en ook van <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass-verificatieupdates {#Pass-Updates}

De Adobe Pass Authentication-service is momenteel afhankelijk van een aantal cookies die vanuit browseroogpunt als cookies van derden worden beschouwd, waaronder Chrome, om te kunnen functioneren in combinatie met sommige platforms en versies van Adobe Pass Authentication SDK&#39;s. Om aan de komende wijzigingen te voldoen en deze cookies in een intersite context van deze oudere SDK&#39;s te blijven leveren, implementeert de Adobe Pass Authentication-service daarom de vereiste wijzigingen in de *adobe-pass-2.55.1* versie.

Deze wijzigingen zijn *adobe-pass-2.55.1* versie voegt *Beveiligen* en *SameSite=None* kenmerken voor alle cookies die worden doorgegeven aan alle Adobe Pass Authentication SDK&#39;s bij gebruik van Chrome-browsers vanaf versie 80 en hoger (behalve versie 82).

In de volgende sectie worden enkele mogelijke problemen weergegeven voor een lijst met platforms en versies van SDK&#39;s voor Adobe Pass-verificatie voor het geval één gebruiker Chrome-browser 80 en hoger gebruikt (behalve versie 82).

## Problemen oplossen {#Troubleshooting}

Houd er tijdens het bladeren door deze sectie rekening mee dat alle cookies van de Adobe Pass-verificatieservice *Beveiligen* kenmerkset in *adobe-pass-2.55.1* versie voor alle browsers, terwijl de *SameSite=None* moet alleen worden ingesteld voor Chrome-browsers versie 80 en hoger (behalve versie 82).


### Algemene probleemoplossing {#General}

1. Belangrijk om op te merken dat sommige gebruikersagenten gekend om onverenigbaar met zijn zijn te zijn *SameSite=None* kenmerk.

   - Versies van Chrome van Chrome 51 tot Chrome 66 (beide uiteinden inbegrepen). Deze Chrome-versies zullen een cookie met *SameSite=None*. Dit is ook van invloed op oudere versies van Chromium-afgeleide browsers en op Android WebView. Dit gedrag was correct volgens de versie van de koekjesspecificatie op dat ogenblik, maar met de toevoeging van de nieuwe waarde &quot;niets&quot;aan de specificatie, is dit gedrag bijgewerkt in Chrome 67 en nieuwer. (Vóór Chrome 51 werd het kenmerk SameSite volledig genegeerd en werden alle cookies behandeld alsof ze waren *SameSite=None*.)
   - Versies van UC-browser op Android voorafgaand aan versie 12.13.2. Oudere versies wijzen een cookie af met *SameSite=None*. Dit gedrag was correct volgens de versie van de koekjesspecificatie op dat ogenblik, maar met de toevoeging van de nieuwe waarde &quot;niets&quot;aan de specificatie, is dit gedrag bijgewerkt in nieuwere versies van Browser UC.
   - Versies van Safari en ingesloten browsers op MacOS 10.14 en alle browsers op iOS 12. Deze versies behandelen cookies die zijn gemarkeerd met *SameSite=None* alsof ze gemerkt zijn *SameSite=Strict*. Dit probleem is opgelost in nieuwere versies van iOS en MacOS.


1. Belangrijk om op te merken dat cookies de *Beveiligen* kenmerk moet worden verzonden over *HTTPS* anders bereikt de cookie de Adobe Pass Authentication Service niet.

   - JavaScript-SDK van AccessEnabler:
      - Verplichte dat de communicatie met *sp.auth.adobe.com* gebruik *HTTPS* voor versies *2,35* en *3.5.0.*, voordat u dynamische clientregistratie introduceert.
   - AccessEnabled iOS/tvOS SDK:
      - Verplichte dat de communicatie met *sp.auth.adobe.com* gebruik *HTTPS* voor versies ouder dan *3.0.0.*, voordat u dynamische clientregistratie introduceert.
   - AccessEnabler Android-SDK:
      - Verplichte dat de communicatie met *sp.auth.adobe.com* gebruik *HTTPS* voor versies ouder dan *3.0.0.*, voordat u dynamische clientregistratie introduceert.
   - AccessEnabled FireOS SDK:
      - Verplichte dat de communicatie met *sp.auth.adobe.com* gebruik *HTTPS* voor versie *2.0.4.*.

</br>

### AccessEnabler JavaScript SDK versie 2.35 Problemen oplossen {#235-Troubleshooting}

De verificatiestroom van de gebruiker kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om te verifiëren vanwege de bovenstaande updates, kunt u het volgende doen:

- Controleer of de *JSESSIONID* cookie wordt ingesteld in browser en heeft de *SameSite=None* en *Beveiligen* attributen set.
- Controleer of de *JSESSIONID* cookie van *https://sp.auth.adobe.com/authenticate/saml* netwerkverzoek komt overeen met *JSESSIONID* cookie van *https://sp.auth.adobe.com/session* netwerkverzoek.


### AccessEnabler JavaScript SDK versie 3.5.0 Problemen oplossen {#350-Troubleshooting}

De verificatiestroom van de gebruiker kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om te verifiëren vanwege de bovenstaande updates, kunt u het volgende doen:

- Controleer of de *JSESSIONID* cookie wordt ingesteld in browser en heeft de *SameSite=None* en *Beveiligen* attributen set.
- Controleer of de *JSESSIONID* cookie van *https://sp.auth.adobe.com/authenticate/saml* netwerkverzoek komt overeen met *JSESSIONID* cookie van *https://sp.auth.adobe.com/session* netwerkverzoek.
- Controleer of de *pass\_sfp* cookie wordt ingesteld in browser en heeft de *SameSite=None* en *Beveiligen* attributen set.
- Controleer of de *pass\_sfp* cookie ingesteld in *https://sp.auth.adobe.com/session* netwerkverzoek.


De machtigingsstroom van de gebruiker kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om naar een beveiligde bron te kijken nadat deze is geverifieerd, is het mogelijk om vanwege de bovenstaande updates:

- Controleer of de *pass\_sfp* cookie wordt ingesteld in browser en heeft de *SameSite=None* en *Beveiligen* attributen set.
- Controleer of de *pass\_sfp* cookie ingesteld in *https://sp.auth.adobe.com/adobe-services/authorize* netwerkverzoek.
- Controleer of de *pass\_sfp* cookie ingesteld in *https://sp.auth.adobe.com/adobe-services/shortAuthorize* netwerkverzoek.
