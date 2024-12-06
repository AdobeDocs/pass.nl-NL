---
title: Cookies-updates - vlaggen SameSite en Secure
description: Cookies-updates - vlaggen SameSite en Secure
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookies-updates - vlaggen SameSite en Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


## Updates {#Updates}

In deze sectie worden de wijzigingen gemarkeerd die zijn aangebracht door de Chrome-browser en Adobe Pass-verificatie voor de verwerking van cookies van andere leveranciers.



### Chrome 80-updates {#Chrome}

Beginnend met versie 80 van Chrome (behalve versie 82), de koekjes die a *SameSite* attributen niet specificeren zullen worden behandeld alsof zij *SameSite=Lax* waren. Daarom moeten de koekjes die in een dwars-plaats context moeten worden geleverd uitdrukkelijk *SameSite=None* specificeren, en moeten ook met het *Veilige* attribuut worden gemerkt en over *HTTPS* worden geleverd. Meer informatie over deze updates vindt u op de officiële chroompagina: <https://www.chromium.org/updates/same-site> en ook vanuit <https://web.dev/samesite-cookies-explained/> .


### Adobe Pass-verificatieupdates {#Pass-Updates}

Adobe Pass Authentication Service is momenteel afhankelijk van een aantal cookies die vanuit browseroogpunt als cookies van derden worden beschouwd, waaronder Chrome, om te kunnen functioneren in combinatie met sommige platforms en versies van Adobe Pass Authentication SDK&#39;s. Daarom om aan de aanstaande veranderingen te voldoen en deze koekjes in een dwars-plaats context van deze oudere SDKs te blijven leveren, voert de dienst van de Authentificatie van Adobe Pass de vereiste veranderingen in *adobe-pas-2.55.1* versie uit.

Deze veranderingen van *adobe-pas-2.55.1* versie impliceren het toevoegen van *Veilig* en *SameSite=None* attributen voor al zijn koekjes die terug naar alle Authentificatie SDKs van Adobe Pass wanneer het gebruiken van de browsers van Chrome die met versie 80 en hierboven (behalve versie 82) beginnen.

In de volgende sectie worden enkele mogelijke problemen weergegeven voor een lijst met platforms en versies van SDK&#39;s voor Adobe Pass-verificatie voor het geval één gebruiker Chrome-browser 80 en hoger gebruikt (behalve versie 82).

## Problemen oplossen {#Troubleshooting}

Terwijl het doorbladeren door deze sectie houdt in mening dat alle de dienstkoekjes van de Authentificatie van Adobe Pass *Veilige* attributen moeten hebben die in *worden geplaatst adobe-pas-2.55.1* versie voor alle browsers, terwijl het ** attribuut SameSite=None slechts voor de browsers van Chrome versie 80 en hierboven (behalve versie 82) wordt geplaatst.


### Algemene probleemoplossing {#General}

1. Belangrijk om op te merken dat sommige gebruikersagenten gekend om met het *SameSite=None* attribuut onverenigbaar zijn.

   - Versies van Chrome van Chrome 51 tot Chrome 66 (beide inbegrepen). Deze versies van Chrome zullen een koekje met *SameSite=None* verwerpen. Dit beïnvloedt ook oudere versies van Chromium-Afgeleide browsers, evenals Android WebView. Dit gedrag was correct volgens de versie van de koekjesspecificatie op dat ogenblik, maar met de toevoeging van de nieuwe waarde &quot;niets&quot;aan de specificatie, is dit gedrag bijgewerkt in Chrome 67 en nieuwer. (Voorafgaand aan Chrome 51, werd het attribuut SameSite volledig genegeerd en alle koekjes werden behandeld alsof zij *SameSite=None* waren.)
   - Versies van UC Browser op Android voorafgaand aan versie 12.13.2. Oudere versies zullen een koekje met *SameSite=None* verwerpen. Dit gedrag was correct volgens de versie van de koekjesspecificatie op dat ogenblik, maar met de toevoeging van de nieuwe waarde &quot;niets&quot;aan de specificatie, is dit gedrag bijgewerkt in nieuwere versies van Browser UC.
   - Versies van Safari en ingesloten browsers op MacOS 10.14 en alle browsers op iOS 12. Deze versies zullen ten onrechte koekjes behandelen duidelijk met *SameSite=None* alsof zij *SameSite=Strict* duidelijk waren. Dit probleem is opgelost in nieuwere versies van iOS en MacOS.


1. Belangrijk om op te merken dat de koekjes die het *Veilige* attribuut hebben over *HTTPS* moeten worden verzonden, anders zal het koekje de dienst van de Authentificatie van Adobe Pass niet bereiken.

   - AccessEnabled JavaScript SDK:
      - Verplicht dat de mededeling met *sp.auth.adobe.com* gebruik *HTTPS* voor versies *2.35* en *3.5.0*, alvorens de Dynamische Registratie van de Cliënt in te voeren.
   - AccessEnabled iOS/tvOS SDK:
      - Verplicht dat de mededeling met *sp.auth.adobe.com* gebruik *HTTPS* voor versies voorafgaand aan *3.0.0*, alvorens de Dynamische Registratie van de Cliënt in te voeren.
   - AccessEnabled Android SDK:
      - Verplicht dat de mededeling met *sp.auth.adobe.com* gebruik *HTTPS* voor versies voorafgaand aan *3.0.0*, alvorens de Dynamische Registratie van de Cliënt in te voeren.
   - AccessEnabled FireOS SDK:
      - Verplicht dat de mededeling met *sp.auth.adobe.com* gebruik *HTTPS* voor versie *2.0.4*.

</br>

### AccessEnabler JavaScript SDK versie 2.35 Problemen oplossen {#235-Troubleshooting}

De verificatiestroom van gebruikers kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om te verifiëren vanwege de bovenstaande updates, kunt u het volgende doen:

- Controleer dat het ** koekje JSESSIONID in browser wordt geplaatst en hebbend *SameSite=None* en *Veilige* geplaatste attributen.
- Controleer dat het ** koekje JSESSIONID van het *https://sp.auth.adobe.com/authenticate/saml* netwerkverzoek het ** koekje JSESSIONID van het *https://sp.auth.adobe.com/session* netwerkverzoek aanpast.


### AccessEnabler JavaScript SDK versie 3.5.0 Problemen oplossen {#350-Troubleshooting}

De verificatiestroom van gebruikers kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om te verifiëren vanwege de bovenstaande updates, kunt u het volgende doen:

- Controleer dat het ** koekje JSESSIONID in browser wordt geplaatst en hebbend *SameSite=None* en *Veilige* geplaatste attributen.
- Controleer dat het ** koekje JSESSIONID van het *https://sp.auth.adobe.com/authenticate/saml* netwerkverzoek het ** koekje JSESSIONID van het *https://sp.auth.adobe.com/session* netwerkverzoek aanpast.
- Controleer dat het *pas \_sfp* koekje in browser wordt geplaatst en hebbend *SameSite=None* en *Veilige* geplaatste attributen.
- Controleer dat het *pas \_sfp* koekje in *https://sp.auth.adobe.com/session* netwerkverzoek wordt geplaatst.


De autorisatiestroom van gebruikers kan worden beïnvloed in Chrome 80 en hoger (behalve versie 82). Om ervoor te zorgen dat de gebruiker geen problemen heeft om naar een beveiligde bron te kijken nadat deze is geverifieerd, is het mogelijk om vanwege de bovenstaande updates:

- Controleer dat het *pas \_sfp* koekje in browser wordt geplaatst en hebbend *SameSite=None* en *Veilige* geplaatste attributen.
- Controleer dat het *pas \_sfp* koekje in *https://sp.auth.adobe.com/adobe-services/authorize* netwerkverzoek wordt geplaatst.
- Controleer dat het *pas \_sfp* koekje in *https://sp.auth.adobe.com/adobe-services/shortAuthorize* netwerkverzoek wordt geplaatst.
