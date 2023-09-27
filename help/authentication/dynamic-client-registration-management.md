---
title: Dynamisch clientregistratiebeheer
description: Dynamisch clientregistratiebeheer
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 0%

---

# Dynamisch clientregistratiebeheer {#dynamic-client-registration-management}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

Met de wijdverbreide goedkeuring van [Aangepaste tabbladen van Android-chroom](https://developer.chrome.com/multidevice/android/customtabs){target_blanck} en [Apple Safari View-controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck} in de toepassingen van onze klanten, werken wij de stroom van de gebruikersauthentificatie in de Authentificatie van Adobe Pass bij. Specifieker, kunnen wij niet meer het doel bereiken om de staat te handhaven zodat de stroom van de gebruikersagent van het voor authentiek verklaren van een abonnee MVPD tussen redirects kan worden gevolgd. Dit is eerder gedaan met HTTP cookies. Deze beperking is het stuurprogramma waarmee u alle API&#39;s naar OAuth 2.0 gaat migreren [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}.

Met deze update worden de Clients van de Authentificatie van de Adobe OAuth 2.0 cliënten en een douane OAuth 2.0 vergunningsserver wordt opgesteld om aan de behoeften van de Dienst van de Authentificatie van Adobe Pass te voldoen.

Opdat de toepassingen van de Cliënt de vergunning OAuth 2.0 gebruiken, moet de server dynamisch registreren om specifieke informatie (cliëntgeloofsbrieven) te verkrijgen om met het te kunnen communiceren. Als deel van het registratieproces, moet de cliënt een reeks ingebouwde meta-gegevens aan het eindpunt van de cliëntregistratie voorleggen.

Deze metagegevens worden doorgegeven als een softwareinstructie, die een &quot;software_id&quot; bevat waarmee onze verificatieserver verschillende exemplaren van een toepassing kan correleren met dezelfde softwareinstructie.

A **software, instructie** is een JSON Web Token (JWT) die meta-gegevenswaarden over de cliëntsoftware als bundel bevestigt. Wanneer voorgesteld aan de vergunningsserver als deel van een verzoek van de cliëntregistratie, moet de softwareverklaring digitaal worden ondertekend of MACed gebruikend de Handtekening van het Web JSON (JWS).

In de officiële documentatie vindt u een gedetailleerdere uitleg van de software-instructies en de manier waarop deze werken [RFC7591](https://tools.ietf.org/html/rfc7591).

De softwareverklaring zou met de toepassing op het apparaat van de gebruiker moeten worden opgesteld.

Voorafgaand aan deze update hadden wij twee mechanismen om toepassingen toe te staan om vraag aan de Authentificatie van Adobe Pass uit te voeren:

* browsergebaseerde clients worden geregistreerd via toegestane [domeinlijst](/help/authentication/programmer-overview.md#reg-and-init)
* native toepassingsclients, zoals iOS- en Android-toepassingen, worden geregistreerd via **ondertekende aanvrager** mechanisme


Met het machtigingsmechanisme voor clientregistratie moet u uw toepassingen toevoegen aan het TVE-dashboard.

Een klant die de nieuwe Android-SDK en de komende iOS SDK wil implementeren, heeft een softwareinstructie nodig. Een softwareinstructie identificeert een toepassing die is gemaakt op het TVE-dashboard.

Voer de stappen in de onderstaande secties uit om een geregistreerde toepassing te maken in het TVE-dashboard.

## Een geregistreerde toepassing maken {#create_app}

U kunt op twee manieren een geregistreerde toepassing maken in het TVE-dashboard:

* [Programmeringsniveau](#prog-level) - kunt u een geregistreerde toepassing maken en deze koppelen aan een of alle programmamakanalen.

* [Kanaalniveau](#channel-level) - hiermee kunt u een geregistreerde toepassing maken die permanent is gekoppeld aan alleen dit kanaal.

### Een geregistreerde toepassing maken op programmeerniveau {#prog-level}

Ga naar **Programmeurs** > **Geregistreerde toepassingen** tab.

![](assets/reg-app-progr-level.png)

Klik op het tabblad Geregistreerde toepassingen op **Nieuwe toepassing toevoegen**. Vul de vereiste velden in het nieuwe venster in.

Zoals u in de onderstaande afbeelding ziet, kunt u de volgende velden invullen:

* **Toepassingsnaam** - de naam van de aanvraag

* **Toegewezen aan kanaal** - de naam van uw kanaal, t</span>Waaraan deze toepassing is gekoppeld. De standaardinstelling in het vervolgkeuzemasker is **Alle kanalen.** Met de interface kunt u één kanaal of alle kanalen selecteren.

* **Toepassingsversie** - standaard is deze ingesteld op &quot;1.0.0&quot;, maar we raden u ten zeerste aan deze te wijzigen met uw eigen toepassingsversie. Als u besluit de versie van uw toepassing te wijzigen, kunt u dit het beste doen door een nieuwe geregistreerde toepassing voor de toepassing te maken.

* **Toepassingsplatforms** - de platforms voor de toepassing waarmee een koppeling tot stand moet worden gebracht. U kunt ze allemaal of meerdere waarden selecteren.

* **Domeinnamen** - de domeinen waarmee de toepassing moet worden gekoppeld. De domeinen in de vervolgkeuzelijst zijn een verenigde selectie van alle domeinen vanuit al uw kanalen. U kunt meerdere domeinen selecteren in de lijst. De betekenis van de domeinen is omleiding van URL&#39;s [RFC6749](https://tools.ietf.org/html/rfc6749). In het clientregistratieproces kan de clienttoepassing vragen om een omleidings-URL te mogen gebruiken voor de voltooiing van de verificatiestroom. Wanneer een clienttoepassing een specifieke omleidings-URL aanvraagt, wordt deze gevalideerd tegen de domeinen die in deze geregistreerde toepassing zijn vermeld die aan de softwareinstructie zijn gekoppeld.


![](assets/new-reg-app.png)


Nadat u de velden met de juiste waarden hebt gevuld, moet u op &quot;Gereed&quot; klikken om de toepassing in de configuratie op te slaan.

Houd er rekening mee dat er **geen optie om een reeds gemaakte toepassing te wijzigen**. Als wordt ontdekt dat iets gecreeerd niet meer aan de vereisten voldoet, zal een nieuwe geregistreerde toepassing moeten worden gecreeerd en worden gebruikt met de cliënttoepassing waarvan het voldoet aan de vereisten.


### Een nieuwe toepassing registreren op kanaalniveau {#channel-level}

Als u een geregistreerde toepassing op kanaalniveau wilt maken, navigeert u naar het menu Kanalen en kiest u de toepassing waarvoor u een toepassing wilt maken. Na het navigeren naar het tabblad &quot;Geregistreerde toepassingen&quot; klikt u op de knop &quot;Nieuwe toepassing toevoegen&quot;.

![](assets/reg-new-app-channel-level.png)

Zoals hieronder getoond, wat hier lichtjes verschillend is, vergeleken bij de zelfde actie die op het niveau van de Programmer wordt uitgevoerd, is de &quot;Toegewezen drop van Kanalen&quot;die niet wordt toegelaten zodat is er geen optie om de geregistreerde toepassing aan andere dan het huidige kanaal te binden.

![](assets/new-reg-app-channel.png)

## Toepassingen weergeven {#list-reg-app}

Na het creëren van de geregistreerde toepassing is er de mogelijkheid om een softwareverklaring te krijgen om de vergunningsserver als deel van een verzoek voor te stellen.

Dit kan worden gedaan door of aan de Programmer of het Kanaal te navigeren waarvoor de geregistreerde toepassingen werden gecreeerd, waar zij vermeld zijn.

Zoals hieronder wordt beschreven, wordt elk item in de lijst geïdentificeerd met een naam, versie en symbolen voor platforms waaraan het is gekoppeld.

![](assets/reg-app-list.png)

U kunt voor elk van de volgende opties instellen:

* [Weergave](#view)
* [Software-instructies downloaden](#download-statement)

### Een geregistreerde toepassing weergeven {#view}

Als u een van de toepassingen in de lijst kiest en op de knop Weergeven klikt, worden de details weergegeven die zijn gebruikt toen de toepassing werd gemaakt. Zoals eerder vermeld, is er geen mogelijkheid om iets te wijzigen.


![](assets/view-reg-app.png)


### Softwareinstructie downloaden {#download-statement}

Als u klikt op de knop &quot;Downloaden&quot; in de lijst waarvoor een softwareinstructie nodig is, wordt een tekstbestand gegenereerd. Dit bestand bevat iets dat lijkt op de onderstaande voorbeelduitvoer.


![](assets/download-software-statement.png)

De naam van het bestand wordt op unieke wijze geïdentificeerd door het bestand vooraf te voorzien van &quot;software_statement&quot; en het huidige tijdstempel toe te voegen.

Houd er rekening mee dat voor dezelfde geregistreerde toepassing elke keer verschillende softwareinstructies worden ontvangen wanneer op de downloadknop wordt geklikt, maar dat dit de eerder verkregen softwareinstructies voor deze toepassing niet ongeldig maakt. Dit gebeurt omdat ze ter plaatse worden gegenereerd, per verzoek om actie.

Er is er één **beperking** met betrekking tot de downloadactie. Als een softwareinstructie wordt gevraagd door kort na het maken van de geregistreerde toepassing op de knop Downloaden te klikken. Deze instructie is nog niet opgeslagen en de configuratiepunt is niet gesynchroniseerd. Hieronder wordt het volgende foutbericht weergegeven.

![](assets/error-sw-statement-notready.png)

Dit omvat HTTP 404 niet Gevonden foutcode die van kern werd ontvangen aangezien identiteitskaart van de geregistreerde toepassing nog niet werd verspreid en de kern geen kennis van het heeft.

De oplossing is, na het creëren van de geregistreerde toepassing, om maximaal 2 minuten op de te synchroniseren configuratie te wachten. Hierna wordt het foutbericht niet meer ontvangen en kan het tekstbestand met de softwareinstructie worden gedownload.

Zie de koppeling in Verwante informatie hieronder naast andere nuttige koppelingen voor meer informatie over hoe het end-to-end proces werkt of om inzicht te krijgen in hoe de aanvragen worden uitgevoerd en welke reacties erop moeten worden verwacht.

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## Functiedemo {#tutorial}

Gelieve te volgen [dit webinar](https://my.adobeconnect.com/pzkp8ujrigg1/) Dit biedt meer context van de functies en bevat een demo over hoe u de softwareinstructies kunt beheren met het TVE-dashboard en hoe u de gegenereerde instructies kunt testen met behulp van een demo-toepassing die door Adobe is geleverd als onderdeel van de Android-SDK.
