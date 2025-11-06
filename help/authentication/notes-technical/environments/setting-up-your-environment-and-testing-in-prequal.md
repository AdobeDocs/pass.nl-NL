---
title: Uw omgeving instellen en testen in een proefversie
description: Uw omgeving instellen en testen in een proefversie
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Uw omgeving instellen en testen in een proefversie{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Het doel van deze technische notitie is onze partners te helpen hun omgeving op te zetten en te beginnen met het testen van een nieuwe build die is geïmplementeerd op de Adobe-voorkwalificatieomgeving.

Aangezien er twee bouwstijlen zijn: ***productie*** en ***het opvoeren***, in dit document zullen wij zich op de productieconfiguratie met de vermelding concentreren dat alle stappen het zelfde voor het opvoeren zijn, slechts zijn URLs verschillend.

Stap 1 en 2 zetten de testomgeving op een van de testmachines in, stap 3 is een verificatie van de basisstroom en stap 4 en 5 geven enkele testrichtsnoeren.

>[!IMPORTANT]
>
> Het is erg belangrijk om stap 1 en 2 uit te voeren telkens als u uw testomgeving wilt veranderen (van het opvoeren naar het productieprofiel of andersom)


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

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **op Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Domeinen die van het antwoord zijn uitgesloten omdat ze niet relevant zijn en van gebruiker tot gebruiker kunnen verschillen.

>[!IMPORTANT]
>
> Deze IP adressen kunnen in de toekomst veranderen en zij zouden niet het zelfde voor gebruikers in verschillende geografische gebieden kunnen zijn.


## STAP 2.  De prekwalificatie van de productieomgeving {#spoofing-the-prequalification-environment}

* Bewerk het *c:\\windows\\System32\\drivers\\etc\\hosts* dossier (in Vensters) of */etc/gastheren* dossier (op Macintosh/Linux/Android) en voeg het volgende toe:

* Profiel van steunkleuren
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**het Vlekken op Android:** om op Android te spoof, moet u een mededinger van Android gebruiken.

* Zodra spoofing op zijn plaats is, kunt u gewone URLs voor de productie en het opvoeren profielen eenvoudig gebruiken: (namelijk `http://sp.auth-staging.adobe.com` en `http://entitlement.auth-staging.adobe.com` en u zult eigenlijk het *pre-kwalificatiemilieu/ productie* van de* nieuwe bouwstijl raken.


## STAP 3.  Controleren of u naar de juiste omgeving wijst {#Verify-you-are-pointing-to-the-right-environment}

**dit is een gemakkelijke stap:**

* lading [ voorrecht milieu ](https://entitlement-prequal.auth.adobe.com/environment.html) en [ recht ](https://entitlement.auth.adobe.com/environment.html). Ze zouden dezelfde reactie moeten teruggeven.


## STAP 4.  Voer een eenvoudige authentificatie/vergunningsstroom uit gebruikend de website van de programmeur {#peform-a-simple-auth-flow}

* Deze stap vereist het de websiteadres van de programmeur en sommige geldige geloofsbrieven van MVPD (een gebruiker dat het voor authentiek en geautoriseerd is).

## STAP 5.  Scènetests uitvoeren met de websites van de programmeur {#perform-scenario-testing-using-programmer-website}

* Na de voltooiing van de milieu opstelling en het verzekeren dat de basisauthentificatie-vergunning stroom werkt kunt u met het testen van complexere scenario&#39;s te werk gaan.


## STAP 6.  Testen met de API-testsite {#perform-testing-using-api-testing-site}

* Als u dieper in het testen van de Authentificatie van Adobe Pass wilt gaan, adviseren wij u de [ API testplaats ](http://entitlement-prequal.auth.adobe.com/apitest/api.html) gebruiken.

U kunt meer details op API testplaats bij [ vinden hoe te om de stromen van de Authentificatie en van de Vergunning te testen gebruikend Adobe API testplaats ](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
