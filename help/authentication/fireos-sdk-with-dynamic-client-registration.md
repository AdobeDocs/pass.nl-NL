---
title: Amazon FireOS SDK met Dynamic Client-registratie
description: Amazon FireOS SDK met Dynamic Client-registratie
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Amazon FireOS SDK met Dynamic Client-registratie {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## <span id=""></span> Inleiding {#Intro}

FireOS AccessEnabler SDK voor FireTV is gewijzigd om verificatie in te schakelen zonder sessiecookies te gebruiken. Aangezien steeds meer browsers de toegang tot cookies beperken, was een andere methode nodig om verificatie toe te staan.

**FireOS SDK 3.0.4** vervangt het huidige mechanisme van de toepassingsregistratie dat op ondertekende identiteitskaart van de aanvrager en de authentificatie van het zittingskoekje met [ het Dynamische Overzicht van de Registratie van de Cliënt ](./dcr-api/dynamic-client-registration-overview.md) wordt gebaseerd.


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
- de parameter redirectUrl voor FireTV wordt geplaatst door SDK aan adobepass://android.app aangezien de authentificatie door unieke instantie AccessEnabler wordt behandeld.

### setRequestor

**Beschrijving:** vestigt de identiteit van het Kanaal. Aan elk kanaal wordt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van het Kanaal in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: het milieu van de Productie van de Versie van de Adobe.

Als een waarde voor de *urls* parameter wordt verstrekt, richt de resulterende netwerkvraag alle URLs die in de *wordt verstrekt urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt Toegangsbeheer URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| public void setRequestor(String requestId) |

**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *requestID*: Unieke identiteitskaart verbonden aan het Kanaal. Geef de unieke id die door Adobe aan uw site is toegewezen door wanneer u zich voor het eerst aanmeldt bij de Adobe Pass-verificatieservice.
- *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt (http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

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

   - registratietoepassing: het gebruiken van **software \_statement**, SDK zal a **cliënt\_id, cliënt\_gehechtheid, client\_id\_issued\_at, omleiding \_uris, subsidie \_types** verkrijgen. Deze informatie wordt opgeslagen in de interne opslag van de toepassing.
   - verkrijg een **toegang \_token** gebruikend cliënt\_id, cliënt\_gehechtheid en gift \_type= &quot;cliënt\_credentials&quot;. Deze access\_token wordt gebruikt voor elke aanroep van de SDK naar Adobe Pass-servers.

| Token-foutreacties: |  |  |
|--- | --- | --- |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Het verzoek mist een vereiste parameter, omvat een niet gestaafde parameterwaarde (buiten giftetype), herhaalt een parameter, omvat veelvoudige geloofsbrieven, gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren, of anders misvormd is. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Clientverificatie is mislukt omdat de client onbekend was. SDK *MOET* opnieuw bij de vergunningsserver registreren. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;unauthorised\_client&quot;} | De geverifieerde client is niet gemachtigd om dit type autorisatieverlening te gebruiken. |

- in het geval MVPD Passieve Authentificatie vereist, zal een WebView openen om passief met dat MVPD uit te voeren en zal sluiten wanneer volledig

- b. checkAuthentication()

   - *waar* : ga naar Vergunning
   - *vals* : ga MVPD selecteren

- c. getAuthentication: SDK zal **access_token** in vraagparameters omvatten

   - mvpd onthouden : ga naar setSelectedProvider(mvpd\_id)
   - mvpd niet geselecteerd : displayProviderDialog
   - mvpd geselecteerd : ga naar setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - mvpd\_id-verificatieURL is geladen in ChromeCustomTabs
   - login succesvol : delegate.setAuthenticationStatus ( SUCCESS )
   - aanmelden geannuleerd: MVPD-selectie opnieuw instellen
   - Het URL-schema is ingesteld als &quot;adobepass://android.app&quot; om vast te leggen wanneer de verificatie is voltooid

- e. get/checkAuthorization: SDK zal **access\_token **in kopbal als Vergunning omvatten: Drager **toegang \_token**

- als de toestemming succesvol is , zal een oproep worden gedaan om het media token te verkrijgen

- f. afmelden:

   - SDK verwijdert een geldig token voor de huidige aanvrager (de verificatie die door andere toepassingen en niet via SSO wordt verkregen, blijft geldig).
   - SDK opent Aangepaste Chrome-tabbladen om het mvpd\_id-logout-eindpunt te bereiken. Nadat de aangepaste Chrome-tabbladen zijn voltooid, worden deze gesloten
   - Het URL-schema is ingesteld als &quot;adobepass://logout&quot; om het moment vast te leggen waarop de afmelding is voltooid
   - Logout activeert een sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) en een callback: setAuthenticationStatus(0,&quot;Logout&quot;)



**Nota:** aangezien elke vraag een **access_token** vereist, worden de mogelijke hieronder foutencodes behandeld in SDK.

| Foutreacties |  |  |
|--- | --- | --- |
| invalid_request | 400 | Het verzoek is onjuist geformuleerd. SDK zou moeten ophouden uitvoerend vraag aan de server. |
| invalid_client | 403 | De client-id mag geen aanvragen meer uitvoeren. De SDK MOET de clientregistratie opnieuw uitvoeren. |
| access_deny | 401 | Access_token is not valid. De sdk MOET om een nieuwe access_token verzoeken. |
