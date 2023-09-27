---
title: Uw omgeving instellen en testen in een proefversie
description: Uw omgeving instellen en testen in een proefversie
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---

# Uw omgeving instellen en testen in Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>De inhoud op deze pagina is uitsluitend ter informatie beschikbaar. Voor het gebruik van deze API is een actuele licentie van Adobe vereist. Onbevoegd gebruik is niet toegestaan.

Het doel van deze technische opmerking is om onze partners te helpen hun omgeving in te stellen en een nieuwe build te testen die wordt gedistribueerd in de pre-kwalificatieomgeving van Adobe.

Omdat er twee samenstellingssmaken zijn: ***productie*** en ***entiëring***, zullen we ons in dit document concentreren op de productie-instelling met de vermelding dat alle stappen hetzelfde zijn voor het voorbereiden, alleen de URL&#39;s zijn verschillend.

Stap 1 en 2 stellen de testomgeving in op een van de testcomputers, stap 3 is een verificatie van de basisstroom en stap 4 &amp; 5 presenteren enkele testrichtlijnen.

>[!IMPORTANT]
>
> Het is erg belangrijk om telkens stap 1 en 2 uit te voeren wanneer u van testomgeving wilt veranderen (van fasering naar productieprofiel of andersom)


## STAP 1. Het oplossen van het domein van de pas aan IP {#resolving-pass-domain-to-an-ip}

* Voer de volgende opdracht uit om een IP van het taakverdelingsmechanisme te zoeken die voor spoofing kan worden gebruikt:

* **Op Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **Op Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>Domeinen die zijn uitgesloten van het antwoord omdat ze niet relevant zijn en per gebruiker kunnen verschillen.

>[!IMPORTANT]
>
> Deze IP-adressen kunnen in de toekomst veranderen en mogelijk zijn ze niet hetzelfde voor gebruikers in verschillende geografische regio&#39;s.


## STAP 2.  Spoofing van de pre-kwalificatie omgeving productie {#spoofing-the-prequalification-environment}

* Bewerk het *c:\\windows\\System32\\drivers\\etc\hosts-bestand* (Windows) of */etc/hosts-bestand* (op Macintosh/Linux/Android) en voeg het volgende toe:

* Spoof-productieprofiel
   * 52.13.71.11 http://entitlement.auth.adobe.com, http://sp.auth.adobe.com http://api.auth.adobe.com

**Gesproken tekst op Android:** Als u wilt dat Android wordt gebruikt, moet u een Android-emulator gebruiken.

* Als de spoofing eenmaal is geïnstalleerd, kunt u gewoon de normale URL&#39;s gebruiken voor de productie- en staging-profielen: (dat wil zeggen `http://sp.auth-staging.adobe.com` en `http://entitlement.auth-staging.adobe.com` en je raakt de *voorkwalificatieomgeving/productie* van de* nieuwe build.


## STAP 3.  Controleren of u naar de juiste omgeving wijst {#Verify-you-are-pointing-to-the-right-environment}

**Dit is een eenvoudige stap:**

* prequale omgeving](https://entitlement-prequal.auth.adobe.com/environment.html) en [machtiging](https://entitlement.auth.adobe.com/environment.html) laden[. Ze moeten hetzelfde antwoord retourneren.


## STAP 4.  Voer een eenvoudige workflow voor verificatie/autorisatie uit via de website van de programmeur {#peform-a-simple-auth-flow}

* Deze stap vereist het websiteadres van de programmeur en enkele geldige MVPD-referenties (een gebruiker die is geverifieerd en geautoriseerd).

## STAP 5.  Scenariotests uitvoeren op de websites van de programmeur {#perform-scenario-testing-using-programmer-website}

* Nadat u de configuratie van de omgeving hebt voltooid en ervoor hebt gezorgd dat de standaardworkflow voor verificatie-autorisatie werkt, kunt u doorgaan met het testen van complexere scenario&#39;s.


## STAP 6.  Testen met de API-testsite {#perform-testing-using-api-testing-site}

* Als je meer wilt weten over het testen van Adobe Pass Authentication, raden we je aan de [API-testsite](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Meer informatie vindt u op de API-testsite op [Verificatie- en autorisatiestromen testen met de API-testsite van de Adobe](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
