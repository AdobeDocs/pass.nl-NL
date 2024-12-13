---
title: Amazon SSO Cookbook (REST API V1)
description: Amazon SSO Cookbook (REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---

# (Verouderd) Amazon SSO Cookbook (REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De Adobe Pass Authentication REST API V1 biedt ondersteuning voor Platform Single Sign-On (SSO) voor eindgebruikers van clienttoepassingen die op FireOS worden uitgevoerd.

Dit document doet dienst als uitbreiding aan het bestaande [ REST API V1 Overzicht ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md) dat een mening op hoog niveau verstrekt.

## Amazon Single Sign-On met identiteitsstromen van platforms {#cookbook}

### Vereisten {#prerequisites}

Voordat u verdergaat met de Amazon Single Sign-On met behulp van platformidentiteitsstromen, moet u controleren of aan de volgende voorwaarden is voldaan.

#### Amazon SSO SDK integreren {#integrate-amazon-sso-sdk}

De het stromen toepassing moet de [ Amazon SSO SDK ](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) bibliotheek voor Enige Sign-On (SSO) in zijn bouwstijl integreren.

* Download en kopieer de nieuwste Amazon SSO SDK-bibliotheek naar een `/SSOEnabler` -map parallel aan de map van de toepassing.

* Werk de manifest- en verloopbestanden bij voor gebruik van de Amazon SSO SDK-bibliotheek.

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

De streamingtoepassing moet de Amazon SSO SDK gebruiken om de payload van het SSO-token (platform identity) te verkrijgen.

De Amazon SSO SDK biedt zowel synchrone als asynchrone API&#39;s om de payload van het SSO-token (platform identity) te verkrijgen.

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

De payload van het Amazon SSO-token (platform identity) moet aanwezig zijn op alle HTTP-aanvragen die worden ingediend tegen de eindpunten van de Adobe Pass-verificatie:

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> De streamingtoepassing kan het verzenden van de payload van het Amazon SSO-token (platformidentiteit) tijdens de `/authenticate` -aanroep overslaan, zoals deze werd aangeboden tijdens de `/regcode` -aanroep.

Adobe Pass-verificatie ondersteunt de volgende methoden om de payload van het SSO-token (platformidentiteit) te ontvangen. Dit is een apparaat- of platformscoped-id:

* Als een header met de naam: `Adobe-Subject-Token`
* Als een queryparameter met de naam: `ast`
* Als parameter post: `ast`

>[!IMPORTANT]
>
> Als verzonden als vraagparameter, dan kan volledige URL zeer lang worden en verworpen.
>
> Als verzonden als vraag/post parameter, dan moet het worden omvat wanneer het produceren van de verzoekhandtekening.

#### Voorbeelden

**die als kopbal** verzendt

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**die als vraagparameter** verzendt

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**die als postparameter** verzendt

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> Als de parameterwaarde `Adobe-Subject-Token` header of `ast` ontbreekt of ongeldig is, worden de aanvragen door Adobe Pass-verificatie verwerkt zonder dat rekening wordt gehouden met Single Sign-On.
