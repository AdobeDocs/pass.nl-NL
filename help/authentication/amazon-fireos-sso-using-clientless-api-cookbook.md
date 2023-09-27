---
title: Amazon FireOS SSO met Cookbook zonder client
description: Amazon FireOS SSO met Cookbook zonder client
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Amazon FireOS SSO met Cookbook zonder client {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Inleiding {#Introduction}

Dit document bevat instructies voor het implementeren van de Amazon SSO-versie van de Adobe Pass Authentication-flow met behulp van clientless API. Het eerste deel van dit document richt zich op het specifieke karakter van de Amazon-versie van de architectuur, voor de vele partners die reeds vertrouwd en ervaren zijn met de implementatie ervan.

Het tweede deel van het document gaat over de belangrijkste stappen voor het implementeren van de Adobe Pass Authentication clientless API.

Voor een breed technisch overzicht van hoe de oplossing zonder clips werkt, raadpleegt u de [REST API-overzicht](/help/authentication/rest-api-overview.md). Adobe is het aangewezen contact voor steun over de algemene architectuur en eerste implementaties.

## Amazon Clientless SSO {#AMZ-Clientless-SSO}

### Architectuur op hoog niveau {#High-Level-Arch}

De Amazon clientless SSO-implementatie is eenvoudig en grotendeels identiek aan de gewone Adobe Primetime Authentication Client less API&#39;s.

U zult SDK van Amazon moeten gebruiken om een gepersonaliseerde lading terug te winnen en het te gebruiken wanneer het roepen van Adobe client-APIs.

Als de lading wordt erkend en aan een voor authentiek verklaarde zitting beantwoordt, zullen clientless APIs onmiddellijk met het teken van uw zitting terugkeren.

### De toepassing maken voor gebruik van Amazon SDK {#Build-entries}

* Download en kopieer de nieuwste [Amazon Stub SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) in een /SSOEnabler-map parallel aan de toepassingsmap
* Manifest-/grijswaardenbestanden bijwerken voor gebruik van de bibliotheek:

  **Voeg de volgende regel toe aan het Manifest-bestand:**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Invoer van het verloopbestand:**

  Onder opslagplaatsen:

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  Onder gebiedsdelen, voeg toe:

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* De afwezigheid van de bij Amazon behorende app afhandelen:

  Mocht de aanvulling waarschijnlijk niet aanwezig zijn op het Amazon-apparaat waarop uw toepassing wordt uitgevoerd, dan komt u bij uitvoering een ClassNotFoundException tegen voor de volgende klasse: `com.amazon.ottssotokenlib.SSOEnabler`.

  Mocht dit gebeuren, dan hoeft u alleen maar de payload-stap over te slaan en terug te vallen op de normale PrimeTime-flow. SSO zal niet worden toegelaten maar de regelmatige autestroom zal normaal gebeuren.

</br>

### Amazon SSO-payload verkrijgen met Amazon SDK {#UseAmazonSSO}

Haal tijdens de initialisatie van uw toepassing een instantie van SSOEnabler op. Gebaseerd op uw toepassingsarchitectuur, zou u tussen een synchrone of asynchrone implementatie moeten beslissen.

Als om het even welke reden, de API vraag geen lading terugkeert, gelieve de regelmatige stroom niet-SSO te gebruiken en uw Amazon en Adobe partners te contacteren om te onderzoeken.

**Asynchrone API**

* Inschakelen van SSO ophalen:

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* De callback instellen

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * De bundel voor succesvolle respons bevat:
      * SSO-token als een tekenreeks met de sleutel &quot;SSOToken&quot;
   * Storing-responsbundel bevat:
      * foutcode als int met sleutel &quot;ErrorCode&quot;
      * foutbeschrijving als een tekenreeks met de sleutel ErrorDescription


* SSO-token ophalen

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* Deze API zal de reactie via callback plaatsen tijdens de init verstrekken.

  **Ex**. oproep met singleton-instantie die tijdens init is gemaakt:

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**Synchrone API&#39;s**

* De instantie SSO Enabler ophalen en de callback instellen

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* SSO-token ophalen

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * Deze API zal de bezoekerdraad blokkeren en met de resultaatbundel antwoorden. Aangezien dit een synchrone vraag is, ben zeker om het niet in uw belangrijkste draad te gebruiken.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * Waarde in milliseconden. Indien ingesteld, overschrijft u de standaardwaarde van 1 minuut voor de synchronisatie-API.


### Adobe Pass Clientless API-update voor gebruik van Dynamic Client Registration {#clientlessdcr}

Als dit uw eerste implementatie is, raadpleegt u **Klantloos technisch overzicht** en neem contact op met de Adobe voor het geval u ondersteuning nodig hebt.

De Adobe Clientless API vereist toepassingen om Dynamische Registratie van de Cliënt te gebruiken om vraag aan de servers van de Adobe te maken.

* Volg de instructies in [Dynamic Client Registration Management om de toepassing te registreren](/help/authentication/dynamic-client-registration-management.md).

* Volg de instructies in [Dynamische API voor clientregistratie](/help/authentication/dynamic-client-registration-api.md) .

### Adobe Pass Clientless API-update voor gebruik van Amazon SSO {#clientlesssso}

Amazon SSO-lading die is verkregen van Amazon SDK moet aanwezig zijn op aanvragen die zijn ingediend bij Adobe Pass Authentication endpoints:

```
      /adobe-services/*
      /reggie/*
      /api/*
```


Alle eindpunten van de Verificatie van Adobe Pass steunen de volgende methodes om het apparaat-scoped herkenningsteken of Platform-Scoped herkenningsteken (huidig in de nuttige lading van SSO van Amazon) te ontvangen:

* Als koptekst: &quot;Adobe-Subject-Token&quot;
* Als queryparameter : &quot;ast&quot;
* Als parameter post : &quot;ast&quot;


>[!NOTE]
>
>Als de apparaat-scoped herkenningsteken of Platform-scoped herkenningsteken als vraag/post parameter wordt verzonden, dan moet het worden omvat wanneer het produceren van de verzoekhandtekening.

>[!NOTE]
>
>Gebruikend vraagparameter &quot;ast&quot;, kan volledige url zeer lang worden en verworpen. Op /authenticate vraag, kan deze parameter worden overgeslagen aangezien het bij /regcode vraag werd verstrekt

**Voorbeelden:**

**Verzenden als aangepaste koptekst**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Verzenden als queryparameter**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**Verzenden als parameter post**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Als Amazon SSO niet aanwezig of ongeldig is, zal de Authentificatie van Adobe Pass de attributen negeren en de vraag zal worden uitgevoerd alsof SSO niet aanwezig is.
