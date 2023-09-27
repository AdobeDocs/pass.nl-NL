---
title: Android-SDK met dynamische clientregistratie
description: Android-SDK met dynamische clientregistratie
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# Android-SDK met dynamische clientregistratie {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#Intro}

De Android AccessEnabler SDK voor Android is gewijzigd om verificatie in te schakelen zonder sessiecookies te gebruiken. Aangezien steeds meer browsers de toegang tot cookies beperken, moet een andere methode worden gebruikt om verificatie toe te staan.

Voor Android beperkt het gebruik van aangepaste Chrome-tabs de toegang tot cookies van andere toepassingen.

>**Android SDK 3.0.0** introduceert:

- dynamische clientregistratie vervangt het huidige mechanisme voor toepassingsregistratie op basis van ondertekende id- en sessiecookie-verificatie van aanvrager
- Aangepaste tabbladen van Chrome voor verificatiestromen

>[!NOTE]
>
>Voor oudere Android-versies zonder Chrome Custom Tabs gebruikt de WebView-verificatie ongeveer zoals in oudere versies van AccessEnabler SDK.


## Dynamische clientregistratie {#DCR}

Android SDK v3.0+ gebruikt de procedure voor registratie van dynamische clients zoals gedefinieerd in [Dynamische clientregistratie](/help/authentication/dynamic-client-registration.md).


## Functiedemo {#Demo}

Gelieve te volgen [dit webinar](https://my.adobeconnect.com/pzkp8ujrigg1/) Dit biedt meer context van de functie en bevat een demo over hoe u de softwareinstructies kunt beheren met het TVE-dashboard en hoe u de gegenereerde instructies kunt testen met behulp van een demo-toepassing die door Adobe is geleverd als onderdeel van de Android-SDK.

## API-wijzigingen {#API}


### Factory.getInstance

**Omschrijving:** Instantieert het Access Enabler-object. Per toepassingsinstantie moet er één instantie van Access Enabler zijn.

| API-aanroep: constructor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException |


**Beschikbaarheid:** v3.0+

**Parameters:**

- *appContext*: Context Android-toepassing
- softwareStatement: waarde verkregen van TVE-dashboard of *null* if &quot;software\_statement&quot; is ingesteld in strings.xml
- redirectUrl : unieke url, een van de domeinen in omgekeerde volgorde die expliciet is toegevoegd aan het TVE-dashboard of *null* if &quot;redirect\_uri&quot; is ingesteld in strings.xml

Opmerking: ongeldige softwareStatement of redirectUrl zorgt ervoor dat de toepassing AccessEnabler niet initialiseert of de toepassing registreert voor Adobe Pass-verificatie en -autorisatie
</br>
Opmerking: de parameter redirectUrl of de omleiding\_uri in strings.xml moet de waarde zijn van het domein dat is toegevoegd in het TVE-dashboard voor de toepassing in omgekeerde volgorde (voor bijvoorbeeld het domein &#39;adobe.com&#39; dat is toegevoegd in het TVE-dashboard, moet de omleidingUrl &#39;com.adobe&#39; zijn.


### setRequestor

**Omschrijving:** Hiermee stelt u de identiteit van het kanaal in. Aan elk kanaal wordt een unieke id toegewezen bij het registreren bij Adobe voor het Adobe Pass-verificatiesysteem. Wanneer het behandelen van SSO en verre tokens kan de authentificatiestatus veranderen wanneer de toepassing op de achtergrond is, kan setRequestor opnieuw worden geroepen wanneer de toepassing in voorgrond wordt gebracht om met de systeemstaat te synchroniseren (haal een verre teken als SSO wordt toegelaten of schrap het lokale teken als een logout in de tussentijd gebeurde).

De serverreactie bevat een lijst van MVPDs samen met wat configuratieinformatie die aan de identiteit van het Kanaal in bijlage is. De serverreactie wordt intern gebruikt door de code van Toegangsbeheer. Alleen de status van de bewerking (SUCCESS/FAIL) wordt via de callback setRequestorComplete() aan uw toepassing gepresenteerd.

Als de *urls* parameter niet wordt gebruikt, richt de resulterende netwerkvraag de standaarddienstverlener URL: de milieu van de Versie van de Adobe/van de Productie.

Als een waarde is opgegeven voor de *urls* parameter, richt de resulterende netwerkvraag alle URLs die in *urls* parameter. Alle configuratieverzoeken worden teweeggebracht gelijktijdig in afzonderlijke draden. De eerste responder krijgt voorrang wanneer het compileren van de lijst van MVPDs. Voor elke MVPD in de lijst, onthoudt Toegangsbeheer URL van de bijbehorende dienstverlener. Alle verdere machtigingsverzoeken worden gericht aan URL verbonden aan de dienstverlener die met doel MVPD tijdens de configuratiefase in paren werd gebracht.

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Beschikbaarheid:** v3.0+

| API-aanroep: configuratie aanvrager |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Beschikbaarheid:** v3.0+

**Parameters:**

- *aanvragerID*: De unieke id die aan het kanaal is gekoppeld. Geef de unieke id die door Adobe is toegewezen door aan uw site wanneer u zich voor het eerst hebt geregistreerd bij de Adobe Pass-verificatieservice.
- *urls*: Optionele parameter; standaard wordt de Adobe-serviceprovider gebruikt [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Deze serie staat u toe om eindpunten voor authentificatie en vergunningsdiensten te specificeren die door Adobe worden verleend (verschillende instanties zouden voor het zuiveren doeleinden kunnen worden gebruikt). U kunt dit gebruiken om meerdere instanties van Adobe Pass-verificatieproviders op te geven. Wanneer het doen van dit, is de lijst MVPD samengesteld uit de eindpunten van alle dienstverleners. Elke MVPD wordt geassocieerd met de snelste dienstverlener; namelijk de leverancier die eerst antwoordde en die die MVPD steunt.

Vervangen:

- *signedRequestorID*: Een kopie van de aanvrager-id die digitaal is ondertekend met uw persoonlijke sleutel. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Callbacks geactiveerd:** `setRequestorComplete()`

### afmelden

**Omschrijving:** Gebruik deze methode om de logout-flow te starten. De logout is het resultaat van een reeks HTTP-omleidingsverrichtingen toe te schrijven aan het feit dat de gebruiker uit zowel de servers van de Authentificatie van Adobe Pass als van de servers van MVPD moet worden geregistreerd. Hierdoor wordt met deze flow een ChromeCustomTab-venster geopend voor het uitvoeren van de aanmeldprocedure.

| API-aanroep: de afmeldingsstroom starten |
| --- |
| public void logout() |

**Beschikbaarheid:** v3.0+

**Parameters:** Geen

**Callbacks geactiveerd:** `setAuthenticationStatus()`
</br></br>

## Implementatiestroom van programma&#39;s {#Progr}

### **1. Toepassing registreren**

a. Vraag software\_statement en omleiding\_uri aan vanuit Adobe Pass ( TVE-dashboard )

b. Er zijn twee opties om deze waarden door te geven aan de Adobe Pass SDK:

In strings.xml voegt u toe:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Call AccessEnabler.getInstance(appContext,softwareStatement, redirectUrl)


### 2. Toepassing configureren

a. setRequestor(aanvrager\_id)

SDK voert de volgende bewerkingen uit:

- registertoepassing: gebruiken **software\_statement**, krijgt SDK een **client\_id, client\_geheime, client\_id\_issued\_at, omleiding\_uris, gift\_types**. Deze informatie wordt opgeslagen in de interne opslag van de toepassing.

- een **access\_token** client\_id gebruiken, client\_geheime en gift\_type=&quot;client\_credentials&quot;. Deze access\_token wordt gebruikt voor elke oproep die door de SDK aan Adobe Pass-servers wordt gedaan

**Token-foutreacties:**

| Foutreacties | | |
| --- | --- | --- |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Het verzoek mist een vereiste parameter, omvat een niet gestaafde parameterwaarde (buiten giftetype), herhaalt een parameter, omvat veelvoudige geloofsbrieven, gebruikt meer dan één mechanisme om de cliënt voor authentiek te verklaren, of anders misvormd is. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Clientverificatie is mislukt omdat de client onbekend was. De SDK MOET zich opnieuw bij de machtigingsserver registreren. |
| HTTP 400 (Ongeldige aanvraag) | {&quot;error&quot;: &quot;unauthorised\_client&quot;} | De geverifieerde client is niet gemachtigd om dit type autorisatieverlening te gebruiken. |

- in het geval dat MVPD Passieve Authentificatie vereist, zal een Lusje van de Douane van het Chrome openen om passief met dat MVPD uit te voeren en zal sluiten wanneer volledig

b. checkAuthentication()

- true : ga naar Autorisatie
- false: ga naar Select MVPD

c. getAuthentication : SDK omvat **access_token** in vraagparameters

- mvpd onthouden : ga naar setSelectedProvider(mvpd_id)
- mvpd niet geselecteerd : displayProviderDialog
- mvpd geselecteerd : ga naar setSelectedProvider(mvpd_id)

d. setSelectedProvider

- mvpd\_id-verificatieURL is geladen in ChromeCustomTabs
- login succesvol : delegate.setAuthenticationStatus ( SUCCESS )
- aanmelden geannuleerd: MVPD-selectie opnieuw instellen
- Het URL-schema is ingesteld als &quot;adobepass://redirect_uri&quot; om vast te leggen wanneer de verificatie is voltooid

e. get/checkAuthorization: SDK zal **access_token** in de koptekst &quot;Autorisatie: Drager&quot; **access_token**

- als de toestemming succesvol is , zal een oproep worden gedaan om het media token te verkrijgen

f. afmelden:

- SDK verwijdert een geldig token voor de huidige aanvrager (de verificatie die door andere toepassingen en niet via SSO wordt verkregen, blijft geldig).
- SDK opent Chrome Custom Tabs om mvpd_id-uitlogeindpunt te bereiken. Nadat de aangepaste tabbladen voor Chrome zijn voltooid, worden deze gesloten
- Het URL-schema is ingesteld als &quot;adobepass://logout&quot; om het moment vast te leggen waarop de afmelding is voltooid
- logout zal sendTrackingData (nieuwe Gebeurtenis (EVENT_LOGOUT, USER_NOT_AUTHENTICATED_ERROR) en callback activeren: setAuthenticationStatus (0, &quot;Logout&quot;)

**Opmerking:** aangezien elke vraag vereist **access_token,** Mogelijke foutcodes worden hieronder afgehandeld in de SDK.


| Foutreacties | | |
| --- | ---|--- |
| invalid_request | 400 | Het verzoek is onjuist geformuleerd. SDK zou moeten ophouden uitvoerend vraag aan de server. |
| invalid_client | 403 | De client-id mag geen aanvragen meer uitvoeren. De SDK MOET de clientregistratie opnieuw uitvoeren. |
| access_deny | 401 | De toegangstoken is ongeldig. De sdk MOET om een nieuwe access_token verzoeken. |
