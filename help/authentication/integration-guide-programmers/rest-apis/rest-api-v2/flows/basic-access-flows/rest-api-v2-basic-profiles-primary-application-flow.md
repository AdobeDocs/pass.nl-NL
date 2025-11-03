---
title: Basisprofielen - primaire toepassing - Stroom
description: REST API V2 - Basisprofielen - Primaire toepassing - Stroom
exl-id: 19ddf382-9a32-4b94-aa84-7611c0e1780e
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '974'
ht-degree: 0%

---

# Stroom van basisprofielen uitgevoerd in primaire toepassing {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

>[!MORELIKETHIS]
>
> Zorg ervoor om [&#x200B; REST API V2 FAQs &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) ook te bezoeken.

De **stroom van Profielen** binnen de rechten van de Authentificatie van Adobe Pass staat de het stromen toepassing toe om tot informatie over actieve gebruikerslogins toegang te hebben.

De basis profielstroom staat u toe om voor de volgende scenario&#39;s te vragen:

* [Profielen ophalen](#retrieve-profiles)
* [Profiel ophalen voor specifieke mvpd](#retrieve-profile-for-specific-mvpd)
* [Profiel ophalen voor specifieke code](#retrieve-profile-for-specific-code)

## Profielen ophalen {#retrieve-profiles}

### Vereisten {#prerequisites-retrieve-profiles}

Controleer voordat u profielen ophaalt of aan de volgende voorwaarden is voldaan:

* De streamingtoepassing wil alle normale profielen ophalen.

### Workflow {#workflow-retrieve-profiles}

Volg de gegeven stappen om de basisprofielen uit te voeren terugwinningsstroom die binnen een primaire toepassing zoals aangetoond in het volgende diagram wordt uitgevoerd.

![&#x200B; wint profielen &#x200B;](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png) terug

*wint profielen* terug

1. **wint profielen terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om alle profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profielen &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

1. **vind regelmatige profielen:** de server van Adobe Pass identificeert alle geldige profielen die op de ontvangen parameters en kopballen worden gebaseerd.

1. **de informatie van de Terugkeer over regelmatige profielen:** de het eindpuntreactie van Profielen bevat informatie over de gevonden profielen verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profielen &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API documentatie voor details over de informatie die in een profielreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **kies een profiel en ga met besluitvormingsstromen te werk:** als de het eindpuntreactie van Profielen profielen bevat, gebruikt de het stromen toepassing zijn interne logica (uiteindelijk door met de eindgebruiker in wisselwerking te staan) om één van de beschikbare profielen te kiezen om met verdere besluitvormingsstromen verder te gaan.

1. **wijs op nieuwe basisauthentificatiestroom:** als de het eindpuntreactie van Profielen geen profiel bevat, wijst de het stromen toepassing op de gebruiker om een nieuwe basisauthentificatiestroom in werking te stellen.

## Profiel ophalen voor specifieke mvpd {#retrieve-profile-for-specific-mvpd}

### Vereisten {#prerequisites-retrieve-profile-for-specific-mvpd}

Voordat u het profiel voor een specifieke MVPD ophaalt, moet u controleren of aan de volgende voorwaarden is voldaan:

* De streamingtoepassing, die een geselecteerde of in cache opgeslagen `mvpd` -id heeft, wil het standaardprofiel voor een specifieke MVPD ophalen.

### Workflow {#workflow-retrieve-profile-for-specific-mvpd}

Volg de gegeven stappen om de basisstroom van de profielherwinning voor een specifieke MVPD uit te voeren die binnen een primaire toepassing zoals aangetoond in het volgende diagram wordt uitgevoerd.

![&#x200B; wint profiel voor specifieke mvpd terug &#x200B;](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*wint profiel voor specifieke mvpd terug*

1. **wint profiel voor specifieke mvpd terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie voor die specifieke MVPD terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profiel voor specifieke mvpd &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider` en `mvpd`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

1. **vind regelmatig profiel:** de server van Adobe Pass identificeert een geldig profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het gevonden profiel verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profiel voor specifieke mvpd &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API documentatie voor details op de informatie die in een profielreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** als de het eindpuntreactie van Profielen een profiel bevat, gebruikt de het stromen toepassing de profielinformatie om met verdere besluitvormingsstromen verder te gaan.

1. **wijs op nieuwe basisauthentificatiestroom:** als de het eindpuntreactie van Profielen geen profiel bevat, wijst de het stromen toepassing op de gebruiker om een nieuwe basisauthentificatiestroom in werking te stellen.

## Profiel ophalen voor specifieke code {#retrieve-profile-for-specific-code}

### Vereisten {#prerequisites-retrieve-profile-for-specific-code}

Voordat u het profiel voor een specifieke verificatiecode ophaalt, moet u controleren of aan de volgende voorwaarden is voldaan:

* De streamingtoepassing, die een `code` heeft waarmee de interactieve verificatie met de MVPD wordt uitgevoerd, wil het profiel voor een specifieke verificatiecode ophalen.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Volg de gegeven stappen om de basisstroom van de profielherwinning voor een specifieke authentificatiecode uit te voeren die binnen een primaire toepassing zoals aangetoond in het volgende diagram wordt uitgevoerd.

![&#x200B; wint profiel voor specifieke code &#x200B;](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png) terug

*wint profiel voor specifieke code* terug

1. **wint profiel voor specifieke code terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie voor die specifieke authentificatiecode terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profiel voor specifieke code &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, en `code`
   > * Alle _vereiste_ kopballen, als `Authorization`
   > * Alle _facultatieve_ parameters en kopballen

1. **vind regelmatig profiel:** de server van Adobe Pass identificeert een geldig profiel dat op de ontvangen parameters en kopballen wordt gebaseerd.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het gevonden profiel verbonden aan de ontvangen parameters en kopballen.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profiel voor specifieke code &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op de informatie die in een profielreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Profielen bevestigt de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   >
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** als de het eindpuntreactie van Profielen een profiel bevat, gebruikt de het stromen toepassing de profielinformatie om met verdere besluitvormingsstromen verder te gaan.

1. **wijs op nieuwe basisauthentificatiestroom:** als de het eindpuntreactie van Profielen geen profiel bevat, wijst de primaire toepassing op de gebruiker om een nieuwe basisauthentificatiestroom in werking te stellen.
