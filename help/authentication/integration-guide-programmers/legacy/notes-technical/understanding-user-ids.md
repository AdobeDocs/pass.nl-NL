---
title: Gebruikersnaam
description: Gebruikersnaam
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 0%

---

# (Verouderd) Gebruikersnaam {#understanding-user-ids}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

Elke gebruiker die een machtigingsstroom start, is gekoppeld aan één unieke gebruikersnaam. Tijdens een machtigingsstroom kan één gebruiker-id echter op verschillende manieren worden weergegeven, afhankelijk van de API waarvan u de id ontvangt.

De `sessionGUID` in de Short Media Token is de veilige vorm van de Gebruiker - identiteitskaart, die via de `sendTrackingData()` vraag beschikbaar is. In alle huidige integratie, is dit een blijvende GUID voor de gebruiker over tijd en apparaten. De bron van GUID begint met Gebruiker - identiteitskaart van de authentificatiereactie die uit MVPD komt. Nochtans, konden sommige MVPDs hun mening in de toekomst veranderen, en beginnen een voorbijgaande GUID te verzenden. Als een programmeur ervoor wil zorgen dat de MVPD-brongebruiker-id in de AuthN-respons blijvend is, moet hij dat regelen in zijn overeenkomsten met de MVPD.

Hier volgen de verschillende manieren waarop de gebruikersnaam wordt weergegeven in de API&#39;s voor Adobe Pass-verificatie:

| Eigenschap | Doel | Onderbroken | Digitaal ondertekend | Beschrijving |
| --- | --- | --- | --- | --- |
| sendTrackingData() GUID-eigenschap | Tracering/analyse | Ja | Nee | - De MVPD-gebruikersnaam, gehasht op Adobe. De gebruikersnaam is niet terug te vinden naar de bron op de MVPD. </br> </br> - Deze vorm van identiteitskaart wordt niet digitaal ondertekend, zodat is het niet veilig voor fraudepreventie. Het is echter goed genoeg voor analyses.  </br> </br> - Deze vorm van de Gebruiker - identiteitskaart wordt verstrekt cliënt-kant op alle gebeurtenissen die de Authentificatie van Adobe Pass in de stroom AuthN/AuthZ produceert. |
| Het bezit sessionGUID van het token van korte media | Fraude bijhouden bij gelijktijdig gebruik | Ja | Ja | - Dit is hetzelfde als de gebruikersnaam via sendTrackingData(), maar deze is digitaal ondertekend om de integriteit ervan te beschermen en is goed genoeg om te worden gebruikt voor het bijhouden van fraude. </br> </br> - Het is bedoeld om op de server te worden verwerkt nadat u onze validatiebibliotheek hebt gebruikt en kan worden geanalyseerd op fraudepatronen voordat de videostream aan de client wordt vrijgegeven.  Het uitvoeren van om het even welk van deze taken is aan de Programmer. |
| getMetadata(), eigenschap userID | Account link, fraude-onderzoek met MVPD | Nee | Nee | - Met deze eigenschap kan de Adobe de feitelijke MVPD-gebruikersnaam van de bron aan de programmeur beschikbaar maken. </br> </br> - In de configuratie van de Adobe kan deze worden ingesteld als gecodeerd (afhankelijk van de MVPD-voorkeur). Als het gecodeerd is, wordt het versleuteld met de openbare sleutel van het certificaat van de programmeur dat aan de Adobe wordt verstrekt, zodat het niet in duidelijke taal aan de client wordt getoond. </br> </br> - Dit geeft de programmeur de daadwerkelijke identiteitskaart van de Gebruiker van de MVPD, zodat is het iets dat voor rekening kan worden gebruikt die of onderzoek direct met de MVPD verbindt of. |


**In conclusie**

* In het algemeen, verstrekt MVPD blijvende unieke identiteitskaart <u> en gaat het tot Adobe over op succesvolle authentificatie </u>. Het is over het algemeen verenigbaar over alle netwerken. De uitzondering hierop is Comcast MVPD, die voor elk kanaal een andere gebruikersnaam biedt.

* De MVPD-gebruikersnaam bevat geen PII en is GEEN rekeningnummer. Het hoeft niet in gecodeerde vorm te worden aangeboden, aangezien we met alle MVPD&#39;s hebben gevalideerd die geen PII worden verzonden.

Hoe je de gebruikersnaam gebruikt, hangt af van het gebruik:

* Als u het voor tracking/analytics nodig hebt, kunt u het beste deze ophalen uit `sendTrackingData()` .
* Als u het op de server-kant voor stroomversie, fraude, of operationele gegevens nodig hebt, kunt u het van de Symbolische validator krijgen van Media Token.
* Als u het voor rekening het verbinden en diepere fraude nodig hebt, controleer met uw contact van de Adobe voor beschikbaarheid.
