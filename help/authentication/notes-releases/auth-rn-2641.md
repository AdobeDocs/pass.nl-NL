---
title: Opmerkingen bij de release Adobe Pass Authentication 2.64.1
description: Opmerkingen bij de release Adobe Pass Authentication 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Opmerkingen bij de release Adobe Pass Authentication 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Op deze pagina worden nieuwe functies, wijzigingen en bekende problemen met deze release beschreven:

## Server Side and Web Clients {#server-side-web-clients-2641}

* [Buildnummer](#build-number-2641)
* [Overzicht van release](#release-overview-2641)

### Buildnummer {#build-number-2641}

De Authentificatie van Adobe Pass: adobe-pas **2.64.1**

Datum van de versie: **01/31/2023 - 02/02/2023**

### Overzicht van release {#release-overview-2641}

Deze release voegt het volgende toe:

* De capaciteit om ongevraagde authentificatiereacties van MVPDs te blokkeren waar de &quot;in_response_to&quot;parameter van de bewering van SAML mist.
* Een betere bevestiging over redirect_url parameter om aan veiligheidsvereisten te voldoen.
* Een verbeterde logboekregistratie van de apparaatinformatie voor verzoeken voor tweede schermverificatie, waarbij gebruik wordt gemaakt van informatie die door het oorspronkelijke apparaat wordt verstrekt.
