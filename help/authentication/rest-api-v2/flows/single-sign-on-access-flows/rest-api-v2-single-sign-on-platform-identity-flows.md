---
title: Single Sign On - Platform Identity - Flows
description: REST API V2 - Single Sign On - Platform Identity - Flows
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# Single Sign-On met gebruik van platformidentiteitsstromen {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

De methode van de Identiteit van het Platform laat veelvoudige toepassingen toe om een unieke platform herkenningsteken te gebruiken om enig sign-on (SSO) op het apparaat of platformniveau te bereiken wanneer het gebruiken van de diensten van Adobe Pass.

De toepassingen zijn verantwoordelijk voor het ophalen van de unieke lading van de platform-id met behulp van apparaatspecifieke identiteitsservices of bibliotheken buiten Adobe Pass-systemen.

De toepassingen zijn verantwoordelijk voor het opnemen van deze unieke lading van de platform-id als onderdeel van de header `Adobe-Subject-Token` voor alle aanvragen die deze aangeven.

Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

## Verificatie uitvoeren via Single Sign-On met behulp van platformidentiteit {#perform-authentication-through-single-sign-on-using-platform-identity}

### Vereisten {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Voordat u de verificatiestroom via Single Sign-On uitvoert met behulp van een platformidentiteit, moet u controleren of aan de volgende voorwaarden is voldaan:

* Het platform moet een identiteitsservice of een bibliotheek bieden die consistente informatie als `JWS` of `JWE` payload retourneert voor alle toepassingen op hetzelfde apparaat of platform.
* De eerste het stromen toepassing moet het unieke platformherkenningsteken terugwinnen en `JWS` of `JWE` nuttige lading als deel van [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) kopbal voor alle verzoeken omvatten die het specificeren.
* De eerste het stromen toepassing moet een MVPD selecteren.
* De eerste streamingtoepassing moet een verificatiesessie starten om u aan te melden bij de geselecteerde MVPD.
* De eerste het stromen toepassing moet met geselecteerde MVPD in een gebruikersagent voor authentiek verklaren.
* De tweede het stromen toepassing moet het unieke platformherkenningsteken terugwinnen en `JWS` of `JWE` nuttige lading als deel van [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) kopbal voor alle verzoeken omvatten die het specificeren.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
> 
> * De eerste het stromen toepassing steunt gebruikersinteractie om een MVPD te selecteren.
> * De eerste het stromen toepassing steunt gebruikersinteractie om met geselecteerde MVPD in een gebruikersagent voor authentiek te verklaren.

### Workflow {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Voer de gegeven stappen uit om de authentificatiestroom door enig teken-op uit te voeren gebruikend een platformidentiteit zoals aangetoond in het volgende diagram.

![ voer authentificatie door enige sign-on uit gebruikend platformidentiteit ](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*voer authentificatie door enige sign-on uit gebruikend platformidentiteit*

1. **wint platformherkenningsteken terug:** de eerste het stromen toepassing roept de identiteitsdienst of de bibliotheek, buiten de systemen van Adobe Pass, om `JWS` of `JWE` lading te verkrijgen verbonden aan het unieke platformherkenningsteken.

1. **terugkeer unieke platformherkenningsteken als JWS of JWE:** de eerste het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan aan:
   * Payload is niet verlopen.
   * Payload is ondertekend of versleuteld.

1. **creeer authentificatiesessie:** de eerste het stromen toepassing verzamelt alle noodzakelijke gegevens om een authentificatiesessie in werking te stellen door het eindpunt van Sessies te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ creeer authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, `domainName`, en `redirectUrl`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   > 
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

1. **wijs op de volgende actie:** De het eindpuntreactie van Sessies bevat de noodzakelijke gegevens om de eerste het stromen toepassing betreffende de volgende actie te begeleiden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ creeer authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details over de informatie die in een zittingsreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Sessies valideert de aanvraaggegevens om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **Open URL in gebruikersagent:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * `url` die kan worden gebruikt om de interactieve authentificatie binnen de MVPD login pagina in werking te stellen.
   * Het kenmerk `actionName` is ingesteld op &quot;authenticate&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot;.

   Als de Adobe Pass-backend geen geldig profiel identificeert, opent de eerste streamingtoepassing een gebruikersagent om de opgegeven `url` te laden, die een aanvraag doet voor het verificateurseindpunt. Deze stroom kan verscheidene omleidingen omvatten, die uiteindelijk de gebruiker aan de MVPD login pagina leiden en geldige geloofsbrieven verstrekken.

1. **Volledige authentificatie MVPD:** als de authentificatiestroom succesvol is, slaat de gebruikersagent interactie een regelmatig profiel in de Adobe Pass achterkant op en bereikt verstrekte `redirectUrl`.

1. **wint profiel voor specifieke code terug:** de eerste het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel voor specifieke code ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider`, `code`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

   >[!NOTE]
   >
   > Suggestie: de streamingtoepassing kan wachten tot de gebruikersagent de opgegeven `redirectUrl` heeft bereikt om te controleren of het standaardprofiel is gegenereerd en opgeslagen.

1. **vind regelmatig profiel:** de server van Adobe Pass identificeert een geldig profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het gevonden profiel verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel voor specifieke code ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op de informatie die in een profielreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** de eerste het stromen toepassing kan met verdere besluitvormingsstromen verdergaan.

   >[!IMPORTANT]
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

1. **wint platformherkenningsteken terug:** de tweede het stromen toepassing roept de identiteitsdienst of de bibliotheek, buiten de systemen van Adobe Pass, om `JWS` of `JWE` lading te verkrijgen verbonden aan het unieke platformherkenningsteken.

1. **terugkeer unieke platformherkenningsteken als JWS of JWE:** De tweede het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan aan:
   * Payload is niet verlopen.
   * Payload is ondertekend of versleuteld.

1. **wint profielen terug:** de tweede het stromen toepassing verzamelt alle noodzakelijke gegevens om alle profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profielen ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   > 
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

1. **vind enig sign-on profiel:** de server van Adobe Pass identificeert een geldig enig sign-on profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **informatie van de Terugkeer over enig sign-on profiel:** de het eindpuntreactie van Profielen bevat informatie over het gevonden profiel verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profielen ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API documentatie voor details over de informatie die in een profielreactie wordt verstrekt.
   >
   > <br/>
   > 
   > Het eindpunt van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** De tweede het stromen toepassing kan met verdere besluitvormingsstromen verdergaan.

   >[!IMPORTANT]
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

## Autorisatiebeslissingen ophalen via Single Sign-On met behulp van platformidentiteit{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Vereisten {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Voordat u de autorisatiestroom via Single Sign-On uitvoert met behulp van een platformidentiteit, moet u controleren of aan de volgende voorwaarden is voldaan:

* Het platform moet een identiteitsservice of een bibliotheek bieden die consistente informatie als `JWS` of `JWE` payload retourneert voor alle toepassingen op hetzelfde apparaat of platform.
* De tweede het stromen toepassing moet het unieke platformherkenningsteken terugwinnen en `JWS` of `JWE` nuttige lading als deel van [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) kopbal voor alle verzoeken omvatten die het specificeren.
* De tweede streamingtoepassing moet een autorisatiebesluit ophalen voordat een door de gebruiker geselecteerde resource wordt afgespeeld.

>[!IMPORTANT]
>
> Veronderstellingen
> 
> <br/>
> 
> * De eerste het stromen toepassing heeft authentificatie uitgevoerd en heeft een geldige waarde voor [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) verzoekkopbal opgenomen.

### Workflow {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Voer de gegeven stappen uit om de vergunningsstroom door enig teken-op uit te voeren gebruikend een platformidentiteit zoals aangetoond in het volgende diagram.

![ wint vergunningsbesluiten door enige sign-on gebruikend platformidentiteit ](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png) terug

*wint vergunningsbesluiten door enige sign-on gebruikend platformidentiteit* terug

1. **wint platformherkenningsteken terug:** de tweede het stromen toepassing roept de identiteitsdienst of de bibliotheek, buiten de systemen van Adobe Pass, om `JWS` of `JWE` lading te verkrijgen verbonden aan het unieke platformherkenningsteken.

1. **terugkeer unieke platformherkenningsteken als JWS of JWE:** De tweede het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan aan:
   * Payload is niet verlopen.
   * Payload is ondertekend of versleuteld.

1. **wint vergunningsbesluit terug:** de tweede het stromen toepassing verzamelt alle noodzakelijke gegevens om een vergunningsbesluit voor een specifiek middel te verkrijgen door Besluiten te roepen machtigt eindpunt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen vergunningsbesluiten gebruikend specifieke mvpd ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `resources`
   > * Alle _vereiste_ kopballen, als `Authorization` en `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   > 
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.

1. **vind enig sign-on profiel:** de server van Adobe Pass identificeert een geldig enig sign-on profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **wint MVPD besluit voor gevraagd middel terug:** de server van Adobe Pass roept het MVPD vergunningseindpunt om een `Permit` of `Deny` besluit voor het specifieke middel te verkrijgen dat van de het stromen toepassing wordt ontvangen.

1. **Terugkeer `Permit` besluit met media teken:** de Besluiten staan eindpuntreactie toe bevat een `Permit` besluit en een media teken.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen vergunningsbesluiten gebruikend specifieke mvpd ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **stroom van het Begin met media teken:** de tweede het stromen toepassing gebruikt het media teken om de inhoud te spelen.

1. **Terugkeer `Deny` besluit met details:** De Besluiten staan eindpuntreactie toe bevat a `Deny` besluit en een foutenlading die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen vergunningsbesluiten gebruikend specifieke mvpd ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API documentatie voor details over de informatie die in een besluitreactie wordt verstrekt.
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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **handvat `Deny` besluitvormingsdetails:** de tweede het stromen toepassing verwerkt de fouteninformatie van de reactie en kan het gebruiken om naar keuze een specifiek bericht op het gebruikersinterface te tonen.

>[!NOTE]
>
> De stappen voor de pre-vergunningsstroom zijn het zelfde als die voor de vergunningsstroom, behalve dat is het gebruikte eindpunt in [ wordt beschreven terugwinnen pre-vergunningsbesluiten gebruikend specifieke mvpd ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) documentatie die.
