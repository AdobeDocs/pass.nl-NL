---
title: Dynamic Client Registration Flow
description: Dynamic Client Registration Flow
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---


# Dynamic Client Registration Flow {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De dynamische implementatie van de Registratie API van de Cliënt wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

## Toegang tot API&#39;s die met Adobe Pass zijn beveiligd {#access-adobe-pass-protected-apis}

### Vereisten {#prerequisites-access-adobe-pass-protected-apis}

Controleer voordat u API&#39;s die met Adobe Pass zijn beveiligd, opent of aan de volgende voorwaarden is voldaan:

* Een cliëntvertegenwoordiger moet een geregistreerde toepassing tot stand brengen zoals die in [ wordt beschreven leidt geregistreerde toepassingen ](../dynamic-client-registration-overview.md#manage-registered-applications) sectie.
* Een cliëntvertegenwoordiger moet een softwareverklaring downloaden en inbedden zoals die in [ wordt beschreven leidt softwareverklaringen ](../dynamic-client-registration-overview.md#manage-software-statements) sectie.

>[!IMPORTANT]
>
> Adobe Pass Authentication SDKs is verantwoordelijk voor het ophalen en vernieuwen van de clientreferenties en het toegangstoken namens de clienttoepassing.
> 
> Voor alle andere door Adobe Pass beveiligde API&#39;s moet de clienttoepassing de onderstaande workflow volgen.

### Workflow {#workflow-access-adobe-pass-protected-apis}

Voer de opgegeven stappen uit om toegang te krijgen tot API&#39;s die met Adobe Pass zijn beveiligd, zoals in het volgende diagram wordt getoond.

![ Toegang Adobe Pass beschermde APIs ](../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Toegang Adobe Pass beschermde APIs*

1. **wint cliëntgeloofsbrieven terug:** de cliënttoepassing verzamelt alle noodzakelijke gegevens om cliëntgeloofsbrieven terug te winnen door het eindpunt van het Register van de Cliënt te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen cliëntgeloofsbrieven ](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `software_statement`
   > * Alle _vereiste_ kopballen, als `Content-Type`, `X-Device-Info`
   > * Alle _facultatieve_ parameters en kopballen

1. **de cliëntgeloofsbrieven van de Terugkeer:** De het eindpuntreactie van het Register van de Cliënt bevat informatie over de cliëntgeloofsbrieven verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen cliëntgeloofsbrieven ](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API documentatie voor details over de informatie die in een reactie van de cliëntgeloofsbrieven wordt verstrekt.
   >
   > <br/>
   >
   > In het clientregister worden de aanvraaggegevens gevalideerd om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, die extra informatie verstrekken die aan [ voldoet wint cliëntgeloofsbrieven ](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API documentatie terug.

   >[!TIP]
   >
   > Suggestie: de clientgegevens moeten in de cache worden opgeslagen en kunnen voor onbepaalde tijd worden gebruikt.

1. **wint toegangstoken terug:** de cliënttoepassing verzamelt alle noodzakelijke gegevens om toegangstoken terug te winnen door het Symbolische eindpunt van de Cliënt te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen toegangstoken ](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `client_id`, `client_secret`, en `grant_type`
   > * Alle _vereiste_ kopballen, als `Content-Type`, `X-Device-Info`
   > * Alle _facultatieve_ parameters en kopballen

1. **het toegangstoken van de Terugkeer:** De het eindpuntreactie van het Symbolische van de Cliënt bevat informatie over het toegangstoken verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ ophalen toegangstoken ](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API documentatie voor details over de informatie die in een reactie van het toegangstoken wordt verstrekt.
   >
   > <br/>
   >
   > De Clienttoken valideert de aanvraaggegevens om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, die extra informatie verstrekken die aan [ voldoet verkrijgt toegangstoken ](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API documentatie.

   >[!TIP]
   >
   > Suggestie: het toegangstoken moet in de cache worden opgeslagen en alleen binnen de opgegeven tijdsduur worden gebruikt (bijvoorbeeld 24-uurs time-to-live). Nadat deze is verlopen, moet de clienttoepassing een nieuw toegangstoken aanvragen.

1. **ga met de toegang tot van beschermde APIs te werk:** de cliënttoepassing gebruikt het toegangstoken om tot andere Adobe Pass beschermde APIs toegang te hebben. De clienttoepassing moet het toegangstoken opnemen in de aanvraagheader van `Authorization` met behulp van het `Bearer` verificatieschema (d.w.z. `Authorization: Bearer <access_token>` ).

   >[!IMPORTANT]
   >
   > De API&#39;s die met Adobe Pass zijn beveiligd, valideren het toegangstoken om te controleren of aan de basisvoorwaarden wordt voldaan:
   >
   > * _access_token_ moet geldig zijn.
   > * _access_token_ moet met geldig _client_id_ en _client_geheime_ worden geassocieerd.
   > * _access_token_ moet met een geldige _software_statement_ worden geassocieerd.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../enhanced-error-codes.md) volgt.
