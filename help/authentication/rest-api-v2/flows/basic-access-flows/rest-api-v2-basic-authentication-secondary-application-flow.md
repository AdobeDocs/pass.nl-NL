---
title: Basisverificatie - Secundaire toepassing - Stroom
description: REST API V2 - Basisverificatie - Secundaire toepassing - Stroom
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '1756'
ht-degree: 0%

---


# Basisverificatiestroom uitgevoerd binnen secundaire toepassing {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> De implementatie van REST API V2 wordt begrensd door de [ Throttling mechanisme ](/help/authentication/throttling-mechanism.md) documentatie.

De **stroom van de Authentificatie** binnen de rechten van de Authentificatie van Adobe Pass staat de het stromen toepassing toe om te verifiëren dat een gebruiker een geldige rekening MVPD heeft. Dit proces vereist de gebruiker om een actieve rekening te hebben MVPD en geldige login geloofsbrieven op de MVPD login pagina in te gaan.

Verificatiestroom is vereist in de volgende gevallen:

* Wanneer de gebruiker een toepassing voor de eerste keer opent.
* Wanneer de vorige verificatie van de gebruiker is verlopen.
* Wanneer de gebruiker zich afmeldt bij de MVPD-account.
* Wanneer de gebruiker met een verschillende MVPD wil voor authentiek verklaren.

In al deze gevallen ontvangt de toepassing die een van de eindpunten van Profielen aanroept, een lege reactie of een of meer profielen, maar voor verschillende MVPD&#39;s.

De **stroom van de Authentificatie** vereist een gebruikersagent (browser) om een reeks vraag van de toepassing aan Adobe Pass achterste, toen aan MVPD login pagina, en definitief terug naar de toepassing te voltooien. Deze stroom kan verscheidene omleidingen aan systemen omvatten MVPD en het beheren van koekjes of zittingen die voor elk domein worden opgeslagen, die kunnen zijn moeilijk te bereiken en zonder een gebruikersagent te beveiligen.

Gebaseerd op de primaire toepassings (het stromen toepassing) mogelijkheden om gebruikersinteractie te steunen om een MVPD te selecteren en met geselecteerde MVPD in een gebruikersagent voor authentiek te verklaren, zijn de authentificatiescenario&#39;s:

* [Verificatie uitvoeren binnen primaire toepassing](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Verificatie uitvoeren binnen secundaire toepassing met vooraf geselecteerde mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Verificatie uitvoeren binnen secundaire toepassing zonder vooraf geselecteerde mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Verificatie uitvoeren binnen secundaire toepassing met vooraf geselecteerde mvpd {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Vereisten {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Alvorens de authentificatiestroom binnen een primaire toepassing te beginnen en het te voltooien door gebruikersinteractie binnen een secundaire toepassing, zorg ervoor de volgende eerste vereisten worden voldaan:

* De streamingtoepassing moet een MVPD selecteren.
* De streamingtoepassing moet een verificatiesessie starten om u aan te melden bij de geselecteerde MVPD.
* De secundaire toepassing moet met geselecteerde MVPD in een gebruikersagent voor authentiek verklaren.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
> 
> * De streamingtoepassing ondersteunt gebruikersinteractie om een MVPD te selecteren.
> * De secundaire toepassing (gewoonlijk op een secundair apparaat) steunt gebruikersinteractie om met geselecteerde MVPD in een gebruikersagent voor authentiek te verklaren.

### Workflow {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Volg de gegeven stappen om de basisauthentificatiestroom uit te voeren die binnen een secundaire toepassing met vooraf verkozen MVPD zoals aangetoond in het volgende diagram wordt uitgevoerd.

![ voer authentificatie binnen secundaire toepassing met vooraf geselecteerde mvpd uit ](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*voer authentificatie binnen secundaire toepassing met vooraf geselecteerde mvpd uit*

1. **creeer authentificatiesessie:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om een authentificatiesessie in werking te stellen door het eindpunt van Sessies te roepen.

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
   > De streaming toepassing moet alle vereiste parameters in één enkele vraag verstrekken wanneer het creëren van de authentificatiesessie.

1. **wijs op de volgende actie:** De het eindpuntreactie van Sessies bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden.

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

1. **ga met besluitvormingsstromen te werk:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * Het attribuut `actionName` wordt ingesteld op &quot;authorize&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.

   Als de Adobe Pass-backend een geldig profiel identificeert, hoeft de streamingtoepassing niet opnieuw te worden geverifieerd met de geselecteerde MVPD, omdat er al een profiel is dat kan worden gebruikt voor volgende beslissingsstromen.

1. **de authentificatiecode van de Vertoning:** de reactie van het eindpunt van Zegelingen bevat de volgende gegevens:
   * `code` die kan worden gebruikt om de authentificatiesessie binnen een secundaire toepassing te hervatten.
   * Het kenmerk `actionName` is ingesteld op &quot;authenticate&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot;.

   Als de Adobe Pass-backend geen geldig profiel herkent, wordt in de streamingtoepassing de `code` weergegeven die kan worden gebruikt om de verificatiesessie in een secundaire toepassing te hervatten.

1. **Open URL in gebruikersagent:** De secundaire toepassing opent een gebruikersagent om het zelf gegevens verwerkte `url` te laden, die een verzoek aan het Authenticate eindpunt doet. Deze stroom kan verscheidene omleidingen omvatten, die uiteindelijk de gebruiker aan de MVPD login pagina leiden en geldige geloofsbrieven verstrekken.

1. **Volledige authentificatie MVPD:** als de authentificatiestroom succesvol is, slaat de gebruikersagent interactie een regelmatig profiel in Adobe Pass achterkant op en bereikt verstrekte `redirectUrl`.

1. **wint profiel voor specifieke code terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel voor specifieke code ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   > 
   > * Alle _vereiste_ parameters, als `serviceProvider` en `code`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

   >[!NOTE]
   >
   > Suggestie: de streamingtoepassing kan een opiniepeilingsmechanisme implementeren met behulp van `code` om te controleren of het standaardprofiel is gegenereerd en opgeslagen.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het regelmatige profiel verbonden aan de ontvangen parameters en kopballen.

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

## Verificatie uitvoeren binnen secundaire toepassing zonder vooraf geselecteerde mvpd {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Vereisten {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Alvorens de authentificatiestroom binnen een primaire toepassing te beginnen en het te voltooien door gebruikersinteractie binnen een secundaire toepassing, zorg ervoor de volgende eerste vereisten worden voldaan:

* De streamingtoepassing moet een verificatiesessie starten wanneer deze zich moet aanmelden.
* De secundaire toepassing moet een MVPD selecteren.
* De secundaire toepassing moet met geselecteerde MVPD in een gebruikersagent voor authentiek verklaren.

>[!IMPORTANT]
>
> Veronderstellingen
>
> <br/>
> 
> * De secundaire toepassing (gewoonlijk op een secundair apparaat) steunt gebruikersinteractie om een MVPD te selecteren.
> * De secundaire toepassing (gewoonlijk op een secundair apparaat) steunt gebruikersinteractie om met geselecteerde MVPD in een gebruikersagent voor authentiek te verklaren.

### Workflow {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Volg de gegeven stappen om de basisauthentificatiestroom uit te voeren die binnen een secundaire toepassing zonder vooraf verkozen MVPD zoals aangetoond in het volgende diagram wordt uitgevoerd.

![ voer authentificatie binnen secundaire toepassing zonder vooraf geselecteerde mvpd uit ](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*voer authentificatie binnen secundaire toepassing zonder vooraf geselecteerde mvpd uit*

1. **creeer authentificatiesessie:** de het stromen toepassing verzamelt enkele noodzakelijke gegevens om een authentificatiesessie in werking te stellen door het eindpunt van Zittingen te roepen.

   >[!IMPORTANT]
   >
   > Verwijs naar [ creeer authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen
   >
   > <br/>
   > 
   > De streaming toepassing kan niet alle vereiste parameters in één enkele vraag verstrekken wanneer het creëren van de authentificatiesessie.

1. **wijs op de volgende actie:** De reactie van het eindpunt van zittingen bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden:
   * `code` die kan worden gebruikt om de authentificatiesessie binnen een secundaire toepassing te hervatten.
   * Het kenmerk `actionName` is ingesteld op &quot;resume&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.

   >[!IMPORTANT]
   >
   > Verwijs naar [ creeer authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API documentatie voor details over de informatie die in een zittingsreactie wordt verstrekt.
   > 
   > <br/>
   > 
   > Het eindpunt van Sessies valideert de aanvraaggegevens om ervoor te zorgen dat aan de basisvoorwaarden wordt voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **de authentificatiecode van de Vertoning:** de het stromen toepassing toont `code` die kan worden gebruikt om de authentificatiesessie binnen een secundaire toepassing te hervatten.

1. **verstrek authentificatiesessie ontbrekende parameters:** de secundaire toepassing verzamelt alle ontbrekende gegevens die worden vereist om de authentificatiesessie te hervatten en roept het eindpunt van Sessies.

   >[!IMPORTANT]
   >
   > Verwijs naar [ hervat authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, `domainName`, en `redirectUrl`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

1. **wijs op de volgende actie:** De het eindpuntreactie van Sessies bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ hervat authentificatiesessie ](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API documentatie voor details over de informatie die in een zittingsreactie wordt verstrekt.
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

1. **wijs bestaand profiel op:** De reactie van het eindpunt van Zegelingen bevat de volgende gegevens:
   * Het attribuut `actionName` wordt ingesteld op &quot;authorize&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;direct&quot;.

   Als de Adobe Pass-backend een geldig profiel identificeert, hoeft de streamingtoepassing niet opnieuw te worden geverifieerd met de geselecteerde MVPD, omdat er al een profiel is dat kan worden gebruikt voor volgende beslissingsstromen.

1. **Open URL in gebruikersagent:** De reactie van het eindpunt van zittingen bevat de volgende gegevens:
   * `url` die kan worden gebruikt om de interactieve authentificatie binnen de MVPD login pagina in werking te stellen.
   * Het kenmerk `actionName` is ingesteld op &quot;authenticate&quot;.
   * Het attribuut `actionType` wordt ingesteld op &quot;interactive&quot;.

   Als de Adobe Pass backend geen geldig profiel identificeert, opent de secundaire toepassing een gebruikersagent om verstrekte `url` te laden, die een verzoek aan het Authenticate eindpunt doet. Deze stroom kan verscheidene omleidingen omvatten, die uiteindelijk de gebruiker aan de MVPD login pagina leiden en geldige geloofsbrieven verstrekken.

1. **Volledige authentificatie MVPD:** als de authentificatiestroom succesvol is, slaat de gebruikersagent interactie een regelmatig profiel in de Adobe Pass achterkant op en bereikt verstrekte `redirectUrl`.

1. **wint profiel voor specifieke code terug:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om profielinformatie terug te winnen door een verzoek naar het eindpunt van Profielen te verzenden.

   >[!IMPORTANT]
   >
   > Verwijs naar [ terugwinnen profiel voor specifieke code ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API documentatie voor details op:
   >
   > * Alle _vereiste_ parameters, als `serviceProvider` en `code`
   > * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   > * Alle _facultatieve_ parameters en kopballen

   >[!NOTE]
   >
   > Suggestie: de streamingtoepassing kan een opiniepeilingsmechanisme implementeren met behulp van `code` om te controleren of het standaardprofiel is gegenereerd en opgeslagen.

1. **de informatie van de Terugkeer over regelmatig profiel:** de het eindpuntreactie van Profielen bevat informatie over het regelmatige profiel verbonden aan de ontvangen parameters en kopballen.

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
