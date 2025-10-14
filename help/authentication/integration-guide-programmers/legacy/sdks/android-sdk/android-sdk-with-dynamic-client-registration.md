---
title: Android SDK met Dynamic Client-registratie
description: Android SDK met Dynamic Client-registratie
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (Verouderd) Android SDK met Dynamic Client-registratie {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#Intro}

Android AccessEnabler SDK for Android is gewijzigd om verificatie in te schakelen zonder sessiecookies te gebruiken. Aangezien steeds meer browsers de toegang tot cookies beperken, moet een andere methode worden gebruikt om verificatie toe te staan.

Voor Android beperkt het gebruik van aangepaste Chrome-tabbladen de toegang tot cookies van andere toepassingen.

>**Android SDK 3.0.0** introduceert:

- dynamische clientregistratie vervangt het huidige mechanisme voor toepassingsregistratie op basis van ondertekende id- en sessiecookie-verificatie van aanvrager
- Aangepaste Chrome-tabbladen voor verificatiestromen

>[!NOTE]
>
>Voor oudere Android-versies zonder Chrome Custom Tabs gebruikt de WebView-verificatie ongeveer zoals in oudere AccessEnabler SDK-versies.


## Dynamische clientregistratie {#DCR}

Android SDK v3.0+ zal de Dynamische procedure van de Registratie van de Cliënt zoals bepaald in [&#x200B; Dynamische het Overzicht van de Registratie van de Cliënt 1&rbrace; gebruiken.](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)


## Functiedemo {#Demo}

Gelieve te letten op [&#x200B; dit webinar &#x200B;](https://my.adobeconnect.com/pzkp8ujrigg1/) dat meer context van de eigenschap geeft en een demo op bevat hoe te om de softwareverklaringen te beheren gebruikend het Dashboard van TVE en hoe te om de geproduceerde degenen te testen gebruikend een duotoepassing die door Adobe als deel van Android SDK wordt verstrekt.

## API-wijzigingen {#API}


### Factory.getInstance

**Beschrijving:** Instantieert het voorwerp van Inschakelen van de Toegang. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| public static AccessEnabler getInstance (Context appContext, String softwareStatement, String redirectUrl) <br>        throws AccessEnablerException |


**Beschikbaarheid:** v3.0+

**Parameters:**

- *appContext*: De toepassingscontext van Android
- softwareStatement: waarde die uit Dashboard van TVE wordt verkregen of *ongeldig* als &quot;software\_statement&quot;in strings.xml wordt geplaatst
- redirectUrl: unieke url, één van de domeinen in omgekeerde orde die uitdrukkelijk in Dashboard TVE of *ongeldig* werd toegevoegd als &quot;redirect\_uri&quot;in strings.xml wordt geplaatst

Opmerking: ongeldige softwareStatement of redirectUrl zorgt ervoor dat de toepassing AccessEnabler niet initialiseert of de toepassing registreert voor Adobe Pass-verificatie en -autorisatie
</br>
Opmerking: de parameter redirectUrl of de omleiding\_uri in strings.xml moet de waarde zijn van het domein dat is toegevoegd in het TVE-dashboard voor de toepassing in omgekeerde volgorde (voor bijvoorbeeld het domein &#39;adobe.com&#39; dat is toegevoegd in het TVE-dashboard, moet de omleidingUrl &#39;com.adobe&#39; zijn.


### setRequestor

**Beschrijving:** vestigt de identiteit van het Kanaal. Aan elk kanaal wordt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van het Kanaal in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: het milieu van de Versie van de Adobe/van de Productie.

Als een waarde voor de *urls* parameter wordt verstrekt, richt de resulterende netwerkvraag alle URLs die in de *wordt verstrekt urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst onthoudt de Access Enabler de URL van de bijbehorende serviceprovider. Alle volgende machtigingsaanvragen worden doorgestuurd naar de URL die is gekoppeld aan de serviceprovider die tijdens de configuratiefase aan de doel-MVPD is gekoppeld.

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *requestID*: Unieke identiteitskaart verbonden aan het Kanaal. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
- *urls*: Facultatieve parameter; door gebrek, wordt de dienstverlener van Adobe gebruikt [&#x200B; http://sp.auth.adobe.com/ &#x200B;](http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Daarbij bestaat de MVPD-lijst uit de eindpunten van alle serviceproviders. Elke MVPD is gekoppeld aan de snelste serviceprovider, dat wil zeggen de provider die eerst heeft gereageerd en die die MVPD ondersteunt.

Vervangen:

- *signedRequestorID*: Een exemplaar van de aanvrageridentiteitskaart die digitaal met uw privé sleutel wordt ondertekend. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)--> .

**teweeggebrachte callbacks:** `setRequestorComplete()`

### afmelden

**Beschrijving:** gebruik deze methode om de logout stroom in werking te stellen. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Hierdoor wordt met deze flow een ChromeCustomTab-venster geopend voor het uitvoeren van de aanmeldprocedure.

| API-aanroep: de afmeldingsstroom starten |
| --- |
| public void logout() |

**Beschikbaarheid:** v3.0+

**Parameters:** niets

**teweeggebrachte callbacks:** `setAuthenticationStatus()`
</br></br>

## Implementatiestroom van programma&#39;s {#Progr}

### **1. Toepassing registreren**

a. Vraag software\_statement en omleiding\_uri aan vanuit Adobe Pass ( TVE-dashboard )

b. Er zijn twee mogelijkheden om deze waarden door te geven aan Adobe Pass SDK:

In strings.xml voegt u toe:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Call AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### 2. Toepassing configureren

a. setRequestor(aanvrager\_id)

SDK voert de volgende bewerkingen uit:

- registratietoepassing: het gebruiken van **software \_statement**, SDK zal a **cliënt\_id, cliënt\_gehechtheid, client\_id\_issued\_at, omleiding \_uris, subsidie \_types** verkrijgen. Deze informatie wordt opgeslagen in de interne opslag van de toepassing.

- verkrijg een **toegang \_token** gebruikend cliënt\_id, cliënt\_gehechtheid en gift \_type= &quot;cliënt\_credentials&quot;. Deze access\_token wordt gebruikt voor elke oproep die door de SDK aan Adobe Pass-servers wordt gedaan

**symbolische Reacties van de Fout:**

| Foutreacties | | |
| --- | --- | --- |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Het verzoek mist een vereiste parameter, omvat een niet gestaafde parameterwaarde (buiten giftetype), herhaalt een parameter, omvat veelvoudige geloofsbrieven, gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren, of anders misvormd is. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Clientverificatie is mislukt omdat de client onbekend was. De SDK MOET zich opnieuw bij de verificatieserver registreren. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;unauthorised\_client&quot;} | De geverifieerde client is niet gemachtigd om dit type autorisatieverlening te gebruiken. |

- als een MVPD Passive Authentication vereist, wordt een aangepaste tab van Chrome geopend voor passieve uitvoering met die MVPD en wordt deze gesloten wanneer deze gereed is

b. checkAuthentication()

- true : ga naar Autorisatie
- false: ga naar Select MVPD

c. getAuthentication: SDK zal **access_token** in vraagparameters omvatten

- mvpd onthouden : ga naar setSelectedProvider(mvpd_id)
- mvpd niet geselecteerd : displayProviderDialog
- mvpd geselecteerd : ga naar setSelectedProvider(mvpd_id)

d. setSelectedProvider

- mvpd\_id-verificatieURL is geladen in ChromeCustomTabs
- login succesvol : delegate.setAuthenticationStatus ( SUCCESS )
- aanmelden geannuleerd: MVPD-selectie opnieuw instellen
- Het URL-schema is ingesteld als &quot;adobepass://redirect_uri&quot; om vast te leggen wanneer de verificatie is voltooid

e. get/checkAuthorization: SDK zal **access_token** in kopbal als Vergunning omvatten: Drager **access_token**

- indien de toestemming succesvol is , zal worden opgeroepen om de
media-token

f. afmelden:

- SDK verwijdert een geldig token voor de huidige aanvrager (de verificatie die door andere toepassingen en niet door SSO wordt verkregen, blijft geldig)
- SDK opent Chrome Custom Tabs om het mvpd_id-uitlogeindpunt te bereiken. Nadat de aangepaste Chrome-tabbladen zijn voltooid, worden deze gesloten
- Het URL-schema is ingesteld als &quot;adobepass://logout&quot; om het moment vast te leggen waarop de afmelding is voltooid
- logout zal sendTrackingData (nieuwe Gebeurtenis (EVENT_LOGOUT, USER_NOT_AUTHENTICATED_ERROR) en callback activeren: setAuthenticationStatus (0, &quot;Logout&quot;)

**Nota:** aangezien elke vraag een **access_token vereist,** de mogelijke foutencodes hieronder worden behandeld in SDK.


| Foutreacties | | |
| --- | ---|--- |
| invalid_request | 400 | Het verzoek is onjuist geformuleerd. De SDK moet stoppen met het uitvoeren van aanroepen naar de server. |
| invalid_client | 403 | De client-id mag geen aanvragen meer uitvoeren. De SDK MOET de clientregistratie opnieuw uitvoeren. |
| access_deny | 401 | De toegangstoken is ongeldig. De sdk MOET om een nieuwe access_token verzoeken. |
