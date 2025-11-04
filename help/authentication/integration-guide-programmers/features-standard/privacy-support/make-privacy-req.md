---
title: Een privacyverzoek indienen
description: Een privacyverzoek indienen
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Een privacyverzoek indienen {#howto-make-privacy-request}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Id&#39;s en naamruimten {#identifier-namespace}

Wanneer het verzenden van een verzoek van de Privacy van de Toegang of van de Schrapping, moet de klantentoepassing de volgende herkenningstekens omvatten:

* **mvpdID** - Unieke herkenningsteken van MVPD.
* **userID** - identificeert uniek de gebruiker van app van een Programmer, maar organiseert van MVPD. Zie Werken met gebruikers-id&#39;s in het overzicht van de programmeur.
* **IMSOrgID** - identiteitskaart van de Organisatie van de Dienst van Adobe Experience Cloud Identity Management, die uniek de Klant in Adobe Experience Cloud identificeert


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
>Gebruikers moeten zijn geverifieerd om privacyverzoeken voor Adobe Pass-verificatie te kunnen genereren. Anders moeten programmeurs andere manieren vinden om de MVPD userID te extraheren.

## Typen verzoeken {#req-type}

Adobe Pass-verificatie ondersteunt aanvragen voor toegang en verwijderen.

### Toegang {#access-req}

Voor een verzoek om toegang:

wij zullen een JSON dossier teruggeven dat een samenvatting van totaal aantal authentificatie en vergunningsverzoeken bevat die voor dat Onderwerp van Gegevens worden gecreeerd.
al deze gebeurtenissen worden per klant gefilterd.


**steekproef van het Verzoek**

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

**steekproef van de Reactie**

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

**steekproef van het Verzoek**

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

**steekproef van de Reactie**

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

Er zijn twee opties waarmee klanten privacyverzoeken naar Adobe kunnen verzenden:

* **manueel** - door [&#x200B; het Gebruikersinterface van Privacy Service te gebruiken &#x200B;](#privacy-service-ui)
* **automatisch** - door [&#x200B; Privacy Service API &#x200B;](#privacy-service-api) te gebruiken

### Door Privacy Service UI te gebruiken {#privacy-service-ui}

A [&#x200B; volledige zelfstudie &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) op hoe te om tot Privacy Service toegang te hebben en gebruikersinterface te gebruiken is online door de diensten van Adobe I/O beschikbaar. Bovendien kunnen klanten deze koppeling gebruiken om toegang te krijgen tot een bibliotheek met video&#39;s en artikelen over privacyregels. Klik op het menu Adobe Experience Cloud en GDPR. Hiermee wordt een aantal video&#39;s geopend - &quot;GDPR UI How-to&quot; legt uit hoe u deze kunt gebruiken.

In de gebruikersinterface moeten klanten hun eigen IMSOrgID laden en een JSON met GDPR-aanvraaggegevens voor elk product.

### Privacy Service API gebruiken {#privacy-service-api}

Adobe Experience Platform Privacy Service biedt een gemeenschappelijke, gecentraliseerde facilitering van toegang tot/verwijderingsverzoeken en opt-out-of-sales aanvragen voor privégegevens.

De **Privacy Service API documentatie** behandelt diepgaand hoe een klant van Adobe met Adobe API kan integreren.

**visualiseer API vraag met Postman (een vrije, derdesoftware):**

* [&#x200B; Privacy Service API Postman inzameling op GitHub &#x200B;](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [&#x200B; de gids van de Video voor het creëren van het milieu van Postman &#x200B;](https://video.tv.adobe.com/v/28832)
* [&#x200B; Stappen voor het invoeren van milieu&#39;s en inzamelingen in Postman &#x200B;](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API wegen:**

* PLATFORM Gateway URL: `https://platform.adobe.io/`
* Basispad voor deze API: `/data/core/privacy/jobs`
* Voorbeeld van een volledig pad: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Vereiste kopballen:**

* Voor alle aanroepen zijn de headers `Authorization` , `x-gw-ims-org-id` en `x-api-key` vereist. Voor meer informatie over hoe te om deze waarden te verkrijgen, zie het **authentificatieleerprogramma**.
* Alle aanvragen met een payload in de aanvraagtekst (zoals POST-, PUT- en PATCH-aanroepen) moeten de header `Content-Type` met de waarde `application/json` bevatten.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
