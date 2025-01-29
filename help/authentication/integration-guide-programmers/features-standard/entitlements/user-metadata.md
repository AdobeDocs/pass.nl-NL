---
title: Metagegevens gebruiker
description: Metagegevens gebruiker
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---

# Metagegevens gebruiker {#user-metadata}

>[!IMPORTANT]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

De meta-gegevens van de gebruiker verwijzen naar gebruiker-specifieke [ attributen ](#attributes) (b.v., zip codes, ouderlijke classificaties, gebruiker IDs, enz.) die door MVPDs worden gehandhaafd en aan Programmers door de Authentificatie van Adobe Pass [ worden verstrekt REST API V2 ](#apis).

De meta-gegevens van de gebruiker kunnen worden gebruikt om verpersoonlijking voor gebruikers te verbeteren, maar zouden ook voor analyses kunnen worden gebruikt. Een programmeur kan bijvoorbeeld de ZIP-code van een gebruiker gebruiken om gelokaliseerde nieuws- of weerupdates te leveren of om ouderlijke controle af te dwingen.

Adobe Pass-verificatie normaliseert de waarden van gebruikersmetagegevens wanneer MVPD&#39;s gegevens in verschillende indelingen leveren. Ook, voor bepaalde attributen (b.v., postcode), kunnen de waarden [ worden gecodeerd ](#encryption) gebruikend het certificaat van een Programmer.

De Authentificatie van Adobe Pass laat Programmeurs toe om de gebruikersmeta-gegevens te herzien die in hun integratie van MVPD beschikbaar worden gesteld en [ beheert ](#management) hen door het [ Dashboard van Adobe Pass TVE ](https://experience.adobe.com/#/pass/authentication).

## Attributen van metagegevens van gebruiker {#attributes}

De volgende lijst maakt een lijst van enkele attributen van gebruikersmeta-gegevens die aan Programmeurs ter beschikking worden gesteld:

| Sleutel | Type | Monster | Vereist codering | Beschrijving | Details |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | String | &quot;1o7241p&quot; | Nee | Account-id. | De kenmerkwaarde kan een huishoudelijke identificatiecode of een subrekeningidentificatiecode zijn. De waarde `userID` verschilt van de waarde `householdID` als de MVPD subaccounts ondersteunt en de huidige gebruiker niet de primaire rekeninghouder is. |
| `upstreamUserID` | String | &quot;1o7241p&quot; | Nee | Account-id voor gelijktijdige bewaking. | De kenmerkwaarde kan worden gebruikt om gelijktijdige limieten in te stellen voor MVPD- en programmeersites en -apps. De waarde `upstreamUserID` is gelijk aan de waarde `userID` voor de meeste MVPD&#39;s. |
| `householdID` | String | &quot;1o7241p&quot; | Nee | Rekeningcode voor ouderlijke controle. | De kenmerkwaarde kan worden gebruikt om onderscheid te maken tussen het gebruik van huishoudens en subrekeningen. Soms kan het als ouderlijke controlesubstituut worden gebruikt als ware classificaties niet beschikbaar zijn, als de gebruiker met de huisrekening werd aangemeld, kunnen zij zien, anders, zou de beoordeelde inhoud niet worden getoond. Er zijn veel verschillen tussen de MVPD&#39;s voor de manier waarop dit wordt weergegeven (bijvoorbeeld de gebruikersnaam van het huishouden, het hoofd van de huiselijke id, het hoofd van de huiselijke vlag, enz.), als de MVPD geen subrekeningen ondersteunt, zal dit gelijk zijn aan `userID` . |
| `primaryOID` | String | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Nee | Account-id. | Het kenmerk is specifiek voor AT&amp;T. De `primaryOID` -waarde is gelijk aan de `userID` -waarde wanneer de `typeID` -waarde wordt ingesteld op &quot;Primair&quot;. |
| `typeID` | String | &quot;Primair&quot; | Nee | Kenmerk dat aangeeft of de huidige gebruiker een primaire of secundaire rekeninghouder is. | Het kenmerk is specifiek voor AT&amp;T. De `primaryOID` -waarde is gelijk aan de `userID` -waarde wanneer de `typeID` -waarde wordt ingesteld op &quot;Primair&quot;. |
| `is_hoh` | String | &quot;1&quot; | Nee | Kenmerk dat aangeeft of de huidige gebruiker al dan niet het hoofd van het huishouden is. | Het kenmerk is specifiek voor Synacor. |
| `hba_status` | Boolean | &quot;true&quot; | Nee | Kenmerk dat aangeeft of de huidige gebruiker door HBA is geverifieerd of niet. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Boolean | &quot;true&quot; | Nee | Kenmerk dat aangeeft of het huidige apparaat het scherm kan spiegelen. | Het kenmerk is specifiek voor Spectrum. |
| `zip` | Array | \[&quot;77754&quot;, &quot;12345&quot;\] | Ja | Postcode van gebruiker. | De kenmerkwaarde kan worden gebruikt om gelokaliseerd nieuws, weerupdates of sportevenementen te leveren. De waarde `zip` vertegenwoordigt gevoelige gegevens die juridische overeenkomsten met de MVPD vereisen. Wanneer de code is gecodeerd, is de representatie van de `zip` -toets een `String` in plaats van een `Array` -toets. |
| `encryptedZip` | String | &quot;&quot; | Ja | Gecodeerde postcode van de gebruiker. | Het kenmerk is specifiek voor Comcast. |
| `channelID` | Array | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | Nee | Lijst met kanalen die de gebruiker mag bekijken. | De attributenwaarde kan worden gebruikt om diverse kanalen van portals te filtreren die veelvoudige netwerken bijeenvoegen. Onze aanbeveling moet [ gebruiken preauthorize API ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) in plaats van dit attribuut van gebruikersmeta-gegevens aan filter uit kanalen die niet beschikbaar aan de gebruiker zijn. |
| `maxRating` | Object | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | Nee | Maximale ouderlijke classificatie voor de huidige gebruiker. | De kenmerkwaarde kan worden gebruikt om inhoud te filteren die niet geschikt is voor de huidige gebruiker op basis van de classificaties &quot;MPAA&quot; of &quot;VCHIP&quot;. |
| `language` | String | &quot;English&quot; | Nee | Taalinstellingen. | De kenmerkwaarde kan worden gebruikt om berichten weer te geven in overeenstemming met de taalvoorkeuren van de gebruiker. |

De kenmerken van de gebruikersmetagegevens die beschikbaar worden gesteld aan een programmeur, zijn afhankelijk van wat een MVPD biedt. In de volgende tabel worden de kenmerken weergegeven die door verschillende MVPD&#39;s beschikbaar zijn gesteld:

|                         | **Ondertekende wettelijke Overeenkomst (zip slechts)** | **identiteitskaart van de Gebruiker op AuthN** | **Upstream identiteitskaart van de Gebruiker op AuthN** | **Huishoudelijke identiteitskaart op AuthN/Z** | **Primaire OID op AuthN** | **identiteitskaart van het Type op AuthN** | **Hoofd van Huishouden op AuthN** | **Status HBA** | **toestaat het Sprongen op AuthZ** | **Postcode op AuthN/Z** | **identiteitskaart van het Kanaal op AuthN** | **Rating op AuthN/Z** | **Taal** | **onNet** | **inHome** | **Nota&#39;s** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Formele Naam** | nvt | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **vereist Encryptie** | nvt | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| **Gevoelig** | nvt | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Adobe-idP | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **ja** | **ja** | **ja** | **Nr** | **Nr** | **ja (slechts AuthN)** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst is niet nodig. |
| Synacor | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **ja (slechts AuthN)** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst die niet alle proxy-VPD&#39;s bestrijkt. Dit is algemene steun voor Synacor en misschien niet opgebouwd tot al hun MVPDs. |
| Dish | **Nr** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | Deze deelt dezelfde lijst als alle Synacor MVPD&#39;s, plus `upstreamUserID` . |
| Comcast | **Nr** | **ja** | **ja** | **ja (slechts AuthZ)** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthZ)** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| AT&amp;T | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| DTV | **ja** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| COX | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Cablevision | **ja** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| Spectrum | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Handvest | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Verizon | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| HTC | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Rogers | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| RCN | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Eastlink | **Nr** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Cogeco | **Nr** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Videotron | **Nr** | **ja** | **ja** | **ja*** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | `householdID` wordt met dezelfde waarde als `userID` beschikbaar gemaakt. |
| Proxy Massilon | **ja** | **ja** | **ja** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| Proxywissing | **ja** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **ja (slechts AuthZ)** | **ja** | **Nr** | **Nr** | Juridische overeenkomst ondertekend. |
| Proxy GLDS | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **ja (slechts AuthN)** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** |                                                                                                                                           |
| Andere MVPD&#39;s | **Nr** | **ja** | **ja** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | **Nr** | Er is nog geen juridische overeenkomst, gevoelige metagegevens zijn niet beschikbaar voor Production. Voor alle MVPD&#39;s is `userID` beschikbaar zonder extra werk. |

>[!IMPORTANT]
>
> Er moeten juridische overeenkomsten worden gesloten met MVPD&#39;s voordat gevoelige metagegevens van gebruikers (bijv. postcode) beschikbaar worden gesteld.

## Metagegevenscodering gebruiker {#encryption}

Om gebruikersmeta-gegevensattributen te coderen en te decrypteren, moet de Programmer een certificaat (openbaar/privé zeer belangrijk paar) produceren en [ ](#management) het certificaat door [ Adobe Pass TVE Dashboard ](https://experience.adobe.com/#/pass/authentication) zelf-vormen of de openbare sleutel met de vertegenwoordigers van de Authentificatie van Adobe Pass te delen.

Voer de onderstaande stappen uit om ervoor te zorgen dat het certificaat op de juiste wijze wordt gegenereerd en geconfigureerd:

1. Download en installeer de OpenSSL-toolkit (http://www.openssl.org).

1. CSR (Certificate Signing Request) genereren:

   * Een sleutelpaar genereren. Op uw bevelterminal stel het volgende in werking:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genereer de CSR. Op uw bevelterminal stel het volgende in werking:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     U wordt gevraagd het wachtwoord voor de persoonlijke sleutel in te voeren.

   * Maak een reservekopie van uw persoonlijke sleutel en wachtwoord. Voorbeeld-CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Verzend CSR naar een Instantie van het Certificaat (CA) (bijvoorbeeld, Verisign).

1. De CA stuurt u het certificaat in .p7b-indeling (PKCS#7, Cryptographic Message Syntax Standard).

1. Implementeer het .p7b-certificaat. Converteer het PKCS#7-bestand (.p7b) naar een PKCS#12-bestand (PFX-bestand, Personal Information Exchange Syntax Standard) met behulp van uw persoonlijke sleutel en geneer het PEM-bestand (samengevoegd certificaatcontainerbestand):

   * PKCS#7-bestand converteren naar een tijdelijk PEM-bestand. Voer de volgende handelingen uit op de opdrachtregel:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Zet het tijdelijke PEM-bestand om in een PFX-bestand. Voer de volgende handelingen uit op de opdrachtregel:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Zet het tijdelijke PEM-bestand om in een definitief PEM-bestand. Voer de volgende handelingen uit op de opdrachtregel:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Gebruik het PEM- dossier om [ ](#management) het certificaat door [ het Dashboard van Adobe Pass te vormen TVE ](https://experience.adobe.com/#/pass/authentication) of het Pvm- dossier naar de vertegenwoordigers van de Authentificatie van Adobe Pass te verzenden.

   * Verwijs naar de volgende sectie voor meer details op hoe te om certificaten door het [ Dashboard van Adobe Pass te beheren TVE ](https://experience.adobe.com/#/pass/authentication).

   * Adobe Pass-verificatie ondersteunt zowel een primair certificaat als een back-upcertificaat. Als uw primaire certificaat op om het even welke manier gecompromitteerd wordt, kunt u het intrekken, en op het secundaire certificaat schakelen. Dit zal een vlotte overgang tussen certificaten met minimale klantengevolgen verzekeren.

## Metagegevensbeheer gebruiker {#management}

>[!IMPORTANT]
>
> Als u geen toegang tot het Dashboard van Adobe Pass TVE hebt, creeer een kaartje door onze [ Zendesk ](https://adobeprimetime.zendesk.com) en vraag uw Technische Manager van de Rekening (TAM) om de aangewezen veranderingen voor u aan te brengen.

Het Adobe Pass TVE-dashboard is een hulpmiddel voor Adobe Pass Authentication-klanten (Programmers) om hun configuratie en gegevens te beheren. Dit zelfbedienings dashboard laat een waaier van functionaliteit toe die in de [ documentatie van de Gids van de Gebruiker van het Dashboard van Adobe Pass TVE ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) wordt beschreven.

Om de attributen van gebruikersmeta-gegevens te herzien en te beheren die door MVPD ter beschikking worden gesteld, volg de stappen in de [ Gids van de Gebruiker van het Dashboard van TVE voor de documentatie van de Integraties ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata).

Om de certificaten te herzien en te beheren die voor het coderen van de attributen van gebruikersmeta-gegevens worden gebruikt, volg de stappen in de [ Gids van de Gebruiker van het Dashboard van TVE voor Programmeurs ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) of de [ Gids van de Gebruiker van het Dashboard van TVE voor de documenten van Kanalen ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates).

## REST API V2 {#rest-api-v2}

De attributen van gebruikersmeta-gegevens kunnen worden teruggewonnen gebruikend de volgende APIs:

* [Profielen ophalen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profiel ophalen voor specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profiel ophalen voor specifieke code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Verwijs naar de **secties van de Reactie** en **Steekproeven** van bovengenoemde APIs om de structuur van de attributen van gebruikersmeta-gegevens te begrijpen.

Raadpleeg de volgende documenten voor meer informatie over hoe en wanneer u de bovenstaande API&#39;s wilt integreren:

* [Stroom van basisprofielen uitgevoerd in primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
