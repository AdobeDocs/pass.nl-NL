---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-verificatie
user-guide-description: Adobe Pass-verificatie is een machtigingsoplossing voor TV Everywhere, die een modulair kader verstrekt om te bepalen of iemand die toegang tot een bron vraagt, daar rechten voor heeft.
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 2%

---


# Help bij Adobe Pass-verificatie {#authentication}

+ [Overzicht van Adobe Pass-verificatie](home.md)
+ Concepten van Adobe Pass-verificatie {#authentication-concepts}
   + [Technisch document](technical-paper.md)
   + [Overzicht voor programmeurs](programmer-overview.md)
   + [MVPD-overzicht](mvpd-overview.md)
+ Hulplijnen bij Kickstart {#kickstart-guides}
   + [Kickstart-handleiding voor programmeurs](programmer-kickstart-guide.md)
   + [MVPD kickstart-handleiding](mvpd-kickstart-guide.md)
+ Programmeringsintegratiehandleiding {#programmer-integration-guide}
   + [Overzicht van de integratiegids voor programmeerprogramma&#39;s](programmer-integration-guide-overview.md)
   + [De machtigingsstroom van de programmeur](entitlement-flow.md)
   + [Gebruiksgevallen voor programmeerprogramma&#39;s](programmer-use-cases.md)
   + [Clientgegevens doorgeven (apparaat, verbinding en toepassing)](passing-client-information-device-connection-and-application.md)
   + [Draaimechanisme](throttling-mechanism.md)
   + REST API V1 {#rest-api-v1}
      + [REST API-overzicht](rest-api-overview.md)
      + [REST API Cookbook (Server-to-Server)](rest-api-cookbook-servertoserver.md)
      + [REST API Cookbook (client-naar-server)](rest-api-cookbook-clienttoserver.md)
      + Referentie voor API opnieuw instellen {#rest-api-reference}
         + [REST API Reference](rest-api-reference.md)
         + [Registratiecode-aanvraag](registration-code-request.md)
         + [Registratierecord retourneren](return-registration-record.md)
         + [Registratierecord verwijderen](delete-registration-record.md)
         + [MVPD-lijst opgeven](provide-mvpd-list.md)
         + [Verificatie starten](initiate-authentication.md)
         + [Verificatietoken controleren](check-authentication-token.md)
         + [Verificatietoken ophalen](retrieve-authentication-token.md)
         + [Autorisatie starten](initiate-authorization.md)
         + [Token voor autorisatie ophalen](retrieve-authorization-token.md)
         + [Token voor korte media verkrijgen](obtain-short-media-token.md)
         + [Verificatiestroom controleren op tweede scherm van webtoepassing](check-authentication-flow-by-second-screen-web-app.md)
         + [Lijst met vooraf gemachtigde bronnen ophalen](retrieve-list-of-preauthorized-resources.md)
         + [Lijst met vooraf geautoriseerde bronnen ophalen via tweede webtoepassing voor scherm](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Afmelden starten](initiate-logout.md)
         + [Metagegevens gebruiker](user-metadata.md)
         + [Profiel-verzoek ophalen](retrieve-profilerequest.md)
         + [Tokenuitwisseling](token-exchange.md)
         + [Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + REST API V2 {#rest-api-v2}
      + API&#39;s {#rest-api-v2-apis}
         + Configuratie {#rest-api-v2-configuration-apis}
            + [Win configuratie voor specifieke dienstverlener terug](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sessies {#rest-api-v2-sessions-apis}
            + [Verificatiesessie maken](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [Verificatiesessie hervatten](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [Verificatiesessie ophalen](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
         + Profielen {#rest-api-v2-profiles-apis}
            + [Profielen ophalen](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [Profiel ophalen voor specifieke mvpd](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md)
            + [Profiel ophalen voor specifieke code](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md)
         + Besluiten {#rest-api-v2-decisions-apis}
            + [Autorisatiebesluiten ophalen met specifieke mvpd](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [ wint pre-vergunningsbesluiten terug gebruikend specifieke mvpd ](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + Afmelden {#rest-api-v2-logout-apis}
            + [Afmelden starten voor specifieke mvpd](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + Single Sign-On voor partners {#rest-api-v2-partner-single-sign-on-apis}
            + [Vraag van partnerverificatie ophalen](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [Profiel ophalen met verificatierespons van partner](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + Stromen {#rest-api-v2-flows}
         + Basisstromen {#rest-api-v2-basic-flows}
            + [Stroom van basisprofielen uitgevoerd in primaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [Basisuitlogingsstroom uitgevoerd in primaire toepassing](./rest-api-v2/flows/basic-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + Verminderde toegangsstromen {#rest-api-v2-degraded-access-flows}
            + [Verminderde toegangsstromen](rest-api-v2/flows/access-degraded-flows/rest-api-v2-access-degraded-flows.md)
         + Tijdelijke toegangsstromen {#rest-api-v2-temporary-access-flows}
            + [ Tijdelijke toegangsstromen ](rest-api-v2/flows/access-temporary-flows/rest-api-v2-access-temporary-flows.md)
         + Single Sign-On-stromen {#rest-api-v2-single-sign-on-flows}
            + [Enig teken-op het gebruiken van partnerstromen](./rest-api-v2/flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [Single Sign-On met gebruik van platformidentiteitsstromen](./rest-api-v2/flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [Single Sign-On die de stromen van het de dienstteken gebruikt](./rest-api-v2/flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [Single Logout-flow](./rest-api-v2/flows/single-sign-on-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Bijlage {#rest-api-v2-appendix}
         + Kopteksten {#rest-api-v2-appendix-headers}
            + [Koptekst - AD-Service-token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [Koptekst - Onderwerptoken voor Adobe](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [Koptekst - AP-apparaat-id](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [Koptekst - AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [Koptekst - AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
            + [Koptekst - X-Apparaat-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [Overzicht van JavaScript SDK](javascript-sdk-overview.md)
         + [JavaScript SDK Cookbook](javascript-sdk-cookbook.md)
         + [JavaScript SDK API-naslag](javascript-sdk-api-reference.md)
         + Richtlijnen {#js-sdk-guidelines}
            + [Aanmelding en afmelding zonder vernieuwen](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [Voorvoegsel](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [Overzicht iOS/tvOS SDK](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK Cookbook](iostvos-sdk-cookbook.md)
         + [ iOS/tvOS SDK API Verwijzing ](iostvos-sdk-api-reference.md)
         + Richtlijnen {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS-toepassingsregistratie](iostvos-application-registration.md)
            + Richtlijnen voor migratie {#migration-guidelines}
               + [Migratiehandleiding voor iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Integriteitscontroles iOS/tvOS-opslag](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [ preAuthze ](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Overzicht van Android SDK](android-sdk-overview.md)
         + [Android SDK Cookbook](android-sdk-cookbook.md)
         + [Android SDK API-naslag](android-sdk-api-reference.md)
         + Richtlijnen {#androidguidelines}
            + [Android-toepassingsregistratie](android-application-registration.md)
            + [Android SDK met Dynamic Client-registratie](android-sdk-with-dynamic-client-registration.md)
         + Android API {#androidapi}
            + [Voorvoegsel](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO - Handleiding voor het starten van de programma&#39;s](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO met Cookbook zonder client](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Technisch overzicht van Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Amazon FireOS Integration Cookbook](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API-naslaggids](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS-toepassingsregistratie](amazon-fireos-application-registration.md)
         + [FireOS SDK met Dynamic Client-registratie](fireos-sdk-with-dynamic-client-registration.md)
   + Platform SSO {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Apple SSO - Overzicht](apple-sso-overview.md)
         + [Apple SSO Cookbook (REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO Cookbook (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [ Roku SSO ](roku-sso-overview.md)
   + Metagegevens inhoud {#content-metadata}
      + [Beveiligde bron identificeren](identify-protected-resources.md)
   + Integratie van inhoudsservers {#content-serv-int}
      + [Hoe te om de Symbolische Verifier van Media te integreren](media-token-verifier-int.md)
   + Bijlagen {#appendices}
      + [Tips voor foutopsporing](appendix-b-debugging-tips.md)
+ MVPD-integratiehandleiding {#mvpd-int-guide}
   + [Integratiefuncties](mvpd-integr-features.md)
   + [Verificatie](authn-usecase.md)
   + [Verificatie met het OAuth 2.0-protocol](authn-oauth2-protocol.md)
   + [Toestemming](authz-usecase.md)
   + [Preflight-autorisatie](mvpd-preflight-authz.md)
   + [MVPD Logout](usecase-mvpd-logout.md)
   + [Uitwisseling van metagegevens voor inhoud](mvpd-content-metadata-exchange.md)
   + [Gegevensuitwisseling van gebruikers](mvpd-user-metadata-exchng.md)
   + [Proxy MVPD-webservice](proxy-mvpd-webserv.md)
   + [Proxy MVPD SAML integratie](proxy-mvpd-saml-int.md)
   + [Serviceleverancier](serv-provider-scoping.md)
   + [MVPD staat IP adressen toe](mvpd-listing-ip-addres.md)
+ Adobe Pass-verificatiefuncties {#auth-features}
   + Adobe Analytics-integratie {#analytics-int}
      + [Gegevens van de Adobe Pass-verificatieserver integreren in Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Experience Cloud-id gebruiken in Adobe Pass-verificatie](exp-cloud-id-authn.md)
   + Bewaking van machtigingsservice {#entitlement-service-monitoring}
      + [Overzicht van Entitlement service-controle](entitlement-service-monitoring-overview.md)
      + [Entitlement service monitoring API](entitlement-service-monitoring-api.md)
      + [Metriek aan de serverzijde](understanding-serverside-metrics.md)
   + Temperatuurcontrole {#temp-pass}
      + [Temperatuurcontrole](temp-pass.md)
      + [Tijdelijke doorloop voor speciale acties](promotional-temp-pass.md)
      + [ Pass van het Terugstellen Temperatuur ](reset-temp-pass.md)
   + Single Sign-On {#sso}
      + [Ondersteuning voor Single Sign-On](sso-support.md)
      + [SSO via passieve verificatie](sso-passive-authn.md)
   + Verificatie op basis van thuisbasis {#home-based-auth}
      + [Thuisgebaseerde verificatie voor tv overal](home-based-authn-tve.md)
      + [HBA-status voor MVPD&#39;s](hba-status-mvpds.md)
   + Metagegevens gebruiker {#user-metadat}
      + [Metagegevens gebruiker](user-metadata-feature.md)
   + [Preflight-autorisatie](preflight-authz.md)
   + Foutrapportage {#error-reportn}
      + [Foutmelding](error-reporting.md)
      + [Verbeterde foutcodes](enhanced-error-codes.md)
   + Clientregistratie {#client-regn}
      + [Dynamische clientregistratie](dynamic-client-registration.md)
      + [Dynamic Client-registratie-API](dynamic-client-registration-api.md)
      + [Dynamisch clientregistratiebeheer](dynamic-client-registration-management.md)
   + Degradatieservice {#degrn-service}
      + [Gradatie-API - overzicht](degradation-api-overview.md)
   + Gereedheid voor privacy {#privacy-readiness}
      + [Overzicht van privé-ondersteuning](privacy-supp-overview.md)
      + [Een privacyverzoek indienen](make-privacy-req.md)
+ Tips en problemen oplossen {#tips-troubleshoot}
   + [MVPD&#39;s toestaan in het dialoogvenster Selectie](allow-mvpd-selectn-dialog.md)
   + [Voorkomen dat MVPD&#39;s het selectiedialoogvenster weergeven](prevent-mvpd-selectn-dialog.md)
+ Ondersteuning {#support}
   + [Doorverwijsprocedures](escalation-procedures.md)
   + [Adobe Pass Adobe PayPal-controle](monitoring-adobe-pay-tv-pass.md)
   + [Minimale systeemvereisten](minimum-system-requirements.md)
+ Opmerkingen bij de release {#release-notes}
   + [Opmerkingen bij de release Adobe Pass Authentication 2.70](auth-rn-270.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.69](auth-rn-269.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.68](auth-rn-268.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.67](auth-rn-267.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.66](auth-rn-266.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.65.1](auth-rn-2651.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.65](auth-rn-265.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.64.1](auth-rn-2641.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.64](auth-rn-264.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.63](auth-rn-263.md)
   + [Opmerkingen bij de release Adobe Pass Authentication 2.62.1](auth-rn-2621.md)
   + Opmerkingen bij de release van JavaScript SDK {#release-notes-javascript}
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Opmerkingen bij de release iOS/tvOS SDK {#release-notes-ios}
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Opmerkingen bij de release van Android SDK {#release-notes-android}
      + [Opmerkingen bij de release Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Technische opmerkingen {#tech-notes}
   + Adobe Pass Authentication SDKs {#primetime-authentication-sdks}
      + [Certificaten Vragen en antwoorden](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [Trackingpreventiebeoordeling - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Trackingpreventiebeoordeling - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Cookies-updates - vlaggen SameSite en Secure](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Access Enabler Android SDK Single Sign-On (SSO) voor Android 10-apps](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass Authentication and the Android 6 &quot;Marshmallow&quot; New Permissions Model](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [WKWebView-ondersteuning op iOS SDK 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Ondersteuning voor SFSafariViewController op iOS SDK 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS-verificatiefout - adobepass.ios.app is niet gevonden](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Tijdelijke controle opnieuw instellen op iOS](reset-temp-pass-on-ios.md)
         + [Fouten opsporen in de AccessEnabler iOS/tvOS SDK met behulp van console-app-logboeken](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [ AccessEnabler iOS/tvOS 3.7.0 de Weg van de Verbetering ](accessenabler-iostvos-370-upgrade-path.md)
   + Verificatieomgevingen doorgeven {#primetime-authentication-environments}
      + [De Adobe omgevingen begrijpen](understanding-the-adobe-environments.md)
      + [Uw omgeving instellen en testen in een proefversie](setting-up-your-environment-and-testing-in-prequal.md)
      + [Verificatie- en autorisatiestromen testen met de testsite voor Adobe API](test-authn-authz-flows-using-adobes-api-test-site.md)
   + Clientless-API {#clientless-api}
      + [Implementatie van de API zonder client - foutcodes / berichten met mogelijke reden / oorzaak](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Clientless API Flow bij afwezigheid van apparaat-id](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Zonder clip: gebruik geen &#39;&amp;&#39;reg_code in /authenticate Request](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Adobe Pass Entitlement Services inschakelen voor een programmeur op Xbox 360 en XboxOne-client](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Apparaattype en afmetingen zonder clip](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Gebruikerservaring {#user-exp}
      + [Hoe te om de MVPD login pagina van iFrame aan popup te migreren](migr-mvpd-login-iframe-popup.md)
      + [Preflight-functie: Het probleem inschakelen, oplossen of vaststellen](preflight-feature.md)
   + Gereedschappen en hulpprogramma&#39;s {#tools-and-utilities}
      + [Charles Proxy gebruiken](using-charles-proxy.md)
   + Concepten {#concepts}
      + [ Begrijpend Gebruiker - IDs ](understanding-user-ids.md)
+ [Gebruikershandleiding voor TVE Dashboard](tve-dashboard-user-guide.md)
+ Nieuwe TVE-dashboardgebruikershandleiding {#user-guide}
   + [Overzicht van TVE-dashboard](/help/authentication/tve-dashboard-overview.md)
   + [Omgevingen](/help/authentication/tve-dashboard-environments.md)
   + [Wijzigingen controleren en duwen](/help/authentication/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/tve-dashboard-home.md)
   + [Kanalen](/help/authentication/tve-dashboard-channels.md)
   + [Programmeurs](/help/authentication/tve-dashboard-programmers.md)
   + [MVPD&#39;s](/help/authentication/tve-dashboard-mvpds.md)
   + [Integraties](/help/authentication/tve-dashboard-integrations.md)
   + [Rapporten](/help/authentication/tve-dashboard-reports.md)
   + [Wijzigingenlogboek](/help/authentication/tve-dashboard-changes-log.md)
+ [Verklarende woordenlijst](glossary.md)

