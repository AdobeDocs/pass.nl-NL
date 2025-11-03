---
title: Metagegevens gebruiker
description: Metagegevens gebruiker
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# (Verouderd) Gebruikersmetagegevens {#user-metadata}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

>[!NOTE]
>
> De implementatie van REST API wordt begrensd door [&#x200B; Throttling mechanisme &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-eindpunten {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Productie - [&#x200B; api.auth.adobe.com &#x200B;](http://api.auth.adobe.com/)
* Het opvoeren - [&#x200B; api.auth-staging.adobe.com &#x200B;](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Productie - [&#x200B; api.auth.adobe.com &#x200B;](http://api.auth.adobe.com/)
* Het opvoeren - [&#x200B; api.auth-staging.adobe.com &#x200B;](http://api.auth-staging.adobe.com/)

</br>

## Beschrijving {#description}

Haal metagegevens op die MVPD over de geverifieerde gebruiker heeft gedeeld.


| Endpoint | Geroepen </br> door | Invoer   </br> Params | HTTP </br> Methode | Antwoord | HTTP-respons </br> |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Streaming App </br></br> of </br></br> de Dienst van de Programmer | &#x200B;1. aanvrager </br> 2.  deviceId (Verplicht) </br> .  device_info/x-apparaat-Info (Verplicht) </br> 4.  deviceType </br> 5.  deviceUser (Afgekeurd) </br> 6.  appId (afgekeurd) | GET | XML of JSON bevatten gebruikersmetagegevens of foutgegevens als dit niet lukt. | 200 - Succes<p>404 - Geen metagegevens gevonden<p>412 - Ongeldige token AuthN (bijvoorbeeld verlopen token) |


| Invoerparameter | Beschrijving |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aanvrager | De programmeeraanvragerId waarvoor deze verrichting geldig is. |
| deviceId | Het apparaat-id bytes. |
| device_info/<p>X-Apparaat-Info | Informatie over streaming apparaat.</br></br> **Nota:** Dit KAN device_info als parameter worden overgegaan URL, maar wegens de potentiële grootte van deze parameter en beperkingen op de lengte van GET URL, ZOU het als x-Apparaat-Info in de HTTP- kopbal moeten worden overgegaan. </br></br> zie de volledige details in [&#x200B; het overgaan Apparaat en de Informatie van de Verbinding &#x200B;](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Het apparaattype (bijvoorbeeld Roku, PC).</br></br> Als deze parameter correct wordt geplaatst, biedt ESM metriek aan die [&#x200B; uitgesplitst per apparatentype &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) wanneer het gebruiken van Clientless zijn, zodat de verschillende soorten analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.</br></br> Zie [&#x200B; Voordelen om clientless apparatentype parameter in de metriek van de Pas te gebruiken &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Nota:** vervangt `device_info` deze parameter. |
| _deviceUser_ | De herkenningsteken van de apparatengebruiker.</br></br> **Nota:** Indien gebruikt, `deviceUser` zou de zelfde waarden moeten hebben zoals in [&#x200B; creeer de verzoek van de Code van de Registratie &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | De toepassings-id/-naam. </br></br> **Nota:** vervangt `device_info` deze parameter. Indien gebruikt, `appId` zou de zelfde waarden moeten hebben zoals in [&#x200B; creeer de verzoek van de Code van de Registratie &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!NOTE]
> 
>De informatie van gebruikersmeta-gegevens zou beschikbaar moeten zijn nadat de authentificatiestroom is voltooid, maar kan op de toestemmingsstroom, afhankelijk van MVPD en het meta-gegevenstype worden bijgewerkt.




## Samplereactie {#sample-response}

Na een geslaagde aanroep zal de server reageren met een XML- (standaard) of JSON-object met een structuur die vergelijkbaar is met de hieronder weergegeven structuur:


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

Aan de basis van het object bevinden zich drie knooppunten:

* *bijgewerkt*: specificeert een timestamp van UNIX die de laatste tijd vertegenwoordigt de meta-gegevens werden bijgewerkt. Dit bezit zal aanvankelijk door de server worden geplaatst wanneer het produceren van de meta-gegevens tijdens de authentificatiefase. Volgende aanroepen (nadat de metagegevens zijn bijgewerkt) resulteren in een verhoogde tijdstempel.
* *gegevens*: bevat de daadwerkelijke meta-gegevenswaarden.
* *gecodeerd*: een serie die van de gecodeerde eigenschappen een lijst maken. Om een specifieke meta-gegevenswaarde te decrypteren, moet de programmeur een Base64 decoderen op de meta-gegevens dan toepassen een decryptie RSA op de resulterende waarde, gebruikend het eigen privé sleutel (Adobe codeert de meta-gegevens op de server gebruikend het openbare certificaat van de Programmer).

Bij een fout retourneert de server een XML- of JSON-object dat een gedetailleerd foutbericht opgeeft.

Voor meer informatie, zie [&#x200B; Metagegevens van de Gebruiker &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

[Terug naar REST API Reference](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
