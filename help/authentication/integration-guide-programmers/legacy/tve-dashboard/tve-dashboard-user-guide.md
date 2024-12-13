---
title: Gebruikershandleiding voor Primetime TVE-dashboard
description: Gebruikershandleiding voor Primetime TVE-dashboard
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '5505'
ht-degree: 0%

---

# (Verouderd) Gebruikershandleiding voor het Primetime TVE-dashboard {#tve-db-user-guide}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#tve-db-intro}

[[!DNL Adobe]  Dashboard van TVE (Dashboard van TVE) ](https://console.auth.adobe.com/) is een zelfbedienings dashboard gericht op gebruikers die voor media bedrijven (Programmers) werken die een bedrijfsverhouding met het het productteam van de Authentificatie van Adobe Pass hebben.

Neem contact op met uw technische accountmanager (TAM) voor toegang. Als u toegang wilt krijgen, moet u twee nieuwe gebruikersgroepen configureren in uw Adobe Marketing Cloud-organisatie:

* TVE-dashboard lezen-schrijven - de leden van deze groep hebben volledige rechten voor alle bewerkbare gedeelten van het dashboard
* Alleen-lezen TVE-dashboard - de leden van deze groep hebben alleen weergaverechten voor het hele dashboard


Voordat u deze gebruikershandleiding dieper ingaat, raden we u aan de volgende bronnen te raadplegen om een goed inzicht te krijgen in de stromen en functies die worden aangeboden door het productteam van Adobe Pass Authentication en om vertrouwd te raken met de termen die worden gebruikt in dit document:

* [TVE - Technisch document](/help/authentication/kickstart/technical-paper.md)
* [Programmer Kickstart Guide](/help/authentication/kickstart/programmer-kickstart-guide.md)
* [Entitlement Flow](/help/authentication/integration-guide-programmers/entitlement-flow.md)
* [Verklarende woordenlijst](/help/authentication/kickstart/glossary.md)


Vervolg aan de volgende secties van deze gebruikershandleiding, zult u manieren ontdekken om verschillende montages voor de Kanalen van uw bedrijf, Programma&#39;s of de Integraties tussen Kanalen en MVPDs (Multichannel Video Programma Distributors) te beheren.

>[!IMPORTANT]
>Het TVE-dashboard biedt de mogelijkheid om te schakelen tussen een Basic- en een Advanced Workspace. U kunt dit doen door het pictogram in de rechterbovenhoek in- of uit te schakelen. De Advanced Workspace is gericht op gebruikers met aanzienlijke technische kennis en geavanceerde kennis van de functies die door het Adobe Pass Authentication-productteam worden aangeboden.

![ de werkruimten van het Dashboard van TVE ](../../../assets/tve-dashboard/old-tve-dashboard/tve-basic-advanced-workspace.png)

*Figuur 1: Het dashboard van Adobe Primetime TVE &quot;Basis/Geavanceerde Workspace&quot;drop-down*

## Omgevingen {#authn-environments}

Afhankelijk van de taken die een gebruiker mogelijk moet uitvoeren, moet hij of zij mogelijk schakelen tussen Adobe Pass-verificatieomgevingen. Voor gedetailleerde informatie over de milieu&#39;s van de Authentificatie van Adobe Pass, gelieve het volgende document te raadplegen: [ Begrijpend de milieu&#39;s van de Authentificatie van Adobe Pass ](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

Het Dashboard van TVE verstrekt twee milieu&#39;s genoemd Prequal (Prequalification) en Versie, elk met twee profielen genoemd Staging en Productie, zoals hieronder getoond:

* [ het Veelvoudige Staging ](https://console-prequal.auth-staging.adobe.com/)
* [ Prequale Productie ](https://console-prequal.auth.adobe.com/)
* [ Staging van de Versie ](https://console.auth-staging.adobe.com/)
* [ Productie van de Versie ](https://console.auth.adobe.com/)

Om tussen milieu&#39;s te schakelen, kan de gebruiker op het gewenste milieu klikken dat door de ingang van het drop-down hieronder afgebeelde element wordt vertegenwoordigd:

![ Dashboardmilieu&#39;s van TVE dropdown ](../../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

*Figuur 2: De milieu&#39;s van het Dashboard van Adobe Pass TVE drop-down*

>[!IMPORTANT]
>
>Het is zeer belangrijk om op te merken dat wanneer het aanbrengen van administratieve veranderingen in uw configuratie van de Authentificatie van Adobe Pass door het Dashboard van TVE, wij u sterk adviseren om de hieronder opeenvolging te volgen om correcte functionaliteit te verzekeren.

Beheerwijzigingen aanbrengen in uw Adobe Pass-verificatieconfiguratie via het TVE-dashboard:

* Voer de veranderingen in [ het Opvoeren van de Versie uit en bevestig hen ](http://sp.auth-staging.adobe.com/apitest/api.html).
* Voer de veranderingen in [ Periodieke Productie uit en bevestig hen ](http://sp.auth-staging.adobe.com/apitest/api.html).
* Voer de veranderingen in [ Productie van de Versie uit en bevestig hen ](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Voor de administratieve veranderingen om levend te gaan, moeten de gebruikers aan de &quot;Overzicht en de Duw sectie van Veranderingen&quot;door de knoop te selecteren, die in het bodem-linkergedeelte van sidebar zal verschijnen, om veranderingen te herzien, een beschrijving voor de pas gecreëerde veranderingen toe te voegen en de configuratieupdate te bevestigen door &quot;de Duw Configuratie&quot;te selecteren.

![ de overzicht van het Dashboard van de Boven en duw- bericht ](../../../assets/tve-dashboard/old-tve-dashboard/tve-review-push-notifications.png)

*Figuur 3: Het bericht van de Veranderingen van het Dashboard van Adobe Primetime TVE en van de Duw*

## Secties {#sections}

Gebruikers die werken voor mediabedrijven (programmeurs) hebben via het zijpaneel toegang tot de volgende gedeelten van het TVE-dashboard:

* **Kanalen** - bevat montages met betrekking tot inhoudsleveranciers
* **Programmeurs** - bevat montages met betrekking tot ouderorganisatie die één of veelvoudige **Kanalen** bijeenvoegen
* **Integraties** - bevat montages met betrekking tot de integratie tussen **Kanalen** en **MVPDs**
* **MVPDs** - bevat montages met betrekking tot beschikbaar **MVPDs**
* **Rapporten** - bevat samengevoegde gegevens voor drie types van rapporten: AuthN TTL, AuthZ TTL, SSO
* **Logboek van de Verandering** - bevat de recentste wijzigingen die op de configuratie van het Dashboard van TVE worden toegepast

![ TVE dashboardsecties ](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-sections.png)

*Figuur 4: De secties van het Dashboard van Adobe Primetime TVE*

### Kanalen {#tve-db-channels-section}

In deze sectie kunt u instellingen voor beschikbare kanalen weergeven en bewerken of een nieuwe kanaal maken. Wanneer u op een van de beschikbare kanalen klikt, wordt een scherm met de volgende tabbladen weergegeven:

* **Gegevens van het Kanaal**
   * **Identiteitskaart van het Kanaal** - unieke identiteitskaart van het Kanaal die in ons systeem wordt gebruikt, die ook als &quot;aanvrager Id&quot;wordt bedoeld.
   * **Naam van de Vertoning** - de commerciële naam van het Kanaal.
* **Algemene Montages**
   * **Configuratie van Analytics** - vorm de gebeurtenissen van de Authentificatie van Adobe Pass die aan Adobe Analytics door:sturen. Gelieve te contacteren Adobe voor meer details over hoe identiteitskaart van de Reeks van het Rapport (RSID) moet worden gevormd alvorens deze eigenschap toe te laten.
* **Certificaten**

  Bevat de lijst van certificaten die in de authentificatiestroom naast hun het uitgeven organisatie, het uitgeven datum en vervaldatum worden gebruikt. Deze certificaten fungeren als openbare/persoonlijke sleutels en worden gebruikt voor validatiedoeleinden.
* **Domeinen**

  Bevat de lijst van domeinen waarvan het respectieve Kanaal met de Authentificatie van Adobe Pass zal communiceren.
* **Integraties**

  Bevat de lijst van integraties met beschikbare MVPDs, naast de status van elke integratie die zou kunnen worden toegelaten of niet. Navigeren naar de pagina Integratie is beschikbaar door op een specifieke vermelding te klikken.
* **Geregistreerde Toepassingen**

  Bevat de lijst met toepassingsregistraties. Voor meer details, herzie het document [ Dynamische Beheer van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **de Schema&#39;s van de Douane**

  Bevat de lijst met aangepaste schema&#39;s. Voor meer details, zie [ iOS/tvOS toepassingsregistratie ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) en [ Dynamisch Beheer van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)

#### Domeinen toevoegen/verwijderen {#add-delete-domains}

Als u een nieuw domein voor het geselecteerde kanaal wilt toevoegen, klikt u op de knop Nieuw domein toevoegen onder de lijst Domeinen. Hiermee wordt een nieuw domeinitem gemaakt waarin u de domeinnaam kunt opgeven. Als er al een generischer domein bestaat in de lijst met domeinen, moet u geen nieuw subdomein toevoegen.

![ voeg een nieuw domein aan een geselecteerde kanaalsectie ](../../../assets/tve-dashboard/old-tve-dashboard/add-domain-to-channel-sec.png) toe

*Cijfer: Het lusje van Domeinen in kanalen*

#### Een geregistreerde toepassing op kanaalniveau maken {#create-registered-application-channel-level}

Als u een geregistreerde toepassing op kanaalniveau wilt maken, navigeert u naar het menu Kanalen en kiest u de toepassing waarvoor u een toepassing wilt maken. Na het navigeren naar het tabblad &quot;Geregistreerde toepassingen&quot; klikt u op de knop &quot;Nieuwe toepassing toevoegen&quot;.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-new-app-channel-level.png)

Zoals u in de onderstaande afbeelding ziet, kunt u de volgende velden invullen:

* **Naam van de Toepassing** - de naam van de toepassing

* **Toegewezen aan Kanaal** - zoals hieronder getoond, wat hier lichtjes verschillend is, vergeleken bij de zelfde actie die op het niveau van de Programmer wordt uitgevoerd, is de &quot;Toegewezen drop van Kanalen&quot;die niet wordt toegelaten zodat is er geen optie om de geregistreerde toepassing aan andere dan het huidige kanaal te binden.

* **Versie van de Toepassing** - door gebrek, wordt dit geplaatst aan &quot;1.0.0&quot;maar wij moedigen u hoogst aan om het met uw eigen toepassingsversie te wijzigen. Als u besluit de versie van uw toepassing te wijzigen, kunt u dit het beste doen door een nieuwe geregistreerde toepassing voor de toepassing te maken.

* **Platforms van de Toepassing** - de platforms voor de toepassing om met worden verbonden. U kunt ze allemaal of meerdere waarden selecteren.

* **Namen van het Domein** - de domeinen voor de toepassing om met worden verbonden. De domeinen in de vervolgkeuzelijst zijn een verenigde selectie van alle domeinen vanuit al uw kanalen. U kunt meerdere domeinen selecteren in de lijst. De betekenis van de domeinen is omleiding URLs [ RFC6749 ](https://tools.ietf.org/html/rfc6749). In het clientregistratieproces kan de clienttoepassing vragen om een omleidings-URL te mogen gebruiken voor de voltooiing van de verificatiestroom. Wanneer een clienttoepassing een specifieke omleidings-URL aanvraagt, wordt deze gevalideerd tegen de domeinen die in deze geregistreerde toepassing zijn vermeld die aan de softwareinstructie zijn gekoppeld.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app-channel.png)

Nadat u de velden met de juiste waarden hebt gevuld, moet u op &quot;Gereed&quot; klikken om de toepassing in de configuratie op te slaan.

Gelieve te zijn zich ervan bewust dat er **geen optie is om een reeds gecreeerd toepassing** te wijzigen. Als wordt ontdekt dat iets gecreeerd niet meer aan de vereisten voldoet, zal een nieuwe geregistreerde toepassing moeten worden gecreeerd en worden gebruikt met de cliënttoepassing waarvan het voldoet aan de vereisten.

##### Software-instructies downloaden {#download-software-statement-channel-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Als u klikt op de knop &quot;Downloaden&quot; in de lijst waarvoor een softwareinstructie nodig is, wordt een tekstbestand gegenereerd. Dit bestand bevat iets dat lijkt op de onderstaande voorbeelduitvoer.

![](../../../assets/download-software-statement.png)

De naam van het bestand wordt op unieke wijze geïdentificeerd door het bestand vooraf te voorzien van &quot;software_statement&quot; en het huidige tijdstempel toe te voegen.

Houd er rekening mee dat voor dezelfde geregistreerde toepassing elke keer verschillende softwareinstructies worden ontvangen wanneer op de downloadknop wordt geklikt, maar dat dit de eerder verkregen softwareinstructies voor deze toepassing niet ongeldig maakt. Dit gebeurt omdat ze ter plaatse worden gegenereerd, per verzoek om actie.

Er is één **beperking** betreffende de downloadactie. Als een softwareinstructie wordt gevraagd door kort na het maken van de geregistreerde toepassing op de knop Downloaden te klikken. Deze instructie is nog niet opgeslagen en de configuratiepunt is niet gesynchroniseerd, verschijnt het volgende foutbericht onder aan de pagina.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Dit omvat HTTP 404 niet Gevonden foutcode die van kern wordt ontvangen aangezien identiteitskaart van de geregistreerde toepassing nog niet werd verspreid en de kern geen kennis van het heeft.

De oplossing is, na het creëren van de geregistreerde toepassing, om maximaal 2 minuten op de te synchroniseren configuratie te wachten. Hierna wordt het foutbericht niet meer ontvangen en kan het tekstbestand met de softwareinstructie worden gedownload.

### Programmeurs {#tve-db-programmers-section}

In deze sectie kunt u instellingen weergeven en bewerken voor beschikbare programmeurs of een nieuwe maken. Wanneer u op een van de beschikbare programmeurs klikt, wordt een scherm met de volgende tabbladen weergegeven:

* **Gegevens van de Programmer**
   * **identiteitskaart van de Programmer** - unieke identiteitskaart van de Programmer die in ons systeem wordt gebruikt.
   * **Naam van de Vertoning** - de commerciële naam van de Programmer.
   * **Url van het Logo** - het commerciële embleem van de Programmer eenvormige middelmerkteken (URL).
   * **Voorproef van het Logo** - de commerciële het logovoorbeeld van de Programmer door het van bovengenoemde eenvormige middellocator (URL) te downloaden.

* **Certificaten**

  Bevat de lijst van certificaten die in de authentificatiestroom naast hun het uitgeven organisatie, het uitgeven datum en vervaldatum worden gebruikt. Deze certificaten fungeren als openbare/persoonlijke sleutels en worden gebruikt voor validatiedoeleinden.

* **Kanalen**

  Bevat de lijst met kanalen die bij deze specifieke programmeur horen. U kunt navigeren naar de sectie Kanalen door op een bepaald item te klikken.

* **Geregistreerde toepassingen**

  Bevat de lijst met toepassingsregistraties. Voor meer details, zie [ Dynamisch Beheer van de Registratie van de Cliënt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **de Schema&#39;s van de Douane**

  Bevat de lijst met aangepaste schema&#39;s. Voor meer details, zie [ iOS/tvOS toepassingsregistratie ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

#### Een geregistreerde toepassing op programmeerniveau maken {#create-registered-application-programmer-level}

Ga naar **Programmeurs** > **Geregistreerde Toepassingen** tabel.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-progr-level.png)

In het Geregistreerde lusje van Toepassingen, voegt de klik **Nieuwe Toepassing** toe. Vul de vereiste velden in het nieuwe venster in.

Zoals u in de onderstaande afbeelding ziet, kunt u de volgende velden invullen:

* **Naam van de Toepassing** - de naam van de toepassing

* **Toegewezen aan Kanaal** - de naam van uw kanaal, t </span> waaraan deze toepassing wordt verbonden. Het gebrek dat in het dropdown masker plaatst is **Alle Kanalen.** Met de interface kunt u één kanaal of alle kanalen selecteren.

* **Versie van de Toepassing** - door gebrek, wordt dit geplaatst aan &quot;1.0.0&quot;maar wij moedigen u hoogst aan om het met uw eigen toepassingsversie te wijzigen. Als u besluit de versie van uw toepassing te wijzigen, kunt u dit het beste doen door een nieuwe geregistreerde toepassing voor de toepassing te maken.

* **Platforms van de Toepassing** - de platforms voor de toepassing om met worden verbonden. U kunt ze allemaal of meerdere waarden selecteren.

* **Namen van het Domein** - de domeinen voor de toepassing om met worden verbonden. De domeinen in de vervolgkeuzelijst zijn een verenigde selectie van alle domeinen vanuit al uw kanalen. U kunt meerdere domeinen selecteren in de lijst. De betekenis van de domeinen is omleiding URLs [ RFC6749 ](https://tools.ietf.org/html/rfc6749). In het clientregistratieproces kan de clienttoepassing vragen om een omleidings-URL te mogen gebruiken voor de voltooiing van de verificatiestroom. Wanneer een clienttoepassing een specifieke omleidings-URL aanvraagt, wordt deze gevalideerd tegen de domeinen die in deze geregistreerde toepassing zijn vermeld die aan de softwareinstructie zijn gekoppeld.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app.png)

Nadat u de velden met de juiste waarden hebt gevuld, moet u op &quot;Gereed&quot; klikken om de toepassing in de configuratie op te slaan.

Gelieve te zijn zich ervan bewust dat er **geen optie is om een reeds gecreeerd toepassing** te wijzigen. Als wordt ontdekt dat iets gecreeerd niet meer aan de vereisten voldoet, zal een nieuwe geregistreerde toepassing moeten worden gecreeerd en worden gebruikt met de cliënttoepassing waarvan het voldoet aan de vereisten.

##### Software-instructies downloaden {#download-software-statement-programmer-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Als u klikt op de knop &quot;Downloaden&quot; in de lijst waarvoor een softwareinstructie nodig is, wordt een tekstbestand gegenereerd. Dit bestand bevat iets dat lijkt op de onderstaande voorbeelduitvoer.

![](../../../assets/download-software-statement.png)

De naam van het bestand wordt op unieke wijze geïdentificeerd door het bestand vooraf te voorzien van &quot;software_statement&quot; en het huidige tijdstempel toe te voegen.

Houd er rekening mee dat voor dezelfde geregistreerde toepassing elke keer verschillende softwareinstructies worden ontvangen wanneer op de downloadknop wordt geklikt, maar dat dit de eerder verkregen softwareinstructies voor deze toepassing niet ongeldig maakt. Dit gebeurt omdat ze ter plaatse worden gegenereerd, per verzoek om actie.

Er is één **beperking** betreffende de downloadactie. Als een softwareinstructie wordt gevraagd door kort na het maken van de geregistreerde toepassing op de knop Downloaden te klikken. Deze instructie is nog niet opgeslagen en de configuratiepunt is niet gesynchroniseerd. Hieronder wordt het volgende foutbericht weergegeven.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Dit omvat HTTP 404 niet Gevonden foutcode die van kern wordt ontvangen aangezien identiteitskaart van de geregistreerde toepassing nog niet werd verspreid en de kern geen kennis van het heeft.

De oplossing is, na het creëren van de geregistreerde toepassing, om maximaal 2 minuten op de te synchroniseren configuratie te wachten. Hierna wordt het foutbericht niet meer ontvangen en kan het tekstbestand met de softwareinstructie worden gedownload.

### Integraties {#tve-db-integrations-sec}

Deze sectie staat het bekijken en het uitgeven montages voor integratie tussen Kanalen en beschikbare MVPDs of het creëren van nieuwe toe. Wanneer u op een van de beschikbare integraties klikt, wordt één pagina geretourneerd wanneer u de Basic Workspace gebruikt of een scherm met de volgende tabbladen wanneer u de Advanced Workspace gebruikt:

* **Gegevens van de Integratie**
   * **Identiteitskaart van de Integratie** - het resultaat van het toevoegen van unieke identiteitskaart MVPDs aan unieke identiteitskaart van het Kanaal die door het &quot;_&quot;karakter wordt gescheiden.
   * **Naam van de Vertoning van het Kanaal** - de commerciële naam van het Kanaal.
   * **Identiteitskaart van het Kanaal** - unieke identiteitskaart van het Kanaal die in ons systeem wordt gebruikt, die ook als &quot;aanvrager Id&quot;wordt bedoeld.
   * **de Naam van de Vertoning van MVPD** - de commerciële naam van MVPD.
   * **identiteitskaart van MVPD** - MVPD unieke identiteitskaart die in ons systeem wordt gebruikt.
* **Algemene Montages**
   * **Sleutels van Meta-gegevens van de Gebruiker** - vorm meta-gegevenssleutels beschikbaar voor de specifieke integratie.
   * **Platform Specifieke Montages** - vorm verschillende montages aan een specifiek platform (bijvoorbeeld, TTLs, SSO, en IFrames).

* **Montages van de Authentificatie**
   * Bevat instellingen die verwant zijn aan de Adobe Pass-verificatiefunctie.
* **Montages van de Vergunning**
   * Bevat instellingen die betrekking hebben op de autorisatiefunctie voor Adobe Pass-verificatie.
* **Logout Montages**
   * Bevat instellingen die verwant zijn aan de functie voor het afmelden van Adobe Pass-verificatie.

#### Integratie maken {#create-integration}

Volg onderstaande stappen om een nieuwe integratie tot stand te brengen:

* Klik op de knop Nieuwe integratie toevoegen
* zoeken en een kanaal selecteren
* zoeken en een MVPD selecteren
* wachten tot TVE-dashboard &quot;Integratie-id&quot; berekent en de beschikbare MVPD-eindpunten weergeven
* selecteer authentificatie, vergunning en logout eindpunten of gebruik de standaardwaarden
* klik op de knop Integratie maken
* afhankelijk van de MVPD-instellingen kan een pop-up verschijnen en om aanvullende eigenschappen vragen, die de MVPD vooraf had moeten verschaffen, anders wordt een omleiding naar de nieuwe integratiepagina uitgevoerd

![](../../../assets/tve-dashboard/old-tve-dashboard/new-integration-window.png)



*Figuur 5. Het venster Nieuwe integratie van het Adobe Primetime TVE-dashboard*


#### Integratie bijwerken {#update-integration}

Als u een bestaande integratie wilt bijwerken, klikt u op de tabelvermelding voor die specifieke integratie vanuit de sectie Integraties of de sectie Kanalen, die een tabblad Integraties bevat.

Wanneer het gebruiken van de BasisWorkspace wijze, zal deze sectie het zien van en het uitgeven van de meest algemeen bijgewerkte montages, zoals authentificatie en toestemmingstoken TTLs (tijd-aan-levende), evenals iFrame montages toestaan. Gelieve te letten op dat de montages van TTL voor de integratie met MVPDs kunnen ontbreken die dynamisch Gedefinieerde Symbolische Persistentie TTL (zie ingang 1.19 van [ de Vereisten van de Integratie van MVPD ](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md)) steunen.



Als u de modus Geavanceerde Workspace gebruikt, kunt u in deze sectie minder gebruikelijke instellingen bekijken en bewerken.



In het geval van zowel de modi Standaard als Geavanceerd van Workspace kunnen deze instellingen op platformniveau worden gewijzigd (selecteer bijvoorbeeld een aangepaste waarde voor het teken voor de machtiging-TTL op Android, standaard op elk ander platform).



>[!IMPORTANT]
>Het is belangrijk dat u de keten van instellingen voor overerving begrijpt: MVPD -> MVPD Endpoint -> Integration -> Platform, waar Platform de meest specifieke waarde heeft en MVPD de meest algemene standaard.

![](../../../assets/tve-dashboard/old-tve-dashboard/inheritance-chain-component.png)


*Figuur 6. De eigenschapoverervingsketen van Adobe Primetime TVE-dashboard*


#### Platformspecifieke instellingen {#platform-sp-settings}

Deze subsectie kan worden gebruikt om de montages voor specifieke platforms met voeten te treden. De beschikbare platforms zijn:

* **Alle Platforms** - plaats waarden die op alle platforms ongeacht de implementaties van de Programmer zullen worden toegepast voor het geval dat er geen andere waarden die voor een specifiek platform worden geplaatst zijn.
* **Android** - plaats waarden die op de implementaties van de Programmer over de Authentificatie van Adobe Pass Android SDK zullen worden toegepast.
* **Clientless REST API** - plaats waarden die op de implementaties van de Programmer over de VERTONING API van de Authentificatie van Adobe Pass zullen worden toegepast.
* **Vuur TV** - plaats waarden die op de implementaties van de Programmer over de Authentificatie van Adobe Pass FireTV SDK zullen worden toegepast.
* **Flash SDK** - Dit platform is afgekeurd. **afgekeurd**
* **JavaScript SDK** - plaats waarden die op de implementaties van de Programmer over de Authentificatie JavaScript SDK van Adobe Pass zullen worden toegepast.
* **Roku** - plaats waarden die op de implementaties van de Programmer over de Authentificatie van Adobe Pass REST API zullen worden toegepast en die &quot;Roku&quot;als apparatentype verzenden. In het geval van Roku-apparaten heeft dit voorrang op de waarden die zijn ingesteld voor het Clientless REST API-platform.
* **Xbox native SDK** - Dit platform is afgekeurd. **afgekeurd**
* **Xbox 360 REST API** - plaats waarden die op de implementaties van de Programmer over de VERWIJZING van de Authentificatie van Adobe Pass API zullen worden toegepast en die &quot;xbox&quot;als apparatentype verzenden. Dit heeft voorrang op de waarden die zijn ingesteld voor het Clientless REST API-platform in het geval van Xbox 360-apparaten.
* **Xbox One REST API** - plaats waarden die op de implementaties van de Programmer over de VERTONING van de Authentificatie van Adobe Pass API zullen worden toegepast en die &quot;xboxOne&quot;als apparatentype verzenden. Dit neemt belangrijkheid over de waarden die voor het Clientless REST API platform in het geval van XboxOne apparaten worden geplaatst.
* **iOS** - plaats waarden die op de implementaties van de Programmer over de Authentificatie van Adobe Pass iOS SDK zullen worden toegepast.
* **tvOS** - plaats waarden die op de implementaties van de Programmer over de Authentificatie tvOS SDK van Adobe Pass zullen worden toegepast.


![](../../../assets/tve-dashboard/old-tve-dashboard/platform-sp-settings.png)

*Figuur 7. De specifieke instellingen van het Adobe Primetime TVE-dashboard Platform*


#### Eenmalige aanmelding platform inschakelen {#enable-platform-sso}

Volg de onderstaande stappen om Single Sign On in-/uit te schakelen voor een specifieke integratie en een specifiek platform:

* zorg ervoor u de Geavanceerde wijze van Workspace gebruikt
* naar de gewenste integratie gaan
* navigeren aan het **Algemene lusje van Montages**
* Selecteer het gewenste platform waarop u Single Sign On wilt in- of uitschakelen
* knevel **toelaten Enig Teken** vlag aan de gewenste waarde (ja/Nr)

  >[!IMPORTANT]
  >Het is belangrijk om op te merken dat **toelaten Enig Teken** vlag slechts voor iOS, tvOS, Roku en platforms FireTV en slechts voor integratie met MVPDs beschikbaar is die Enig Teken voor die platforms steunen.

* knevel de **1} vlag van de Toestemming van het Platform van de Afdwingen {aan de gewenste waarde (ja/Nr)**

  >[!IMPORTANT]
  >Het is belangrijk om op te merken dat **de vlagcontroles van de Toestemming van het Platform van de Afdwingen** als het besluit van de gebruiker om platformtoegang tot zijn/haar abonnement van TV-leverancier toe te staan of te ontkennen al dan niet zal worden afgedwongen. Gezien het scenario wanneer **Enig Teken** toelaten vlag aan &quot;ja&quot;wordt geplaatst, **wordt de vlag van de Toestemming van het Platform van de Afdwingen** ook geplaatst aan &quot;ja&quot;en de gebruiker verkiest om platformtoegang tot zijn/haar abonnement van de Leverancier van TV te ontkennen, dan zal de respectieve toepassing (kanaal) niet het teken van de Authentificatie van Adobe Pass kunnen gebruiken dat door een andere toepassing (kanaal) wordt verkregen.


#### Verificatie op thuisbasis inschakelen {#enable-hba}

Gelieve te volgen de stappen hieronder om Home-Base Authentificatie voor **OAuth2** gebaseerde MVPDs toe te laten/onbruikbaar te maken:

* zorg ervoor u de Geavanceerde wijze van Workspace gebruikt
* naar de gewenste integratie gaan
* navigeer aan **Montages van de Authentificatie** lusje
* navigeer aan **AuthN Dynamische Regels** sub-tab
* knevel de **markering van HBA van de Poging** aan de gewenste waarde (ja/Nr)


>[!IMPORTANT]
>Houd er rekening mee dat de waarde van &quot;HBA AuthN TTL&quot; nooit mag worden overschreven, anders zou de stroom van de vergunning onverwacht kunnen mislukken.

Bereik uit aan **tve-support@adobe.com** voor informatie bij het toelaten van BasisAuthentificatie voor SAML gebaseerde MVPDs.

### MVPD&#39;s {#tve-db-mvpds-sec}

Deze sectie staat het bekijken montages voor beschikbare MVPDs toe. Wanneer u op een van de beschikbare MVPD&#39;s klikt, wordt een scherm met de volgende tabbladen weergegeven:

* **Gegevens van MVPD**
   * **identiteitskaart van MVPD** - MVPD unieke identiteitskaart die in ons systeem wordt gebruikt.
   * **Naam van de Vertoning** - de commerciële naam van MVPD die in de plukker van de gebruiker zou kunnen worden gebruikt.
   * **Url van het Logo** - het commerciële embleem van MVPD eenvormige middellocator (URL).
   * **Voorproef van het Logo** - de commerciële het logovoorbeeld van MVPD door het van bovengenoemde eenvormige middellocator (URL) te downloaden.
* **Algemene montages**
   * **Sleutels van Meta-gegevens van de Gebruiker**
      * Metagegevenstoetsen beschikbaar voor de specifieke MVPD.
   * **Eigenschappen van de Gegevens van de Cliënt**
      * **Auth/Aggregator** - als reeks aan &quot;ja&quot;, dan een nieuw authentificatietoken nodig is voor elk nieuw Kanaal de gebruiker probeert toegang te hebben.
      * **Passieve AuthN Toegelaten** - als de Auth/de vlag van de Samenvoeger aan &quot;ja&quot;wordt geplaatst en als Passive AuthN Toegelaten aan &quot;ja&quot;wordt geplaatst, dan zal het authentificatieproces met een ander Kanaal op de achtergrond zonder de behoefte aan volledige browser omleiding en de plukker gebeuren die wordt getoond.
      * **Auth/browser zitting** - als reeks aan &quot;ja&quot;, dan zal de gebruiker na het sluiten van browser worden geregistreerd. Als deze optie is ingesteld op &quot;Nee&quot;, kan de gebruiker de browser opnieuw starten en blijven aanmelden.
      * **IFrame Vereiste** - als reeks aan &quot;ja&quot;dan wijst het erop dat het login van MVPD venster een iFrame vereist. De velden &quot;iFrame-breedte&quot; en &quot;iFrame-hoogte&quot; geven de grootte aan die nodig is voor de iFrame die de MVPD-aanmeldingspagina laadt.
* **Montages van de Authentificatie**
   * **Uitgezochte Eindpunt**
      * Dit veld geeft het (de) verificatieeindpunt(en) aan dat (die) door de MVPD is (zijn) bekendgemaakt. Het eindpunt kan verschillen volgens het gebruikte authentificatieprotocol.
   * **AuthN Algemene Montages**
      * Dit subtabblad geeft het verificatieprotocol weer dat door de MVPD wordt gebruikt en informatie over het protocol.
   * **AuthN Certificates**
      * Op dit subtabblad worden de certificaten weergegeven die de MVPD gebruikt in de verificatiestroom, naast de organisatie van de uitgever, de datum van afgifte en de vervaldatum. Deze certificaten fungeren als openbare/persoonlijke sleutels en worden gebruikt voor validatiedoeleinden.
   * **AuthN Dynamische Regels**
      * Op dit subtabblad worden de regels weergegeven die van toepassing zijn op het verificatieproces. Door op het Verzoek/de Reactie/Token van het diagram te drukken, kunt u als benadrukte parameters zien die op dat deel van de authentificatiestroom worden toegepast.
* **Montages van de Vergunning**
   * **Uitgezochte Eindpunt**
      * In dit veld wordt het door de MVPD blootgestelde eindpunt van de vergunning vermeld. Het eindpunt kan verschillen afhankelijk van het gebruikte vergunningsprotocol. De beschikbare vergunningsprotocollen zijn SOAP, REST (voor cliëntless apparaten), SAML, XACML en OAUTH.
   * **AuthZ Algemene Montages**
      * Dit subtabblad geeft het machtigingsprotocol weer dat door de MVPD wordt gebruikt en informatie over het protocol.
      * **Preflight Configuratie**
         * Het beschrijft het aantal middelen die door een MVPD in één enkele vraag kunnen worden gepreautoriseerd, het gebruikte model PreFlight, evenals de onderbrekingsdrempel. Soms kan het aantal bronnen voor een bepaalde integratie verschillen. Dit kan worden beheerd door het &quot;**Max Aantal Preflight Middelen**&quot;bezit uit te geven, beschikbaar onder het Algemene lusje van Montages. Deze eigenschap is alleen beschikbaar voor een bepaalde integratie en als deze is ingesteld, wordt deze gebruikt in plaats van de waarde die is gedefinieerd in Autorisatie-instellingen -> PreFlight-configuratie -> PreFlight Max Resources.
      * **DOS Bescherming**
         * Het beschrijft de ontkenning-van-dienst bescherming op het de vergunningseindpunt van MVPD. Voor een exacte beschrijving van elk veld bekijkt u de knopinfo door de muisaanwijzer op de velden DOS Protection te plaatsen.
      * Als MVPD a **TempPass** is, dan bevat de **Algemene Montages AuthZ** ook informatie betreffende de duur TempPass.
      * Als MVPD a **FlexibleTempPass** is, dan bevat de **Algemene Montages AuthZ** ook informatie betreffende de duur TempPass, maximumaantal middelen en identificerend gebied (zie het beeld hieronder).
   * **Certificaten AuthZ**
      * Op dit subtabblad worden de certificaten weergegeven die de MVPD in de vergunningsstroom gebruikt, naast de organisatie van de uitgevende instelling, de datum van afgifte en de vervaldatum. Deze certificaten fungeren als openbare/persoonlijke sleutels en worden gebruikt voor validatiedoeleinden.
   * **AuthZ Dynamische Regels**
      * Dit subtabblad bevat de regels die van toepassing zijn op het machtigingsproces. Door op het van het diagram **Verzoek/Reactie/Symbolisch** te drukken, kunt u als benadrukte parameters zien die op dat deel van de vergunningsstroom worden toegepast.
* **Logout Montages**
   * **Uitgezochte Eindpunt**
      * Dit gebied wijst op het logout eindpunt dat door MVPD wordt blootgesteld. De verstrekte protocollen kunnen of SAML of OAuth2 zijn.
      * **Logout Algemene Montages**
         * Dit subtabblad geeft het logout-protocol weer dat door de MVPD wordt gebruikt en informatie over het protocol.
         * **Vereist Ondertekende Reactie van de Logout** - als reeks aan &quot;ja&quot;, dan moet de reactie door een vertrouwd op certificaat worden ondertekend.
      * **Logout Certificates**
         * Op dit subtabblad worden de certificaten weergegeven die de MVPD in de logout-flow gebruikt, naast de organisatie van de uitgever, de datum van afgifte en de vervaldatum. Deze certificaten fungeren als openbare/persoonlijke sleutels en worden gebruikt voor validatiedoeleinden.
      * **Logout Dynamische Regels**
         * Op dit subtabblad worden de regels weergegeven die van toepassing zijn op het afmeldingsproces. Door op het van het diagram **Verzoek/Reactie/Symbolisch** te drukken, kunt u als benadrukte parameters zien die op dat deel van de logout stroom worden toegepast.

### Rapporten {#tve-db-reports-sec}

Om aan deze sectie te navigeren, gelieve &quot;Rapporten&quot;in het &quot;[ menu van de Secties van het Dashboard ](#sections)&quot;te klikken. Dit zal aan een scherm met 3 lusjes navigeren, die in detail in de volgende subsecties zullen worden voorgesteld: [ AuthN TTL Rapporten ](#authn-ttl-reports), [ de Rapporten van TTL AuthZ ](#authz-ttl-reports), [ Sso- Rapporten ](#sso-reports).

Deze sectie staat het bekijken en het uitvoeren van samengevoegde gegevens voor verscheidene types van rapporten voor uw integratie/s van het Kanaal/van s met diverse MVPDs over alle platforms toe.

#### Platforms {#report-platforms}

Alle rapporten groeperen waarden over de volgende platforms:

**BROWSERS**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via Adobe Pass Authentication JavaScript SDK.

**MOBILE: IOS**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via Adobe Pass Authentication iOS SDK.

**MOBILE: ANDROID**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via Adobe Pass Authentication Android SDK.

**MOBILE: ANDERE**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via de Adobe Pass Authentication REST API die is ontwikkeld voor mobiele apparaten.

**TVCD: ROKU**
Hiermee geeft u waarden weer die via de Adobe Pass Authentication REST API op de programmeerimplementaties worden toegepast en die &quot;Roku&quot; verzenden als apparaattype.

**TVCD: FIRETV**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via Adobe Pass Authentication FireTV SDK.

**TVCD: APPLETV**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via Adobe Pass Authentication tvOS SDK.

**TVCD: ANDERE**
Geeft waarden weer die worden toegepast op de programmeerimplementaties via de Adobe Pass Authentication REST API die is ontwikkeld voor apparaten die zijn aangesloten op een televisie.

**PLATFORM: ONBEKEND**
De waarden van vertoningen die op de implementaties van de Programmer zullen worden toegepast waarvoor de diensten van de Authentificatie van Adobe Pass een onbekend apparatentype ontdekken.

Herzie het mechanisme van [ die cliëntinformatie ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) tot de Authentificatie van Adobe Pass REST APIs of SDKs voor meer details over overgaan hoe te om het gewenste apparatentype (bijvoorbeeld, &quot;Roku&quot;) te verzenden.

Alle rapporten verzamelen gegevens verwerkte waarden op basis van de configuratie die voor elke milieu van de Authentificatie van Adobe Pass specifiek is. Daarom kunt u verschillende rapportgegevens verwachten wanneer u overschakelt tussen verschillende TVE-dashboardomgevingen.

Gelieve te herzien de [ sectie van Milieu&#39;s ](#authn-environments) voor meer details met betrekking tot de beschikbare milieu&#39;s van de Authentificatie van Adobe Pass.


##### Het selecteren van specifieke Kanalen/MVPDs {#selecting-specific-channels-mvpds}

Alle rapporten staan het gebruiken van filters toe door specifieke Kanalen te selecteren of specifieke MVPDs te selecteren die in de resulterende rapporten moeten worden omvat.

Om één of veelvoudige Kanaal/s te selecteren gelieve **dropdown lijst** te gebruiken die na &quot;Kanalen voor rapport&quot;etiket worden geplaatst. Zie figuur 8./9./10. afbeeldingen van beneden.

Om één of veelvoudige MVPD/s te selecteren gelieve **dropdown lijst** te gebruiken die na &quot;MVPDs voor rapport&quot;etiket wordt geplaatst. Zie figuur 8./9./10. afbeeldingen van beneden.

Door gebrek, worden de gegevens samengevoegd over alle Kanalen van uw bedrijf (&quot;Alle Kanalen&quot;) en MVPDs waarmee zij (&quot;Alle MVPDs&quot;) geïntegreerd zijn.

Als u de optie Alle kanalen of Alle MVPD&#39;s uitschakelt zonder specifieke opties te kiezen, wordt in de interface een tijdelijke aanduiding &quot;Geen gegevens beschikbaar&quot; weergegeven.


##### Rapport exporteren {#export-report}

Met alle rapporten kunt u gegevens exporteren in een CSV-bestand (comma-separated Values).

Als u gegevens wilt exporteren, klikt u op de knop Rapport exporteren in de rechterbovenhoek van het venster. Zie figuur 8./9./10. afbeeldingen van beneden.

Een dossier genoemd **Report.csv** zal automatisch aan uw computer worden gedownload. Controleer daarom of de browserinstellingen het downloaden van bestanden toestaan.

Het ladingspictogram &quot;het Uitvoeren van Gegevens&quot;zal op het scherm aanwezig zijn terwijl het Report.csv- dossier wordt gegevens verwerkt, dat **aan een paar notulen** afhankelijk van de grootte van gegevens kan opnemen u wilt uitvoeren.

#### AuthN TTL-rapporten (#authn-ttl-reports)

Dit rapport toont tijd-aan-Levende (TTL) van het authentificatietoken dat voor uw integratie/s van Kanaal/s met diverse MVPDs over alle platforms wordt gevormd.

Het authentificatietoken Tijd-aan-Levend, die ook als **wordt bedoeld AuthN TTL**, wordt getoond in menselijke leesbare waarden zoals: **dagen, uren, notulen, seconden**.

In termen van gebruikerservaring, staan de rapporten van TTL AuthN u toe om visueel te inspecteren de hoeveelheid tijd een gebruiker zal worden voor authentiek verklaard gebruikend een specifieke MVPD en een specifiek platform.

Als u naar dit type rapport wilt navigeren, klikt u op het tabblad &quot;AuthN TTL Reports&quot; in de sectie &quot;Reports&quot;.

![ de rapporten van TTL van AuthN ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Figuur 8: Het Dashboard van Adobe Primetime TVE AuthN TTL- Rapport tabel*

De tabel AuthN TTL-rapporten bevat pagina&#39;s die horizontaal en verticaal schuifbaar zijn, afhankelijk van de schermgrootte.

Voor het geval u overweegt makend een verandering in een waarde AuthN TTL, gelieve de [ sectie van de Integraties ](#tve-db-integrations-sec) te herzien.

>[!IMPORTANT]
>&quot;**die door MVPD**&quot;wordt geplaatst placeholder wordt gebruikt in gevallen wanneer MVPD zal zijn die de waarde AuthN TTL en niet de configuratie van de Authentificatie van Adobe Pass afdwingt.


#### AuthZ TTL-rapporten {#authz-ttl-reports}

Dit rapport toont tijd-aan-Levende (TTL) van het toestemmingstoken dat voor uw integratie/s van Kanaal/s met diverse MVPDs over alle platforms wordt gevormd.

Het toestemmingstoken Tijd-aan-Levend, die ook als **wordt bedoeld AuthZ TTL**, wordt getoond in menselijke leesbare waarden zoals: **dagen, uren, notulen, seconden**.

Wat gebruikerservaring betreft, staan de rapporten van TTL AuthZ u toe om visueel te inspecteren de hoeveelheid tijd een gebruiker zal worden geautoriseerd met inachtneming van een specifieke MVPD en een specifiek platform.

Als u naar dit type rapport wilt navigeren, klikt u op het tabblad &quot;AuthZ TTL Reports&quot; in de sectie &quot;Reports&quot;.

![ de rapporten van TTL van AuthZ ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Figuur 9. Het Adobe Primetime TVE-dashboard AuthZ TTL-rapport, tabblad*

De tabel AuthZ TTL-rapporten bevat pagina&#39;s en is horizontaal en verticaal schuifbaar, afhankelijk van de schermgrootte.

Als u overweegt makend een verandering in een waarde AuthZ TTL, zie de [ sectie van de Integraties ](#tve-db-integrations-sec).

>[!IMPORTANT]
>&quot;**die door MVPD**&quot;wordt geplaatst placeholder wordt gebruikt in gevallen wanneer MVPD de waarde zal zijn die AuthZ TTL en niet de configuratie van de Authentificatie van Adobe Pass afdwingt.


#### SSO-rapporten {#sso-reports}

Dit rapport toont Single Sign-On (SSO) status die voor uw integratie/s van Kanaal/s met diverse MVPDs over alle platforms wordt gevormd.

De Enige Sign-On status, die ook als **status SSO** wordt bedoeld, wordt getoond als tri-staat met de volgende mogelijke waarden: **SSO Uitgeschakeld, SSO Toegelaten, SSO Onzeker**.

Wat de gebruikerservaring betreft, kunt u met de SSO-rapporten visueel de verwachte gebruikerverificatie-SSO-ervaring inspecteren, rekening houdend met een specifieke MVPD en een specifiek platform.

Om aan dit type van rapport te navigeren gelieve te klikken &quot;**Sso- Rapporten**&quot;lusje van &quot;**Rapporten**&quot;sectie.


![ de rapportenLusje van DashboardSSO van TVE ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)


*Figuur 10: Het Dashboard van Adobe Primetime TVE Sso- Rapporten tabel*

De tabel SSO-rapporten bevat pagina&#39;s en is horizontaal en verticaal schuifbaar, afhankelijk van de schermgrootte.

Voor het geval u overweegt het aanbrengen van een verandering in een status SSO, gelieve de [ sectie van de Integraties ](#tve-db-integrations-sec) te herzien.

>[!IMPORTANT]
>&quot;**SSO onzekere**&quot;placeholder wordt gebruikt in gevallen waar SSO wordt toegelaten en mogelijk, maar de montages van het gebruikersplatform/gebruikersbesluiten (bijvoorbeeld, gebruikersbrowser optie om derderdekoekjes te blokkeren, gebruiker verkiest om platformtoegang tot zijn/haar abonnement van de Leverancier van TV) of de montages van MVPD (bijvoorbeeld, MVPD die authentificatie voor elk Kanaal) verzoekt zouden SSO kunnen verhinderen te gebeuren.

### Logbestand wijzigen {#tve-db-changelog-sec}

In deze sectie wordt een lijst weergegeven met alle wijzigingen die via het TVE-dashboard worden doorgevoerd in de Adobe Pass-verificatieomgeving en -configuratie.

Er zijn kolommen die de pushdatum aangeven, de gebruiker die de wijziging heeft uitgevoerd en de status van de push.

Deze sectie staat ook de vergelijking van twee lijstingangen toe om de specifieke wijzigingen te beperken u wilt inspecteren en zelfs de vergelijking als postpunt delen.

### Feedback {#tve-db-feedback-sec}

In deze sectie kunnen gebruikers feedback verzenden. Voer de volgende stappen uit om feedback te geven aan het productteam voor Adobe Pass-verificatie:

* Klik op de knop Feedback rechts op het scherm
* onderwerp invoeren
* Voer het bericht in
* upload zo nodig een screenshot naar het bericht door op de knop &quot;Schermafbeelding uploaden&quot; te klikken
* Klik op Verzenden

![ tve dashboard terugkoppelt vorm ](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-feedback.png)

*Figuur 11: De sectie van de Terugkoppeling van het Dashboard van Adobe Primetime TVE*

Zie de onderstaande koppelingen voor instructies over het vastleggen van schermafbeeldingen:

* [ hoe te om schermafbeeldingen op Vensters ](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7) te vangen

* [ hoe te om screenshots op Mac ](https://support.apple.com/en-us/HT201361) te vangen

## Problemen oplossen {#tve-db-troubleshoot}

### Onderhoudsmodus {#maintenance-mode}

![ TVE App op onderhoudswijze ](../../../assets/tve-dashboard/old-tve-dashboard/tveapp-maintenance-mode.png)


*Cijfer: TVE App op de wijze van het Onderhoud*


Als het TVE-dashboard zich in de &quot;onderhoudsmodus&quot; bevindt, kunnen gebruikers geen nieuwe wijzigingen bekijken of aanbrengen.

Als dit gebeurt, moet u wachten tot het technische team voor Adobe Pass-verificatie het onderhoud van het TVE-dashboard heeft voltooid.

### Verminderde toestand {#degraded-state}

![ TVE App in degraded staat ](../../../assets/tve-dashboard/old-tve-dashboard/tve-degraded-state.png)


*Cijfer: De Toepassing van TVE in degraded staat*

Als het TVE-dashboard zich in een &#39;gedegradeerde status&#39; bevindt, zullen gebruikers niet over zoekmogelijkheden en sorteermogelijkheden beschikken, maar kunnen gebruikers wel nieuwe wijzigingen bekijken of aanbrengen.

Als dit gebeurt, moet u wachten tot het technische team voor Adobe Pass-verificatie het onderhoud van het TVE-dashboard heeft voltooid.
