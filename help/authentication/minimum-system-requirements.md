---
title: Minimale systeemvereisten
description: Minimale systeemvereisten
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 0%

---

# Minimale systeemvereisten {#minimum-system-requirements}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Overzicht {#overview}

Dit document bevat de huidige software- en hardwarevereisten voor de implementatie van Adobe Pass Authentication-integratie op ondersteunde platforms. Alle ondersteunde web-/mobiele browsers en besturingssystemen die hieronder worden vermeld, krijgen volledige ondersteuning van het Adobe Pass Authentication-team, dat gebonden is aan de overeengekomen SLA&#39;s.

Hoewel wij als Adobe Pass Authentication-team het gebruik van de nieuwste stabiele versies van de browsers en besturingssystemen aanmoedigen, erkennen wij ook het bestaan van niet-compatibele/oudere platforms en browsers die momenteel in gebruik zijn. Deze verouderde apparaten werken nog steeds zonder problemen, maar ze zijn meer vatbaar voor fouten.

De eerste aanpak om problemen op deze verouderde platforms te verhelpen, moet bestaan uit het upgraden naar de nieuwste versies. Dit kan de OS-versie, de browserversie of de versie van de geïnstalleerde toepassing zijn.

Alle problemen die op deze platforms worden weergegeven, worden als best-inspanningsbasis opgelost door het Adobe Pass-verificatieteam.

Adobe Pass moedigt onze klanten en partners aan om te overwegen om te upgraden naar de nieuwste versies om te profiteren van de volledige ondersteuning van de Adobe bij alle mogelijke problemen, naast prestatieverbeteringen, efficiëntie en beveiligingsverbeteringen.


## Vereisten voor browser en besturingssysteem {#browser-OS-system-requirements}


| Web/mobiele browser (†) | Ondersteunde versies |
|---|---|
| Google Chrome | **70** of hoger |
| Mozilla Firefox | **57** of hoger |
| Apple Safari | **14** of hoger |
| Microsoft Edge | **100** of hoger |

(†) Adobe adviseert tegen het gebruiken van privé of incognito wijze.

| Besturingssysteem | Ondersteunde versies |
|---|---|
| *Android* | **7,0** (Nougat) of hoger |
| *iOS* | **14** of hoger |
| *iPadOS* | **14** of hoger |
| *tvOS* | **14** of hoger |
| *Fire OS* | **5 (Android 5.1)** of hoger |
| *MAC OS* | **10,13** of hoger |
| *Microsoft Windows* | **10** of hoger |




>[!NOTE]
>
>Cookies van derden - Adobe Pass-verificatierechten kunnen mislukken wanneer cookies van derden zijn uitgeschakeld.  Dit probleem kan alleen worden afgespeeld wanneer de browserinstellingen worden gewijzigd. Voor alle ondersteunde browsers moet de Adobe Pass-verificatie functioneren met de standaardinstellingen.


## Apparaatvereisten voor implementaties zonder client (REST) {#general_clientless_reqs}


Om het even welk apparaat dat de Diensten van de Authentificatie van Adobe Pass door cliëntless implementaties zal gebruiken **moet in staat zijn**:

* Geef een unieke hashed-apparaat-id op. Als het apparaat geen unieke hashed apparaat-id heeft, moet het apparaat een unieke id kunnen blijven gebruiken die door Adobe Pass-verificatie wordt geleverd. Het apparaat moet de unieke id permanent in de lokale opslag kunnen behouden en de unieke id als de apparaat-id kunnen opgeven wanneer het apparaat wordt aangeroepen voor de Adobe Pass-verificatie-API&#39;s.
* Digitale handtekeningen genereren met het HMAC-SHA1-algoritme
* Willekeurige HTTP-headers instellen
* RESTful-webservices gebruiken
* XML- en JSON-gegevensindelingen parseren
* Verkeer verzenden met HTTPS
* HTTP-foutcodes afhandelen
