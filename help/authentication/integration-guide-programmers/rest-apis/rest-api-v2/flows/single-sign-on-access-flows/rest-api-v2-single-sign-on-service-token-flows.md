---
title: Single Sign On - Service Token - Flows
description: REST API V2 - Single Sign On - Service Token - Flows
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# Single Sign-On die de stromen van het de dienstteken gebruikt{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

>[!MORELIKETHIS]
>
> Zorg ervoor om [ REST API V2 FAQs ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) ook te bezoeken.

Met de methode Servicetokken kunnen meerdere toepassingen een unieke gebruikersnaam gebruiken om SSO (Single Sign-On) op meerdere apparaten en platforms te bereiken bij het gebruik van Adobe Pass-services.

De toepassingen zijn verantwoordelijk voor het ophalen van de unieke lading van de gebruikersnaam via externe identiteitsservices, buiten Adobe Pass-systemen, zoals:

* Een DTC-service (Direct-to-Consumer), waarbij gebruikers zich op elk apparaat aanmelden met dezelfde referenties en aan dezelfde gebruikersnaam of gebruikersnaam zijn gekoppeld.
* Een externe verificatieservice, zoals Google of Facebook, waarbij gebruikers zich op elk apparaat aanmelden met dezelfde referenties en aan hetzelfde e-mailadres zijn gekoppeld.

De toepassingen zijn verantwoordelijk voor het opnemen van deze unieke lading van de gebruikers-id als onderdeel van de header `AD-Service-Token` voor alle aanvragen die deze opgeven.

Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

## Verificatie uitvoeren via Single Sign-On met gebruik van servicetoken {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Vereisten {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Alvorens de authentificatiestroom door enig teken-binnen uit te voeren gebruikend een de dienstteken, zorg ervoor de volgende eerste vereisten worden voldaan:

* De externe identiteitsservice moet consistente informatie retourneren als `JWS` lading in alle toepassingen op meerdere apparaten en platforms.
* De eerste het stromen toepassing moet het unieke gebruikersidentificatie terugwinnen en `JWS` nuttige lading als deel van [ a.u.b.-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) kopbal voor alle verzoeken omvatten die het specificeren.
* De eerste streamingtoepassing moet een MVPD selecteren.
* De eerste streamingtoepassing moet een verificatiesessie starten om u aan te melden bij de geselecteerde MVPD.
* De eerste streamingtoepassing moet worden geverifieerd met de geselecteerde MVPD in een gebruikersagent.
* De tweede het stromen toepassing moet het unieke gebruikersidentificatie terugwinnen en `JWS` nuttige lading als deel van [ a.u.b.-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) kopbal voor alle verzoeken omvatten die het specificeren.

>[!IMPORTANT]
>
> Veronderstellingen
> 
> <br/>
> 
> * De eerste streamingtoepassing ondersteunt gebruikersinteractie bij het selecteren van een MVPD.
> * De eerste streamingtoepassing ondersteunt gebruikersinteractie voor verificatie met de geselecteerde MVPD in een gebruikersagent.

### Workflow {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Voer de bepaalde stappen uit om de authentificatiestroom door enige sign-on uit te voeren gebruikend een de dienstteken zoals aangetoond in het volgende diagram.

![ voer authentificatie door enig teken-op gebruikend de dienstteken ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png) uit

*voer authentificatie door enig teken-op gebruikend de dienstteken* uit

1. **verklaart met de identiteitsdienst voor authentiek:** de eerste het stromen toepassing roept de identiteitsdienst, buiten de systemen van Adobe Pass, om de `JWS` lading te verkrijgen verbonden aan het unieke gebruikersidentificatie.

1. **terugkeer unieke gebruikersidentificatie als JWS:** De eerste het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan:
   * Payload is niet verlopen.
   * Payload is ondertekend.

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
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke gebruikersnaam bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **Open URL in gebruikersagent:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * De `url` die kan worden gebruikt om de interactieve verificatie te starten op de MVPD-aanmeldingspagina.
   * Het kenmerk `actionName` is ingesteld op &quot;authenticate&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot;.

   Als de Adobe Pass-backend geen geldig profiel identificeert, opent de eerste streamingtoepassing een gebruikersagent om de opgegeven `url` te laden, die een aanvraag doet voor het verificateurseindpunt. Deze stroom kan verschillende omleidingen bevatten, die de gebruiker uiteindelijk naar de MVPD-aanmeldingspagina leiden en geldige gegevens bieden.

1. **Volledige authentificatie van MVPD:** als de authentificatiestroom succesvol is, slaat de gebruikersagent interactie een regelmatig profiel in de achtergrond van Adobe Pass op en bereikt verstrekte `redirectUrl`.

1. **wint profiel voor specifieke code terug:** de eerste het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel voor specifieke code ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider`, `code`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

   >[!TIP]
   >
   > De streamingtoepassing moet wachten totdat de gebruikersagent de opgegeven `redirectUrl` heeft bereikt om te controleren of het standaardprofiel is gegenereerd en opgeslagen.

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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** de eerste het stromen toepassing kan met verdere besluitvormingsstromen verdergaan.

   >[!IMPORTANT]
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke gebruikersnaam bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

1. **verklaart met de identiteitsdienst voor authentiek:** de tweede het stromen toepassing roept de identiteitsdienst, buiten de systemen van Adobe Pass, om de `JWS` lading te verkrijgen verbonden aan het unieke gebruikersidentificatie.

1. **terugkeer unieke gebruikersidentificatie als JWS:** De tweede het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan:
   * Payload is niet verlopen.
   * Payload is ondertekend.

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
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke gebruikersnaam bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** De tweede het stromen toepassing kan met verdere besluitvormingsstromen verdergaan.

   >[!IMPORTANT]
   >
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke gebruikersnaam bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

## Haal vergunningsbesluiten door enig sign-on gebruikend de dienstteken terug {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Vereisten {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Alvorens de vergunningsstroom door enige sign-on het gebruiken van een de dienstteken uit te voeren, zorg ervoor de volgende eerste vereisten worden voldaan:

* De externe identiteitsservice moet consistente informatie retourneren als `JWS` lading in alle toepassingen op meerdere apparaten en platforms.
* De eerste het stromen toepassing moet het unieke gebruikersidentificatie terugwinnen en `JWS` nuttige lading als deel van [ a.u.b.-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) kopbal voor alle verzoeken omvatten die het specificeren.
* De tweede streamingtoepassing moet een autorisatiebesluit ophalen voordat een door de gebruiker geselecteerde resource wordt afgespeeld.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
> 
> * De eerste het stromen toepassing heeft authentificatie uitgevoerd en een geldige waarde voor [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) verzoekkopbal omvat.

### Workflow {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Voer de gegeven stappen uit om de vergunningsstroom door enige sign-on uit te voeren gebruikend een de dienstteken zoals aangetoond in het volgende diagram.

![ wint vergunningsbesluiten door enige sign-on het gebruiken van de dienstteken ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png) terug

*wint vergunningsbesluiten door enige sign-on het gebruiken van de dienstteken* terug

1. **verklaart met de identiteitsdienst voor authentiek:** de tweede het stromen toepassing roept de identiteitsdienst, buiten de systemen van Adobe Pass, om de `JWS` lading te verkrijgen verbonden aan het unieke gebruikersidentificatie.

1. **terugkeer unieke gebruikersidentificatie als JWS:** De tweede het stromen toepassing bevestigt de reactiegegevens om ervoor te zorgen dat de basisveiligheidsvoorwaarden worden voldaan:
   * Payload is niet verlopen.
   * Payload is ondertekend.

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
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke gebruikersnaam bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

1. **vind enig sign-on profiel:** de server van Adobe Pass identificeert een geldig enig sign-on profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **wint het besluit van MVPD voor gevraagde middel terug:** de server van Adobe Pass roept het de vergunningseindpunt van MVPD om een `Permit` of `Deny` besluit voor het specifieke middel te verkrijgen dat van de het stromen toepassing wordt ontvangen.

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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **stroom van het Begin met media teken:** de tweede het stromen toepassing gebruikt het media teken om de inhoud te spelen.

1. **Terugkeer `Deny` besluit met details:** De Besluiten staan eindpuntreactie toe bevat a `Deny` besluit en een foutenlading die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **handvat `Deny` besluitvormingsdetails:** de tweede het stromen toepassing verwerkt de fouteninformatie van de reactie en kan het gebruiken om naar keuze een specifiek bericht op het gebruikersinterface te tonen.

>[!NOTE]
>
> De stappen voor de pre-vergunningsstroom zijn het zelfde als die voor de vergunningsstroom, behalve dat is het gebruikte eindpunt in [ wordt beschreven terugwinnen pre-vergunningsbesluiten gebruikend specifieke mvpd ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) documentatie die.
