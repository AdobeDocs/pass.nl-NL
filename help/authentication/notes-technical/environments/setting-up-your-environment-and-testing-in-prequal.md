---
title: Uw omgeving instellen en testen in een proefversie
description: Uw omgeving instellen en testen in een proefversie
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Het inrichten van je omgeving en testen in Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>De inhoud van deze pagina is uitsluitend bedoeld voor informatieve doeleinden. Voor het gebruik van deze API is een actuele licentie van Adobe vereist. Ongeoorloofd gebruik is niet toegestaan.

Het doel van deze technische notitie is om onze partners te helpen bij het instellen van hun omgeving en om te beginnen met het testen van een nieuwe build die is geïmplementeerd in de prekwalificatieomgeving van Adobe.

Aangezien er twee build-smaken zijn: ***productie*** en ***staging***, zullen we ons in dit document concentreren op de productie-instelling met de vermelding dat alle stappen hetzelfde zijn voor staging, alleen de URL&#39;s zijn anders.

Stap 1 en 2 zijn het inrichten van de testomgeving op een van de testmachines, stap 3 is een verificatie van de basisflow en stap 4 &amp; 5 geven een aantal testrichtlijnen.

>[!IMPORTANT]
>
> Het is erg belangrijk om stap 1 en 2 uit te voeren telkens wanneer u uw testomgeving wilt wijzigen (overschakelen van staging- naar productieprofiel of andersom)


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


* **Op Linux/Mac**

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
>Domeinen die niet worden beantwoord omdat ze niet relevant zijn en van gebruiker tot gebruiker kunnen verschillen.

>[!IMPORTANT]
>
> Deze IP-adressen kunnen in de toekomst veranderen en zijn mogelijk niet hetzelfde voor gebruikers in verschillende geografische regio&#39;s.


## STAP 2.  Spoofing van de pre-kwalificatieomgeving om productie te zijn {#spoofing-the-prequalification-environment}

* Bewerk het *bestand c:\\windows\\System32\\drivers\\etc\\hosts* (in Windows) of */etc/hosts* bestand (op Macintosh/Linux/Android) en voeg het volgende toe:

* Spoof productieprofiel
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.19.212.171 entitlement.auth.adobe.com

**het Vlekken op Android:** om op Android te spoof, moet u een mededinger van Android gebruiken.

* Zodra spoofing op zijn plaats is, kunt u gewone URLs voor de productie en het opvoeren profielen eenvoudig gebruiken: (namelijk `http://sp.auth-staging.adobe.com` en `http://entitlement.auth-staging.adobe.com` en u zult eigenlijk het *pre-kwalificatiemilieu/ productie* van de* nieuwe bouwstijl raken.


## STAP 3.  Controleer of u naar de juiste omgeving verwijst {#Verify-you-are-pointing-to-the-right-environment}

**Dit is een eenvoudige stap:**

* Laadrecht [prequal omgeving](https://entitlement-prequal.auth.adobe.com/environment.html) en [rechten](https://entitlement.auth.adobe.com/environment.html). Ze zouden hetzelfde antwoord moeten teruggeven.


## STAP 4.  Voer een eenvoudige authenticatie-/autorisatiestroom uit met behulp van de website van de programmeur {#peform-a-simple-auth-flow}

* Deze stap vereist het websiteadres van de programmeur en enkele geldige MVPD-inloggegevens (een gebruiker die is geverifieerd en geautoriseerd).

## STAP 5.  Scènetests uitvoeren met de websites van de programmeur {#perform-scenario-testing-using-programmer-website}

* Na de voltooiing van de milieu opstelling en het verzekeren dat de basisauthentificatie-vergunning stroom werkt kunt u met het testen van complexere scenario&#39;s te werk gaan.


## STAP 6.  Testen met de API-testsite {#perform-testing-using-api-testing-site}

* Als u dieper in het testen van de Authentificatie van Adobe Pass wilt gaan, adviseren wij u de [ API testplaats ](http://entitlement-prequal.auth.adobe.com/apitest/api.html) gebruiken.

Meer informatie over de API-testsite vindt u op [Verificatie- en autorisatiestromen testen met behulp van de API-testsite](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md) van Adobe.
