---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-verificatie
user-guide-description: Adobe Pass-verificatie is een machtigingsoplossing voor TV Everywhere, die een modulair kader verstrekt om te bepalen of iemand die toegang tot een bron vraagt, daar rechten voor heeft.
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 2%

---


# Help bij Adobe Pass-verificatie {#authentication}

+ [Adobe Pass-verificatie](home.md)
+ Kickstart {#kickstart}
   + [Technisch document](kickstart/technical-paper.md)
   + [Overzicht van programmeerprogramma](kickstart/programmer-overview.md)
   + [MVPD-overzicht](kickstart/mvpd-overview.md)
   + [Kickstart-handleiding voor programmeurs](kickstart/programmer-kickstart-guide.md)
   + [MVPD kickstart-handleiding](kickstart/mvpd-kickstart-guide.md)
   + [Doorverwijsprocedures](notes-technical/escalation-procedures.md)
   + [Verklarende woordenlijst](kickstart/glossary.md)
+ Integratiehandleiding voor programmeurs {#integration-guide-programmers}
   + REST API&#39;s {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [Overzicht van dynamische clientregistratie](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + API&#39;s {#rest-api-dcr-apis}
            + [Client-referenties ophalen](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Toegangstoken ophalen](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Stromen {#rest-api-dcr-flows}
            + [Dynamische clientregistratiestroom](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [Overzicht van REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Woordenlijst REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [Veelgestelde vragen over REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API&#39;s {#rest-api-v2-apis}
            + [REST API V2 API&#39;s - Overzicht](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Configuratie {#rest-api-v2-configuration-apis}
               + [Win configuratie voor specifieke dienstverlener terug](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessies {#rest-api-v2-sessions-apis}
               + [Verificatiesessie maken](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Verificatiesessie hervatten](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Verificatiesessie ophalen](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Verificatie uitvoeren in gebruikersagent](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profielen {#rest-api-v2-profiles-apis}
               + [Profielen ophalen](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Profiel ophalen voor specifieke mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Profiel ophalen voor specifieke code](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Besluiten {#rest-api-v2-decisions-apis}
               + [Autorisatiebesluiten ophalen met specifieke mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Besluiten vóór toelating met specifieke mvpd ophalen](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Afmelden {#rest-api-v2-logout-apis}
               + [Afmelden starten voor specifieke mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Single Sign-On voor partners {#rest-api-v2-partner-single-sign-on-apis}
               + [Vraag van partnerverificatie ophalen](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Profiel ophalen met verificatierespons van partner](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Stromen {#rest-api-v2-flows}
            + [Overzicht van REST API V2-stromen](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Basistoegangsstromen {#rest-api-v2-basic-access-flows}
               + [Stroom van basisprofielen uitgevoerd in primaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [De stroom van basisprofielen die binnen secundaire toepassing wordt uitgevoerd](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Standaardverificatiestroom uitgevoerd binnen primaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Basisverificatiestroom uitgevoerd binnen secundaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Basis preautorisatiestroom uitgevoerd binnen primaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Basisuitlogingsstroom uitgevoerd in primaire toepassing](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Verminderde toegangsstromen {#rest-api-v2-degraded-access-flows}
               + [Verminderde toegangsstromen](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Tijdelijke toegangsstromen {#rest-api-v2-temporary-access-flows}
               + [Tijdelijke toegangsstromen](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Single Sign-On toegangsstromen {#rest-api-v2-single-sign-on-access-flows}
               + [Enig teken-op het gebruiken van partnerstromen](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Single Sign-On met gebruik van platformidentiteitsstromen](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Single Sign-On die de stromen van het de dienstteken gebruikt](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Single Logout-flow](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbooks {#rest-api-v2-cookbooks}
            + [REST API V2 Cookbook (client-naar-server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2 Cookbook (Server-to-Server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Bijlage {#rest-api-v2-appendix}
            + Kopteksten {#rest-api-v2-appendix-headers}
               + [Koptekst - Autorisatie](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Koptekst - AP-apparaat-id](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Koptekst - X-Apparaat-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Koptekst - AD-Service-token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Koptekst - Onderwerptoken voor Adobe](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Koptekst - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Koptekst - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Standaardfuncties {#standard-features}
      + Entitlements {#entitlements}
         + [Beveiligde bron identificeren](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [Preflight-autorisatie](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Hoe te om de Symbolische Verifier van Media te integreren](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [Metagegevens gebruiker](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [Metagegevenscertificaat van gebruiker voor versleuteling](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + Fout bij rapporteren {#error-reporting}
         + [Verbeterde foutcodes](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [Foutmelding](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + Single Sign-On Access {#sso-access}
         + Single Sign-On voor partners {#partner-sso}
            + Apple Single Sign-On {#apple-sso}
               + [Apple SSO - Overzicht](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Apple SSO Cookbook (REST API V1)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Apple SSO Cookbook (iOS/tvOS SDK)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Platform Single Sign-On {#platform-sso}
            + Amazon Single Sign-On {#amazon-sso}
               + [Amazon SSO Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Amazon SSO Cookbook (REST API V1)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Single Sign-On {#roku-sso}
               + [Overzicht van Roku SSO](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [Ondersteuning voor Single Sign-On](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [SSO via passieve verificatie](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + Toegang tot verificatie op thuisbasis {#hba-access}
         + [Thuisgebaseerde verificatie voor tv overal](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [HBA-status voor MVPD&#39;s](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Privacy-ondersteuning {#privacy-support}
         + [Overzicht van privé-ondersteuning](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Een privacyverzoek indienen](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Premiumfuncties {#features-premium}
      + Tijdelijke toegang {#temporary-access}
         + [Temperatuurcontrole](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [Tijdelijke doorloop voor speciale acties](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Tijdelijke controle opnieuw instellen](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Verminderde toegang {#degraded-access}
         + [ Verslechterings API Overzicht ](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Overzicht van Entitlement service-controle](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [Entitlement service monitoring API](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Metriek aan de serverzijde](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Gegevens van de Adobe Pass-verificatieserver integreren in Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Experience Cloud-id gebruiken in Adobe Pass-verificatie](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Verouderd {#legacy}
      + (Verouderd) REST API V1 {#rest-api-v1}
         + [Overzicht van REST API V1](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [REST API V1 Reference](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + (Verouderd) API&#39;s {#rest-api-v1-apis}
            + [Registratiecode-aanvraag](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Registratierecord retourneren](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Registratierecord verwijderen](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [MVPD-lijst opgeven](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [Verificatie starten](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Verificatietoken controleren](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [Verificatietoken ophalen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [Autorisatie starten](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [Token voor autorisatie ophalen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [Token voor korte media verkrijgen](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [Verificatiestroom controleren op tweede scherm van webtoepassing](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [Lijst met vooraf gemachtigde bronnen ophalen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [Lijst met vooraf geautoriseerde bronnen ophalen via tweede webtoepassing voor scherm](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [Afmelden starten](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Metagegevens gebruiker](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [Profiel-verzoek ophalen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Tokenuitwisseling](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Gratis voorvertoning voor tijdelijke controle en tijdelijke controle voor speciale acties](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + (Verouderd) Cookbooks {#rest-api-v1-cookbooks}
            + [REST API V1 Cookbook (client-naar-server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [REST API V1 Cookbook (Server-to-Server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + (Verouderd) SDK&#39;s {#sdks}
         + (Verouderd) JavaScript SDK {#javascript-sdk}
            + [Overzicht van JavaScript SDK](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [JavaScript SDK Cookbook](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [JavaScript SDK API-naslag](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [JavaScript SDK API vooraf autoriseren](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + (Verouderd) Richtlijnen {#javascript-sdk-guidelines}
               + [Aanmelding en afmelding zonder vernieuwen](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Verouderd) iOS/tvOS SDK {#ios-tvos-sdk}
            + [Overzicht iOS/tvOS SDK](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [iOS/tvOS SDK Cookbook](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [iOS/tvOS SDK API-naslaggids](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [iOS/tvOS SDK API Voorkeuren](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + (Verouderd) Richtlijnen {#ios-tvos-sdk-guidelines}
               + [iOS/tvOS-toepassingsregistratie](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Migratiehandleiding voor iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Integriteitscontroles iOS/tvOS-opslag](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Verouderd) Android SDK {#android-sdk}
            + [Overzicht van Android SDK](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Android SDK Cookbook](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Android SDK API-naslag](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Android SDK API vooraf autoriseren](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + (Verouderd) Richtlijnen {#android-sdk-guidelines}
               + [Android-toepassingsregistratie](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [Android SDK met Dynamic Client-registratie](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Verouderd) FireOS SDK {#fireos-sdk}
            + [Technisch overzicht van Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Amazon FireOS Integration Cookbook](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Amazon FireOS API-naslaggids](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Amazon FireOS-toepassingsregistratie](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [FireOS SDK met Dynamic Client-registratie](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [Amazon FireOS SSO - Handleiding voor het starten van de programma&#39;s](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [Overzicht van de integratiegids voor programmeerprogramma&#39;s](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Draaimechanisme](integration-guide-programmers/throttling-mechanism.md)
   + [Minimale systeemvereisten](integration-guide-programmers/minimum-system-requirements.md)
   + [Machtigingsstroom voor programmeerprogramma&#39;s](integration-guide-programmers/entitlement-flow.md)
   + [Gebruiksgevallen voor programmeerprogramma&#39;s](integration-guide-programmers/programmer-use-cases.md)
   + [Clientgegevens doorgeven (apparaat, verbinding en toepassing)](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ Integratiehandleiding voor MVPD&#39;s {#integration-guide-mvpds}
   + [Integratiefuncties](integration-guide-mvpds/mvpd-integr-features.md)
   + [Verificatie](integration-guide-mvpds/authn-usecase.md)
   + [Verificatie met het OAuth 2.0-protocol](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Toestemming](integration-guide-mvpds/authz-usecase.md)
   + [Preflight-autorisatie](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD Logout](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Uitwisseling van metagegevens voor inhoud](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Gegevensuitwisseling van gebruikers](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Proxy MVPD-webservice](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Proxy MVPD SAML integratie](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Serviceleverancier](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD staat IP adressen toe](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Handboek voor TVE-dashboard {#user-guide-tve-dashboard}
   + [Overzicht van TVE-dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Omgevingen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Wijzigingen controleren en duwen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Kanalen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmeurs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD&#39;s](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integraties](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapporten](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Wijzigingenlogboek](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [Gebruikershandleiding voor TVE Dashboard](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ Opmerkingen bij de release {#release-notes}
   + 2024 {#release-notes-2024}
      + [Opmerkingen bij de release Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#release-notes-2023}
      + [Opmerkingen bij de release Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Opmerkingen bij de release Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#release-notes-2022}
      + [Opmerkingen bij de release Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md)
      + [Opmerkingen bij de release Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md)
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#release-notes-2021}
      + [Opmerkingen bij de release Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Opmerkingen bij de release Adobe Pass Authentication iOS / tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ Technische opmerkingen {#tech-notes}
   + Omgevingen {#tech-notes-environments}
      + [De Adobe omgevingen begrijpen](notes-technical/understanding-the-adobe-environments.md)
      + [Uw omgeving instellen en testen in een proefversie](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [Verificatie- en autorisatiestromen testen met de testsite voor Adobe API](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + Gebruikerservaring {#tech-notes-user-experience}
      + [Hoe te om de MVPD login pagina van iFrame aan popup te migreren](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [Preflight-functie: Het probleem inschakelen, oplossen of vaststellen](notes-technical/preflight-feature.md)
      + [MVPD&#39;s toestaan in het dialoogvenster Selectie](notes-technical/allow-mvpd-selectn-dialog.md)
      + [Voorkomen dat MVPD&#39;s het selectiedialoogvenster weergeven](notes-technical/prevent-mvpd-selectn-dialog.md)
   + REST API V1 {#tech-notes-rest-api-v1}
      + [Implementatie van de API zonder client - foutcodes / berichten met mogelijke reden / oorzaak](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Clientless API Flow bij afwezigheid van apparaat-id](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [Zonder clip: gebruik geen &#39;&amp;&#39;reg_code in /authenticate Request](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Adobe Pass Entitlement Services inschakelen voor een programmeur op Xbox 360 en XboxOne-client](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Apparaattype en afmetingen zonder clip](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDK&#39;s {#tech-notes-sdks}
      + [Certificaten Vragen en antwoorden](notes-technical/certificates-qa.md)
      + [Gebruikersnaam](notes-technical/understanding-user-ids.md)
      + JavaScript SDK {#tech-notes-javascript-sdk}
         + [Trackingpreventiebeoordeling - Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [Trackingpreventiebeoordeling - Google Chrome](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Cookies-updates - vlaggen SameSite en Secure](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [Tips voor foutopsporing](notes-technical/appendix-b-debugging-tips.md)
      + Android SDK {#tech-notes-android-sdk}
         + [Access Enabler Android SDK Single Sign-On (SSO) voor Android 10-apps](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass Authentication and the Android 6 &quot;Marshmallow&quot; New Permissions Model](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#tech-notes-ios-tvos-sdk}
         + [WKWebView-ondersteuning op iOS SDK 3.1+](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [Ondersteuning voor SFSafariViewController op iOS SDK 3.2+](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO op iOS bij gebruik van Adobe Pass Authentication Access Enabler](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS-verificatiefout - adobepass.ios.app is niet gevonden](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Tijdelijke controle opnieuw instellen op iOS](notes-technical/reset-temp-pass-on-ios.md)
         + [Fouten opsporen in de AccessEnabler iOS/tvOS SDK met behulp van console-app-logboeken](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Upgradepad voor iOS/tvOS 3.7.0 inschakelen](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + Problemen oplossen {#tech-notes-troubleshooting}
      + [Charles Proxy gebruiken](notes-technical/using-charles-proxy.md)
      + [Adobe Pass Adobe PayPal-controle](notes-technical/monitoring-adobe-pay-tv-pass.md)
