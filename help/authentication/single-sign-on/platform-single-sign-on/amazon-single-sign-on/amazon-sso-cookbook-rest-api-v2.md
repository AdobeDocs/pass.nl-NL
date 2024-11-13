---
title: Amazon SSO Cookbook (REST API V2)
description: Amazon SSO Cookbook (REST API V2)
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Amazon SSO Cookbook (REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De Adobe Pass Authentication REST API V2 biedt ondersteuning voor Platform Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op FireOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan het bestaande [ REST API V2 Overzicht ](/help/authentication/rest-api-v2/rest-api-v2-overview.md) dat een mening op hoog niveau en het document verstrekt dat beschrijft hoe te om [ Enige sign-on uit te voeren gebruikend de stromen van de platformidentiteit ](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Amazon Single Sign-On met identiteitsstromen van platforms {#cookbook}

### Vereisten {#prerequisites}

Voordat u verdergaat met de Amazon Single Sign-On met behulp van platformidentiteitsstromen, moet u controleren of aan de volgende voorwaarden is voldaan.

#### Amazon SSO SDK integreren {#integrate-amazon-sso-sdk}

De het stromen toepassing moet de ](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) bibliotheek van 0} Amazon SSO SDK {voor Enige Sign-On (SSO) in zijn bouwstijl integreren.[

* Download en kopieer de nieuwste Amazon SSO SDK-bibliotheek naar een `/SSOEnabler` -map die parallel loopt met de map van de toepassing.

* Werk de manifest en de Gradle dossiers bij om de bibliotheek van SDK van Amazon te gebruiken SSO.

  **Manifest:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Onder opslagplaatsen:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Onder afhankelijkheden:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Amazon SSO SDK gebruiken {#use-amazon-sso-sdk}

De streamingtoepassing moet de Amazon SSO SDK gebruiken om de SSO-token (platformidentiteit) te verkrijgen.

De SDK van Amazon SSO biedt zowel synchrone als asynchrone API&#39;s om de payload van het SSO-token (platform identity) te verkrijgen.

De streamingtoepassing kan op basis van de architectuur een van de twee opties kiezen.

##### Asynchrone API&#39;s

* Hiermee wordt de instantie `SSOEnabler` opgehaald en ingesteld: `SSOEnablerCallback`

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Dit kan worden gedaan tijdens de initialisering van de het stromen toepassing.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  De SSO symbolische bundel van de succesreactie zal bevatten:
   * Een SSO-token als een `string` met de sleutel &quot;SSOToken&quot;.

  <br/>

  De SSO-bundel voor de respons op een token bevat:
   * Een foutcode als een `int` met de sleutel ErrorCode.
   * Een foutbeschrijving als een `string` met de sleutel &quot;ErrorDescription&quot;.

  <br/>

* Haal het SSO-token op:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Deze API zal de reactie via callback plaatsen tijdens de initialisering verstrekken.

##### Synchrone API&#39;s

* Hiermee wordt de instantie `SSOEnabler` opgehaald:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Haal het SSO-token op:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Deze API zal de bezoekerdraad blokkeren en met de resultaatbundel antwoorden. Aangezien dit een synchrone vraag is, ben zeker om het niet in uw belangrijkste draad te gebruiken.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Deze API zal de onderbrekingswaarde voor de synchrone vraag plaatsen. De standaardtime-outwaarde is 1 minuut.

#### Extra back-up voor Amazon SSO {#fallback-amazon-sso}

De streamingtoepassing moet terugvalscenario&#39;s van de Amazon SSO-stroom naar de normale verificatiestroom afhandelen.

Zorg ervoor dat de streamingtoepassing de volgende handelingen uitvoert:

* De afwezigheid van de bij Amazon behorende toepassing die op het Amazon-apparaat moet worden uitgevoerd.
   * De streamingtoepassing kan een `ClassNotFoundException` bij uitvoering tegenkomen voor de volgende klasse `com.amazon.ottssotokenlib.SSOEnabler` .

* Het ontbreken van de SSO-token (platform identity) lading die door de bovenstaande API&#39;s moet worden geretourneerd.
   * De streamingtoepassing kan contact opnemen met de vertegenwoordigers van Amazon en de Adobe om dit te onderzoeken.

### Workflow {#workflow}

De payload van het Amazon SSO-token (platform identity) moet aanwezig zijn op alle HTTP-aanvragen die zijn ingediend tegen de eindpunten van de Adobe Pass Authentication REST API V2:

```
/api/v2/*
```

Adobe Pass Authentication REST API V2 ondersteunt de volgende methoden voor het ontvangen van de SSO token (platform identity) payload die een apparaat-scoped of platform-scoped identifier is:

* Als een header met de naam: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

#### Voorbeelden

**die als kopbal** verzendt

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Als de headerwaarde van `Adobe-Subject-Token` ontbreekt of ongeldig is, zal Adobe Pass Authentication de aanvragen verwerken zonder dat Single Sign-On hiermee rekening wordt gehouden.
