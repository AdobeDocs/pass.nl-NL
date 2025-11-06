---
title: Amazon FireOS SDK met Dynamic Client-registratie
description: Amazon FireOS SDK met Dynamic Client-registratie
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---


# (Verouderd) Amazon FireOS SDK met registratie van dynamische clients {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

## <span id=""></span> Inleiding  {#Intro}

FireOS AccessEnabler SDK for FireTV is gewijzigd om verificatie in te schakelen zonder sessiecookies te gebruiken. Aangezien steeds meer browsers de toegang tot cookies beperken, was een andere methode nodig om verificatie toe te staan.

**FireOS SDK 3.0.4** vervangt het huidige mechanisme van de toepassingsregistratie dat op ondertekende identiteitskaart van de aanvrager en de authentificatie van het zittingskoekje met [&#x200B; Dynamisch Overzicht van de Registratie van de Cliënt &#x200B;](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) wordt gebaseerd.


## API-wijzigingen {#API}

### Factory.getInstance

**Beschrijving:** Instantieert het voorwerp van Inschakelen van de Toegang. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| public static AccessEnabler getInstance (Context appContext, String softwareStatement, String redirectUrl) <br>        throws AccessEnablerException |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *appContext*: De toepassingscontext van Android
- *softwareStatement*: waarde die van Dashboard van TVE wordt verkregen of *ongeldig* als &quot;software\_statement&quot;in strings.xml wordt geplaatst
- *redirectUrl* : voor implementaties FireTV zou deze parameter ongeldig moeten zijn. Alle instellingen voor dit kenmerk worden genegeerd.

**Nota&#39;s**

- ongeldige softwareStatement zal de toepassing veroorzaken om AccessEnabler niet te initialiseren of toepassing voor de Authentificatie van Adobe Pass en vergunning te registreren
- redirectUrl de parameter voor FireTV wordt geplaatst door SDK aan adobepass://android.app aangezien de authentificatie door unieke instantie AccessEnabler wordt behandeld.

### setRequestor

**Beschrijving:** vestigt de identiteit van het Kanaal. Aan elk kanaal wordt een unieke id toegewezen wanneer u zich registreert bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van het Kanaal in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: het milieu van de Productie van de Versie van Adobe.

Als een waarde voor de *urls* parameter wordt verstrekt, richt de resulterende netwerkvraag alle URLs die in de *wordt verstrekt urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst onthoudt de Access Enabler de URL van de bijbehorende serviceprovider. Alle volgende machtigingsaanvragen worden doorgestuurd naar de URL die is gekoppeld aan de serviceprovider die tijdens de configuratiefase aan de doel-MVPD is gekoppeld.

| API-aanroep: configuratie aanvrager |
| --- |
| public void setRequestor(String requestId) |

**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *requestID*: Unieke identiteitskaart verbonden aan het Kanaal. Geef de unieke id die Adobe aan uw site heeft toegewezen door aan wanneer u zich voor het eerst aanmeldt bij de Adobe Pass-verificatieservice.
- *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Met deze array kunt u eindpunten opgeven voor verificatie- en verificatieservices die door Adobe worden geleverd (verschillende instanties kunnen worden gebruikt voor foutopsporingsdoeleinden). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Daarbij bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD is gekoppeld aan de snelste serviceprovider, dat wil zeggen de provider die eerst heeft gereageerd en die die MVPD ondersteunt.

Vervangen:

- *signedRequestorID*: Een exemplaar van de aanvrageridentiteitskaart die digitaal met uw privé sleutel wordt ondertekend. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)--> .

**teweeggebrachte callbacks:** `setRequestorComplete()`

</br>

### afmelden

**Beschrijving:** gebruik deze methode om de logout stroom in werking te stellen. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Hierdoor wordt met deze flow een ChromeCustomTab-venster geopend voor het uitvoeren van de aanmeldprocedure.

| API-aanroep: de afmeldingsstroom starten |
| --- |
| public void logout() |

**Beschikbaarheid:** v3.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus()`

## Implementatiestroom van programma&#39;s {#Progr}

### **1. Toepassing registreren**

1. Software\_statement ophalen van Adobe Pass ( TVE-dashboard )
1. Er zijn twee opties om deze waarden door te geven aan Adobe Pass SDK:
   - In strings.xml voegt u toe:

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Call AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Toepassing configureren**

- a. setRequestor(aanvrager\_id)

  De SDK voert de volgende bewerkingen uit:

   - registratietoepassing: het gebruiken van **software \_statement**, zal SDK a **cliënt\_id, cliënt\_gehechtheid, client\_id\_issued\_at, omleiding \_uris, subsidie \_types** verkrijgen. Deze informatie wordt opgeslagen in de interne opslag van de toepassing.
   - verkrijg een **toegang \_token** gebruikend cliënt\_id, cliënt\_gehechtheid en gift \_type= &quot;cliënt\_credentials&quot;. Deze access\_token wordt gebruikt voor elke oproep die door de SDK aan Adobe Pass-servers wordt gedaan.

| Token-foutreacties: |  |  |
|--- | --- | --- |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Het verzoek mist een vereiste parameter, omvat een niet gestaafde parameterwaarde (buiten giftetype), herhaalt een parameter, omvat veelvoudige geloofsbrieven, gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren, of anders misvormd is. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Clientverificatie is mislukt omdat de client onbekend was. SDK *MOET* opnieuw bij de vergunningsserver registreren. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;unauthorised\_client&quot;} | De geverifieerde client is niet gemachtigd om dit type autorisatieverlening te gebruiken. |

- als een MVPD Passieve Authentificatie vereist, zal een WebView openen om passief met die MVPD uit te voeren en zal sluiten wanneer volledig

- b. checkAuthentication()

   - *waar* : ga naar Vergunning
   - *vals* : ga om MVPD te selecteren

- c. getAuthentication: SDK zal **access_token** in vraagparameters omvatten

   - mvpd onthouden : ga naar setSelectedProvider(mvpd\_id)
   - mvpd niet geselecteerd : displayProviderDialog
   - mvpd geselecteerd : ga naar setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - mvpd\_id-verificatieURL is geladen in ChromeCustomTabs
   - login succesvol : delegate.setAuthenticationStatus ( SUCCESS )
   - aanmelden geannuleerd: MVPD-selectie opnieuw instellen
   - Het URL-schema is ingesteld als &quot;adobepass://android.app&quot; om vast te leggen wanneer de verificatie is voltooid

- e. get/checkAuthorization: SDK zal **access\_token &#x200B;** in kopbal als Vergunning omvatten: Drager **toegang \_token**

- als de toestemming succesvol is , zal een oproep worden gedaan om het media token te verkrijgen

- f. afmelden:

   - SDK verwijdert een geldig token voor de huidige aanvrager (de verificatie die door andere toepassingen en niet door SSO wordt verkregen, blijft geldig)
   - SDK opent Aangepaste Chrome-tabbladen om het mvpd\_id-uitlogeindpunt te bereiken. Nadat de aangepaste Chrome-tabbladen zijn voltooid, worden deze gesloten
   - Het URL-schema is ingesteld als &quot;adobepass://logout&quot; om het moment vast te leggen waarop de afmelding is voltooid
   - Logout activeert een sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) en een callback: setAuthenticationStatus(0,&quot;Logout&quot;)



**Nota:** aangezien elke vraag een **access_token** vereist, worden de mogelijke hieronder foutencodes behandeld in SDK.

| Foutreacties |  |  |
|--- | --- | --- |
| invalid_request | 400 | Het verzoek is onjuist geformuleerd. De SDK moet stoppen met het uitvoeren van aanroepen naar de server. |
| invalid_client | 403 | De client-id mag geen aanvragen meer uitvoeren. De SDK MOET de clientregistratie opnieuw uitvoeren. |
| access_deny | 401 | Access_token is not valid. De sdk MOET om een nieuwe access_token verzoeken. |
