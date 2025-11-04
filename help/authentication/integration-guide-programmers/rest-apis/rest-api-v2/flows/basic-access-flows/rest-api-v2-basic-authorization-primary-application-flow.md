---
title: Basisautorisatie - Primaire toepassing - Stroom
description: REST API V2 - Basisautorisatie - Primaire toepassing - Stroom
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# Basisvergunningsstroom uitgevoerd binnen primaire toepassing {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

De **stroom van de Vergunning** binnen de bevoegdheid van de Authentificatie van Adobe Pass staat de het stromen toepassing toe om te bepalen of een MVPD het verzoek van de gebruiker om inhoud toelaat of ontkent te stromen. Als de beslissing `Permit` is, bevat de reactie een media-token. Adobe Pass-server ondertekent het mediatoken en stelt de streamingtoepassing in staat de verificatiebibliotheek voor mediatoken te gebruiken om de authenticiteit van de token te controleren voordat de stream wordt vrijgegeven.

De verificatie met de verificatiebibliotheek van het mediatoken moet plaatsvinden op de streamingtoepassing backend-service die is gekoppeld in de reeks machtigingen voor het vrijgeven van een stream vanuit de CDN.

## Autorisatiebesluiten ophalen met specifieke mvpd {#retrieve-authorization-decisions-using-specific-mvpd}

### Vereisten {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Voordat u autorisatiebesluiten ophaalt met een specifieke MVPD, moet u controleren of aan de volgende voorwaarden is voldaan:

* De streamingtoepassing moet een geldig regelmatig profiel hebben dat met succes is gemaakt voor de MVPD en dat een van de basisverificatiestromen gebruikt:
   * [Verificatie uitvoeren binnen primaire toepassing](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Verificatie uitvoeren binnen secundaire toepassing met vooraf geselecteerde mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Verificatie uitvoeren binnen secundaire toepassing zonder vooraf geselecteerde mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* De streamingtoepassing moet een autorisatiebesluit ophalen voordat een door de gebruiker geselecteerde bron wordt afgespeeld.

### Workflow {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Volg de gegeven stappen om de basisvergunningsstroom uit te voeren gebruikend een specifieke MVPD die binnen een primaire toepassing zoals aangetoond in het volgende diagram wordt uitgevoerd.

![&#x200B; wint vergunningsbesluiten terug gebruikend specifieke mvpd &#x200B;](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*wint vergunningsbesluiten terug gebruikend specifieke mvpd*

1. **wint vergunningsbesluit terug:** De het stromen toepassing verzamelt alle noodzakelijke gegevens om een vergunningsbesluit voor een specifiek middel te verkrijgen door Besluiten te roepen machtigt eindpunt.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen vergunningsbesluiten gebruikend specifieke mvpd &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `resources`
   > * Alle _vereiste_ kopballen, als `Authorization` en `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

1. **vind regelmatig profiel:** de server van Adobe Pass identificeert een geldig profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **wint het besluit van MVPD voor gevraagde middel terug:** de server van Adobe Pass roept het de vergunningseindpunt van MVPD om een `Permit` of `Deny` besluit voor het specifieke middel te verkrijgen dat van de het stromen toepassing wordt ontvangen.

1. **Terugkeer `Permit` besluit met media teken:** de Besluiten staan eindpuntreactie toe bevat een `Permit` besluit en een media teken.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen vergunningsbesluiten gebruikend specifieke mvpd &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > De Besluiten machtigen eindpunt bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **stroom van het Begin met media teken:** de het stromen toepassing gebruikt het media teken om de inhoud te spelen.

1. **Terugkeer `Deny` besluit met details:** De Besluiten staan eindpuntreactie toe bevat a `Deny` besluit en een foutenlading die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen vergunningsbesluiten gebruikend specifieke mvpd &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > De Besluiten machtigen eindpunt bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **handvat `Deny` besluitvormingsdetails:** de het stromen toepassing verwerkt de fouteninformatie van de reactie en kan het gebruiken om naar keuze een specifiek bericht op het gebruikersinterface te tonen.
