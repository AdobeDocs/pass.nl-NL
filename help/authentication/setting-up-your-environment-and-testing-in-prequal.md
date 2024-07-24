---
title: Uw omgeving instellen en testen in een proefversie
description: Uw omgeving instellen en testen in een proefversie
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 3a6a5633c728398a3847ee3e341e82aba915f0d9
workflow-type: tm+mt
source-wordcount: '477'
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

* **op Vensters**

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
   * 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**het Vlekken op Android:** om op Android te spoof, moet u een mededinger van Android gebruiken.

* Zodra spoofing op zijn plaats is, kunt u gewone URLs voor de productie en het opvoeren profielen eenvoudig gebruiken: (namelijk `http://sp.auth-staging.adobe.com` en `http://entitlement.auth-staging.adobe.com` en u zult eigenlijk het *pre-kwalificatiemilieu/ productie* van de* nieuwe bouwstijl raken.


## STAP 3.  Controleren of u naar de juiste omgeving wijst {#Verify-you-are-pointing-to-the-right-environment}

**Dit is een eenvoudige stap:**

* prequale omgeving](https://entitlement-prequal.auth.adobe.com/environment.html) en [machtiging](https://entitlement.auth.adobe.com/environment.html) laden[. Ze moeten hetzelfde antwoord retourneren.


## STAP 4.  Voer een eenvoudige workflow voor verificatie/autorisatie uit via de website van de programmeur {#peform-a-simple-auth-flow}

* Deze stap vereist het websiteadres van de programmeur en enkele geldige MVPD-referenties (een gebruiker die is geverifieerd en geautoriseerd).

## STAP 5.  Scenariotests uitvoeren op de websites van de programmeur {#perform-scenario-testing-using-programmer-website}

* Nadat u de configuratie van de omgeving hebt voltooid en ervoor hebt gezorgd dat de standaardworkflow voor verificatie-autorisatie werkt, kunt u doorgaan met het testen van complexere scenario&#39;s.


## STAP 6.  Testen met de API-testsite {#perform-testing-using-api-testing-site}

* Als u dieper in het testen van de Authentificatie van Adobe Pass wilt gaan, adviseren wij u de [ API testplaats ](http://entitlement-prequal.auth.adobe.com/apitest/api.html) gebruiken.

U kunt meer details op API testplaats bij [ vinden hoe te om de stromen van de Authentificatie en van de Toestemming te testen gebruikend de de testplaats van API van de Adobe ](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
