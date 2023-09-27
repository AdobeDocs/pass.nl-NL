---
title: Een privacyverzoek indienen
description: Een privacyverzoek indienen
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Een privacyverzoek indienen {#howto-make-privacy-request}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Id&#39;s en naamruimten {#identifier-namespace}

Wanneer het verzenden van een verzoek van de Privacy van de Toegang of van de Schrapping, moet de klantentoepassing de volgende herkenningstekens omvatten:

* **mvpdID** - Unieke identificatiecode van de MVPD.
* **userID** - De gebruiker van de app van een programmeur wordt op unieke wijze geïdentificeerd, maar is afkomstig van de MVPD. Zie Werken met gebruikers-id&#39;s in het overzicht van de programmeur.
* **IMSOrgID** - de Adobe Experience Cloud Identity Management Service Organization ID, die de klant in de Adobe Experience Cloud op unieke wijze identificeert


Controleer het onderstaande voorbeeld:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Gebruikers moeten zijn geverifieerd om privacyverzoeken voor Adobe Pass-verificatie te kunnen genereren. Anders, moeten de programmeurs andere middelen vinden om MVPD gebruikerID te halen.

## Typen verzoeken {#req-type}

Adobe Pass-verificatie ondersteunt aanvragen voor toegang en verwijderen.

### Toegang {#access-req}

Voor een verzoek om toegang:

wij zullen een JSON dossier teruggeven dat een samenvatting van totaal aantal authentificatie en vergunningsverzoeken bevat die voor dat Onderwerp van Gegevens worden gecreeerd.
al deze gebeurtenissen worden per klant gefilterd.


**Voorbeeld aanvragen**

U moet een JSON uploaden met de Adobe Pass-verificatie-id&#39;s waarvoor u de aanvraag voor gegevenstoegang verzendt. Zie dit voorbeeld voor informatie over hoe een goed gevormde JSON eruitziet:

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Antwoordmonster**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Verwijderen {#delete-req}

U moet een JSON uploaden met de id&#39;s voor Adobe Pass-verificatie waarvoor u de aanvraag tot gegevensverwijdering verzendt. Zie dit voorbeeld voor informatie over hoe een goed gevormde JSON eruitziet:

**Voorbeeld aanvragen**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Antwoordmonster**

Voor een verwijderaanvraag:

* wij delen slechts een ontvangstbewijs dat de gegevens werden verwijderd, niet een geaggregeerd dossier met alle gegevens die werden verwijderd.
* het ontvangstbewijs dat in de reactie is opgenomen, bevat een samenvatting van het totale aantal verificatie- en autorisatietokens dat voor die betrokkene is aangetroffen.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Hoe te om een verzoek in werking te stellen {#trigger-req}

Er zijn twee opties voor klanten om privacyverzoeken naar Adobe te verzenden:

* **handmatig** - door [Gebruikersinterface Privacy Service](#privacy-service-ui)
* **automatisch** - door [Privacy Service-API](#privacy-service-api)

### Door Privacy Service UI te gebruiken {#privacy-service-ui}

A [complete zelfstudie](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) voor toegang tot en gebruik van de gebruikersinterface van de Privacy Service is online beschikbaar via de services van de Adobe I/O. Bovendien kunnen klanten deze koppeling gebruiken om toegang te krijgen tot een bibliotheek met video&#39;s en artikelen over privacyregels. Klik op het menu Adobe Experience Cloud en GDPR. Hiermee wordt een aantal video&#39;s geopend - &quot;GDPR UI How-to&quot; legt uit hoe u deze kunt gebruiken.

In de gebruikersinterface moeten klanten hun eigen IMSOrgID laden en een JSON met GDPR-aanvraaggegevens voor elk product.

### Door Privacy Service API te gebruiken {#privacy-service-api}

Adobe Experience Platform Privacy Service biedt een gemeenschappelijke, gecentraliseerde facilitering van toegang tot/verwijderingsverzoeken en opt-out-of-sales aanvragen voor privégegevens.

De **Privacy Service API-documentatie** behandelt diepgaand hoe een Adobe klant met Adobe API kan integreren.

**API-aanroepen visualiseren met Postman (een gratis software van derden):**

* [Privacy Service API Postman-verzameling op GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Videohulplijn voor het maken van de Postman-omgeving](https://video.tv.adobe.com/v/28832)
* [Stappen voor het importeren van omgevingen en verzamelingen in Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API-paden:**

* URL PLATFORM-gateway: `https://platform.adobe.io/`
* Basispad voor deze API: `/data/core/privacy/jobs`
* Voorbeeld van een volledig pad: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Vereiste kopteksten:**

* Alle vraag vereist de kopballen `Authorization`, `x-gw-ims-org-id`, en `x-api-key`. Zie voor meer informatie over het verkrijgen van deze waarden de **verificatiezelfstudie**.
* Alle verzoeken met een lading in het verzoeklichaam (zoals POST, PUT, en de vraag van PATCH) moeten de kopbal omvatten `Content-Type` met een waarde van `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
