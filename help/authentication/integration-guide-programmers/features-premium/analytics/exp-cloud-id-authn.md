---
title: Experience Cloud-id gebruiken in Adobe Pass-verificatie
description: Experience Cloud-id gebruiken in Adobe Pass-verificatie
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Experience Cloud-id gebruiken in Adobe Pass-verificatie

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Wat is Experience Cloud-id en hoe kan ik deze verkrijgen? {#what-exp-cloud-id-obtain}

De Experience Cloud-id (ECID for short) is een unieke id die door Adobe Experience Cloud wordt gegenereerd voor elke afzonderlijke gebruiker in uw toepassing/website. ECID wordt veel gebruikt in alle Experience Cloud-rapporten die worden gebruikt om informatie over een specifieke gebruiker te koppelen in meerdere toepassingen/websites.

Als u al een systeem hebt dat een bezoekersidentiteitskaart verstrekt, zou u zelfde identiteitskaart voor het werkingsgebied van dit document moeten gebruiken.

Een manier om de ECID te verkrijgen, is door gebruik te maken van Experience Cloud ID Service. U kunt het gewenste implementatietype gebruiken op basis van TDM, JS-bibliotheek, server, directe integratie of native bibliotheken voor mobiele platforms. Voor een uitgebreide weergave van beschikbare services, bibliotheken, SDK en implementatiegidsen raadpleegt u: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=nl-NL>

## Wat is het voordeel van het gebruik van de Experience Cloud-id in Adobe Pass-verificatie? {#benefit-ex-cloud-id}

Als u onze SDK&#39;s en client REST API configureert voor gebruik van uw ECID, kunt u de gegevens die door Adobe Pass Authentication zijn verzameld later koppelen aan uw bestaande Experience Cloud-oplossingen. Hierdoor kunt u de reis en ervaring van uw klanten beter begrijpen voor alle oplossingen die door Adobe worden geleverd.

## Hoe kan ik de Experience Cloud-id gebruiken in Adobe Pass-verificatie? {#how-to-ex-cloud-id-authn}

Nadat u de ECID hebt ontvangen (zie hierboven), moet u deze informatie doorgeven aan onze SDK&#39;s en onze client-less REST API. Deze informatie zal later aan onze servers op elke netwerkvraag worden overgegaan die de SDK maakt. Het configuratieproces is voor elke SDK als volgt verschillend:

### JS SDK {#js-sdk}

Voor JavaScript moet u de ECID in een kaart doorgeven als de derde parameter aan de aanroep setRequestor.

**Voorbeeld van het Gebruik:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

Voor iOS/tvOS SDK is er een speciale methode genaamd setOptions.

**Voorbeeld van het Gebruik:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Voor Android/fireTV SDK is het mechanisme vergelijkbaar met iOS. Alleen de parameternaam is anders. De API wordt hier beschreven.

**Voorbeeld van het Gebruik:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### Clientloze API {#clientless-api}

Wanneer het gebruiken van Adobe Pass via het REST API v1 is, zou de **ECID** waarde **op alle APIs** als parameter genoemd **&quot;ap_vi&quot;** moeten worden verzonden.

**Voorbeeld van het Gebruik:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

Wanneer het gebruiken van Adobe Pass via het REST API v2 is, zou de **ECID** waarde **op alle APIs** als kopbal moeten worden verzonden genoemd **&quot;AP-Bezoeker-Herkenningsteken&quot;**.

**Voorbeeld van het Gebruik:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
Kopteksten:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

