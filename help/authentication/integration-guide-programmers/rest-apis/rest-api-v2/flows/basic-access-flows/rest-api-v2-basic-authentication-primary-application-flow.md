---
title: Basisverificatie - Primaire toepassing - Stroom
description: REST API V2 - Basisverificatie - Primaire toepassing - Stroom
exl-id: 8122108d-e9da-43c5-9abb-ab177cb21eb6
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Standaardverificatiestroom uitgevoerd binnen primaire toepassing {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) documentatie.

>[!MORELIKETHIS]
>
> Zorg ervoor om [&#x200B; REST API V2 FAQs &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) ook te bezoeken.

De **stroom van de Authentificatie** binnen de rechten van de Authentificatie van Adobe Pass staat de het stromen toepassing toe om te verifiëren dat een gebruiker een geldige rekening van MVPD heeft. Voor dit proces moet de gebruiker een actieve MVPD-account hebben en geldige aanmeldingsgegevens invoeren op de MVPD-aanmeldingspagina.

Verificatiestroom is vereist in de volgende gevallen:

* Wanneer de gebruiker een toepassing voor de eerste keer opent.
* Wanneer de vorige verificatie van de gebruiker is verlopen.
* Wanneer de gebruiker zich afmeldt bij de MVPD-account.
* Wanneer de gebruiker wil verifiëren met een andere MVPD.

In al deze gevallen ontvangt de toepassing die een van de eindpunten van Profielen aanroept, een lege reactie of een of meer profielen, maar voor verschillende MVPD&#39;s.

De **stroom van de Authentificatie** vereist een gebruikersagent (browser) om een reeks vraag van de toepassing aan Adobe Pass achterste, toen aan de login van MVPD pagina, en definitief terug naar de toepassing te voltooien. Deze stroom kan verscheidene omleidingen aan de systemen van MVPD omvatten en het beheren van koekjes of zittingen die voor elk domein worden opgeslagen, die kunnen zijn moeilijk te bereiken en zonder een gebruikersagent te beveiligen.

Gebaseerd op de primaire toepassingsmogelijkheden (het stromen toepassing) om gebruikersinteractie te steunen om een MVPD te selecteren en met de geselecteerde MVPD in een gebruikersagent voor authentiek te verklaren, zijn de authentificatiescenario&#39;s:

* [Verificatie uitvoeren binnen primaire toepassing](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Verificatie uitvoeren binnen secundaire toepassing met vooraf geselecteerde mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Verificatie uitvoeren binnen secundaire toepassing zonder vooraf geselecteerde mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)

## Verificatie uitvoeren binnen primaire toepassing {#perform-authentication-within-primary-application}

### Vereisten {#prerequisites-perform-authentication-within-primary-application}

Voordat u verificatie uitvoert via gebruikersinteractie binnen een primaire toepassing, moet u controleren of aan de volgende voorwaarden is voldaan:

* De streamingtoepassing moet een MVPD selecteren.
* De streamingtoepassing moet een verificatiesessie starten om u aan te melden bij de geselecteerde MVPD.
* De streamingtoepassing moet worden geverifieerd met de geselecteerde MVPD in een gebruikersagent.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
> 
> * De streamingtoepassing ondersteunt gebruikersinteractie bij het selecteren van een MVPD.
> * De streamingtoepassing ondersteunt gebruikersinteractie om verificatie uit te voeren met de geselecteerde MVPD in een gebruikersagent.

### Workflow {#workflow-perform-authentication-completed-on-primary-application}

Volg de gegeven stappen om de basisauthentificatiestroom uit te voeren die binnen een primaire toepassing zoals aangetoond in het volgende diagram wordt uitgevoerd.

![&#x200B; voer authentificatie binnen primaire toepassing uit &#x200B;](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*voer authentificatie binnen primaire toepassing uit*

1. **creeer authentificatiesessie:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om een authentificatiesessie in werking te stellen door het eindpunt van Sessies te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; creeer authentificatiesessie &#x200B;](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, `domainName`, en `redirectUrl`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   > 
   > <br/>
   > 
   > De streaming toepassing moet alle vereiste parameters in één enkele vraag verstrekken wanneer het creëren van de authentificatiesessie.

1. **wijs op de volgende actie:** De het eindpuntreactie van Sessies bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; creeer authentificatiesessie &#x200B;](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details over de informatie die in een zittingsreactie wordt verstrekt.
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
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [&#x200B; Verbeterde documentatie van de Codes van de Fout &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) volgt.

1. **ga met besluitvormingsstromen te werk:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * Het attribuut `actionName` wordt ingesteld op &quot;authorize&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.

   Als de Adobe Pass-backend een geldig profiel identificeert, hoeft de streamingtoepassing niet opnieuw te worden geverifieerd met de geselecteerde MVPD, omdat er al een profiel is dat kan worden gebruikt voor volgende beslissingsstromen.

1. **Open URL in gebruikersagent:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * De `url` die kan worden gebruikt om de interactieve verificatie te starten op de MVPD-aanmeldingspagina.
   * Het kenmerk `actionName` is ingesteld op &quot;authenticate&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot;.

   Als de Adobe Pass-backend geen geldig profiel identificeert, opent de streamingtoepassing een gebruikersagent om de opgegeven `url` te laden, die een aanvraag doet voor het verificatieeindpunt. Deze stroom kan verschillende omleidingen bevatten, die de gebruiker uiteindelijk naar de MVPD-aanmeldingspagina leiden en geldige gegevens bieden.

1. **Volledige authentificatie van MVPD:** als de authentificatiestroom succesvol is, slaat de gebruikersagent interactie een regelmatig profiel in de achtergrond van Adobe Pass op en bereikt verstrekte `redirectUrl`.

1. **wint profiel voor specifieke code terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [&#x200B; terugwinnen profiel voor specifieke code &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `code`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

   >[!TIP]
   >
   > De streamingtoepassing moet wachten totdat de gebruikersagent de opgegeven `redirectUrl` heeft bereikt om te controleren of het standaardprofiel is gegenereerd en opgeslagen.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het regelmatige profiel verbonden aan de ontvangen parameters en kopballen.

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
