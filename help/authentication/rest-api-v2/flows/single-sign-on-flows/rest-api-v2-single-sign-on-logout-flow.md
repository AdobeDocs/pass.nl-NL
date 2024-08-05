---
title: Single Logout, Flow
description: REST API V2 - Single Logout - Flow
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---


# Single Logout-flow {#single-logout-flow}

## Eén aanmelding voor specifieke mvpd starten {#initiate-single-logout-for-specific-mvpd}

### Vereisten {#prerequisites-initiate-single-logout-for-specific-mvpd}

Alvorens enige logout voor een specifieke MVPD in werking te stellen, zorg ervoor dat aan de volgende voorwaarden wordt voldaan:

* De tweede streamingtoepassing moet een geldig Single Sign-On-profiel hebben dat is gemaakt voor de MVPD met één van de Single Sign-On verificatiestromen:
   * [Verificatie uitvoeren via Single Sign-On met behulp van platformidentiteit](./rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Verificatie uitvoeren via Single Sign-On met gebruik van servicetoken](./rest-api-v2-single-sign-on-service-token-flows.md)
* De tweede het stromen toepassing moet de enige logout stroom in werking stellen wanneer het uit MVPD moet ondertekenen.

>[!IMPORTANT]
> 
> Veronderstellingen
>
> <br/>
> 
> * De eerste en tweede streamingtoepassing krijgen dezelfde unieke platform-id als `JWS` of `JWE` , of dezelfde unieke gebruikersnaam als `JWS` .

### Workflow {#workflow-initiate-single-logout-for-specific-mvpd}

Voer de gegeven stappen uit om de enige logout stroom voor een specifieke MVPD zoals aangetoond in het volgende diagram uit te voeren.

![ Begin enige logout voor specifieke mvpd ](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Begin enige logout voor specifieke mvpd*

1. **stelt Adobe Pass logout in werking:** de het stromen toepassing verzamelt alle noodzakelijke gegevens om de logout stroom in werking te stellen door het de Logout van Adobe Pass eindpunt te roepen.

   Verwijs naar [ Logout van het Begin voor specifieke mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API documentatie voor details op:
   * Alle _vereiste_ parameters, als `serviceProvider`, `mvpd`, en `redirectUrl`
   * Alle _vereiste_ kopballen, als `Authorization`, `AP-Device-Identifier`
   * Alle _facultatieve_ parameters en kopballen

   >[!IMPORTANT]
   > 
   > De streamingtoepassing moet ervoor zorgen dat deze een geldige waarde voor de unieke platform-id of de unieke gebruikers-id bevat voordat u een aanvraag indient.
   >
   > <br/>
   > 
   > Voor meer details over `Adobe-Subject-Token` kopbal, verwijs naar [ Adobe-Onderwerp-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentatie.
   > 
   > <br/>
   > 
   > Voor meer details over `AD-Service-Token` kopbal, verwijs naar [ AD-dienst-Symbolische ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) documentatie.

1. **vind regelmatige en enige sign-on profielen:** de server van Adobe Pass identificeert zowel regelmatige als enige teken-op geldige profielen die op de ontvangen parameters en kopballen worden gebaseerd.

1. **Schrap regelmatige en enige sign-on profielen:** de server van Adobe Pass schrapt de geïdentificeerde regelmatige en enige sign-on profielen van de achtergrond van Adobe Pass.

1. **wijs op de volgende actie:** De het eindpuntreactie van het Logout van Adobe Pass bevat de noodzakelijke gegevens om de het stromen toepassing betreffende de volgende actie te begeleiden.

   Verwijs naar [ Logout van het Begin voor specifieke mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API documentatie voor details op de informatie die in een logout reactie wordt verstrekt.

   >[!IMPORTANT]
   >
   > Het Adobe Pass Logout eindpunt valideert de verzoekgegevens om ervoor te zorgen dat de basisvoorwaarden worden voldaan:
   >
   > * De _vereiste_ parameters en de kopballen moeten geldig zijn.
   > * De integratie tussen de opgegeven `serviceProvider` en `mvpd` moet actief zijn.
   >
   > <br/>
   > 
   > Als de bevestiging ontbreekt, zal een foutenreactie worden geproduceerd, verstrekkend extra informatie die aan de [ Verbeterde documentatie van de Codes van de Fout ](../../../enhanced-error-codes.md) volgt.

1. **wijs logout volledig op:** als MVPD niet de logout stroom steunt, verwerkt de het stromen toepassing de reactie en kan het naar keuze gebruiken om een specifiek bericht op het gebruikersinterface te tonen.

1. **initieert MVPD logout:** als MVPD de logout stroom steunt, verwerkt de het stromen toepassing de reactie en gebruikt een gebruikersagent om de logout stroom met MVPD in werking te stellen. De stroom kan verscheidene omleidingen aan systemen omvatten MVPD. Toch is het resultaat dat de MVPD zijn interne schoonmaak uitvoert en de definitieve logout bevestiging terug naar de Adobe Pass backend verzendt.

1. **wijs logout volledig op:** de het stromen toepassing kan op de gebruikersagent wachten om verstrekte `redirectUrl` te bereiken en kan het als signaal gebruiken om naar keuze een specifiek bericht op het gebruikersinterface te tonen.

>[!NOTE]
>
> De stappen voor de enige logout-flow zijn dezelfde als hierboven, als deze worden gestart vanaf de eerste streamingtoepassing.
