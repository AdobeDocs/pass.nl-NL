---
title: Gradatie-API - overzicht
description: Gradatie-API - overzicht
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 95c3b1cbce4a591ce387ae3b242721e50ba2ddb1
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Gradatie-API - overzicht {#degradation-api-overview}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Controleer voordat u de afbraakAPI gebruikt of aan de volgende voorwaarden is voldaan:
>
> * Haal de cliëntgeloofsbrieven zoals die in [ worden beschreven terug cliëntgeloofsbrieven ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
> * Haal het toegangstoken zoals die in [ wordt beschreven terug toegangstoken ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
>
> Verwijs naar de [ Dynamische documentatie van het Overzicht van de Registratie van de Cliënt ](./dcr-api/dynamic-client-registration-overview.md) voor meer informatie over hoe te om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

## Algemene informatie {#general_info}

>[!NOTE]
>
>Deze API is niet algemeen beschikbaar. Neem contact op met uw Adobe voor updates van de beschikbaarheid.

Deze eigenschap verstrekt om het even welke drie belangrijkste partijen in een integratie (Programmers, MVPDs, en Adobe) met de capaciteit om specifieke eindpunten van de Authentificatie en van de Vergunning tijdelijk over te slaan MVPD. Meestal is het de programmeur die een dergelijke actie initieert, maar ongeacht wie een degradatiegebeurtenis activeert, is de actie afhankelijk van eerder overeengekomen regelingen met de betrokken MVPD&#39;s.

Het hoofdgebruik voor deze functie vindt plaats tijdens livesporten of grote evenementen. In dergelijke hoge verkeersscenario&#39;s is het mogelijk voor de lading op een specifiek eindpunt MVPD om te hoog te worden, resulterend in zeer lange reactietijden voor gebruikers. Om een goede gebruikerservaring tijdens zulk een scenario te bewaren, kan de programmeur besluiten om een degradatieregel teweeg te brengen die gebruikers tijdelijk auto-authenticate/auto-autoriseert, of MVPD onbruikbaar te maken door het uit de beschikbare lijst MVPDs te verwijderen.

Een degradatieregel wordt slechts voor een bepaalde periode toegepast. Hoewel de primaire klanten voor deze eigenschap sportkanalen en levende nieuwskanalen zijn, zou om het even welke Programmer toegang tot deze eigenschap kunnen willen hebben, aangezien de diensten MVPDs van tijd tot tijd dalen.

Afbraaknotities:

- Deze functie is ontworpen voor gebruik in combinatie met de API voor gebruiksbewaking, die realtime informatie biedt over het aantal verificaties en autorisaties per MVPD, de gemiddelde vertraging bij autorisatie en andere metriek die nodig is voor een volledig serviceoverzicht.
- Met deze functie kan de Adobe Pass-verificatieservice niet worden overgeslagen. Als de Authentificatie van Adobe Pass neer is is er geen mechanisme binnen de dienst die kan worden gebruikt om gebruikers toe te staan om inhoud te zien. De sites of apps kunnen echter zelf de Adobe Pass-verificatie omzeilen.
- Adobe leidt momenteel niet tot een directe verslechtering; het besluit moet altijd in handen zijn van een specifieke programmeur die met de MVPD&#39;s heeft ingestemd. In de toekomst kan Adobe Pass Authentication proactief zijn in het activeren van degradatieregels als overeenkomsten (bescherming van SLA) kunnen worden gesloten met MVPD&#39;s.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
